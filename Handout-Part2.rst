The Relational Model
====================

.. role:: sidenote

          
The ontological structure described in the previous section maps
precisely onto the **Relational Model** used in relational databases.
The relational model was introduced by Codd [cite] in 1970. Insights
from over 40 years of working with this model can be usefully
applied to the structure of our ontology.

.. tip:: This close correspondence to the relational model does not
   mean that the data file format must be a relational database or
   related structure. As discussed above, the ontology is applicable
   to any data file format, and the format should be chosen to be
   effective given the desired usage patterns (e.g. archiving,
   interchange, storage of massive datasets, centralised access,
   manual editing...)

Description of the relational model
-----------------------------------

A **relation** can be represented by a table satisfying a few conditions:

1. Columns can appear in any order
2. Rows can appear in any order
3. The values in each column are drawn from the same **domain** (however defined)
4. No two rows contain completely identical values

The column headings are called **attributes**.  An m-row relation with
n attributes is, in fact, just an m-element **set** of n-element
**tuples**, where elements are indexed by attribute, not by column
order. In the following the words "tables", "columns", "headings" and
"rows" refer to the relational understanding of these words.

The set of attributes whose values taken together uniquely identify a
row are called a **key**. Keys that are most relevant to us are:

Candidate key:
  a set of headings that taken together form a key.

Primary key:
  the set of headings that is actually used by other
  relations when referring to this relation

Foreign key:
  a heading that is a primary key in a different relation
  Note that the relation that has the foreign key is often called the
  child relation, and the relation for which the foreign key is a
  primary key is called the parent relation.

Correspondence to our ontology
------------------------------

Clearly, data names are attributes, categories are tables, and the key
data names form the primary key.  Identifiers appearing as non-key
data names are usually foreign keys.

Insights from Relational Databases: Normalisation
=================================================

Database normalisation aims to define and allocate headings amongst tables in order
to meet certain goals. The goals that are relevant to us are:

- Remove redundancy.
  
      Each piece of information should appear in only one place to
      avoid the possibility of inconsistency and to save space.

- Minimise redesign when including new headings and tables.

        This is crucial for us as we cannot change the way in which
        data names are interpreted once the standard is embodied in
        distributed software. In our image example in the previous
        section, it would be possible to provide the same images in a
        variety of compressions and encodings with no change to the
        ontology.

Important normalisations that can be applied are:

- No embedded information within a data value.  :sidenote:`"unnormalised form" -> "first normal form"`

- No repeated data names in a single relation (e.g. gas 1, gas 2)

- No non-key data names that depend on only a proper subset of the key
  data names.  :sidenote:`"second normal form"` This means that any
  non-key data name value in a category requires knowledge of all key
  data name values in order to be determined or interpreted. This
  avoids repetition. For example, if a relation has a key composed of
  headings 'K1' and 'K2', and heading 'C' has a value that depends
  only on heading 'K1', if values for 'K1' are often repeated then
  values for 'C' will also be repeated, whereas if a separate table
  had 'C' tabulated against 'K1', this repetition would be avoided.
    
- No non-key data names depend on the key data names via a different
  non-key data name. :sidenote:`"third normal form"` Repetition is
  likely to be reduced if such non-key datanames are moved to a
  separate table, where the non-key data name on which the others
  depend becomes the key data name, and a foreign key in the original
  table.

Normalisation techniques
------------------------

The gas chamber example from part 1 is an example of a
**many-to-many** relationship.  Each gas chamber can have many gases,
and each gas can be in many gas chambers.  The standard way to express
such relationships while maintaining a properly normalised database is
through an **associative table**, as created in that example.
**one-to-many** and **many-to-one** relationships are simply
expressed by using a (key data name) identifier for the **many** and
tabulating the **one** against it.
