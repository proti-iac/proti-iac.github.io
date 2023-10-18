---
layout: page
title: "Solution: ProTI"
description: ProTI implements Automated Configuration Testing for Pulumi TypeScript.
nav: true
nav_rank: 5
nav_title: Solution
nav_url: /#solution # Url to navigate to, e.g., on article page (defaults to .url)
---

<div class="row">
    <div class="col-12 col-lg-6" markdown="1">
We present ProTI,
our implementation of the Automated Configuration Testing framework for Pulumi TypeScript IaC programs.
ProTI builds upon the popular JavaScript testing framework [Jest](https://jestjs.io/){: target="_blank"},
implementing Jest runner, test-runner, and reporter plugins that jointly achieve automated IaC program testing.
The respective NPM packages are `@proti/runner`, `@proti/test-runner`, and `@proti/reporter`.
`@proti/core` implements the core mechanisms and abstractions,
i.e., module loading, scheduling, plugin interfaces, and utilities.
For random-based test abstractions, we reuse the popular JavaScript property-based testing implementation [fast-check](https://fast-check.dev){: target="_blank" }.
`@proti/spec` implements the inline specifications syntax,
which – only when run in ProTI – hooks into ProTI's central test scheduling and plugin interface mechanism.

Beyond the core infrastructure and abstractions,
we implement the first plugins in `@proti/pulumi-packages-schema`.
They are type-based, leveraging input and output configuration type metadata from [Pulumi package schemas](https://www.pulumi.com/docs/using-pulumi/pulumi-packages/schema/){: target="_blank" }.
By design, these are available for all resource types distributed in [Pulumi packages](https://www.pulumi.com/product/packages/){: target="_blank" }.
The generator plugin composes primitive fast-check arbitraries to a complex arbitrary
of the shape of the resource type's output configuration type
and draws random output configuration values from such arbitrary.
The oracle plugin checks all received input configurations for type compliance with the resource type's input configuration type. 

The focus of future work on ProTI is on the development of more sophisticated generator and oracle plugins.
Leveraging advanced test generation and guidance techniques, e.g., symbolic execution,
and additional and more precise models of cloud configurations for generation and validation
is the essence of further improving the effectiveness of ACT and ProTI.
</div>
    <div class="col-12 col-lg-6">
        <figure class="card border">
            <img src="{{ '/assets/img/proti-packages.svg' | relative_url }}" alt="Overview of the ProTI NPM packages" class="card-header card-img-top" />
            <figcaption class="card-footer figure-caption text-center border-0">Overview of the ProTI NPM packages.</figcaption>
        </figure>
    </div>
</div>