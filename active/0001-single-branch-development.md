---
start_date: 2016-08-28
rfc_issue: #5
---

# Single-brach development

Get rid of release branches and do all development on the master branch.

## Motivation

Nanoc currently uses a development approach where two branches are active at a given time:

* A _release_ branch (such as `release-4.3.x`), which is started when a new minor release is made, and lives until the next minor version is released.

* The _master_ branch, on which new features are added for the next minor version. This branch is branched into a release branch once the next minor version is released.

The release branch is regularly merged into master. This approach is less than ideal for the following reasons:

* Having two branches leads to regular merge conflicts. Even small changes made in a release branch sometimes have to be fundamentally rewritten for the master branch.

* Large refactorings have to be delayed until around a new minor release, because no work is being done on a release branch at that point.

* Code that is written for a new major release cannot easily be tested in real-world situations until it is released.

* The distinction between master and release branch is confusing, as evidenced by many contributed bug fixes being made to the master branch.

## Detailed design

Features will be developed on the master branch, as before, but hidden behind a feature flag. At the point of a minor release, these feature flags will be removed, making these features permanently accessible.

Bug fixes will be made on the master branch, rather than on a release branch. Patch releases will be made off the master branch.

### Feature flags

Nanoc will read the `NANOC_FEATURES` environment variable to determine which unreleased features to make accessible. By default, all unreleased features are disabled. The `NANOC_FEATURES` environment variable will be a comma-separated list of feature names.

For example, the following will enable the `profiler` and `environments` features:

```
NANOC_FEATURES=profiler,environments
```

The special feature name `all` will enable all features:

```
NANOC_FEATURES=all
```

### Switching to the new approach

To switch to this new approach, follow these steps:

1. Merge the release branch into the master branch.

2. Ensure all unreleased features are hidden behind a feature flag.

3. Ensure that the version number is that of the last released version, and that the release notes do not contain information about unreleased versions.

### New release strategy

The following steps will need to be performed at the start of the release process:

1. Set `Nanoc::VERSION`.

2. Add the release notes to the _NEWS.md_ file. Ensure no release notes for unreleased are present.

3. Enable all features to be released by removing the feature flag.

## Drawbacks

* It will be impossible to make a bugfix on a previous minor release without creating a branch.

* Making backwards-incompatible changes for a future major release will need to be done on a branch.

* The release notes for the new minor release cannot be written ahead of time, and must be written at the point of release.

## Alternatives

* Retain what we have.

## Unresolved questions

(none)
