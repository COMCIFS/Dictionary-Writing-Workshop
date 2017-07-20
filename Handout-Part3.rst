Creating formal dictionaries
============================

.. role:: sidenote

The earlier part of this workshop described how to create a scientific
ontology that would underpin a fully-functional data transfer
framework. That ontology was just a collection of plain-text 
data name definitions that satisfied a few basic criteria. 

A CIF dictionary is a machine-readable version of that ontology. Just
like the original human-readable version, the ontology expressed in a
CIF dictionary describes data names that could appear in any format.
Among other things, machine-readable ontologies are
useful for:

- automated data checking. Relationships between data names can be 
  checked for consistency in data files.
   
- assistance in constructing the ontology. Tools can ensure that 
  all required information is provided and mutually consistent with 
  other definitions
   
- assistance in accessing the ontology. Large ontologies benefit
  from 
  automated tools that can perform searches, locate related data names,   
  and show relationships between data names. The ontology can also be 
  automatically transformed into different presentation formats, such
  as 
  HTML or PDF.

The structure of a CIF dictionary
---------------------------------

.. raw:: latex

   \begin{figure*}
   \caption{General structure of a CIF dictionary}
   \includegraphics[width=\textwidth]{twin_dic_annotated.eps}
   \end{figure*}
             
Syntactically, a CIF dictionary is a CIF file. It contains a single
CIF data block, within which each data name definition is defined in a
separate **save frame**. Just as for a normal CIF data file, all
information in a dictionary is created by associating one or more
values with textual tags. :sidenote:`The "CIF" dictionary could itself
be written in any file format. It is written in CIF format both to
save CIF users implementing or learning a separate format, and because
of other positive attributes of the CIF format for standards work,
such as easy readability, long-term support and easy transformation to
a variety of presentations.`  In a normal CIF data file, these tags are
called "data names". To avoid ambiguity, the tags used in a dictionary
are referred to as **attributes**. The collection of attributes that
can appear in a dictionary is called the *Dictionary Definition
Language* or DDL. Two similar DDLs are in common use in the CIF
framework, **DDL2** and **DDLm**. The earlier DDL1 is no longer used
in new dictionaries.


Earlier we listed the core elements of a data name definition: (i) a 
description of the data values (ii) a list of the key data names (iii)
a description of how the data values are interpreted given the values
of the key data names. In a practical dictionary, further tags give 
audit and management information such as the date on which a 
definition was edited, other names for this data name, and version 
information for the dictionary as a whole.

.. raw:: latex

   \begin{fullwidth}

.. table:: Selected DDLm attributes.

    +-----------------+------------------------------------------------------------+
    |Role             | Data names                                                 |
    +=================+============================================================+
    | Naming          |  \_alias.definition\_id, **_definition.id**,               |
    |                 |  **\_name.object\_id**                                     |
    +-----------------+------------------------------------------------------------+
    | |               | | \_enumeration.\{def\_index\_id/default/mandatory/range\},|
    | | Describing    | | \_enumeration\_default.\{index/value\},                  |
    | | Values        | | **\_enumeration\_set.\{detail/state\}**,                 |
    |                 | | \_type.\{**container** / **contents** / dimension\},     |
    |                 | | **\_units.code**                                         |
    +-----------------+------------------------------------------------------------+
    | Key data names  | **\_name.category\_id** , **\_category\_key.name**         |
    +-----------------+------------------------------------------------------------+
    ||                || **_description**.{common/key\_words/**text**},            |
    || Interpretation || \_description\_example.{case/detail},                     |
    ||                || \_name.linked\_item\_id, \_method.{purpose/expression}    |
    +-----------------+------------------------------------------------------------+
    | Technical       | \_definition.{class/scope},\_import.\*                     |
    +-----------------+------------------------------------------------------------+
    |                 | | \_dictionary.{title/class/version/date/uri/              |
    |                 | | ddl\_conformance/namespace/\*},                          |
    | |               | | \_dictionary\_audit.{version/date/revision},             |
    | |               | | \_alias.{deprecation\_date/dictionary\_uri},             |
    | | Management    | | \_definition.{replaced\_by/**update**/xref\_code},       |
    | |               | | \_dictionary\_xref.{code/date/format/name/uri},          |
    |                 | | \_enumeration\_set.{xref\_code/xref\_dictionary},        |
    |                 | | \_type.{purpose/source}                                  |
    +-----------------+------------------------------------------------------------+
    
.. table:: Selected DDL2 attributes

    +-----------------+-----------------------------------------------------------+
    |Role             | Data names                                                |
    +=================+===========================================================+
    ||                || **\_item.name**, \_item\_aliases.alias\_name,            |
    || Naming         || **\_category.id**                                        |
    +-----------------+-----------------------------------------------------------+
    ||                || \_item\_enumeration.{value/detail},\_item\_default.value |
    ||                || \_item\_range.minimum, \_item\_range.maximum             |
    || Describing     || \_item\_structure.{code/organisation},                   |
    || values         || \_item\_structure\_list.{index/code/dimension}           |
    |                 || **\_item\_type.code**, **\_item\_units.code**            |
    |                 || \_item\_type\_list.{code/detail/construct}               |
    |                 || \_item\_units\_list.{code/detail}                        |
    +-----------------+-----------------------------------------------------------+
    | Key data names  | **\_item.category\_id** , **\_category\_key.name**        |
    +-----------------+-----------------------------------------------------------+
    ||                || **\_item\_description.description**,                     |
    ||                || **\_category.description**, \_category.examples          |
    || Interpretation || \_item\_examples.{case/detail},                          |
    ||                || \_item\_linked.parent\_name, \_item\_linked.child\_name  |
    |                 || \_item\_methods.method_id                                |
    |                 || \_method\_list.{id/detail/inline/language/code}          |
    +-----------------+-----------------------------------------------------------+
    ||                || \_dictionary.{title/version/datablock_id}                |
    || Management     || \_datablock.id, \_datablock.description                  |
    ||                || \_dictionary_history.{revision/update/version}           |
    +-----------------+-----------------------------------------------------------+

.. raw:: latex
             
   \end{fullwidth}

Differences between DDL2 and DDLm dictionaries
----------------------------------------------

* DDL2 has a system for describing values in terms of regular
  expressions, which the dictionary author may customise, whereas DDLm
  dictionaries are restricted to the types provided in the DDLm
  attribute dictionary and do not use regular expressions.
  The same customisable approach is adopted by DDL2 for units.

* DDL2 dictionaries specify data name parent-child relationships (see
  below) in the top-most category definition, and optionally at the
  site of the child data name definition.

* DDL2 assumes all data names can take multiple values, whereas DDLm
  allows explicit restriction of some data names to single values
  within a data block
  
* DDL2 allows data transformation methods to be specified in any
  programming language, whereas DDLm requires environment-agnostic
  dREL code

* DDLm has a well-specified protocol for combining dictionaries and
  using templates to simplify dictionary construction.

* DDLm assigns meaning to category parent-child relationships.

Understanding and composing DDLm dictionaries
=============================================

Parent and child data names
---------------------------

Our earlier discussion supposed that key data names might appear in
multiple categories.  When data from these categories is presented in
CIF loops, identical data names would appear in each loop. CIF,
however, strictly forbids data names to appear more than once in a
data block; therefore we must define distinct data names for each
category in which a key data name appears. Note that our suggested
`<category>.<name>` naming strategy automatically takes care of this
for us.

We can arrange these data names into a parent-child hierarchy, where
child data names are expected to take a subset of the values of the
parent data names.  In most situations, there will be only a single
child level in this hierarchy.

Semantic structure
------------------

As discussed earlier, the data names in an ontology can be distributed
into categories, within which every data name shares the same key data
names or is itself a key data name for the non-key data names. CIF
dictionaries describe each of these categories in their own save
frames. Every data name definition states the category to which it
belongs, and likewise every category has a single parent
category. :sidenote:`The meaning of the category parent-child
relationship is discussed later.` The category definition at the top
of this hierarchy is called a 'Head' category.

A category corresponds to the set of data names appearing in loop
within a CIF file.

Data blocks and CIF
-------------------

Data names that are restricted to single values within a CIF data
block are assigned to ``Set`` categories. :sidenote:`The type of
category is indicated by setting_definition.class
to Set or Loop` All other data name definitions must belong to ``Loop``
categories.

The values of Set category key data names do not need to be provided
in a data file, as these values are both arbitrary and unique by
virtue of being the only value in the block. :sidenote:`A mechanism
exists in CIF to add back these key data names in supplementary
dictionaries, should they be required.` The same consideration applies
to any child data names. Key data names from Set categories, and their
children, can therefore be completely dropped from the CIF dictionary.

Writing a DDLm dictionary from scratch
======================================

You may find it convenient to use a shorthand notation while developing
the dictionary, that can be automatically transformed later to CIF. For example,
writing::

   #N my_data_name
   #C my_category
   #D this is the definition
   #L value1: stuff, value2: more stuff, value3: no more 

   
instead of


.. code:: cif
      
    _definition.id '_my_category.my_data_name'
    _name.object_id  my_data_name
    _name.category_id my_category
    _description.text 'This is the definition'
    loop_
    _enumeration_set.state
    _enumeration_set.detail
        value1      stuff
        value2      'more stuff'
        value3      'no more'


Step 1. Name, and write a definition for, the Head category.
------------------------------------------------------------

The name of the Head category is purely for internal use. The
``_definition.id`` and ``name.object_id`` attributes hold the
name of the category. ``_name.category_id`` should be set
to the value of the ``_dictionary.title`` attribute. Here is a template:

.. code:: cif

     save_CIF_CORE

     _definition.id CIF_CORE
     _definition.scope Category
     _definition.class Head
     _definition.update 2014-06-18
     _description.text
     ;
     The CIF_CORE category contains the definitions of data items that
     are common to all domains of crystallographic studies.
     ;
     _name.category_id CIF_DIC   #_dictionary.title from enclosing block
     _name.object_id CIF_CORE

     save_


Step 2. Write the category definitions.
---------------------------------------

     You will need to assign the following attributes:

- ``_definition.id``: the name of the category
- ``_definition.scope``: ``Category``
- ``_definition.class``: ``Set`` or ``Loop``. See above for explanations of these terms.
- ``_definition.update``: the date the definition was written YYYY-MM-DD
- ``_description.text``: a human-readable description of this category
- ``_name.category_id``: the category this belongs to (usually the Head category).
- ``_name.object_id``: the name of the category (again)
- ``_category_key.name``: (possibly looped) the list of key data names for this category.  Only necessary for ``Loop`` categories.

Here is a simple Loop category definition:
    
.. code:: cif
        
   save_DIFFRN_ATTENUATOR
   _definition.id     DIFFRN_ATTENUATOR
   _definition.scope  Category
   _definition.class  Loop
   _definition.update 2013-09-08
   _description.text
   ;
   The CATEGORY of data items which specify the attenuators used in the
   diffraction source.
   ;
   _name.category_id  DIFFRN
   _name.object_id    DIFFRN_ATTENUATOR
   _category.key_id   '_diffrn_attenuator.code'
   loop_
      _category_key.name
          '_diffrn_attenuator.code'
   save_


Step 3. Write the data name definitions
---------------------------------------

You will need to assign the following attributes:

-  ``_definition.id``: the data name, usually ``<category>.<object>``
-  ``_definition.update``: the date the definition was written YYYY-MM-DD
-  ``_description.text``: a human-readable description of the data name
-  ``_name.category_id``: the category this belongs to
-  ``_name.object_id``: the name within the category
-  ``_type.container``: ``List/Array/Matrix/Table`` for compound data
   values,
   ``Single`` otherwise
-  ``_type.contents``: the nature of the individual values taken by this
   data name
-  ``_units.code``: the units for the data values. Leave out or put
   ``none``
   if none.
-  ``_type.source``: where values come from (optional but useful)
-  ``_type.purpose``: a classification of the values into some general
   classes. Useful 
   information for dictionary tools.

Some common situations covered by further attributes are:

* | If your data name can take values from a restricted set, use 
  | ``_enumeration_set.{state/detail}`` to list and describe each option.

* If your data name corresponds to a key data name from another
  category :sidenote:`a link within a single category is also possible` that
  appears in this category, set ``_name.linked_item_id`` to that other
  data name and set ``_type.purpose`` to ``Link``.

* If your data name directly replaces another data name, assign the 
  older name to ``_alias.definition_id``

* If your data name is the standard uncertainty of another data name,
  set ``_type.purpose`` to ``SU`` and ``_name.linked_item_id`` to that
  other data name.

* If a specific value can be safely assumed when the data name is
  missing from a data file, specify this with
  ``_enumeration.default``.

* If your data value is a list, array or matrix give the dimensions
  using ``_type.dimension``.  An asterisk (``*``) can be used for
  arbitrary values.

.. code:: cif

   save__diffrn_source.device
   _definition.id 'diffrn_source.device'
    loop_
   _alias.definition_id
       '_diffrn_radiation_source'
       '_diffrn_source.source'
   _definition.update 2013-08-09
   _description.text
   ;
   Enumerated code for the device providing the source of radiation.
   ;
   _name.category_id diffrn_source
   _name.object_id device
   _type.purpose State
   _type.source Assigned
   _type.container Single
   _type.contents Code
    loop_
   _enumeration_set.state
   _enumeration_set.detail
   tube                'sealed X-ray tube'
   nuclear             'nuclear reactor'
   spallation          'spallation source'
   elect-micro         'electron microscope'
   rot_anode           'rotating-anode X-ray tube'
   synch                synchrotron
   _enumeration.default tube
   save_

 
Step 4. Create the enclosing data block
---------------------------------------

Assign the various ``dictionary.*`` attributes at the top of the data
block. Arrange the save frames (definitions) in alphabetical order,
unless you are sure some other order is more natural for a human
reader searching for a particular definition.

Adding to a pre-existing DDLm dictionary
----------------------------------------

Examine the currently-existing categories in the dictionary to determine
if any of them have matching keys. If not, follow step 2 to add new categories.
Add your new data names as in Step 3 above.

Further DDLm dictionary enhancements
====================================

Imports
-------

DDLm dictionaries allow groups of attributes to be imported from 
another dictionary using the ``_import.get`` attribute. The value 
for ``_import.get`` is a list of tables. Each table has a few keys 
that are set to guide the import process:

**file**:
    the file to import from

**save**:
    the name of the save frame to import from the file

**mode**:
    ``Contents``: only the contents of the save frame are
    inserted; 
    ``Full``: the entire save frame and semantic children are included.


Importation is useful when many of the attributes for a set of data
names are identical. In this case, the attributes are put into a save
frame in a separate CIF "template" dictionary, and the relevant save
frame is simply imported into each of the definitions.

.. code:: cif
    
    save__diffrn_standard_refln.index_h
    
    _definition.id         '_diffrn_standard_refln.index_h'
    _import.get            [{'save':Miller_index 'file':templ_attr.cif}]
    _name.category_id      diffrn_standard_refln
    _name.object_id        index_h
    
    save_
    
The file ``templ_attr.cif`` contains save frame:

.. code:: cif
    
    save_Miller_index
   _definition.update           2013-04-16
   _description.text
   ;
   The index of a reciprocal space vector.
   ;
   _type.purpose                Number
   _type.source                 Recorded
   _type.container              Single
   _type.contents               Integer
   _enumeration.range           -1000:1000
   _units.code                  none
   save_
    

Another use for this feature is to put long lists of enumerated items
into separate files to avoid cluttering the main dictionary. Examples
include element names, and units.

A separate use for imports is to
specify dictionaries that your dictionary builds on. If your
dictionary adds (or changes) data names from any categories in another
dictionary, it is sufficient for the Head category of your dictionary
to include an import of the Head category of the dictionary it builds
upon. In this case, the import ``mode`` should be set to
``Full``. Semantically, any definitions in the importing dictionary
with the same ``_definition.id`` will replace definitions in the
imported dictionary unless you have set the import key ``if_dupl`` to
``ignore``.

.. code:: cif
          
   save_PD_GROUP

   _definition.id               PD_GROUP
   _definition.scope            Category
   _definition.class            Head
   _definition.update           2014-06-20
   _description.text                   

   ;
     Groups all of the categories of definitions in the powder
     diffraction study of materials.
   ;
   _name.category_id            CIF_POW
   _name.object_id              PD_GROUP
   _import.get
            [{"file":"cif_core.dic" "save":"CIF_CORE" "mode":"Full"}]

   save_

Child categories
----------------

In DDLm, Loop categories may be child categories not only of the Head
category, but also of other Loop categories.  A child category is
obtained by splitting a single category into two parts, so that each
part retains the same key data names, but the remaining data names
differ.  For most purposes, the two categories can be considered to
be a single category that have been split for convenience, for example,
when some subset of data names are likely to have undefined values for
some of the key data name values. :sidenote:`The combined category is
obtained by a **left outer join** of the parent with the child.`
It is permissible for data files to
present all of the data names from both categories in a single loop.

.. note:: The DDLm core CIF dictionary allows anisotropic displacement
   parameters to be presented in a separate loop, described in the
   ``atom_site_aniso`` category, which is a child category of
   ``atom_site``.  This allows those experiments for which many atoms
   do not have well-determined ADPs to save space in the `atom_site`
   listing.

.. note:: The magnetic structures dictionary makes `atom_site_moment`
   a child category of `atom_site`, in recognition of the fact that
   for many structures only a few atomic sites will have associated
   moments. In such cases, the moments can be listed in a separate
   loop.

The child category after the split is the category that might take only
a subset of the key data values.  In the examples above, not all atom
sites have to be listed in the `atom_site_moment` or
`_atom_site_aniso` loops, so these are the child categories.
   

dREL
----

Mathematical relationships between data names can be expressed using 
the ``_method.expression`` attribute. The value of this attribute is 
computer-parseable program code written in dREL. A dREL expression in
a data name definition describes how a value for the data name is 
calculated from the values of other data names. The DDLm cif core 
dictionary contains many examples of dREL expressions. dREL is 
described in Spadaccini `et. al.`, `J. Chem. Inf. Model.`, 2012, **52**, 1917-1925.

The advantage of dREL over concrete programming languages is that no
CIF programming library or language environment is assumed, that is,
dREL is environment-agnostic.  As a result, in order to execute dREL
code it must first be transformed to a concrete language, inserting
the particular function calls required by the language and CIF API.

DDLm Dictionary extensions
==========================

_audit.schema
-------------

When writing the dictionary, you had to make a decision regarding
which categories would allow only single-valued data names. This set
of single-valued names would ideally satisfy the majority of data
transfer scenarios in your discipline. For those users who need some
(or all) of those data names to take multiple values (e.g. multiple
samples per run), it is possible to define an extension
dictionary. This dictionary ``_imports`` the original dictionary, and
then redefines the relevant category to be a ``Loop`` category,
creates and assigns the key data names for the category, and adds the
child key data names to all affected categories.

Note that software written assuming the behaviour in the original
dictionary is susceptible to behaving incorrectly if it does not know
which dictionaries a given data file is written to conform to. At the
same time, programmers are not necessarily prepared to read and parse
entire dictionaries at run-time in order to check the interpretation
of potentially only a few data names. The ``_audit.schema`` data name
in core CIF has therefore been introduced as a shorthand flag that a
data file is using a non-default set of single-valued data names. It
is sufficient for software to check this and the following data name
to ensure that the file contents are correctly understood.

_audit.formalism
----------------

Where a dictionary redefines one or more data names from the base 
dictionary, for example, by enhancing a structural model (magnetism, 
powder, modulated structures), the change in definition is flagged 
via the ``_audit.formalism`` tag.

Further reading
===============

* "DDLm: A new dictionary definition language", Spadaccini, N. and Hall, S. R.,
  `J. Chem. Inf. Model.`, 2012, **52**, 1907-1916

* "dREL: A relational expression language for dictionary methods",
  Spadaccini, N.  Castelden, I.R., du Boulay, D. and Hall, S. R.,
  `J. Chem. Inf. Model.`, 2012, **52**, 1917-1925

* "Specification of a relational dictionary definition language
  (DDL2)", Westbrook, J. D., Berman, H. D., and Hall, S. R.,
  `International Tables for Crystallography, Volume G`, 61-72

* `The DDLm attribute dictionary
  <http://github.com/COMCIFS/cif_core/blob/cif2-conversion/ddl.dic>`_
  (``http://github.com/COMCIFS/cif_core/blob/cif2-conversion/ddl.dic``)


  
