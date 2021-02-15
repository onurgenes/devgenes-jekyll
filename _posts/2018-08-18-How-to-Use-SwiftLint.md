---
title: How to Use SwiftLint?
author: Onur Genes
date: 2019-08-18 16:51:00 +0300
tags: [ios]
math: false
categories: [iOS]
toc: true
image:
    src: /assets/img/swiftlint-header.png
    alt: Watch Your Language! - Swift Lint
---


# How to Use SwiftLint?
SwiftLint is a tool for making your code clean and maybe avoid bugs.

Basically this is a code styling tool. Which is not going to make you a “10x Developer” but anyway…

## Getting Started
First of all we need to get “SwiftLint” for “linting” our codebase.

There are couple ways to do that:
1. Download with [HomeBrew](https://brew.sh/).
2. Use CocoaPods.
3. Using [Mint](https://github.com/yonaskolb/mint)  etc.

But in this tutorial we will use HomeBrew.

It is really easy to install with HomeBrew:
```bash
brew install swiftlint
```

## How to use?
Well, there are 2 ways to use.

- Go to root directory of your project and run 
```bash
swiftlint
```
- The other and recommended way is adding a `Run Script Phase` to Xcode:
	- On `Project Navigator` click on the blue project icon
	- Go to `Build Phases` and click `+`
	- Select `New Run Script Phase`
	- Add paste the following:
```bash
if which swiftlint >/dev/null; then
  swiftlint
else
  echo "warning: SwiftLint not installed, download from https://github.com/realm/SwiftLint"
fi
```

Now every time you build your project, SwiftLint will run and check for warnings and errors.

## Basic rules
With default settings of SwiftLint, you can get many errors even on a basic project. Also it will check files which in `CocoaPods`or `Carthage`folders.

To avoid that we have to use a file called `.swiftlint.yml`.

You can check out all rules on terminal with
```bash
swiftlint rules
```

I am giving you a basic yml rules for excluding unnecessary rules. It is working for me but you may have to change it a little bit:

```yaml
disabled_rules: # rule identifiers to exclude from running
  - trailing_whitespace
# opt_in_rules: # some rules are only opt-in
#   - empty_count
#   # Find all the available rules by running:
#   # swiftlint rules
# included: # paths to include during linting. `--path` is ignored if present.
#   - Source
excluded: # paths to ignore during linting. Takes precedence over `included`.
  - Carthage
  - Pods
  - Source/ExcludedFolder
  - Source/ExcludedFile.swift
  - Source/*/ExcludedFile.swift # Exclude files with a wildcard
# analyzer_rules: # Rules run by `swiftlint analyze` (experimental)
#   - explicit_self

# # configurable rules can be customized from this configuration file
# # binary rules can set their severity level
# force_cast: warning # implicitly
# force_try:
#   severity: warning # explicitly
# # rules that have both warning and error levels, can set just the warning level
# # implicitly
line_length: 160
# # they can set both implicitly with an array
# type_body_length:
#   - 300 # warning
#   - 400 # error
# # or they can set both explicitly
# file_length:
#   warning: 500
#   error: 1200
# # naming rules can set warnings/errors for min_length and max_length
# # additionally they can set excluded names
# type_name:
#   min_length: 4 # only warning
#   max_length: # warning and error
#     warning: 40
#     error: 50
#   excluded: iPhone # excluded via string
# identifier_name:
#   min_length: # only min_length
#     error: 4 # only error
#   excluded: # excluded via string array
#     - id
#     - URL
#     - GlobalAPIKey
# reporter: "xcode" # reporter type (xcode, json, csv, checkstyle, junit, html, emoji, sonarqube, markdown)
```

You can play around with them to fit your project styling.

# Conclusion
[SwiftLint](https://github.com/realm/SwiftLint) is a really great rule for keeping consistent styled code base. Also you can customize freely for your own rules.

Try to implement it right away and if you have any questions, feel free to reach me out.
