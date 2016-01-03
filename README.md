# Nanoc RFCs

For Nanoc 4, we’ve set up a process for collecting ideas and refining them before implementation. The RFC (_request for comment_) process is intended to provide a consistent and controlled path for new features to enter the product.

## Proposing a change

In order to propose a substantial change to Nanoc, write a RFC, open a pull request, and get it accepted. In detail:

0. Fork this repository.

0. Copy _0000-template.md_ to _active/0000-description.md_, with _description_ replaced by a short description of the proposed change.

0. Write the RFC. Fill in the necessary metadata at the top of the file.

0. Submit a pull request.

0. Collect and integrate feedback into the RFC.

At some point after this, the RFC will either be accepted by merging the pull request and assigning the RFC a number, thereby marking the RFC as active, or rejected by closing the pull request.

Modifications to active RFCs, if necessary, can be done in follow-up pull requests.

Anyone is invited to participate in the RFC review process.

## Implementing a change

The author of an RFC is not obligated to implement it. Of course, the RFC author, like any other developer, is welcome to post an implementation for review after the RFC has been accepted.

## Acknowledgements

This nanoc RFC process is greatly inspired by the [Rust RFC process](https://github.com/rust-lang/rfcs) and the [Ember RFC process](https://github.com/emberjs/rfcs).
