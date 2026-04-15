---
layout: documentation
title: Userguide
---
# Contributing to the Userguide

This page covers contributing to the **in-application Userguide** — the documentation that ships inside Konine modules and is rendered at `/guide` when the Userguide module is enabled locally.

[!!] If you want to improve the guides on this website (konine.dev), see [Contributing to the Documentation](/documentation/kohana#contribute-to-the-documentation) instead.

## What is the in-app Userguide?

Each Konine module can ship its own documentation as Markdown files under `guide/<modulename>/`. When the Userguide module is enabled in a local application, these files are rendered alongside an API browser. Contributing to these docs means editing those files inside the main framework repository.

## How to contribute

 1. Fork [hospicedev/konine](https://github.com/hospicedev/konine) on GitHub.

 1. Clone your fork and check out the `devel` branch:

		git clone https://github.com/<your name>/konine.git
		cd konine
		git checkout devel

 1. Create a branch off `devel` for your changes:

		git checkout -b docs/my-improvement

 1. Make your changes. Module guide pages live at:

		modules/<modulename>/guide/<modulename>/

	For example, the ORM guide pages are in `modules/orm/guide/orm/`.

 1. Commit with a clear, descriptive message:

		git commit -m "docs(orm): clarify has_many relationship examples"

 1. Push your branch and open a pull request back to the `devel` branch of `hospicedev/konine`:

		git push origin docs/my-improvement

## Guidelines

- Use complete sentences and good grammar.
- Include example code where helpful; follow Konine coding conventions.
- Keep commits focused — one or two files per commit makes review easier.
- Use descriptive commit messages. Bad: `"Fixed typos"` — Good: `"Fixed typos on the ORM query builder page"`
