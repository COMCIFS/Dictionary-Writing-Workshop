Components of a Data Transfer Framework
=======================================

.. role:: sidenote

          
Any system for transferring data, whether it is a three-column ASCII
file attached to an email or a CIF file, can be broken into four parts:

1. Data values: the values to be transferred. Values may be anything
   that we can plausibly transfer *via* computer, including numbers,
   text, vectors, sets, lists and images.
2. An ontology: a shared understanding of the meanings of the data
   values. This can be accomplished by attaching values to *data names*.
   The data name is then used to communicate how the one or more
   associated values are to be interpreted. The data names are collected
   into a data name *dictionary* , describing the ontology.
3. One or more file formats: structured arrangements of data values
4. Linkage between a format and an ontology: a description of how the
   data names and values from the ontology are encoded in a given
   format.

.. raw:: latex

   \begin{figure}
   \caption{The four data transfer components in an email message}
   \includegraphics[width=\textwidth]{email-ontology.eps}
   \end{figure}
             
The absence of any one of the above four components must lead to a
failure in data communication. No data values means no data; no shared
ontology means that the sender and receiver would understand the data
differently; no format means no electronic encoding of the data; and no
linkage means no way of encoding the data values in the format in a
mutually understandable way.

The ontology and format-ontology linkage are often only informally or
partially specified, relying on a common store of scientific knowledge
and obvious meanings of shorthand data names.  This approach is
reasonably effective in some contexts. A more formal approach
describes the ontology using a set of standard attributes in a
machine-readable file. Later in this course we will show how to use
CIF tools to write such a formal ontology.

Note that the ontology may be cleanly separated from the particular
format used to encode the data values. This is in a sense obvious when
we consider that observations and results should not depend on the form
in which they are communicated. The first part of this course focuses
only on describing the data names (that is, developing the ontology).
The resulting ontology is applicable to any format.

Data names and their relationships.
===================================

As described above, each of our data values belongs to a data name,
which is a label for the concept that the value is an instance of.
Examples include 'wavelength', 'measurement point', 'user name', 'sample
composition'. Any data name will have multiple possible values - if a
data name could only ever have one value, then there is no point
including it in a data transfer framework, as the data file would not be
providing any new information for this data name.

Context
-------

Given that any data name could have multiple values, we need some
further information to understand what distinguishes the values from
one another. We could call this the "context" of a data value. The
context is described by assigning specific values to some set of data
names {K}. If every value of the data names in {K} is the same, then
our target data name *must* always have the same value (or the
logically equivalent statement: if the value of our data name changes,
then at least one value of the data names in {K} *must* be
different). :sidenote:`The values taken by the data name are a
mathematical mapping (function) of the values in {K}.` If this is not
the case, we add further data names to {K} until it is true. As a
logical consequence, if any one of the values for the data names in
{K} is different, then our target dataname *may* have a different
value. We will call the list of data names {K} the *key* data names
for the data name that is defined.

.. important:: Work out possible key data names for each data name in the following list.

   | "atomic position"   
   | "atomic element"   
   | "monochromator setting"   
   | "observed intensity"   
   | "calculated intensity"  

Observations and Calculations
-----------------------------

For convenience we now divide data names into "observations" and
"calculations". Calculated values will always be the same if the same
input values are used, so the set of key data names for calculated
values is just an exhaustive list of parameters for the calculation.
Under "observations" we include not just the particular quantities
being measured, but also any descriptive information about the
experiment. What are the key data names for such observational data
names?  Scanned quantities cannot satisfy our criteria. For example,
if we measure at a series of energies and measure intensity at each
energy point, we may think that our observed intensity values are
distinguished from one another by the energy at which they were
measured; however, the same value of energy could possibly lead to a
different intensity on a subsequent measurement due to statistical
variation; so it does not satisfy our criterion that identical values
of the key data names *must* correspond to identical values of the
defined data name. :sidenote:`It is important not to confuse our model
of the world, in which we expect the measured value to be determined
by the values of some other parameters, with the actual experiment, in
which a series of observations are made.` The key data name for
observed quantities is therefore simply "a measurement point", with
arbitrary values that we assign (see below).

Making values of "a measurement point" globally unique is cumbersome. We
can add a further key data name, something like "dataset id", to avoid
this. If measurements are divided into a series of scans, with different
settings for each scan, "scan id" might also be a useful additional key
data name.

Identifiers
-----------

We have introduced a third class of data names, those that are
"identifiers". In our example above, these are the measurement points.
For "identifier" data names, the actual values are irrelevant, as the
values are only used to distinguish or label different
members. Examples of identifier data names include measurement points,
serial numbers, atom sites, sample numbers and run numbers. While we
often use numbers for identifier data values because it is easy to
pick a unique one if we label sequentially, their numerical properties
should not be used; if an identifier value has some other meaning, or
is used in calculations, then we run the risk that a situation will
arise where the same value should be assigned to distinct items, and
so our values can no longer serve as identifiers. For example, we may
decide to identify image frames in a data collection by numbering
sequentially from zero, with each frame corresponding to a small
uniform change in a sample orientation axis. If we then fall into the
trap of using the image number multiplied by the axis step to get the
axis value, we can no longer cope with a situation where the same
orientation was recollected, for whatever experimental reason.

When choosing identifiers, consider the human user and suggest a
natural system of labeling in your definition - labels that are
meaningful to humans are just as good as random strings, but the
labels should never be involved in manipulations specified in other
definitions.

Unlike other data names, *identifiers do not always have key data
names* . :sidenote:`Mathematically, identifiers are their own key data
names.` Identifiers can appear both as key and non-key data names in
the ontology: for example, in our description of a structure an atom
site may have an 'atom type' giving the element occupying that site.
Elsewhere in our ontology we might have 'form factor' 'valence'
'isotope', which have 'atom type' as the key data name.  The values of
the former 'atom type' are drawn from the values of the latter. It is
clearly important to distinguish these two uses of 'atom type', as
their interpretation is different: one is "the atom type at a given
atomic site", and the other is "the atom type to which this valence/
isotope/form factor relates" :sidenote:`The full interpretation might
be "the atom type at the atomic site in the structural solution for
this dataset"`. For this reason the two distinct uses must be assigned
different data names, for example "atom site atom type" and "atom
type".

Summary
-------

In order to define a data name we need to identify the key data names,
the values that our data name can take, and how to use the values of the
key data names to interpret values of the defined data name. A data name
describing an observed fact could be defined as simply as "the value of
A when the measurement was taken", whereas a data name defining a
derived quantity would need to identify all of the parameters of the
calculation and the equations involved. References to published works
for calculations may be sufficient as the target of our definition is a
human reader (probably a scientist-programmer).

Practice questions:
-------------------

    Q 1. During a synchrotron experiment, monitor counts are recorded in
    a gas filled ion chamber. Which of the following data names are key
    data names for the counts measured in the ion chamber?

    | A: the gas pressure
    | B: the gas mixture
    | C: the ion chamber length
    | D: all of the above
    | E: none of the above

    Q 2. During the same synchrotron experiment, variation of
    transmitted intensity as a sample is moved across the beam is
    recorded. The expected measured intensity is calculated following
    the experiment. Which of the following data names are key datanames
    for this calculated intensity?

    | A: the monitor ion chamber measured intensity
    | B: the sample thickness
    | C: the detector voltage
    | D: all of the above
    | E: none of the above

Creating the ontology, step by step
===================================

Step 1: Brainstorm data names
-----------------------------

Write down all of the concepts that might be included in a data
file. For convenience, assign some short, descriptive names to them
(these names may change later). Some data names may be implied by what
is already in your list.

-  Think in terms of objects and their properties, for example, "an
   experimenter" may have properties "name", "address" "role"
   "photograph".
-  Look at the nouns in your definitions for indications of identifier
   data names.
-  Locate identifiers and consider whether these could or should be
   classified more finely, just as we divided "measurement id" into
   "dataset id" "scan id" "measurement id" above.  Such divisions are
   purely for convenience, and make sense if you expect each
   identifier to have many values in a given data file *and* you can
   think of relevant properties that are constant for each value of
   the identifier. For example, within a single scan the scan step
   or some orientation might be constant.
-  Look at the data files that are already used in your field and
   extract data names from them. To locate data names, remember every
   scientifically useful value in a data file belongs to a data name.
   Examine the context of these values to find key data names. The
   context in a hierarchical structure consists of the nodes above the
   value of interest, and the values attached to the same node.
   Further context might be indicated in the specifications.

Step 2: Sharpen up the definitions
----------------------------------

For each of your datanames from Step 1, write a definition that
conveys unambiguously to a human reader the following three
things:

1. the nature of the data values (e.g. arbitrary identifier,
   real number, text, vector, integer, yes/no, image)

2. the key data names

3. how to interpret this dataname given the values of the key data names

Add any further data names that you have overlooked. A classification
into "observations","calculations" and "identifiers" may help, with
identifiers often associated with indefinite nouns like "an image" "a
measurement" "an experimenter". You could use well-defined terminology
from your field and references to literature to keep your definition
short.

.. note::  Finding key data names.
  What are the key data names for "an experimental role",
  which we have defined as "the role performed by an experimenter during
  the experiment"? 

  "An experimental role" is observational, so
  {"measurement id","dataset id"} are key data names.  Our
  definitional sentence includes nouns "role" and "experimenter", both
  of which could become identifiers.  If we have a measurement and a
  person, do we have a single unique role identified?  In other words,
  could one person perform two roles at once?  If not (we did after
  all write "**the** role"), then {"measurement id","dataset id" and
  "experimenter id"} are sufficient.

.. important::

   Work out how to represent simultaneous roles.  Possible
    roles might include {"principal" "attending" "experimenter" "dogsbody"
    "programmer" "instrument responsible"} . See the ionisation chamber example
    below for ideas.

Step 3: Make your definitions computationally useful
----------------------------------------------------

Remember that an important reason for this work is to convey information
in a way that is manipulable by computer.

1. Any data name that ends up having values that are free text strings
(e.g. "sample description") is essentially using the data file as a
glorified word processor format, and has a much reduced value in
automated settings. So look over your datanames, and where you have
quantitative information in free or formatted text, rework it into
observational or calculated data names that take numerical values.

2. Where you have two or more identifier data names that refer to the
same type of thing, with the same key data names, you should rework
your ontology as follows.  Create a new key data name that will be
used to identify combinations of values for these duplicate datanames
(let's call it "C"). Now create a second key data name that will take
the values that your original data names were supposed to take.
Finally, replace your duplicate data names by a single identifier data
name that draws from the values of C.  The same information is now
representable in an extensible way.

.. note::

    Ion chambers used at synchrotrons have sensitivity to the X-ray
    beam running through them tuned by adjusting the gas or mix of
    gases in them.  We wish to record this information in our data
    files.

    Our starting definition is: **gas mix** "the mixture of gases in
    an ion chamber, in format element-percent-element-percent", with
    key data name "ion chamber id".  If we tabulate this, we might
    have:

     +----------------+-------------+
     | ion chamber id | gas mix     |
     +----------------+-------------+
     | BB25           | He-50-N-50  |
     +----------------+-------------+
     | XYZ            | Ar-100      |
     +----------------+-------------+
     | Old-G          | Ar-100      |
     +----------------+-------------+
     
   As described in point (1) above, this embeds data items into the
   value, essentially making them unavailable elsewhere in our
   ontology.  To remedy this, we create data names "first gas" "first
   gas percent" "second gas" "second gas percent".

     +----------------+-------------+-------------+-------------+--------------+
     | ion chamber id | first gas   | first gas % | second gas  | second gas % |
     +----------------+-------------+-------------+-------------+--------------+
     | BB25           | He          |   50        |    N        |    50        |
     +----------------+-------------+-------------+-------------+--------------+
     | XYZ            | Ar          |   100       |    .        |     .        |
     +----------------+-------------+-------------+-------------+--------------+
     | Old-G          | Ar          |   100       |    .        |     .        |
     +----------------+-------------+-------------+-------------+--------------+

   Now we are in the situation described by point (2).  The gases and
   gas percentages are of the same type (with the same key data name),
   and in a situation where three or more gases are used we would need
   to define new data names.  Following the prescription in Point (2)
   we create a new identifier **gas mix id** and replace the original
   identifier data names "first/second gas" by **gas**.  If we have a
   gas mix id and a gas, we can assign a percentage, so we make these
   two data names key data names for a new data name "gas percentage"
   and drop "first/second gas percent".  Now, given an ionisation
   chamber, it is sufficient for us to nominate the gas mix id to
   completely identify the gas components - but recall from earlier
   that the gas mix id that has ionisation chamber as its key data
   name must have a different data name.
    
    Now we can tabulate all of our mixes:
    
    +------------+--------------+------------------+
    | Gas name   | gas mix id   | gas percentage   |
    +============+==============+==================+
    | Ar         | C            | 100              |
    +------------+--------------+------------------+
    | He         | A            | 50               |
    +------------+--------------+------------------+
    | N          | A            | 50               |
    +------------+--------------+------------------+
    
    And so we might then also describe our detectors as follows:
    
    +-----------------+-----------------------+-------------------+------------+
    | detector name   | detector gas mix id   | detector length   | location   |
    +=================+=======================+===================+============+
    | BB25            | A                     | 5                 | monitor    |
    +-----------------+-----------------------+-------------------+------------+
    | XYZ             | C                     | 5                 | detector   |
    +-----------------+-----------------------+-------------------+------------+
    | Old-G           | C                     | 10                | foil       |
    +-----------------+-----------------------+-------------------+------------+

3. Where there are limited choices for the value of a data name,
   explicitly define each of these choices and assign a number or string
   to them. For example, instead of a dataname "location", with a
   description of position left up to the software author, you might
   define "monitor": before the sample "detector": measure signal from
   sample "foil": measure signal after sample and calibration foil.

4. Units. Some file formats offer structures that allow the file writer
   to specify units. Avoid using these as they create extra work for the
   file reading software in anticipating every possible unit that is
   appropriate. Usually only one or two units are in common use, so
   choose and specify a unit in your definition. If the community has
   not converged on a particular unit, create a second definition that
   differs only in the unit used.

.. tip:: Units.  if you allow units to be specified in the data file
      instead of the definition of some data name A, you could be
      considered to be creating a new key data name "units for A". One
      of these key data names will exist for every data name that
      takes units, and the definition for each of these key data names
      should list all possible values for the unit in question. This
      listing could be done explicitly and somewhat economically by
      referring to external standards, which has the downside that, if
      these standards are updated, your ontology will also "auto
      update", whether you like it or not. This can be difficult for
      programmers who wish to track your ontology.

      An alternative viewpoint is that your data file is simply providing a 
      missing part of the ontological specification.

5. Software-specific names. Any data name that essentially refers to the
   input or output of some software package calculation has value in
   proportion to the number of people with access to the version of the
   software in question, or to the extent to which the software
   setting/output can be linked to specific calculations through
   documentation or source code. Given this, the value of such data
   names is likely to decline over time. Therefore, where such data
   names occur, attempt to rephrase them in non-software-specific terms.
   Instead of "multiplicity as calculated by XYZ Version 1.2", write
   "the number of special positions divided by the number of general
   positions".

6. Instrument-specific names. Similarly, any data name whose
   definition refers to the configuration of an instrument in a way
   that is insufficient to enable reproduction in a different lab or
   independent modelling is unlikely to be of use outside the lab that
   produced it.  Instead of "Position of motor mom" think
   "monochromator takeoff angle". Of course, a large facility may
   choose to create an ontology for in-house use in which case such
   definitions might be sufficient for internal purposes when combined
   with local knowledge.

At the completion of this step, your ontology has all the information
necessary to use it for data transfer. We now draw out some important
features of the ontology for practical use.

Step 4: Data blocks
-------------------

When you consider your data names, it is likely that some of them will
have the same value over the entire data set that you wish to transfer
(e.g. user names, beamline, equipment). If we were to actually record
these in our data file for every measurement point, it would be a real
waste of space. "Data blocks" group our data values into blocks, and
within each block these constant-valued data names are understood to
apply to all data values within the block for which they are relevant
according to the ontology, like global variables in programming.

Of course, the precise choice of constant data names depends on the
experiment. Many current data transfer frameworks suppose a particular
set of constant data names, and this assumption carries across to
software. Explicit labelling of typical sets of constant data names
will both aid software authors, and serve as reminder that all data
names could conceivably take multiple values.

.. important:: define at least two sets of constant-valued datanames for
    your field. 

Step 5: Categories
------------------

Group your data names so that data names in the same group have the
same key data names. These groups of data names are called
*categories* in CIF. If all the separate values of the key data names
are listed in side-by-side columns, the corresponding values of the
other data names in the category can be compactly tabulated together
with them. Using this strategy, together with separately listing
values for data names that do not change within a data block, leads to
considerable space reduction when encoding values into files.

Step 6: Naming
--------------

It is organisationally useful to name the categories, and then name the
data names within them using the category name as a prefix. In this case
(i) data names that are closely related will often be close when listed
alphabetically (ii) it will be easier for a human reader to recognise
which key data names a given data name is related to.

Whether or not you choose to include the category in your name, you must
eventually decide on permanent names to each of your draft data names.
Short names are good for programmers, but potentially confusing - is
"temp" short for temperature or temporary? Whitespace is not an issue
for modern programming languages, but in some contexts (e.g. operating
system shell scripts) can be annoying.

Summary
-------

You should now have a list of data names, with associated meanings that
are unambiguous and fit for use in data transfer contexts. You have
defined one or more data blocks.

Using the ontology
==================

As discussed in the introduction, the ontology must be mated with one
or more formats in order to transfer data. While this is largely
outside the scope of this workshop, a few general points can be made
about format selection:

1. the data values must be representable within the format. This is
   generally trivially possible, as any value can be represented as
   text, that is, a sequence of bytes with a specified text encoding.

2. the correspondence between each data value and its key data names'
   values must be representable. This requirement is met by any
   format that can put data values into ordered lists; in this case
   values at the same position in a list can be considered to
   correspond.

3. The format must be extensible to an arbitrary number of multiple-valued
   datanames, to allow for future growth.
   
4. All other format considerations would be based on non-ontological
   criteria, such as software support, efficiency, or long-term archival
   support.

.. important:: Consider any data formats that you are familiar
                  with. How well do they meet requirements (1)-(3),
                  and your particular requirements?
                 
      
Advanced topics
===============

Aggregate calculations
----------------------

    Q: Give a key data name for data name "average measured intensity"

Calculations that depend on a whole collection of data values, such as
"number of measurements", averages, observed uncertainties, and
Fourier transforms, have key data names that identify whole sets of
data. :sidenote:`The individual "observational" data names clearly
have some relationship to this "measurement set id". A particular
measurement can be derived from a measurement set by assigning some
unique identifier to each member of each set (which could be our
"measurement id"), and then specifying a measurement set and the
particular identifier.` For a typical data block, there would be only
one set of data and so an identifier for the whole data set could be
left out of the data file because it is both arbitrary and
single-valued. Its existence only becomes apparent when multiple data
sets are handled, and some way of referring to a particular set of
measurements is needed.

A value that is the result of a Fourier transform will depend on a
similar "set id" that in many cases is also isomorphic to a dataset id.
Explicit inclusion of this "set id" would only become necessary when
there are multiple runs of data requiring separate Fourier
transformation and recording of the result prior to, for example,
merging. Such merging also constitutes an aggregate function that might
entail a new id if multiple separate merging processes are to be
recorded. And so forth.

Adding and redefining data names
--------------------------------

Once an ontology is published, the relationships between data names and
their key data names become enshrined in software that is then
distributed and relied upon. If we change these relationships later, we
risk silent misinterpretation of new data files by legacy software.

Adding new non-key data names is unproblematic. This is often the case
for "observational" data names, for example, providing a new data name
to report humidity during data collection does not affect the
intepretation of any other "observational" information. Similarly, whole
new categories (data names and their key data names) can be added with
no effect on existing data names.

Adding new *key* data names to already-existing data names would, in
theory, never happen as the context was supposed to have been completely
defined when we selected our original key data names. However, as time
goes on calculations are improved by the addition of new parameters, or
models are expanded. For example, our original ontology for single
crystal crystallography may have listed model structure factor against
key data name "hkl". When we expand this ontology to include
incommensurate structures, we need to add the extra indices as
additional key data names. We can avoid the software errors mentioned
above by simply duplicating our original category with data names
redefined to include the new key data names, but this has the drawback
that any categories that referred to data names appearing in our
original category will also need redefinition if the link is to be
preserved.

A simple solution to this proliferation of data names that mean almost
the same thing is to define a data name that identifies the model used.
This is an additional key data name that is usually constant for a given
data set. Such a data name should be defined when an ontology is first
published, so that a check of its value is incorporated into all
software.

Further reading
===============

[cite Spivak][cite Hester]
