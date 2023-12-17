[<img src="logo.svg" align="right" width="11%">][qwik]

_[Qwik]_ Build
==============
GitHub [Action] to `build` your Qwik[[_City_]] [static] site for GitHub [Pages].

Usage
-----
This action will look in [`package.`][[`yaml`]/`json`] [`scripts`] for the first script
containing a `qwik`[`build`] command and [`run`] it with [[`p`]]`npm` or [`yarn`], as [appropriate]:
~~~ js
// package.json
"devDependencies": {
  "@builder.io/qwik": "^1.x",
  "eslint": "^8.x",
  "vite": "^5.x"
},
"scripts": {
  "lint": "eslint .",
  "build": "qwik build", // [p]npm/yarn run build
  "dev": "vite --mode ssr"
}
~~~
it will also [config]ure Qwik for [static] site generation, unless you already `run qwik add static`.

Because it runs the appropriate `build` script, support for _[Civet]_
—a language that compiles to [TypeScript] and [JSX]—can be added:
~~~ yaml
# package.yaml
packageManager: pnpm@8.12.1
devDependencies:
  "@builder.io/qwik": ^1.x
  "@danielx/civet": ^0.x
scripts:
  prebuild: civet --compile */*.civet --output .tsx
  build: qwik build # pnpm run build
~~~
Although a more official [integration] is preferred.

Options
---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
| [`inputs`]            | Default             | Description                                                                                                                     |
|:----------------------|:--------------------|:--------------------------------------------------------------------------------------------------------------------------------|
| `access-token`        | [`github.token`]    | Provide a token with permission to automatically enable Pages. [Generate] an [access token], then add to your repo [`secrets`]. |
| `branch`              | [`github.ref_name`] | Optionally specify a particular `branch` of your repository.                                                                    |
| `working-dir`         | `github.workspace`  | Optionally specify a subfolder containing source files.                                                                         |
| `build-dir`           | [`dist`]            | Optionally specify an alternative `build` folder.                                                                               |
| `node-version`        |                     | Optionally specify a _[SemVer]_ range or particular version of _[Node.js]_.                                                     |
| [`node-version-file`] | [`package.json`]    | Optionally specify a file containing the correct version of Node.                                                               |

Example
-------
`.github/workflows/pages.yml`:
~~~ yaml
on:
  push:
    branches: site

permissions:
  pages: write
  id-token: write

concurrency:
  group: pages
  cancel-in-progress: false

jobs:
  build:
    runs-on: ubuntu-latest
    environment:
      name: github-pages
      url: ${{ steps.qwik-build.outputs.page-url }}

    steps:
    - name: Qwik Build
      id: qwik-build
      uses: danielbayley/qwik-build@main
      with:
        branch: site
~~~

License
-------
[MIT] © [Daniel Bayley]

[MIT]:                  LICENSE.md
[Daniel Bayley]:        https://github.com/danielbayley

[action]:               https://docs.github.com/actions
[`inputs`]:             https://docs.github.com/actions/creating-actions/metadata-syntax-for-github-actions#inputs
[generate]:             https://github.com/settings/tokens
[access token]:         https://docs.github.com/authentication/keeping-your-account-and-data-secure/creating-a-personal-access-token
[`github.token`]:       https://docs.github.com/actions/security-guides/automatic-token-authentication
[`secrets`]:            https://docs.github.com/actions/security-guides/encrypted-secrets
[`github.ref_name`]:    https://docs.github.com/actions/learn-github-actions/contexts#github-context
[pages]:                https://pages.github.com

[semver]:               https://semver.org
[node.js]:              https://nodejs.org
[`node-version-file`]:  https://github.com/actions/setup-node/blob/main/docs/advanced-usage.md#node-version-file
[`package.json`]:       https://docs.npmjs.com/cli/configuring-npm/package-json#engines
[`package.`]:           https://docs.npmjs.com/cli/configuring-npm/package-json
[`yaml`]:               https://github.com/pnpm/pnpm/pull/1799
[`scripts`]:            https://docs.npmjs.com/cli/using-npm/scripts
[appropriate]:          https://github.com/lerepo/workspace-tools/tree/master/packages/detect-pm-cli#readme
[`yarn`]:               https://yarnpkg.com/cli
[`p`]:                  https://pnpm.io
[`run`]:                https://pnpm.io/cli/run

[qwik]:                 https://qwik.builder.io
[_city_]:               https://qwik.builder.io/docs/qwikcity#qwik-city
[static]:               https://qwik.builder.io/docs/guides/static-site-generation
[config]:               https://qwik.builder.io/docs/guides/static-site-generation#static-site-generation-config
[`build`]:              https://qwik.builder.io/docs/deployments/gcp-cloud-run#production-build
[`dist`]:               https://qwik.builder.io/docs/advanced/custom-build-dir

[typescript]:           https://typescriptlang.org
[jsx]:                  https://react.dev/learn/writing-markup-with-jsx
[civet]:                https://civet.dev
[integration]:          https://civet.dev/getting-started#building-a-project
