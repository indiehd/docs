# Intro
This documentation will outline some guidelines as well as some information you as a contributor will need to know.
 
## Organization Name

When using the name of the organization, make sure it is one of the following:
  * `indieHD`, `Indie HD`, `indiehd.com` => generally acceptable (along with other variants)
  * `INDIE HD, LLC` => required in every legal context (such as in copyright notices)
  
## Our Community

**IndieHD, LLC and Team** would like to invite you to join [our discord server](https://discord.gg/hDC3Yw6).

**Discord** is a way to collaborate with everyone involved in the project. Its a great
place to ask questions that are not covered in this documentation.

[Discord](https://discordapp.com/) can either be installed on your computer `(Recommended)` or they offer a web based client.

***We are currently ONLY on Discord chat. Slack may come later.***

## Contributor Covenant Code of Conduct

### Our Pledge

In the interest of fostering an open and welcoming environment, we as
contributors and maintainers pledge to making participation in our project and
our community a harassment-free experience for everyone, regardless of age, body
size, disability, ethnicity, sex characteristics, gender identity and expression,
level of experience, education, socio-economic status, nationality, personal
appearance, race, religion, or sexual identity and orientation.

### Our Standards

Examples of behavior that contributes to creating a positive environment
include:

 * Using welcoming and inclusive language
 * Being respectful of differing viewpoints and experiences
 * Gracefully accepting constructive criticism
 * Focusing on what is best for the community
 * Showing empathy towards other community members

Examples of unacceptable behavior by participants include:

 * The use of sexualized language or imagery and unwelcome sexual attention or advances
 * Trolling, insulting/derogatory comments, and personal or political attacks
 * Public or private harassment
 * Publishing others' private information, such as a physical or electronic address, without 
 explicit permission
 * Other conduct which could reasonably be considered inappropriate in a professional setting

### Our Responsibilities

Project maintainers are responsible for clarifying the standards of acceptable
behavior and are expected to take appropriate and fair corrective action in
response to any instances of unacceptable behavior.

Project maintainers have the right and responsibility to remove, edit, or
reject comments, commits, code, wiki edits, issues, and other contributions
that are not aligned to this Code of Conduct, or to ban temporarily or
permanently any contributor for other behaviors that they deem inappropriate,
threatening, offensive, or harmful.

### The Scope

This Code of Conduct applies both within project spaces and in public spaces
when an individual is representing the project or its community. Examples of
representing a project or community include using an official project e-mail
address, posting via an official social media account, or acting as an appointed
representative at an online or offline event. Representation of a project may be
further defined and clarified by project maintainers.

### Enforcement

Instances of abusive, harassing, or otherwise unacceptable behavior may be
reported by contacting the project team at `abuse@indiehd.com`. All
complaints will be reviewed and investigated and will result in a response that
is deemed necessary and appropriate to the circumstances. The project team is
obligated to maintain confidentiality with regard to the reporter of an incident.
Further details of specific enforcement policies may be posted separately.

Project maintainers who do not follow or enforce the Code of Conduct in good
faith may face temporary or permanent repercussions as determined by other
members of the project's leadership.

## Licensing
Unless otherwise specified, All Rights Reserved applies!

**IndieHD** is still determining under which License(s) the various project components will be made 
available. All Rights Reserved should be assumed until further notice (or other licensing terms are 
specified explicitly).

## Git Commit Messages

* Use the present tense ("Add feature" not "Added feature")
* Use the imperative mood ("Move cursor to..." not "Moves cursor to...")
* Limit the first line to 72 characters or less
* Reference issues and pull requests liberally after the first line
* No punctuation is allowed in the ***Subject*** line (. ! ?)

* Regex pattern used: [regex101.com](https://regex101.com/r/zZfwvc/4)

* Commit conventions follow this pattern:
    ```
    type[(scope)]: subject
    <BLANK LINE>
    [body]
    <BLANK LINE>
    [footer]
    ```
        
* Examples of **GOOD** patterns:
    
    ```
    api: should pass
    fix(scope): should pass
    api(This Scope): should pass
    wip(Scope): should pass #409 @ - , ' / \ " blah_blah
    Merge branch 'branch-name-here' blah blah blah
    Merge remote-tracking branch 'upstream/master'
    Merge pull request #490 from origin/master
    ```
    
* Examples of **BAD** patterns
    ```
    should fail because i can't just put whetever i want here
    wip(Scope): should fail because no punctionation in subject is allowed. ! ?
    api(.scope): should fail
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
    ```
    
* Optional (anything inside `[...]`):    
    * `(scope)`
    * `body`
    * `footer`
    
* Common Examples:
    * `build: Add compiled files`
    * `chore(gitignore): Add node_modules to git ignore file`
    * `fix: Fix issue with blah blah`
    * `style: Remove useless elses`
    * `ui: Add image to profile view`
 
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
        
## First Contribution
`Documentation under construction`

## Issues
`Documentation under construction`

## Pull Requests
Below you will find a set of rules to follow for every pull request.

* *`Every`* PR should request *`at-least`* a review from `cbj4074` and/or `poppabear8883` 

* Prefixing a PR with `WIP` means ***`don't merge`***, only review ...
    * `This workflow is useful for requesting reviews while you are in-progress of adding new features`
    * `Only once you are fully reviewed and all conversations have been resolved should you remove WIP from the title`

* The author of a PR ***`cannot`*** merge their own PR.
    * `This only applies to Core Developers, Contruibutors won't have this ability anyways.`

