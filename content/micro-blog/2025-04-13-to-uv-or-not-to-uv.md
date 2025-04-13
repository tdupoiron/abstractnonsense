---
title: To `uv` or not to `uv`
blog_section:
  - micro-blog
date: "2025-04-13"
description: The magic behind uv's speed
link: https://www.youtube.com/watch?v=gSKTfG1GXYQ
---

[`uv`](https://github.com/astral-sh/uv) is a blazing fast Python package manager that aims to displace `pip`. It's a really slick tool that lets you go from `git clone` to 🚀 seamlessly. All the standard accolades apply: written in Rust, beautiful terminal UI, written by the same people that gave the Python community [`ruff`](https://github.com/astral-sh/ruff)....

But what makes it so damn fast? Under the hood, the magic is even more impressive. Charlie Marsh, the project lead, presented to Jane Street and revealed some of the inner workings. The whole talk is super interesting, but some standout highlights are:

1. `uv` has it's own [SAT solver](https://en.wikipedia.org/wiki/SAT_solver) implementation to resolve dependency package versions much faster than e.g. `pip` or `conda`.
2. There's some interesting discussion around implementing efficient representations of dependency metadata in Rust data structures with zero-copy serialisation to allow super-fast solves. (They effectively wrote their own Python version number parser and represented them cleverly inside a single `u32` to enable fast dependency comparisons)
3. Some very clever usage of [HTTP Range Requests](https://developer.mozilla.org/en-US/docs/Web/HTTP/Guides/Range_requests) to slice into [PyPi](https://pypi.org) package code to parse dependency information from compressed packages as zip files, without having to retrieve the _entire_ python library from PyPi first.
4. Some very clever cache design to enable super fast ephemeral Python [venv](https://docs.python.org/3/library/venv.html) creation.

<iframe
    width="560"
    height="315"
    src="https://www.youtube.com/embed/gSKTfG1GXYQ?si=1d7T1zEokffr4G-O"
    title="YouTube video player"
    frameborder="0"
    allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share"
    referrerpolicy="strict-origin-when-cross-origin"
    allowfullscreen
></iframe>
