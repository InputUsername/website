+++
title = "I finished my master's thesis!"
date = 2024-08-07
+++

Last Thursday, August 1<sup>st</sup>, I finally submitted my master's thesis. Over the past six months, I researched how developers of cloud software address cost issues in their Infrastructure as Code codebases. I analyzed a set of commits on Terraform files to find cost-effective and -ineffective practices, and extracted a catalog of patterns and antipatterns for cost management. The catalog was actually turned into a paper and accepted at the *50th Euromicro Conference Series for Software Engineering and Advanced Applications (SEAA) 2024*, which is pretty cool.

After creating the catalog, I implemented rules in two popular IaC linters, [Checkov](https://checkov.io) and [TFLint](https://github.com/terraform-linters/tflint) to detect instances of several of the patterns and antipatterns, to hopefully help developers find out when they are writing cost-inefficient Terraform. The thesis was awarded a grade of 9.5/10, and I'm proud of the end result. You can read it [here](https://fse.studenttheses.ub.rug.nl/id/eprint/33811).

Finishing the thesis also means I've graduated (with honors) from my master's degree in Software Engineering and Distributed Systems. I'm excited to see what the future holds!
