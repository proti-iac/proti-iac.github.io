---
layout: page
title: "Approach: Automated Configuration Testing"
description: Automated Configuration Testing (ACT) is our approach to solve the IaC program testing problem.
nav: true
nav_rank: 4
nav_title: Approach
nav_url: /#approach # Url to navigate to, e.g., on article page (defaults to .url)
---

<div class="row">
    <div class="col-12 col-lg-6" markdown="1">
To solve the problem of testing IaC programs,
we propose the Automated Configuration Testing (ACT) framework.
ACT automatically mocks the IaC program under test.
Its mock receives all resource input configurations and returns for each realistic output configuration.
To obtain the output configurations,
it employs an interface for *generator plugins*.
Such an interface enables the development and reuse of generalized test generation strategies as exchangeable plugins.
Similarly, ACT has an interface for *oracle plugins*,
implementing reusable input configuration validation strategies.
ACT fully automates mocking with test generation and validation,
allowing to automatically test the IaC program in many different configurations.

ACT moves the major effort of mocking IaC programs for unit testing
from the development of an individual IaC program to the community,
just like Pulumi's providers do for resource type classes.
Once the community developed the framework and a set of suitable generator and oracle plugins,
new and existing IaC programs can easily be unit tested,
often without writing a single line of testing code.
</div>
    <div class="col-12 col-lg-6">
        <figure class="card border">
            <img src="{{ '/assets/img/act.svg' | relative_url }}" alt="Overview of the Automated Configuration Testing (ACT) approach." class="card-header card-img-top mt-3" />
            <figcaption class="card-footer figure-caption text-center border-0">Overview of our Automated Configuration Testing (ACT) approach.</figcaption>
        </figure>
    </div>
</div>


<div class="row">
    <div class="col-12 col-lg-6" markdown="1">
Reusable ACT test generator and oracle plugins implement generalized and reusable
testing strategies and models.
Still, there may be cases where developers can leverage deployment-specific knowledge
for more precise test generation and validation.
For these cases, we propose inline specifications,
where developers can annotate more precise test generation and validation hints directly in the program.
During regular execution, such inline specifications are ignored.
During testing, they guide the test generator and oracles.
For instance,
this code is an extract of the random word webpage example with added ACT inline specifications.
In line&nbsp;1, we surrounded `rng.result` with `ps.generate(...).with(ps.integer(rngRange))`
to specify a more precise value generator for `rng.result`.
In lines&nbsp;7 and&nbsp;8, we surrounded `words[wordId].toLowerCase()`
with `ps.expect(...).to((s) => s.length > 0)` 
to add a check that the index' content is not empty.
</div>
    <div class="col-12 col-lg-6">
        <figure class="card border">
            <div class="card-header">
                <div class="row justify-content-center">
                    <div class="col-12" style="font-size: 0.9em" markdown="1">
```ts
1  ps.generate(rng.result).with(ps.integer(rngRange))
2    .apply((wordId) => {
3      new aws.s3.BucketObject("index", {
4          bucket: bucket, key: "index.html",
5         contentType: "text/html; charset=utf-8",
6          content: "<!DOCTYPE html>" +
7              ps.expect(words[wordId].toLowerCase())
8                .to((s) => s.length > 0)
9      });
10 });
```
{: .mb-n3 }
</div>
                </div>
            </div>
            <figcaption class="card-footer figure-caption text-center border-0">Exerpt of the example with ACT inline specifications.</figcaption>
        </figure>
    </div>
</div>