# Testing
At **IndieHD** we test all of our implementations. There are different types of tests depending on 
the repository you are working with and depending on the type of test you are performing 
(Unit/Feature). 

##### Note: Contributors
`If you plan on contributing, you will be expected to provide tests for your implementations also. 
All tests must pass before any PR's are even considered!`

In this documentation we will try and explain how we specifically handle our tests and what would be
expected of you as a contributor.

#### Sections
 * [Unit vs Feature](#unit-test-vs-feature-test)
 * [Examples](#example-of-a-feature-test)
 * [web-api Directory Structure](#directory-structure-web-api)
    
 * [Jest](#jest)
     * [Run all tests](#jest-run-all-tests)
     * [Run unit tests](#jest-run-unit-tests)
     * [Run e2e tests](#jest-run-e2e-tests)
     * [Linting (ESLint)](#eslint-linting)    
     
 
 `Jest` (website-ui)
```javascript
describe('Home.vue', () => {
  it('should render correct contents', () => {
    const Constructor = Vue.extend(Home);
    const vm = new Constructor().$mount();
    expect(vm.$el.querySelector('.home-view h1').textContent)
    .toEqual('Welcome to IndieHD');
  });
});
```



### Jest
#### `jest` Run all tests

```
npm run test
```

#### `jest` Run `unit` tests
```
npm run unit
```

#### `jest` Run `e2e` tests
```
npm run e2e
```

#### `eslint` Linting
```
npm run lint
```
