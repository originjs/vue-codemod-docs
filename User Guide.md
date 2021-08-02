- [Introduction](#Introduction)
- [Transformation Rates](#Transformation-Rates)
- [Getting Started](#Getting-Started)
  - [Installation](#Installation)
    - [`npm/yarn`](#npmyarn)
    - [Install form GitHub](#Install-form-GitHub)
  - [Usage](#Usage)
    - [Execute All Rules](#Execute-All-Rules)
    - [Execute Specific Rule](#Execute-Specific-Rule)
    - [Output Format](#Output-Format)
    - [Manual Migration Guide](#Manual-Migration-Guide)
    - [Help](#Help)
- [Rule List](#Rule-List)

## Introduction

`vue-codemod` is a Vue 2 to Vue 3 migration tool, which can directly upgrade most of the Vue 2 syntax to Vue 3 syntax. With `vue-codemod` transformation and a small amount of manual modification, users can complete the smooth migration from Vue 2 to Vue 3.

> ⚠️Attention: Since the [`vue-codemod`](https://github.com/vuejs/vue-codemod) from Vue.js team has not been maintained for some time, the following repo is forked from Vue.js team. We are continuing to develop new features in the fork repo.

*Repository URL*: https://github.com/originjs/vue-codemod

## Transformation Rates

> ![equ](https://latex.codecogs.com/gif.latex?TransformationRates=\frac{AutoUpgraded}{(ManuallyUpgraded+AutoUpgraded)})
>
> **Manually Upgraded**: Number of patterns require manually upgrade
>
> **Auto Upgraded**: Number of patterns upgraded by `vue-codemod`

| No. | Project                                                   | Manually Upgraded | Auto Upgraded | Transformation Rates |
| :--: | :----------------------------------------------------------- | :--------- | -------- | :------ |
|  1   | [vue2-element-touzi-admin](https://github.com/wdlhao/vue2-element-touzi-admin) | 8          | 63       | 88.73%  |
|  2   | [vue-web-os](https://github.com/a241978181/vue-web-os)       | 19         | 19       | 50.00%  |
|  3   | [amKnow](https://github.com/Arlisol/amKnow)                  | 10         | 30       | 75.00%  |
|  4   | [vue-template](https://github.com/Ashyoo/vue-template)       | 61         | 28       | 31.46%  |
|  5   | [coreui-free-vue-admin-template](https://github.com/coreui/coreui-free-vue-admin-template) | 4          | 63       | 94.03%  |
|  6   | [vue-weixin](https://github.com/bailichen/vue-weixin)        | 1          | 54       | 98.19%  |
|  7   | [vue-WeChat](https://github.com/zhaohaodang/vue-WeChat)      | 9          | 35       | 79.55%  |
|  8   | [vue2-management-platform](https://github.com/suweiteng/vue2-management-platform) | 2          | 11       | 84.62%  |
|  9   | [vue-netease-music](https://github.com/sl1673495/vue-netease-music) | 18         | 37       | 67.27%  |
|  10  | [Mall-Vue](https://github.com/PowerDos/Mall-Vue)             | 2          | 26       | 92.86%  |
|  11  | [vue-demo-kugou](https://github.com/lavyun/vue-demo-kugou)   | 4          | 13       | 76.47%  |
| 12 | [codemod-demo](https://github.com/2kjiejie/codemod-demo/tree/master) | 0 | 24 | 100% |

## Getting Started

### Installation

>  `npm/yarn` install is recommended. Users can also try beta version from GitHub

#### Install from `npm/yarn`  

``` bash
npm install @originjs/vue-codemod -g
//or
yarn global add @originjs/vue-codemod
```

#### Install form GitHub

1. Clone from `dev`

```bash
git clone https://github.com/originjs/vue-codemod
```

2. Install Dependencies

```bash
cd vue-codemod

npm install
//or
yarn install
```

3. Install `vue-codemod`  Globally

```bash
npm run build
npm install . -g
//or
yarn run build
yarn global add .
```

### Usage

`vue-codemod` consists of different migration rules. Those rules are defined in `transformation/index.ts` and  `vue-transformation/index.ts`.

``` bash
npx vue-codemod <path> -t/-a [transformation params][...additional options]
```

1. `<path>` indicates the path of execution, which can be files and folders
2. `-a` means executing all rules.
3. `-t` means executing one specific rule (Conflicts with `-a`). When `-t` is used, parameter is required. 

#### Execute All Rules

``` bash
npx vue-codemod src -a
```

`src` indicates the path of execution. `-a` means executing all rules.

#### Execute Specific Rule

```bash
npx vue-codemod src -t new-global-api
```

`src` indicates the path of execution. `-t new-global-api` means only executing the rule of `new-global-api` .

[Here](#Rule-List) is the detailed rule list.

#### Output Format

```bash
npx vue-codemod src -a -f log
```

The `-f` option is used to specify the output format. The optional parameters are `table`, `detail`,`log`. If no parameters are specified, the default is `table`.

>  `table` : output the executed rules as a table
>
> `detail` : list transformed files for each rule
>
>  `log` : output a report in the form of log file

Here is a sample output of `-f table`:

```bash
╔═══════════════════════════════╤═══════╗
║ Rule Names                    │ Count ║
╟───────────────────────────────┼───────╢
║ new-component-api             │   1   ║
║ new-global-api                │   1   ║
║ vue-router-v4                 │   1   ║
║ vuex-v4                       │   1   ║
║ new-directive-api             │   1   ║
║ remove-vue-set-and-delete     │   2   ║
║ rename-lifecycle              │   2   ║
║ add-emit-declarations         │   1   ║
║ tree-shaking                  │   1   ║
║ slot-attribute                │   1   ║
║ slot-default                  │   1   ║
║ slot-scope-attribute          │   1   ║
║ v-for-template-key            │   2   ║
║ v-else-if-key                 │   3   ║
║ transition-group-root         │   1   ║
║ v-for-v-if-precedence-changed │   1   ║
║ remove-listeners              │   1   ║
║ remove-v-on-native            │   1   ║
╚═══════════════════════════════╧═══════╝
```

#### Manual Migration Guide

During the operation of `vue-codemod`, the patterns that need to be manually upgraded will be identified and printed to the console as an object (will create a log file if  `-f log` is declared). For more details, please refer to [Manual Migration Guide](Manual%20Migration%20Guide.md).

Here is the example: 

```null
The list that you need to migrate your codes mannually:
index: 1
{
  path: 'src/main.js',
  position: '[33,0]',
  name: 'remove Vue(global api)',
  suggest: "The rule of thumb is any APIs that globally mutate Vue's behavior are now moved to the app instance.",
  website: 'https://v3.vuejs.org/guide/migration/global-api.html#a-new-global-api-createapp'
}
```

#### Help

```bash
npx vue-codemod --help
```

Output:

``` bash
npx vue-codemod --help
Usage: vue-codemod [file pattern]

Options:
  -t, --transformation        Name or path of the transformation module [string]
  -p, --params                Custom params to the transformation
  -a, --runAllTransformation  run all transformation module            [boolean]
  -f, --reportFormatter       Specify an output report formatter
                                                    [string] [default: "detail"]
  -h, --help                  Show help                                [boolean]
  -v, --version               Show version number                      [boolean]

Examples:
  npx vue-codemod ./src -a                  Run all rules to convert all
                                            relevant files in the ./src folder
  npx vue-codemod                           Run slot-attribute rule to convert
  ./src/components/HelloWorld.vue -t        HelloWorld.vue
  slot-attribute
```

## Rule List

| Rule Names                        | Descriptions or links                                        |
| --------------------------------- | ------------------------------------------------------------ |
| new-component-api                 | https://v3.vuejs.org/guide/migration/global-api.html#a-new-global-api-createapp |
| vue-class-component-v8            | https://github.com/vuejs/vue-class-component/issues/406      |
| new-global-api                    | https://v3.vuejs.org/guide/migration/global-api.html#a-new-global-api-createapp |
| vue-router-v4                     | https://next.router.vuejs.org/guide/migration/index.html#new-router-becomes-createrouter<br>https://next.router.vuejs.org/guide/migration/index.html#new-history-option-to-replace-mode<br>https://next.router.vuejs.org/guide/migration/index.html#replaced-onready-with-isready |
| vuex-v4                           | new Store (...) => createStore (...)                         |
| define-component                  | Vue.extend (...) => defineComponent (...)                    |
| new-vue-to-create-app             | https://v3.vuejs.org/guide/migration/global-api.html#a-new-global-api-createapp |
| scoped-slots-to-slots             | https://v3.vuejs.org/guide/migration/slots-unification.html#overview |
| new-directive-api                 | https://v3.vuejs.org/guide/migration/custom-directives.html#overview |
| remove-vue-set-and-delete         | https://v3.vuejs.org/guide/migration/introduction.html#removed-apis |
| rename-lifecycle                  | https://v3.vuejs.org/guide/migration/introduction.html#other-minor-changes |
| add-emit-declaration              | https://v3.vuejs.org/guide/migration/emits-option.html#overview |
| tree-shaking                      | https://v3.vuejs.org/guide/migration/global-api-treeshaking.html |
| v-model                           | https://v3.vuejs.org/guide/migration/v-model.html#overview   |
| render-to-resolveComponent        | https://v3.vuejs.org/guide/migration/render-function-api.html#registered-component |
| remove-contextual-h-from-render   | https://v3.vuejs.org/guide/migration/render-function-api.html#render-function-argument |
| remove-production-tip             | https://v3.vuejs.org/guide/migration/global-api.html#a-new-global-api-createapp |
| remove-trivial-root               | createApp ({ render: () => h (App) })  =>  createApp (App)   |
| vue-as-namespace-import           | import Vue from "vue"  => import * as Vue from "vue"         |
| slot-attribute                    | https://vuejs.org/v2/guide/components-slots.html#Deprecated-Syntax |
| slot-default                      | If  component tag did not contain a `<slot>` element, any content provided between its opening and closing tag would be discarded. |
| slot-scope-attribute              | https://vuejs.org/v2/guide/components-slots.html#Scoped-Slots-with-the-slot-scope-Attribute |
| v-for-template-key                | https://v3.vuejs.org/guide/migration/key-attribute.html#overview |
| v-else-if-key                     | https://v3.vuejs.org/guide/migration/key-attribute.html#overview |
| transition-group-root             | https://v3.vuejs.org/guide/migration/transition-group.html#overview |
| v-bind-order-sensitive            | https://v3.vuejs.org/guide/migration/v-bind.html#overview    |
| v-for-v-if-precedence-changed     | https://v3.vuejs.org/guide/migration/v-if-v-for.html#overview |
| remove-listeners                  | https://v3.vuejs.org/guide/migration/listeners-removed.html#overview |
| v-bind-sync                       | https://v3.vuejs.org/guide/migration/v-model.html#overview   |
| remove-v-on-native                | https://v3.vuejs.org/guide/migration/v-on-native-modifier-removed.html#overview |
| router-link-event-tag             | https://next.router.vuejs.org/guide/migration/index.html#removal-of-event-and-tag-props-in-router-link |
| router-link-exact                 | https://next.router.vuejs.org/guide/migration/index.html#removal-of-the-exact-prop-in-router-link |
| router-view-keep-alive-transition | https://next.router.vuejs.org/guide/migration/index.html#router-view-keep-alive-and-transition |
