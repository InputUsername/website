+++
title = "SadServers solutions"
date = 2026-09-14
draft = true
+++

# [Saint John](https://sadservers.com/scenario/saint-john)

```terminal
$ bad_pid=$(lsof -a -d 0-256 | grep '/var/log/bad.log' | awk '{print $2}')
$ kill $bad_pid
```

# [Saskatoon](https://sadservers.com/scenario/saskatoon)

```terminal
cat /home/admin/access.log | awk '{print $1}' sort | uniq -c | sort -bg
```

# [Nara](https://sadservers.com/scenario/nara)

...
