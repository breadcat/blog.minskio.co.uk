---
title: "Formatting and checking bash scripts"
date: 2022-02-01T11:52:00
lastmod: 2022-02-01T11:52:00
tags: ["Linux", "Software", "Snippets"]
---

Everyone's head of [shellcheck](https://github.com/koalaman/shellcheck) for checking over scripts to ensure obvious (and not-so-obvious) mistakes aren't being made. Afterwards I usually quite like to beautify/prettify/format up code to get all the usual readability improvements gained from this. In the past, I've used [beautysh](https://github.com/lovesegfault/beautysh) and while it's worked, I generally don't like using python programs when alternatives are available, and especially don't like manually installing programs when it can be done via the package manager. In steps [shfmt](https://github.com/mvdan/sh), a handy go program (no dependencies, portable, etc) that works exactly how you'd expect it to be run.

It can either be installed via [your package manager](https://github.com/mvdan/sh#readme), or you can install it from source using go:
```
go install mvdan.cc/sh/v3/cmd/shfmt@latest
```

Once installed, test it's runnable
```
shfmt --version
```

With that confirmed, you can run it against a bash script as such:
```
shfmt -l -d path/to/your/script.sh
```

In the above example, the `-l` flag formats the text and the `-d` flag shows a diff of what's been changed.
If you're happy with the changes shown, you can replace the `-d` flag with `-w` to overwrite the original file:

```
shfmt -l -w path/to/your/script.sh
```