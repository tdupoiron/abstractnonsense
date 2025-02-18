---
title: Surely you're `jq`ing
blog_section:
  - Microblog
date: "2025-02-18"
description: Learning `jq` by reading the manual end-to-end.
link: https://jqlang.org/manual/
---

Today I read through the [`jq`](https://jqlang.org) [manual](https://jqlang.org/manual/) cover-to-cover. For those unaware, `jq` is a popular CLI tool to query and manipulate JSON. It's a mini-language with nice functional semantics and fits into the ethos of composable CLI tools.

It was an exemplar of well-written technical documentation. Concise, well-written, littered with examples, and linking to an interactive playground to test-and-learn.

Some learnings:
1. It's surprisingly functional! You can implement recursive functions and use higher-order functions! For example, here's factorial in `jq`:
```jq
jq '[.,1]|until(.[0] < 1; [.[0] - 1, .[1] * .[0]])|.[1]'
```
2. It supports [string interpolation](https://jqlang.org/manual/#string-interpolation) - this is really nice if you're piping stuff from JSON into a string. Coupled with [format strings](https://jqlang.org/manual/#format-strings-and-escaping) this becomes frictionless:
```jq
$ echo '{"search":"hello; world"}' | jq -r '@uri "https://www.google.com/search?q=\(.search)"'
# https://www.google.com/search?q=hello%3B%20world
```
3. You can define functions that accept functions[^1], and [control structures](https://jqlang.org/manual/#breaking-out-of-control-structures) that allow labelling.
```jq
echo '[[1,2],[10,20]]' | jq -r 'def addvalue(f): . + [f]; map(addvalue(.[0]))'
[[1,2,1], [10,20,10]]
```
4. You can traverse complex data structures with [first-class pathing](https://jqlang.org/manual/#path) support. And you can easily [modify nested structures](https://jqlang.org/manual/#complex-assignments) to extend objects.

**Side note**: This is one of my goals for 2025 - read through documentation end-to-end to develop mastery over tools. I'm trying to prioritise selectively depth over breadth.

[^1]: The line between "functions" and "filters" is a little blurry to me.
