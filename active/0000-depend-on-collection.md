---
start_date: 2017-06-16
rfc_issue: (leave this empty)
---

# Track dependency onto document collections

By tracking dependencies not on individual items/layout, but also on a collection of items/layouts, Nanoc will be able to determine whether or not an existing item has a potential dependency on a newly added item, and prevent unneeded recompilation.

## Motivation

Currently, adding a new item to a site will cause all items to be recompiled. The reason for this is that Nanoc currently can’t be sure whether the addition of a new item will affect an existing item. While it doesn’t yield incorrect results, it can be slow.

## Detailed design

For each item, track whether or not it accesses `@items` and/or `@layouts`. When a new item (resp. layout) is added, only consider items outdated when it depends on `@items` (resp. `@layouts`).

PENDING

## Drawbacks

None.

## Alternatives

None.

## Unresolved questions

None.
