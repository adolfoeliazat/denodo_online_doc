===============================
Generating Navigation Sequences
===============================


.. toctree::
   :hidden:

   description_of_the_navigation_sequences_generator_interface/description_of_the_navigation_sequences_generator_interface.rst
   steps_for_generating_a_navigation_sequence/steps_for_generating_a_navigation_sequence.rst
   the_selectframe_action/the_selectframe_action.rst
   the_transpose_table_button_and_action/the_transpose_table_button_and_action.rst
   the_cancelnavigation_button/the_cancelnavigation_button.rst
   recording_sequences_with_passwords/recording_sequences_with_passwords.rst
   properties_of_the_navigation_bar/properties_of_the_navigation_bar.rst
   xpath_generator_utility/xpath_generator_utility.rst

Denodo ITPilot allows creating programs (also called “wrappers”) that
perform automation and data extraction tasks on websites. These tasks
normally imply automating complex navigation sequences through Web sites
that may involve authentication processes, form filling, frame
selecting, etc.



Denodo ITPilot includes a command language called NSEQL (Navigation
SEQuence Language) for defining complex navigation sequences that are
run using a pool of automated browsers. As explained in the sections
:ref:`Component Configuration` and :ref:`Comparison between MSIE and Denodo
Browser`, there are two types of browsers:

#. Instances of Microsoft Internet Explorer (MSIE).
#. Instances of Denodo Browser. Denodo Browser is a GUI-less browser
   that embeds a HTTP client and a JavaScript engine that allow it to
   perform complex web navigations.



When to use one browser type or another is discussed in section :ref:`Comparison between MSIE and Denodo
Browser`.



NSEQL navigation sequences do not have to be manually written: Denodo
ITPilot allows to record new navigation sequences of any complexity by
entirely graphical means.



Navigation sequences are recorded using the “Navigation Sequences
Generator”, that takes the form of a toolbar that is installed in an
MSIE browser. Once installed, it can be used to generate any navigation
sequence on the user’s browser using a process very similar to normal
browsing, the recorded sequences are automatically translated into an
NSEQL program that replicates the navigation sequence.



.. note:: It is important to note that the necessary events for running
   a navigation sequence in a browser type do not always match those
   necessary in another type. This means that the NSEQL programs produced
   by the Navigation Sequence Generator may have to be adapted before being
   run with a browser pool configured to use Denodo browsers.
   
   

This generator may also generate browse sequences using *pattern http*
requests; their characteristics and differences with NSEQL are explained
in section :ref:`Advanced options`.
