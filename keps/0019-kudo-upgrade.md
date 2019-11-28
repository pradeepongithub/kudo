---
kep-number: 19
title: Upgrades of KUDO installation
authors:
  - "@aneumann"
owners:
  - "@aneumann"
editor: TBD
creation-date: 2019-11-28
last-updated: 2019-11-28
status: provisional
see-also:
  - KEP-1
  - KEP-2
replaces:
  - KEP-3
superseded-by:
  - KEP-100
---

# Upgrading KUDO

This is the title of the KUDO Enhancement Proposal (KEP).
Keep it simple and descriptive.
A good title can help communicate what the KEP is and should be considered as part of any review.

The title should be lowercased and spaces/punctuation should be replaced with `-`.

To get started with this template:
1. **Make a copy of this template.**
  Name it `YYYYMMDD-my-title.md`.
1. **Fill out the "overview" sections.**
  This includes the Summary and Motivation sections.
1. **Create a PR.**
  Name it `[KEP NUMBER] Title`, e.g. `[KEP 0002] Initial work on Dynamic CRDs`.
  Assign it to owner(s) that are sponsoring this process.
1. **Merge early.**
  Avoid getting hung up on specific details and instead aim to get the goal of the KEP merged quickly.
  The best way to do this is to just start with the "Overview" sections and fill out details incrementally in follow on PRs.
  View anything marked as a `provisional` as a working document and subject to change.
  Aim for single topic PRs to keep discussions focused.
  If you disagree with what is already in a document, open a new PR with suggested changes.

The canonical place for the latest set of instructions (and the likely source of this file) is [here](/keps/0000-kep-template.md).

The `Metadata` section above is intended to support the creation of tooling around the KEP process.
This will be a YAML section that is fenced as a code block.
See the [KEP process](/keps/0001-kep-process.md) for details on each of these items.

## Table of Contents

A table of contents is helpful for quickly jumping to sections of a KEP and for highlighting any additional information provided beyond the standard KEP template.
[Tools for generating][] a table of contents from markdown are available.

* [Table of Contents](#table-of-contents)
* [Summary](#summary)
* [Motivation](#motivation)
    * [Goals](#goals)
    * [Non-Goals](#non-goals)
* [Proposal](#proposal)
    * [User Stories [optional]](#user-stories-optional)
      * [Story 1](#story-1)
      * [Story 2](#story-2)
    * [Implementation Details/Notes/Constraints [optional]](#implementation-detailsnotesconstraints-optional)
    * [Risks and Mitigations](#risks-and-mitigations)
* [Graduation Criteria](#graduation-criteria)
* [Implementation History](#implementation-history)
* [Drawbacks [optional]](#drawbacks-optional)
* [Alternatives [optional]](#alternatives-optional)
* [Infrastructure Needed [optional]](#infrastructure-needed-optional)

[Tools for generating]: https://github.com/ekalinin/github-markdown-toc

## Summary

TBD

## Motivation

We strive for quick and regular updates of KUDO. We need a process for upgrading all the moving parts of KUDO, and how 
the different parts interact, and what kind of compatibility we want to provide

### Goals

- How updates of KUDO are executed
  - Updates of CRDs
  - Updates of the KUDO manager
  - Updates of prerequisites (Namespaces, RoleBindings, ServiceAccounts, etc.)
- How and if multiple versions of CRDs are maintained
- How different versions of KUDO control work with different installed versions
- How to handle operators that are not supported by a new KUDO version

### Non-Goals

- Updates/Upgrades of operators and operator versions itself
- Versioning of KUDO itself

## Proposal

### KUDO Prerequisites

Expected update frequency: Low
Versioned: No Version, but closely tied to KUDO manager

The KUDO manager has a set of prerequisites that need to be present for the manager to run successfully. They are
the least likely to change, but probably the most specific. If we make changes here, we need to implement custom
migration code.
- Can we just delete them all and reinstall them? Probably not,  
- TODO: List all specific prereqs

### KUDO Manager

Expected update frequency: High
Versioned: Yes

The KUDO Manager is defined by an image version in a deployment set. To update, the deployment must be updated.
- Do we need to ensure that the manager is not doing meaningful work at the moment, or can we just update the deployment?

### CRDs

Expected update frequency: Medium
Versioned: Yes, with a CRD-Version

The CRDs are used to store installed operators and running instances. New features will regularly require us to add new
fields. 
- CRDs need to be migrated to new versions
- K8s CRD support
    - Which K8s version do we want/need to support?
    - TODO: Which K8s version supports versioned CRDs with which features?
    - At the moment, we use v1beta1

### KUDO control

Expected update frequency: High

KUDO control is the command line tool to manage KUDO. It will be often updated to add new features and fix bugs. It needs
to be in sync with the installed CRDs, as it's writing them directly with the K8s extension API.

- Do we allow an older KUDO ctrl to be used with a newer KUDO installation?

### User Stories

#### Story 1

An Operator wants to upgrade KUDO to the latest version to utilize new features. All installed operators should continue
to work.

#### Story 2

An Operator manages two K8s clusters with different KUDO versions installed. How does he manage to control both in the
most easy way?

### Implementation Details/Notes/Constraints TODO [optional]

What are the caveats to the implementation?
What are some important details that didn't come across above.
Go in to as much detail as necessary here.
This might be a good place to talk about core concepts and how they releate.

### Risks and Mitigations TODO

What are the risks of this proposal and how do we mitigate.
Think broadly.
For example, consider both security and how this will impact the larger kubernetes ecosystem.

## Graduation Criteria TODO

How will we know that this has succeeded?
Gathering user feedback is crucial for building high quality experiences and owners have the important responsibility of setting milestones for stability and completeness.
Hopefully the content previously contained in [umbrella issues][] will be tracked in the `Graduation Criteria` section.

[umbrella issues]: https://github.com/kubernetes/kubernetes/issues/42752

## Implementation History TODO

Major milestones in the life cycle of a KEP should be tracked in `Implementation History`.
Major milestones might include

- the `Summary` and `Motivation` sections being merged signaling owner acceptance
- the `Proposal` section being merged signaling agreement on a proposed design
- the date implementation started
- the first KUDO release where an initial version of the KEP was available
- the version of KUDO where the KEP graduated to general availability
- when the KEP was retired or superseded

## Drawbacks TODO [optional, TODO]

Why should this KEP _not_ be implemented.

## Alternatives TODO [optional]

Similar to the `Drawbacks` section the `Alternatives` section is used to highlight and record other possible approaches to delivering the value proposed by a KEP.

## Infrastructure Needed TODO [optional]

Use this section if you need things from the project/owner.
Examples include a new subproject, repos requested, github details.
Listing these here allows an owner to get the process for these resources started right away.