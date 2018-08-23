# Contributing to IndieHD

The following is a set of rules for contributing to IndieHD and its packages, which are hosted in the [IndieHD Organization](https://github.com/indiehd) on GitHub. These rules are considered when approving submitted contributions. Feel free to propose changes to this document in a pull request.

#### Table Of Contents

[Organization name](#organization-name)

[Styleguides](#styleguides)
  * [Git Commit Messages](#git-commit-messages)
  * [JavaScript Styleguide](#javascript-styleguide)

## Organization name

When using the name of the organization, make sure it is one of the following:
  * `indieHD`, `Indie HD`, `indiehd.com` => generally acceptable (along with other variants)
  * `INDIE HD, LLC` => required in every legal context (such as in copyright notices)

## Styleguides
[Back To Top](#table-of-contents)

### Git Commit Messages

* Use the present tense ("Add feature" not "Added feature")
* Use the imperative mood ("Move cursor to..." not "Moves cursor to...")
* Limit the first line to 72 characters or less
* Reference issues and pull requests liberally after the first line

* Commit conventions follow this pattern:
    ```
    type[(scope)]: subject
    <BLANK LINE>
    [body]
    <BLANK LINE>
    [footer]
    ```
    
 * Regex pattern used:
    
    ```
    ^(api|build|chore|refact|revert|feat|tests|docs|style|ci|ui|scaff|perf|fix|wip)(\([a-zA-Z.\s]+\))?:\s[A-Za-z0-9,;'\"\\ ]{1,72}?$
    ```
        
* Examples of **GOOD** patterns:
    
    ```
    api: should pass
    fix(scope): should pass
    wip(Scope): should pass
    api(This Scope): should pass
    api(.scope): should pass
    ```
    
* Examples of **BAD** patterns
    ```
    whatever i want
    (api) should fail
    api[scope] should fail
    api should fail
    api(scope) should fail
    api (scope): should fail
    api (scope) should fail
    Api: should fail
    aPi: should fail
    aPI(scope): should fail
    api[scope]: should fail
    api(.scope): should fail because it is super long should fail because it is super long should fail because it is super long
    ```
    
* Optional (anything inside `[...]`):    
    * `(scope)`
    * `body`
    * `footer`
    
* Common Examples:
    * `build: Add compiled files`
    * `chore(.gitignore): Add node_modules`
    * `fix: Fix for issue #12`
    * `style: Remove useless elses`
    * `ui: Add button to ... I think you get the point`
 
* Accepted types:

    * `api:`    *Related to **API***
    * `wip:`    *Related to **Work In Progress***
    * `build:`  *Related to **Compiled Files***
    * `chore:`  *Related to **Environment Changes***
    * `ci:`     *Related to **Coding Continuous Integration** e.g. TravisCI*
    * `docs:`   *Related to **Documentation***
    * `feat:`   *Related to **Features***
    * `fix:`    *Related to **Bugs or Issue Fixes***
    * `perf:`   *Related to **Performance***
    * `refact:` *Related to **Refactoring***
    * `revert:` *Related to **Reverting** a previous commit*
    * `style:`  *Related to **Coding Style or Linting** (not to be confused with `UI` styles)*
    * `tests:`  *Related to **Tests***
    * `ui:`     *Related to **UI or UX** design or markup*
    * `scaff:`  *Related to **Scaffolding or Boilerplate***

### JavaScript Styleguide
[Back To Top](#table-of-contents)

All JavaScript must adhere to [JavaScript Airbnb Style](https://github.com/airbnb/javascript/), 
including additional custom rules, which are defined in the `rules` object in the [.eslintrc.js file](https://github.com/indiehd/website-ui/blob/master/.eslintrc.js).