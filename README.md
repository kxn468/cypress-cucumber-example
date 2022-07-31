# cypress-cucumber-example
Initial example of using Cypress with Cucumber.

# Run example tests

```
npm install
npm test
```  

# Tags usage

### Tagging tests
You can use tags to select which test should run using [cucumber's tag expressions](https://github.com/cucumber/cucumber/tree/master/tag-expressions).
Keep in mind we are using newer syntax, eg. `'not @foo and (@bar or @zap)'`.
In order to initialize tests using tags you will have to run cypress and pass TAGS environment variable.

To make things faster and skip cypress opening a browser for every feature file (taking a couple seconds for each one), even the ones we want ignored, we use our own cypress-tags wrapper. It passes all the arguments to cypress, so use it the same way you would use cypress CLI. The only difference is it will first filter out the files we don't care about, based on the tags provided. 

### Examples:

There are a few tagged tests in these files:

```
@feature-tag
Feature: The Facebook

  I want to open a social network page

  @tag-to-include
  Scenario: Opening a social network page
    Given I open Facebook page
    Then I see "Facebook" in the title

  @another-tag-to-include @some-other-tag
  Scenario: Different kind of opening
    Given I kinda open Facebook page
    Then I am very happy

```

```
@feature-tag @github-tag
Feature: The GitHub

  I want to tweet things

  @tag-to-include
  Scenario: Opening GitHub
    Given I open GitHub page
    Then I see "GitHub" in the title

  @another-tag-to-include
  Scenario: Opening GitHub again
    Given I open GitHub page
    Then I see "GitHub" in the title
```

###### Simple Example
  Run ```./node_modules/.bin/cypress-tags run -e TAGS='@feature-tag'``` in this repo. As both `Facebook.feature` and `GitHub.feature` 
  have `@feature-tag` above the feature name, and `Google.feature` has no tags, the result should be: 
  
  ```
      Spec                                                Tests  Passing  Failing  Pending  Skipped
  ┌────────────────────────────────────────────────────────────────────────────────────────────────┐
  │ ✔ socialNetworks/Facebook.feature           00:04        2        2        -        -        - │
  ├────────────────────────────────────────────────────────────────────────────────────────────────┤
  │ ✔ socialNetworks/GitHub.feature            00:05        2        2        -        -        - │
  └────────────────────────────────────────────────────────────────────────────────────────────────┘
    All specs passed!                           00:09        4        4        -        -        -
```

###### usage of `not`

Run ```./node_modules/.bin/cypress-tags run -e TAGS='not @github-tag'``` in this repo. `Facebook.feature` and `Google.feature` will run, as only `GitHub.feature` has the unwanted tag. The result should be: 

```
      Spec                                                Tests  Passing  Failing  Pending  Skipped
  ┌────────────────────────────────────────────────────────────────────────────────────────────────┐
  │ ✔ socialNetworks/Facebook.feature           00:05        2        2        -        -        - │
  └────────────────────────────────────────────────────────────────────────────────────────────────┘
    All specs passed!                           00:05        2        2        -        -        -
```

###### usage of `and` 

Run ```./node_modules/.bin/cypress-tags run -e TAGS='@another-tag-to-include and @some-other-tag'``` in this repo. There is only one scenario that has both the tags, in `Facebook.feature`. The result should be:  

```
     Spec                                                Tests  Passing  Failing  Pending  Skipped
  ┌────────────────────────────────────────────────────────────────────────────────────────────────┐
  │ ✔ socialNetworks/Facebook.feature           00:03        1        1        -        -        - │
  ├────────────────────────────────────────────────────────────────────────────────────────────────┤
    All specs passed!                           00:03        1        1        -        -        -

```

