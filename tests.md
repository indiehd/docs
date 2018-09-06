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

 * [Phpunit](#phpunit)
    * [Run all tests](#phpunit-run-all-tests)
    * [Run all tests in a directory](#phpunit-run-all-tests-in-a-specific-directory)
    * [Run a single test class](#phpunit-run-a-single-test-class)
    * [Run a single test method](#phpunit-run-a-single-test-method)
    * [Dealing with cache](#phpunit-dealing-with-cache)
    * [Linting (PHP Code Sniffer)](#phpcs-linting)
    
 * [Jest](#jest)
     * [Run all tests](#jest-run-all-tests)
     * [Run unit tests](#jest-run-unit-tests)
     * [Run e2e tests](#jest-run-e2e-tests)
     * [Linting (ESLint)](#eslint-linting)    
     
#### Directory Structure (`web-api`)
 
 The below image should give you a good idea how our tests are organized. Mostly you see Feature 
 tests here!
 
 ![](images/tests_dir_structure.png)
 
 #### `Unit Test` vs `Feature Test`
 
 This is the way we like to interpret it:
 
 * ***`Unit Test`*** - testing an individual unit, such as a method (function) in a class, with all 
 dependencies mocked up.
 
 * ***`Feature (aka Functional/Integration) Test`*** - testing a slice of functionality in a system. 
 This will test many methods and may interact with dependencies like Databases or Web Services.
 
 #### Example of a ***`Feature Test`***
 
 `Phpunit` (web-api)
 ```php
/**
* Ensure the method create() creates a new record in the database and creates a profile for
* said Artist.
*
* @return void
*/
public function test_method_create_storesNewModel()
{
    $profile = factory($this->profile->class())->make()->toArray();
    
    $artist = $this->repo->create($profile);
    
    $this->assertInstanceOf($this->repo->class(), $artist);
    $this->assertInstanceOf($this->profile->class(), $artist->profile);
}
 ```
 
 #### Example of a ***`Unit Test`***
 
 `Phpunit` (web-api)
 
 *This is just a crude example and not related to this project. At the time of this writing we do not
  have a example of a Unit test in this project.*
 ```php
 public function testDivideByZero() {
    $calcMock=$this->getMock('\Calculator',array('getNumberFromUserInput'));
    $calcMock->expects($this->never())
        ->method('getNumberFromUserInput')
        ->will($this->returnValue(10));
    $this->assertEquals(NAN, $calcMock->divideBy(0));
}
 ```
 
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

#### Phpunit
##### `phpunit` Run all tests
 * Run the following command from the project root directory:
```
vendor/bin/phpunit
```

#### `phpunit` Run all tests in a specific `directory`
 * `PATH/TO/DIRECTORY` - Full path to the directory relative to the project root
 ***Note: don't include `.php` in the path***
```
vendor/bin/phpunit PATH/TO/DIRECTORY 
```

#### `phpunit` Run a single test `class`
 * `CLASSNAME` - The name of the class of which your method is in
 * `PATH/TO/FILE` - Full path to the file itself relative to the project root
 ***Note: don't include `.php` in the path***
```
vendor/bin/phpunit --filter CLASSNAME PATH/TO/FILE 
```

#### `phpunit` Run a single test `method`
 * `METHODNAME` - The name of the method you wish to test
 * `CLASSNAME` - The name of the class of which your method is in
 * `PATH/TO/FILE` - Full path to the file itself relative to the project root
 ***Note: don't include `.php` in the path***
``` 
vendor/bin/phpunit --filter METHODNAME CLASSNAME PATH/TO/FILE
```

#### `phpunit` Dealing with cache
Often times you will have to test `Model Repositories` that normally would fetch data from the 
database. This is fine until the repository implements a caching layer in which you will get
inaccurate test results.

To avoid this we suggest during all `CUD` (Create Update Delete) operations that you use the 
`->fresh()` method on the `Model` instance.

***Example***

In this example we are updating the resource in the database, then we need to validate the 
persistence of the data so we ***MUST*** fetch a `fresh` result from the database, otherwise this
test could be compromised if it returns a cached result.
 
```php

public function test_method_update_updatesResource()
{
    $artist = factory($this->artist->class())->create();

    $profile = factory($this->repo->class())->create([
        'profilable_id' => $artist->id,
        'profilable_type' => $this->repo->class(),
    ]);

    $newValue = 'Foo Bar';

    $property = 'moniker';

    $profile = $this->repo->update($profile->id, [
        $property => $newValue,
    ]);

    $this->assertTrue(
        $profile->fresh()->{$property} === $newValue
    );
}
```

#### `phpcs` Linting
We use [GrumPHP](https://github.com/phpro/grumphp/blob/master/README.md) as a task runner which will simply run the 
`phpcs` for php linting.

***Note: Be sure to run [phpunit](#phpunit-run-all-tests) tests after fixing linting issues!***

**Running GrumPHP**
```
vendor/bin/grumphp run
```

**Running Php Code Sniffer**
 * `FILEPATH` - The full path to the directory or file you wish to check relative to the project root
 
 *Note: You can specify more than one FILEPATH separated with a SPACE*
```
vendor/bin/phpcs --standard=PSR2 FILEPATH
```

**Fixing linting errors automatically**
 * `FILEPATH` - The full path to the directory or file you wish to check relative to the project root
 
 *Note: You can specify more than one FILEPATH separated with a SPACE*
```
vendor/bin/phpcbf --standard=PSR2 FILEPATH 
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
