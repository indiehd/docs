# Git Commit Messages

* Use the present tense ("Add feature" not "Added feature")
* Use the imperative mood ("Move cursor to..." not "Moves cursor to...")
* Limit the first line to 72 characters or less
* Reference issues and pull requests liberally after the first line
* No trailing punctuation is allowed in the ***Subject*** line

## Commit Pattern

```
type(scope): subject
<BLANK LINE>
[body]
<BLANK LINE>
[footer]
```

**Optional**

* (scope)
* body
* footer
        
## Good Examples
    
* api: should pass
* fix(scope): should pass
* api(This Scope): should pass
* wip(Scope): should pass #409 @ - , ' / \ " blah_blah
* Merge branch 'branch-name-here' blah blah blah
* Merge remote-tracking branch 'upstream/master'
* Merge pull request #490 from origin/master
    
## Bad Examples

* should fail because i can't just put whetever i want here
* api(.scope): should fail because i can't use a `.` in the scope
* api[scope]: should fail because i can't wrap scope with `[]`
* (api) should fail because i don't follow the [Pattern](#commit-pattern)

    
## Common Examples

* build: Add compiled files
* chore(gitignore): Add node_modules to git ignore file
* fix: Fix issue with blah blah
* style: Remove useless elses
* ui: Add image to profile view
 
## Accepted Types

* [api](#api)
* [wip](#wip)
* [build](#build)
* [chore](#chore)
* [ci](#ci)
* [docs](#docs)
* [feat](#feat)
* [fix](#fix)
* [perf](#perf)
* [refact](#refact)
* [revert](#revert)
* [style](#style)
* [tests](#tests)
* [ui](#ui)
* [scaff](#scaff)

### api {docsify-ignore}

*Related to **API***

```
API thats not associated with a new feature

Addition, update or removal of Api
```

### wip {docsify-ignore}

*Related to **Work In Progress***

```
If you commit with this type you are
aknolowledgeing that tests are failing or 
code is incomplete

You should be prepared to squash your wip
commits before submitting a PR. We want to 
avoid them in the master repository
```

### build {docsify-ignore}

*Related to **Compiled Files***

```
If for whatever reason it makes since to
version control compiled sources you would
use this commit type when pushing said files
```

### chore {docsify-ignore}

*Related to **Environment Changes***

```
If the enviroment changes including 
dependencies, tooling or .env files
```
    
### ci {docsify-ignore}

*Related to **Coding Continuous Integration***

```
Changes or fixes to CI Tools
example: TravisCI
```

### docs {docsify-ignore}

*Related to **Documentation***

```
This could include doc blocks and comments 
```
    
### feat {docsify-ignore}

*Related to **Features**

```
When ADDING, UPDATING OR REMOVING a feature 
```

### fix {docsify-ignore}

*Related to **Bugs or Issue Fixes***

```
This is self explainatory ...

Often this type would be asscoaited with
a github issue # exlicitly referenced in
the body (or more commonly in the footer)
```
    
### perf {docsify-ignore} 

*Related to **Performance***

```
Any optimizations made to increase performance 
```
    
### refact {docsify-ignore}

*Related to **Refactoring***

```
When MOVING, RENAMING code

Has no effect on the functionallity of the code
more so just for organization or proper terminolgy 
```

### revert {docsify-ignore}

*Related to **Reverting** a previous commit*

```
If the commit is UN-DOING a previous commit you
would use this commit type
```

### style {docsify-ignore}

*Related to **Coding Style***

```
For example Linting or making changes to adhere
to certain coding style guidlines

Not to be confused with `UI` styles
```

### tests {docsify-ignore}

*Related to **Tests***

```
When ADDING, UPDATING or REMOVING tests 
```

### ui {docsify-ignore}

*Related to **UI or UX** design or markup*

```
When you have made changes that the user can see
graphically in the browser 
```

### scaff {docsify-ignore}

*Related to **Scaffolding or Boilerplate***

```
Sometime you just want to add some empty directories
or maybe want to setup some files with just some basic
boilerplate

Consider using WIP type before using this. We reserve
the right to determine if this type is appropriate 
```