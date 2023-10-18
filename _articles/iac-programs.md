---
layout: page
title: IaC Programs
description: Explanation of IaC programs with the random word webpage example.
nav: true
nav_rank: 2
nav_url: /#iac-programs # Url to navigate to, e.g., on article page (defaults to .url)
---

<div class="d-none d-lg-block d-xl-none" markdown="1">
With Programming Languages Infrastructure as Code (PL-IaC) solutions like [Pulumi](https://pulumi.com){: target="_blank" },
developers implement IaC programs in a general-purpose programming language like TypeScript or Python.
The execution of such an imperative program constructs the declarative target state of the deployment,
which is then set up by the deployment engine.
The target state is a directed, acyclic graph where nodes are resources with their configuration,
and arcs are dependencies between the resources.
A node is defined by calling the constructor of the resource's class with the intended input configuration.
Once defined, the deployment engine deploys the resource and returns its output configuration,
which is accessible in the remainder of the program execution on the resource's object.
An arc in the target state is defined explicitly by referencing another resource object in a resource's configuration
or implicitly by defining the resource in a code block that depends on the output configuration of another resource.
Once defined, resource nodes and dependency arcs are immutable,
yielding that the target state graph is append-only.
</div>

<div class="row align-items-end">
    <div class="col-12 col-lg-4 col-xl-6" markdown="1">
<div class="d-lg-none d-xl-block" markdown="1">
With Programming Languages Infrastructure as Code (PL-IaC) solutions like [Pulumi](https://pulumi.com){: target="_blank" },
developers implement IaC programs in a general-purpose programming language like TypeScript or Python.
The execution of such an imperative program constructs the declarative target state of the deployment,
which is then set up by the deployment engine.
The target state is a directed, acyclic graph where nodes are resources with their configuration,
and arcs are dependencies between the resources.
A node is defined by calling the constructor of the resource's class with the intended input configuration.
Once defined, the deployment engine deploys the resource and returns its output configuration,
which is accessible in the remainder of the program execution on the resource's object.
An arc in the target state is defined explicitly by referencing another resource object in a resource's configuration
or implicitly by defining the resource in a code block that depends on the output configuration of another resource.
Once defined, resource nodes and dependency arcs are immutable,
yielding that the target state graph is append-only.
</div>
<figure class="card border float-start d-lg-none d-xl-block me-4">
    <img src="{{ '/assets/img/laptop-great.svg' | relative_url }}" alt="Random word webpage deployment illustration" class="card-header card-img-top" />
</figure>
As an example, we demonstrate the random word webpage,
a simple static website hosted in an AWS S3 bucket
that displays one randomly selected word from the `words` array defined in line&nbsp;5.
Running the Pulumi TypeScript program constructs the target state graph we show next to the code.
Lines&nbsp;6–8 define the AWS S3 `Bucket` node,
lines&nbsp;10–11 the `RandomInteger` node,
and lines&nbsp;14–19 the `BucketObject` node index.
The arc from index to bucket is defined by referencing
`bucket` in the index' input configuration in line&nbsp;15.
The arc from index to the random integer is defined implicitly
by defining the bucket object in the `apply` callback in lines&nbsp;13–20,
which is a code block that depends on the output configuration of the random integer.
Specifically, line&nbsp;13 defines that the callback is run on the `result` output of the `rng` resource object,
which is the randomly drawn number after the deployment of the random integer resource.
The drawn number is provided as the first parameter `wordId` to the callback
and used to configure the index' content in line&nbsp;18.
</div>
    <div class="col-12 col-lg-8 col-xl-6">
        <figure class="card border w-25 d-none d-lg-block d-xl-none">
            <img src="{{ '/assets/img/laptop-great.svg' | relative_url }}" alt="Random word webpage deployment illustration" class="card-header card-img-top" />
        </figure>
        <figure class="card border">
            <div class="card-header">
                <div class="row justify-content-center">
                    <img class="me-3" style="position: absolute; top: 100px; right: 0; width: 200px;" src="{{ '/assets/img/example-target-state.svg' | relative_url }}" alt="Webpage deployment resource graph" />
                    <div class="col-12" style="font-size: 0.9em" markdown="1">
```ts
 1 import * as pulumi from "@pulumi/pulumi";
 2 import * as aws from "@pulumi/aws";
 3 import * as random from "@pulumi/random";
 4
 5 const words = ["software", "is", "great"];
 6 const bucket = new aws.s3.Bucket("website", {
 7     website: { indexDocument: "index.html" }
 8 });
 9
10 const rng = new random.RandomInteger("word-id", {
11     min: 0, max: words.length
12 });
13 rng.result.apply((wordId) => {
14     new aws.s3.BucketObject("index", {
15         bucket: bucket, key: "index.html",
16         contentType: "text/html; charset=utf-8",
17         content: "<!DOCTYPE html>" +
18                  words[wordId].toLowerCase()
19     });
20 });
21
22 export const url = bucket.websiteEndpoint;
```
{: .mb-n3 }
</div>
                </div>
            </div>
            <figcaption class="card-footer figure-caption text-center border-0">Webpage deployment as Pulumi TypeScript IaC program and the resource graph it defines.</figcaption>
        </figure>
    </div>
</div>