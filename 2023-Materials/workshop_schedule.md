# 2023 Dictionary Writing Workshop Schedule

## 9.25am Opening (James)

## 9.30am Introduction to the CIF Ontology

* Tour through CIF dictionaries (Brian)

## 10.00am The relational underpinnings of CIF (James)

* The role of dictionaries
   - Define data names associated with values in a file
   - Machine and human readable
   - Reference

* Relational model
   - Description
   - Functional perspective
   - Category theory perspective
   - Normal forms
   - Practical uses of above information
   - How data blocks relate to relations

## 10.45am Morning tea

## 11.00am Structure of a CIF dictionary (Brian)

* Dictionary definition languages
   - DDLm
   - DDL2 (not covered here)
   - DDL1

* Dictionary layout

* Main pieces of information in a definition
     - `_description.text`
     - `category_id`, `object_id`
     - `_definition.id`
     - `_name.linked_item_id`
* Optional information

## 11.30am Dictionary creation exercise (James)
  
  * Decide on the extent of a single data block (one microsymposium)
  * Collect datanames that we would like
     - Include observational, e.g. actual number of speakers, abstracts, present for each talk
     - Include derived, e.g. total number of distinct attendees
  * Make more computer-friendly
     - List of authors associated with an abstract
  * Arrange data names into categories
  * Write some definitions
  * Show how to build into a full conference dictionary

## 12.30pm Lunch

## 2.00pm Dictionary creation exercise - conclusion

## 2.30pm Using Github (Matthew)
  * Basic navigation
     - Looking at dictionaries
     - Other features of the interface
  * PRs
     - Looking at PRs, open and closed
     - What they mean
  * Issues
     - Creating issues
     - Looking at open and closed issues
  * Managing your own fork
     - Creating branches
     - Synchronising
  * Running automated checks
     - In your own area
     - NB issue for us: Github will probably be unresponsive if all 57 participants execute a check at the same time.

## Using Github: some simple workflows (Matthew)
  * Live demonstrate a PR on our practical example

## 4.00pm Afternoon tea

## 4.30pm Writing CIF Software (Antanas?)
  * Survey of available libraries (C/Fortran/Python/Perl/Julia)
  * Useful tools (cod-tools at least)
  * Using dictionaries
  * Checking `_audit.schema`
  * Other tips?
