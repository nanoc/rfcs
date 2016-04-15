---
start_date: 2016-04-15
rfc_issue: (leave this empty)
nanoc_issue: (leave this empty)
---

# Immutable preprocessor

Building an immutable preprocessor allows analyzing its effects, allowing further performance optimizations.

## Motivation

The preprocessor can change the state of anything in the site. This makes it hard to analyze the effects of change, which in turn prevents useful optimizations.

An immutable preprocessor is a prerequisite for letting data sources find objects (see [#1](https://github.com/nanoc/rfcs/pull/1)).

## Detailed design

TODO

## Drawbacks

TODO

## Alternatives

TODO

## Unresolved questions

TODO
