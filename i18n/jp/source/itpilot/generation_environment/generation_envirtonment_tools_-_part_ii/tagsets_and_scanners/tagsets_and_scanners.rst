====================
Tagsets and Scanners
====================


.. toctree::
   :hidden:

   understanding_tagsets.rst
   graphical_creation_of_new_tagsets.rst
   tagsets_and_scanners_included_in_the_distribution.rst
   lexer_types.rst

Tagsets and scanners are key elements in the operation of the Extractor
component (see section :ref:`Configuration of the Extractor Component`).
Usually, the user does not need to create his/her own tagsets and
scanners because: 1) the Extractor component includes an option
(activated by default) to auto-generate the tagsets and scanners
required by an Extractor from the examples provided by the user (see
section :ref:`Configuration of the Extractor Component`), and 2) ITPilot
includes a set of pre-generated tagsets and scanners that are enough in
most situations (see section :ref:`Tagsets and scanners included in the
distribution`).



However, there are times in which advanced users may want to create
their own tagsets, if the Extractor specification has been built
manually instead of providing examples, or if the generated and/or
included tagsets are not enough in some specific situation.



This section will start providing basic information in order to
understand the tagset fundamentals (section :ref:`Understanding Tagsets`).
It will then be explained how to create them by using the ITPilot
generation tool (section :ref:`Graphical creation of new tagsets`). The section
:ref:`Tagsets and scanners included in the distribution` lists and describes
the scanners and tagsets included in ITPilot. At last, section :ref:`Lexer
Types <itp_gen_environment_guide_lexer_types>` defines the concept of “lexer type”.



