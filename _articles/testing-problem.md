---
layout: page
title: Testing Problem
description: Demonstration of the IaC program testing problem.
nav: true
nav_rank: 3
nav_url: /#testing-problem # Url to navigate to, e.g., on article page (defaults to .url)
---

<div class="row mb-6 align-items-end">
    <div class="col-12 col-lg-6" markdown="1">
The correctness of IaC programs is crucial.
Faults can cause the entire deployment to malfunction.
Even worse, a faulty IaC program can also yield a working deployment with severe security vulnerabilities.
As IaC programs are implemented in popular general-purpose programming languages,
they are eligible for the respective testing tools.
Still, unit testing – despite being a common practice in traditional software – is barely applied to IaC programs.
We found in August 2022 that less than 1% of all public Pulumi projects on GitHub implemented a unit test.
Instead, developers solely rely on integration testing,
which is slow and resource-intensive in the case of IaC programs.

Why do developers skip unit testing IaC programs?
We conjecture it is the high development effort compared to the IaC programs themselves.
IaC programs often do not implement much complex logic,
e.g., there is no algorithmic code in the random word webpage example above.
Typically, most of the code is instantiating resource objects.
Each of these object instantiations integrates with the cloud,
which directly receives the input configuration, creates the resource,
and returns the resource's output configuration that can afterward be accessed on the resource's object in the remaining program execution.
In a unit test, this cloud integration has to be mocked.

Mocking all resource instantiations is simple.
For instance, Pulumi's runtime provides a mocking feature,
which enables developers to mock all resource instantiations with a simple mock in less than 20 lines of code.
However, unit tests with such a simplistic mock are useless.
Why? The mocks have two crucial roles.
First, they return the resources' output configuration, which is test input for the remainder of the execution.
In our example above, the mock of the random integer must return a realistic value for `rng.result`
to test the callback in lines&nbsp;14–19.
Thus, the mocks must implement a good *test generator*.
Second, to provide insight, the mocks have to validate all input configurations.
Hence, they need to implement good *test oracles*.

A suitable unit test for the random word webpage example
has to implement at least a test generator that provides values
for `rng.result` and `bucket.websiteEndpoint`.
Further, the mock should validate all provided input configurations.
As the image shows, such a test is roughly 50 lines long for our example – more than double the size of the IaC program.
And even worse, if the IaC program grows, the unit test is likely to grow even faster.
Also, the test reimplements most of the logic of the IaC program and even some logic of the cloud.
Hence, it is comparatively big and tightly coupled,
causing high development effort and slowing down future changes in the IaC program itself.

</div>
    <div class="col-12 col-lg-6">
        <figure class="card border bottom-0">
            <img class="card-header card-img-top" src="{{ '/assets/img/iac-unit-test.svg' | relative_url }}" alt="The random word webpage IaC program next to a unit test for it, which implements a suitable test generator and oracle." />
            <figcaption class="card-footer figure-caption text-center border-0">The random word webpage IaC program next to a unit test for it, which implements a suitable test generator and oracle.</figcaption>
        </figure>
    </div>
</div>