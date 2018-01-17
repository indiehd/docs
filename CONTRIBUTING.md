# Contributing to IndieHD

The following is a set of rules for contributing to IndieHD and its packages, which are hosted in the [IndieHD Organization](https://github.com/indiehd) on GitHub. These rules are considered when approving submitted contributions. Feel free to propose changes to this document in a pull request.

#### Table Of Contents

[Organization name](#organization-name)

[Styleguides](#styleguides)
  * [Git Commit Messages](#git-commit-messages)
  * [JavaScript Styleguide](#javascript-styleguide)

## Organization name

When using the name of the organization, make sure it is one of the following:
  * `IndieHD`, `Indie HD`, `IndieHD.com` => generally acceptable
  * `INDIE HD, LLC` => required in every legal context (such as in copyright notices)

## Styleguides

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
    
* Optional (anything inside `[...]`):    
    * `(scope)`
    * `body`
    * `footer`
    
* Common Examples:
    * `build: Add node_modules to gitignore`
    * `chore(gitignore): Add node_modules`
    * `fix: Fix for issue #12`
    * `style: Remove useless elses ;)`
    * `ui: Add button to ... I think you get the point`
 
* Accepted types:

    * `api:`    *Related to **API** commits*
    * `wip:`    *Related to **WIP** commits*
    * `build:`  *Related to **Build or Environment***
    * `chore:`  *Also Accepted for **Build or Environment***
    * `ci:`     *Related to **Coding Continuous Integration** e.g. TravisCI*
    * `docs:`   *Related to **Documentation***
    * `feat:`   *Related to **Features***
    * `fix:`    *Related to **Bug or Issue Fixes***
    * `perf:`   *Related to **Performance***
    * `refact:` *Related to **Refactoring** or moving code around*
    * `revert:` *Related to **reverting** a previous commit **(this is discouraged)***
    * `style:`  *Related to **Coding Style or Linting** (not to be confused with `ui` styles)*
    * `tests:`  *Related to **Tests***
    * `ui:`     *Related to **UI (Web/Interface)***
    * `scaff:`   *Realted to adding **Scaffolding or Boilerplate***

### JavaScript Styleguide

All JavaScript must adhere to [JavaScript Airbnb Style](https://github.com/airbnb/javascript/), 
including additional custom rules, which are defined in the `rules` object in the [.eslintrc.js file](https://github.com/indiehd/website-ui/blob/master/.eslintrc.js).