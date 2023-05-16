# 2023 Dictionary Writing Workshop Curriculum

## Introduction to dictionaries
* The role of dictionaries
   - Define data names associated with values in a file
   - Machine and human readable
   - Reference

* Relational model
   - Justification
   - Everything is in a table
   - Keys

* Dictionary definition languages
   - DDLm
   - DDL2 (not covered here)
   - DDL1

## Creating a definition
  * Main pieces of information
     - `_description.text`
     - `category_id`, `object_id`
     - `_definition.id`
  * Optional information

## Creating a dictionary
  * Why?
  * Overall structure
  * Identifying categories

## Using Github
  * Basic navigation
     - Looking at dictionaries
     - Other features of the interface
  * PRs
     - Looking at PRs, open and closed
     - What they mean
  * Issues
     - Creating issues
     - Looking at open and closed issues
  * Cloning to your own area
  * Running automated checks
     - In your own area
     - NB issue for us: Github will probably be unresponsive if all 57 participants execute a check at the same time.

## Using Github: some simple workflows
  * Live demonstrate a PR on our practical example
    - Ask participants to submit some PRs and we will comment on them - note the live check delay may be annoying

## Practical example: a conference microsymposium dictionary
  * Collect datanames that we would like
     - Include observational, e.g. actual number of speakers, abstracts, present for each talk
     - Include derived, e.g. total number of distinct attendees
  * Decide on the extent of a single data block (one microsymposium)
  * Make more computer-friendly
     - The EXAFS gas1, gas2 example but with ?
  * Arrange data names into categories
  * Write some definitions
  * Show how to build into a full conference dictionary
