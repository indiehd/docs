# Pull Requests

!> Failure to follow the the following guidelines will result in the PR being closed `with explanation`!

## Branching

Always base your changes on the `master` branch.

When you create a pull request, always select the `master` branch as the target.

## Tests

All `feature` pull requests will `require` unit and/or feature tests. 
 
## Bug Fixes

If your pull request addresses an existing issue, you are `required` to liberally reference the issue # in the 
description. 

Otherwise, you `must` fully describe the issue that the PR addresses in detail with 
steps to reproduce the issue. 

?> Branch naming convention example `fix-password-hashing`, *always* prefixed with `fix-`!

## Proposals

If your pull request addresses a issue tagged as `feature` or `enhancement`, you are `required` to liberally reference 
the issue # in the description *AND* you `must` fully describe the implementation in detail.

Otherwise, just simply describe the implementation in detail.

?> Branch naming convention example `feat-stripe`, *always* prefixed with `feat-`!

## Patches

What is a patch ? We consider a patch as fixing `typos, grammar,` or any other type of `refactoring`.

If your pull request addresses a issue already created, you are `required` to liberally reference 
the issue # in the description.

Otherwise, you `must` fully describe the issue that the PR addresses in detail. 

?> Branch naming convention example `patch-artist-controller` or `refact-artist-repository`, 
*always* prefixed with `patch-` or `refact-`!

## Reviews

* Every PR should request *at-least* a review from `cbj4074` and/or `poppabear8883`

## WIP

Prefixing a PR with `WIP` means ***`don't merge`***, only review!
    
This workflow is useful for requesting reviews while you are in-progress of adding new features

Only once you are fully reviewed and all conversations have been resolved should you remove WIP from the title
   
## Merging

The author of a PR *cannot* merge their own PR.

?> This only applies to Core Developers, as Contributors won't have this ability anyways.
