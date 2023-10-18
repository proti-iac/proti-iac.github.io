---
layout: page
title: Getting Started
description: Instructions on how to start using ProTI.
nav: true
nav_rank: 6
nav_url: /#getting-started # Url to navigate to, e.g., on article page (defaults to .url)
---

To work with ProTI you require an installation of [NodeJS](https://nodejs.org/){: target="_blank"} with NPM.
ProTI is developed with and supports NodeJS 18 LTS.

<div class="row">
    <div class="col-12 col-lg-6" markdown="1">

### Using ProTI

1. [Set up Jest](https://jestjs.io/docs/getting-started){: target="_blank"} in the project. Using NPM and `ts-jest` for the transpilation, you can run these commands in the project directory:
```bash
npm install --save-dev jest ts-jest
```

2. Install [`@proti-iac/runner`](https://github.com/proti-iac/proti/tree/main/proti-runner/){: target="_blank"} and [`@proti-iac/test-runner`](https://github.com/proti-iac/proti/tree/main/proti-test-runner){: target="_blank"}:
```bash
npm install --save-dev @proti-iac/runner @proti-iac/test-runner
```

3. Configure Jest to invoke ProTI. The easiest way to do so is to inherit the ProTI configuration preset from [`@proti-iac/test-runner`](https://github.com/proti-iac/proti/tree/main/proti-test-runner){: target="_blank"}. You can configure Jest by creating a `jest.config.js` file in the root of your project with this content:
```js
/**
 * For a detailed explanation regarding each configuration property, visit:
 * https://jestjs.io/docs/configuration
 */
/** @type {import('jest').Config} */
const config = {
    preset: "@proti-iac/test-runner",
};
module.exports = config;
```
Add further configuration to the file to augment Jest's, ts-jest's, and ProTI's default configuration. The default configuration configures a simple empty state test generator and an oracle that only checks URN uniqueness across all resources, which are implemented in [`@proti-iac/core`](https://github.com/proti-iac/proti/tree/main/proti-core){: target="_blank"}. Most likely, you want to configure more sophisticated generator and generator plugins. [Configuring ProTI](https://github.com/proti-iac/proti/#configuring-proti){: target="_blank"} describes how. Concretely, [@proti-iac/pulumi-packages-schema's README](https://github.com/proti-iac/proti/tree/main/proti-pulumi-packages-schema){: target="_blank"} describes how to install and configure our first type-based plugins.

4. Run ProTI by running Jest on the project:
```bash
npx jest
```
</div>
<div class="col-12 col-lg-6" markdown="1">
### Using ProTI's Inline Specifications

To use ProTI's inline specification syntax, additionally, install the [`@proti-iac/spec`](https://github.com/proti-iac/proti/tree/main/proti-spec){: target="_blank"} package as a dependency (not only as a development dependency):

```bash
npm install --save @proti-iac/spec
```

Simply import the package in your IaC program's code and use the syntax it exports:

```ts
import * as ps from "@proti-iac/spec";
```

As an example of its use, you can have a look at the [correct random word website example with ProTI inline specifications](https://github.com/proti-iac/proti/blob/main/examples/random-word-webpage/correct-proti-spec/index.ts){: target="_blank"}.
{: .mb-6 }

### Detailed Reporting

For detailed reporting in CSV format, additionally install the [`@proti-iac/reporter`](https://github.com/proti-iac/proti/tree/main/proti-reporter){: target="_blank"} package, and [configure it as Jest reporter](https://jestjs.io/docs/configuration#reporters-arraymodulename--modulename-options){: target="_blank"}.
{: .mb-6 }

### Developing ProTI Plugins

ProTI is extended through generator and oracle plugins. Implementing either is simple and demonstrated in [`@proti-iac/plugins-demo`](https://github.com/proti-iac/proti/tree/main/proti-plugins-demo){: target="_blank"}. This package serves as a blueprint for new plugins. Please refer to its code and documentation for further details.
</div>
</div>