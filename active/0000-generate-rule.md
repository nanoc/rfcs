---
start_date: 2016-11-15
rfc_issue: (leave this empty)
---

# Generator rule

Introduce a `generate` rule that allows generating in-memory items in a declarative way.

## Motivation

The current preprocessor is a black box, and its effects cannot be analysed. In particular, the preprocessor makes it impossible to do the following:

* Directly querying data sources from item collections is not possible, because the data from the data sources can have been mutated.

* As a result of this, sites must be loaded fully into memory, which is problematic for larger sites.

* The preprocessor must be run in full before every compilation. There are no optimisations that can be made here. This can be the cause of a significant slowdown.

The preprocessor is commonly used for generating in-memory items, such as year and tag archive pages. A declarative method for describing the generation of such items could be a full replacement for the preprocessor, without its problems.

## Detailed design

Introduce a `generate` rule that looks as follows:

```ruby
generate(@items.find_all('/blog/articles/*').map { |i| i[:year] }) do |year|
  new_item("Year #{year}", {}, "/archive/year-#{year}")
end

generate(@items.flat_map { |i| i[:tags] }) do |tag|
  new_item("tag #{tag}", {}, "/archive/tag-#{tag}")
end

generate do
  new_item('Some random item', {}, '/random')
end
```

The `generate` rule has a selector, which specifies which items to consider as the input. It also has a block, to which each element of the resulting array is yielded.

An alternate `generator` form with no selector exist, and will be called with no block argument.

## Drawbacks

TODO

## Alternatives

TODO

## Unresolved questions

TODO
