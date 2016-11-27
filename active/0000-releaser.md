---
start_date: 2016-11-27
rfc_issue: (leave this empty)
---

# Releaser

Have a single-command release process.

## Motivation

The current [release guidelines](http://nanoc.ws/contributing/#releasing-nanoc) are extensive and error-prone. The Nanoc repository has a release script that automates part of the release steps needed, but it still remains a manual process.

## Detailed design

Before a release, the following needs to be done manually (as it has been before):

* Set the new release version in `lib/nanoc/version.rb`
* Add the release notes to `NEWS.md`

The release script will perform the following steps:

1. Verify prerequisites
  * Verify push access to GitHub
  * Verify push access to RubyGems
  * Verify write access to the Nanoc Google Group
  * Verify write access to Twitter
2. Verify version about to be released
  * Verify release notes
    * Verify presence
    * Verify monotonicity of release dates
    * Verify that release date is today
  * Verify existing artifacts
    * Verify that the Git tag for this version does not yet exist
    * Verify that this gem has not yet been released
  * Verify no open issues in associated milestone
  * Run tests
  * Compile nanoc.ws twice
3. Release
  * Build and push gem
  * Create Git tag and push it to GitHub
4. Announce
  * Create GitHub release
  * Build nanoc.ws and deploy
  * Send tweet
  * Send e-mail to nanoc@googlegroups.com
5. Prepare for next release
  * Close existing milestone
  * Create new milestone

The release script should have the following properties:

* **safe**: If an erroneous condition arises, and continuing could lead to a broken release, the release script should abort. For example, if any of the pre-release verifications fail, the release should not continue.

* **resilient**: If a release cannot be made properly due to a dependent service being unavailable, the release script should be able to retry the step, or skip it if the step is deemed to be optional.

* **idempotent**: The script should be able to be run multiple times for the same version, and steps that have already executed should not cause additional effects. For example, if publishing the gem succeeds, but pushing to GitHub fails, the script should be able to be run again and pushing to GitHub should succeed without failing on the gem push step.

## Drawbacks

* Is the effort needed to write a proper release script worth the time?

## Alternatives

* Are there alternative release scrips that would work for Nanoc?

## Unresolved questions

(none)