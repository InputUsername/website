+++
title = "NextAuth v4 middleware and database session strategy"
date = 2024-02-05
description = "How to use NextAuth (v4) in your Next.js middleware while using the database session strategy"
+++

Next.js supports middleware that runs before a request is completed. When using NextAuth v4 [^1], you can use this to require authentication to access routes. NextAuth even includes a convenient default middleware that simply protects all routes. Unfortunately, when you try to use this default middleware in combination with the `database` session strategy (which is the default when using a database adapter), things break in subtle ways.

When using NextAuth's middleware, users are redirected to the `/api/auth/signin` route whenever they try to access any route without being signed in. There, the user can sign in (using whatever `Provider` you have configured). After successfully signing in, they are redirected back to the page they tried to access. When using the database strategy, though, users are just redirected back to the signin page again, without any indication what went wrong.

To see why this happens, let's look at the [source code] of the middleware:

[source code]: https://github.com/nextauthjs/next-auth/blob/v4/packages/next-auth/src/next/middleware.ts

```typescript,hl_lines=36-41
async function handleMiddleware(
  req: NextRequest,
  options: NextAuthMiddlewareOptions | undefined,
  onSuccess?: (token: JWT | null) => Promise<NextMiddlewareResult>
) {
  const { pathname, search, origin, basePath } = req.nextUrl

  const signInPage = options?.pages?.signIn ?? "/api/auth/signin"
  const errorPage = options?.pages?.error ?? "/api/auth/error"
  const authPath = parseUrl(process.env.NEXTAUTH_URL).path
  const publicPaths = ["/_next", "/favicon.ico"]

  // Avoid infinite redirects/invalid response
  // on paths that never require authentication
  if (
    `${basePath}${pathname}`.startsWith(authPath) ||
    [signInPage, errorPage].includes(pathname) ||
    publicPaths.some((p) => pathname.startsWith(p))
  ) {
    return
  }

  const secret = options?.secret ?? process.env.NEXTAUTH_SECRET
  if (!secret) {
    console.error(
      `[next-auth][error][NO_SECRET]`,
      `\nhttps://next-auth.js.org/errors#no_secret`
    )

    const errorUrl = new URL(`${basePath}${errorPage}`, origin)
    errorUrl.searchParams.append("error", "Configuration")

    return NextResponse.redirect(errorUrl)
  }

  const token = await getToken({
    req,
    decode: options?.jwt?.decode,
    cookieName: options?.cookies?.sessionToken?.name,
    secret,
  })

  const isAuthorized =
    (await options?.callbacks?.authorized?.({ req, token })) ?? !!token

  // the user is authorized, let the middleware handle the rest
  if (isAuthorized) return await onSuccess?.(token)

  // the user is not logged in, redirect to the sign-in page
  const signInUrl = new URL(`${basePath}${signInPage}`, origin)
  signInUrl.searchParams.append(
    "callbackUrl",
    `${basePath}${pathname}${search}`
  )
  return NextResponse.redirect(signInUrl)
}
```

The issue lies in the highlighted lines. The middleware tries to retrieve the session token (JWT) based on the user's request - but when using the database session strategy, this token doesn't exist, and `getToken` returns `null`. This means that even if you sign in successfully, the middleware will think you're not signed in and send you back to the signin page. Now, this is mentioned in a small note all the way at the bottom of the [middleware documentation]:

>Only supports the "jwt" session strategy. We need to wait until databases at the Edge become mature enough to ensure a fast experience. (If you know of an Edge-compatible database, we would like if you proposed a new Adapter)

[middleware documentation]: https://next-auth.js.org/configuration/nextjs#middleware

But it's very easy to miss, and it might just cost you two days of debugging. -_-

## The solution

The easiest way around this is to manually enable the `jwt` session strategy in `[...nextauth].ts`. There are [many downsides] to this though, and so this wasn't a solution for the project I was working on.

[many downsides]: http://cryto.net/%7Ejoepie91/blog/2016/06/13/stop-using-jwt-for-sessions/

Another approach I tried was to adapt the default middleware, replacing the `getToken` with `getServerSession`. This breaks however because for some reason you cannot reference the auth options defined in `[...nextauth].ts` from within the middleware.

The next thing I attempted was manually checking the database for a valid session in the middleware based on the user's session cookie using Prisma. This also doesn't work, because Prisma errors thinking it is being used from the edge, even though I was using Next's standalone server instead of deploying on a platform supporting the edge runtime (e.g. Vercel).

Luckily, the solution ended up being quite simple: instead of `getServerSession` we can use `getSession` to check if the user has a valid session. This sadly does mean performing an additional HTTP request to the `/auth/session` endpoint on every request so it's not a permanent solution, but it's much easier than manually checking the session on every route.

The final code looks like this:

```typescript
const middleware = async (req: NextRequest) => {
  const { pathname, search, origin, basePath } = req.nextUrl;

  const signInPage = "/auth/signin";
  const errorPage = "/auth/error";
  const publicPaths = ["/_next", "favicon.ico"];

  if (
    pathname.startsWith("/api/auth") ||
    [signInPage, errorPage].includes(pathname) ||
    publicPaths.some((p) => pathname.startsWith(p))
  ) {
    return;
  }

  const sessionCookie =
    req.cookies.get("__Secure-next-auth.session-token") ??
    req.cookies.get("next-auth.session-token");

  if (sessionCookie) {
    const sessionUrl = new URL(`${basePath}/api/auth/session`, origin);
    const response = await fetch(sessionUrl, {
      headers: {
        Cookie: `${sessionCookie.name}=${sessionCookie.value}`,
      },
    });
    if (response.ok) {
      const session = (await response.json()) as Session;
      // The session is active if the returned object is not empty
      if (Object.keys(session).length > 0) {
        return;
      }
    }
  }

  // The user is not logged in; return 401 for API routes, redirect otherwise

  if (pathname.startsWith("/api")) {
    return NextResponse.json(
      { error: "Not authenticated" },
      { status: StatusCodes.UNAUTHORIZED }
    );
  }

  const signInUrl = new URL(`${basePath}${signInPage}`, origin);
  signInUrl.searchParams.append(
    "callbackUrl",
    `${basePath}${pathname}${search}`
  );
  return NextResponse.redirect(signInUrl);
};
```

[^1]: The project I was working on was stuck on NextAuth v4. Newer versions handle middleware differently, so it's possible that this issue is now resolved.
