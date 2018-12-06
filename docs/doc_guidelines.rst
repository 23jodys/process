########################
Documentation Guidelines
########################

***********
Line Length
***********

The line length of the raw rst formatted text should be 79 or less, but should not
exceed 120 characters. Do whatever makes it the easiest to read in its raw form.


********
Headings
********

Use the following symbols to create headings:

* Top level document, e.g. SRS, SDS, etc: ``#`` with overline
* Section in document: ``*`` with overline
* Subsections:

  * ``=``
  * ``-``
  * ``^``
  * ``"``

As an example:

.. code-block:: rst

  ##################
  H1: document title
  ##################

  Introduction text.


  *********
  Sample H2
  *********

  Sample content.


  **********
  Another H2
  **********

  Sample H3
  =========

  Sample H4
  ---------

  Sample H5
  ^^^^^^^^^

  Sample H6
  """""""""

  And some text.

If you need more than heading level 4 (i.e. H5 or H6), then you should consider
creating a new document.

There should be only one H1 in a document.

.. note::

  See also `Sphinx's documentation about sections`_.

*****
Lists
*****

Use auto-enumerated unless you have a good reason not to.

::

    #. Item 1
    #. Item 2

Renders as:

#. Item 1
#. Item 2


***********
Code blocks
***********

Use the ``code-block`` directive **and** specify the programming language. As
an example:

.. code-block:: rst

  .. code-block:: python

    import this

******************
Pre-formatted text
******************

Text can be rendered pre-formatted in a fixed width font with::

  You just need to end a paragraph like this::

    and the stuff indented here is pre-formatted

This renders like:

You just need to end a paragraph like this::

  and the stuff indented here is pre-formatted

***********
Whitespaces
***********

Indentation
===========

Indent with 2 spaces.

Except:

* ``toctree`` directive requires a 3 spaces indentation.

Blank lines
===========

Two blank lines before overlined sections, i.e. before H1 and H2.
One blank line before other sections.
See `Headings`_ for an example.

One blank line to separate directives.

.. code-block:: rst

  Some text before.

  .. note::

    Some note.

Exception: directives can be written without blank lines if they are only one
line long.

.. code-block:: rst

  .. note:: A short note.


**********
Glossaries
**********

If you find yourself using a term repeatedly that does not already
have a precise definition, define it! At the bottom of your document

.. code-block rst::

  .. glossary::

    term
      The terms definition

    other term
      The other terms definition

This will render like

.. glossary::

  term
    The terms definition

  other term
    The other terms definition

Now that you've defined them you can cross reference them in your 
text::

  In the text, I use :term:`term` and :term:`other term`.

This is how it will render.

In the text, I use :term:`term` and :term:`other term`.

********
Diagrams
********

Diagrams should be created using ASCII art and included in a code-block.
Diagrams can be created using `Monodraw <https://github.com/archerdxinc/docs/wiki/Monodraw-Ascii-Art-Tool>`_

Diagrams should be exported using ASCII, not unicode.

.. image:: images/monodraw_ascii.gif
   :alt: Selecting ascii character set in monodraw

Example::

  .. code-block:: text

     +------------+   +--------------------------------------+
     |Git describe|   |         1.0.0-34-0-acc2365a          |
     +------------+   +--------------------------------------+

Renders as 

.. code-block:: text

  +------------+   +--------------------------------------+
  |Git describe|   |         1.0.0-34-0-acc2365a          |
  +------------+   +--------------------------------------+



********************
Links and references
********************

There are several things that you should cross reference inside
of your documentation

* References to other documents, ``:doc:`blah```
* References to sections in other documents. The references is the
  document file path (without extension and with respect to ``ivd-base``) plus
  the section, e.g. :ref:`docs/documentation-sop:References`
  ::

  :ref:`docs/documentation-sop:References`
* References to sections in this document, ```Name of Section`_``
* References to design elements, ``:item:`SRS0001```
* References to terms defined in the glossary, ``:term:`my term```
* References to URLs, ```link name <http://www.example.com>`_``
  
.. _traceability:

************
Traceability
************

We maintain numerous traceability links, as defined below. Each design element
is given a unique identifier for its element type. An identifier should
**never** be re-used. The uniqueness of these identifiers is created by using
`bldr-dev-req01 <http://bldr-dev-req01/>`_ which is designed to hand out ever
increasing identifiers. The following table indicates which kind of identifiers
are used for which element type.

============================     =======================================================
Element Type                     Identifier
----------------------------     -------------------------------------------------------
System requirements              ?
Software requirements            `CGSRSnnnn <http://bldr-dev-req01/prefix/view/CGSRS/>`_
Software System tests            `CGTSTnnnn <http://bldr-dev-req01/prefix/view/CGTST/>`_
Risk control                     `CGRSKnnnn <http://bldr-dev-req01/prefix/view/CGRSK/>`_
Identified Hazard                `CGHAZnnnn <http://bldr-dev-req01/prefix/view/CGHAZ/>`_
software design                  `CGSDSnnnn <http://bldr-dev-req01/prefix/view/CGSDS/>`_
Common component                 `ARSDnnnn <http://bldr-dev-req01/prefix/view/ARSD/>`_
Common component test            `ARSTnnnn <http://bldr-dev-req01/prefix/view/ARST/>`_
Inspection log                   `RV_$year$month$day_$index`
Implementation                   Fully qualified path, e.g. cg_ivd.example.ExampleClass
Unit tests                       Same
============================     =======================================================

To implement this all software controlled documents are built using Sphinx and
their raw marked up text is stored in version control along with the source
code and tests.

The design element identifiers are then used to define "traceability items"
which are a feature of the mlx-traceability extension to Sphinx. The sections
following document the correct use of these "traceability items".

.. todo:: What do we need to do to document mlx-traceability which is automating our quality system?, @jodystephens, end of phase 3

Adding a new element
====================

#. Get a new number for the element type from http://bldr-dev-req01.
#. Create the `..item::` node in the document.
#. The short description should be very concise (less than five words, if
   possible)
#. Add forward and backward traces as required (replace the ``:relation1:`` and
   ``:relation2:`` with the appropriate forward and backward
   `Relationships`_).

Example::

  .. item:: mmXXXnnnn Short description
    :relation1: mmXXXnnn
    :relation2: mmXXXnnn

    Here goes the actual element

Complex Example::

  .. item:: ARSD0002 Pairwise aligner
    :depends_on: SOUP0001

    .. item:: ARSD0001.1 Smith-Waterman alignment algorithm implementation
    :implemented_by: SOUP0001
    :tested_by: ARTST0001
    :confirmed_by: RV_20171002

  .. item:: SOUP0001: SSW
    :confirmed_by: RV_20171002

  .. item:: RV_20171002

    :date: Oct 02 2017
    :attendees: @aaronberlin @jodystephens @kennychesney

    We reviewed the vendors testing plans for :item:`SOUP0001` for x, y, and z and we approve of them
    for that version, links to reports _here_.


Relationships
=============

There are several possible relations that can be defined and an item can
backwards or forwards trace to any number of elements.

===============  ==================
**Forward**      **Backward**
---------------  ------------------
validates        validated_by
tests	         tested_by
implements       implemented_by
mitigates        mitigated_by
depends_on       impacts_on
fulfills         fulfilled_by
trace            traced_by
refines          refined_by
not_implemented  made_incomplete_by
===============  ==================

Which relationship to use
-------------------------

.. graphviz:: /docs/diagrams/traceability_relationships.dot
    :name: Design Element Relationships


* fulfills - Design -> Requirement
* refines - Low level design -> High level design also RSK --> HAZ
* implements - Code docstring -> Design or Requirement
* mitigates - requirement, inspection or static mitigation (SRS, INS, MIT) -> Risk (RSK)


Not Implemented Blocks
----------------------

The `not_implemented` relationship allows the developer to mark a design level
item as not yet implemented.  The item numbers the depend on the unimplemented
item should be listed so the trace is complete

Example::

  .. item:: mmSDSnnnn Do something important
    :fulfills: mmSRS123
    :refines: mmSDS123
    :not_implemented: mmSRS123 mmSDS123

    What it will do once implemented


Adding a source code or unit test trace
=======================================

There are two possible ways to do tracing for source code (including
unit test files).

#. Create a traceability item using Sphinx markup in the appropriate docstring.

   * This is most useful when the translation of a design element into an
     implmentation is fairly direct.
   * Create a traceability item, but use the fully qualified python name for
     the class/method as the identifier.
   * Example in the file ``ivd-base/python/core/utils.py``
     ::

             class SolveCancer(object):
                 def doit(self, path):
                     """
                     .. item:: core.utils.SolveCancer.doit
                         :implements: CGSRS9999

                         Here we solve cancer.

                     :param str path: the cancer pathology

                     """
                     pass

#. Place a comment tag at appropriate line

   * This is most useful for situations in which the design element
     is spread all over the place.
   * Place the trace in a comment directly adjacent to the line of
     code or block of code that is appropriate.
   * Example that has a normal code trace, as well as a focused trace.
     ::

             class SolveCancer(object):
                 def doit(self):
                     """
                     .. item:: core.utils.SolveCancer.doit

                         Here we solve cancer.

                     :param str path: the cancer pathology

                     """

                     print "Doing part one"
                     print "Doing the hard part"  # [mmxxxnnnn]

.. vim: set ft=rst:
