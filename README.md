# vue-cypress-example

Vue Unit and E2E Test with Code Coverage using Cypress setup example

For Vue-Typescript version please [check here](https://github.com/janumedia/vue-typescript-cypress-example)

## Step by step setup

> This project example is based on Vue CLI 3.

1. Create Vue project using Vue CLI
   ```
   vue create your-app
   cd your-app
   ```
   > Make sure to select Cypress as your E2E test
2. Add `@vue/test-utils`
   ```
   yarn add -D @vue/test-utils
   ```
3. Add `@cypress/webpack-preprocessor`, `find-webpack` and `babel-plugin-istanbul`
   ```
   yarn add -D @cypress/webpack-preprocessor find-webpack babel-plugin-istanbul
   ```
4. Edit Cypress plugins setting locate at `tests/e2e/plugins/index.js`

   4.a. Import `webpack-preprocessor`

   ```
   const webpackPreprocessor = require("@cypress/webpack-preprocessor");
   ```

   4.b. Get webpackOptions

   ```
   const fw = require("find-webpack");
   const webpackOptions = fw.getWebpackOptions();
   ```

   4.c. Add on `file:preprocessor` setting

   ```
   on(
       "file:preprocessor",
       webpackPreprocessor({ webpackOptions })
   );
   ```

   > This setting will allow Cypress to parse .vue files in unit test

   4.d. Setup code coverage task
   ```
   require("@cypress/code-coverage/task")(on, config);
   ```
5. Edit Cypress support setting locate at `tests/e2e/support/index.js`
    ```
    import "@cypress/code-coverage/support";
    ```
6. Add `istanbul`as Babel plugins config
    ```
    plugins: ["istanbul"]
    ```
7. Create `.nycrc` or`nyc` config file with following
    ```
    {
      "extension": [ ".js", ".vue" ]
    }
    ```
> Step 4.d, 5 - 7 are related to Code Coverage setup 

## Writes your test specs

> Vue-CLI by default set test files location in `tests/e2e/specs`, but you can customize them in `cypress.json` or in plugins settings

#### Example E2E test with Cypress
```
//tests/e2e/specs/test.js

describe("My First Test", () => {
    it("Visits the app root url", () => {
        cy.visit("/");
        cy.contains("h1", "Welcome to Your Vue.js App");
    });
});
```    
> More detail about Cypress API see [Cypress API Docs](https://docs.cypress.io/api/introduction/api.html)

#### Example Unit test with `@vue/test-utils` and Cypress

```
//tests/e2e/specs/test.unit.js

import { shallowMount } from "@vue/test-utils";
import HelloWorld from "@/components/HelloWorld.vue";

describe("HelloWorld", () => {
  const msg = "Vue Unit and E2E Test with Cypress";
  it(`Title should display '${msg}'`, () => {
    HelloWorld.components = HelloWorld.components || {};
    let wrapper = shallowMount(HelloWorld, { propsData: { msg } });
    expect(wrapper.find("h1").text()).eq(msg);
  });
  it(`Title should be empty`, () => {
    HelloWorld.components = HelloWorld.components || {};
    let wrapper = shallowMount(HelloWorld, {});
    expect(wrapper.find("h1").text()).to.be.empty;
  });
});
```

> Make sure to put unit test files the same folder as E2E test files and make it available in Cypress UI

> Writing unit test usually help to get 100% code coverage

> More detail about `@vue/test-utils` see [API Docs](https://vue-test-utils.vuejs.org/api/)

## Run test

```
yarn test:e2e
```
