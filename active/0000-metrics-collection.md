---
start_date: 2016-09-04
rfc_issue: (leave this empty)
nanoc_issue: (leave this empty)
---

# Metrics collection

Collect high-level information about Nanoc usage in order to focus future development.

## Motivation

Because Nanoc is an offline application, we have no idea about how is it used. Having metrics on usage can given  clarity on where pain points lie. Specifically, it can answer the following questions:

* **Which version of Nanoc do people use?** Nanoc should be easy to upgrade as it uses semantic versioning. I don’t expect to see older versions still commonly used.

* **What operating systems is Nanoc used on?** While Nanoc is typically used on Unix-like systems, is is also being used elsewhere (Windows, specifically). Figuring out which OSes and which OS versions are used will allow us to determine which platforms Nanoc is supported on.

* **What Ruby versions is Nanoc run on?** Answering this question will allow us to drop, or not drop, support for certain Ruby versions or platforms (JRuby, …).

* **How often are the legacy identifiers and/or patterns used?** Upgrading from Nanoc 3.x to Nanoc 4.x is especially easy when sticking to legacy identifiers and patterns, but their use is discouraged. A large fraction of legacy usage could indicate that the upgrade documentation is below expectations.

* **How long do Nanoc sites take to compile?** Answering these questions will clarify how worth it is to focus on optimizing the compilation of large sites.

* **How many of items in a site are binary? How much time is spent compiling binary items?** This will give an indication on how/if asset management is used.

* **Do people use the preprocessor? How long does the preprocessor take to run?** The preprocessor is a historical anomaly, and the current thinking is to rethink it fundamentally (see e.g. [RFC 4](https://github.com/nanoc/rfcs/pull/4)). If the preprocessor is commonly used, then perhaps Nanoc lacks power in its compilation flow, or perhaps the preprocessor is just too easy to use.

* **Do people use the postprocessor? How long does the postprocessor take to run?** The preprocessor is a fairly new feature. If the postprocessor takes a long time to run, then this could indicate that the postprocessor is used instead of Nanoc’s compilation workflow.

## Detailed design

Nanoc will emit events at certain points, enrich them with information about the system and the site, and send these events to nanoc.ws, where they will be collected, deduplicated and spam-filtered, and finally shown in some sort of UI for analysis.

### Events

Nanoc will emit the following events:

* **Nanoc started**
  * Ruby version (`RbConfig::CONFIG` — `'MAJOR'`, `'MINOR'`)
  * Nanoc version (`Nanoc::VERSION`)
  * OS (`RbConfig::CONFIG['arch']` — also see [Rubygems’ `plaform.rb`](https://github.com/rubygems/rubygems/blob/v2.6.3/lib/rubygems/platform.rb#L19-L112))
* **Compilation started**
  * Configuration details
    * `string_pattern_type`
    * `identifier_type`
    * `enable_output_diff`
    * `auto_prune`
  * Site details
    * Number of items, as base-10 logarithm, rounded down
    * Number of layouts, as base-10 logarithm, rounded down
* **Compilation ended**
  * Success or failure?
  * Preprocessing duration
  * Compilation duration
  * Number of outdated items
* **Command invoked**
  * Name of command (if part of standard set of commands)

### Privacy considerations

In order to not violate anyone’s privacy, the data that is sent out for collection will be anonymized and reduced to the bare usable minimum.

* When sending numeric metrics, do not send the exact number, but rather bucket them. For instance, if compilation takes 17.3s, report the duration as being in the 10-20s bucket.

* When sending string metrics, such as the Ruby version or the string pattern type, match the value agaist well-known predefined values, and send the predefined value instead. For instance, if we were to send a list of invoked commands, classify all command names that the Nanoc source code does now know about as “other”.

Additionally, the Nanoc CLI will ask on start-up for permission to send this anonymous usage information. (This should happen only once, and only when in an interactive terminal, e.g. using `Nanoc::CLI.tty?`.) When the collected data changes (e.g. a certain metric becomes collected more accurately, or new metrics are collected), Nanoc will ask for permission again.

### Collection and analysis

Each event will have a unique identifier for the site and the computer that it originates from. This unique identifier will be used for deduplication purposes.

**TODO:** Figure out how to build this unique identifier.

**TODO:** Describe how to collect events.

**TODO:** Describe how to analyse events.

### Other considerations

* **Be generic:** This metrics collection could be implemented in a generic fashion, as to be reusable for other projects.

* **Be resilient:** Networks can be down or slow, and Nanoc should continue to work with little or no impact on performance. If metrics cannot be sent, either discard them or queue them on-disk for future emission. Correctness is not a goal; events can and should be dropped if necessary.

* **Be safe:** Adding an endpoint on the Nanoc web site for metric collection means creating an attack vector.

* **Be spam-proof**

## Drawbacks

TODO

## Alternatives

None.

## Unresolved questions

None.
