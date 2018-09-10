## Git Commit Messages

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
    /^(api|build|chore|refact|revert|feat|tests|docs|style|ci|ui|scaff|perf|fix|wip)(\([a-zA-Z.\s]+\))?(:[ ])([A-Za-z0-9,;_`'\"\\ ]{1,72})([\r\n|\r|\n]{1,2}[A-Za-z0-9,;_.#`@'\"\\ ]{1,72}){1,}$/
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

    * `api:` *Related to **API***
        ```
        API thats not associated with a new feature
        
        Addition, update or removal of Api
        ```
    * `wip:` *Related to **Work In Progress***
        ```
        If you commit with this type you are
        aknolowledgeing that tests are failing or 
        code is incomplete
        
        You should be prepared to squash your wip
        commits before submitting a PR. We want to 
        avoid them in the master repository
        ```
    * `build:`  *Related to **Compiled Files***
        ```
        If for whatever reason it makes since to
        version control compiled sources you would
        use this commit type when pushing said files
        ```
    * `chore:`  *Related to **Environment Changes***
        ```
        If the enviroment changes including 
        dependencies, tooling or .env files
        ```
    * `ci:`     *Related to **Coding Continuous Integration***
        ```
        Changes or fixes to CI Tools
        example: TravisCI
        ```
    * `docs:`   *Related to **Documentation***
        ```
        This could include doc blocks and comments 
        ```
    * `feat:`   *Related to **Features***
        ```
        When ADDING, UPDATING OR REMOVING a feature 
        ```
    * `fix:`    *Related to **Bugs or Issue Fixes***
        ```
        This is self explainatory ...
        
        Often this type would be asscoaited with
        a github issue # exlicitly referenced in
        the body (or more commonly in the footer)
        ```
    * `perf:`   *Related to **Performance***
        ```
        Any optimizations made to increase performance 
        ```
    * `refact:` *Related to **Refactoring***
        ```
        When MOVING, RENAMING code
        
        Has no effect on the functionallity of the code
        more so just for organization or proper terminolgy 
        ```
    * `revert:` *Related to **Reverting** a previous commit*
        ```
        If the commit is UN-DOING a previous commit you
        would use this commit type
        ```
    * `style:`  *Related to **Coding Style***
        ```
        For example Linting or making changes to adhere
        to certain coding style guidlines
        
        Not to be confused with `UI` styles
        ```
    * `tests:`  *Related to **Tests***
        ```
        When ADDING, UPDATING or REMOVING tests 
        ```
    * `ui:`     *Related to **UI or UX** design or markup*
        ```
        When you have made changes that the user can see
        graphically in the browser 
        ```
    * `scaff:`  *Related to **Scaffolding or Boilerplate***
        ```
        Sometime you just want to add some empty directories
        or maybe want to setup some files with just some basic
        boilerplate
        
        Consider using WIP type before using this. We reserve
        the right to determine if this type is appropriate 
        ```
