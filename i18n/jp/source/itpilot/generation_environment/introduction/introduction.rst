============
Introduction
============


Presentation
============

This document focuses on the graphic tools from Denodo Technologies
which allow to visually extract information from web sources. They can
also be used to extract information from documents in Microsoft Word,
Microsoft Excel and/or PDF format.

Development of Component-Based Wrappers
=======================================

Most information obtained from WWW (Worldwide Web) sources is presented
using the HTML language, which is aimed to the visualization of data by
human beings but it is not adequate to allow processing by software
applications.



Denodo ITPilot allows graphically creating web “wrappers”; wrappers are
programs able to automate a certain process on a website. This usually
involves automatically reproducing navigation sequences and extracting
data from the website pages. In ITPilot, wrappers are defined as a
process flow comprising a series of components. Each component
accomplishes a specific task, and their behavior depends on the input
data they receive. Denodo components allow practically any operation on
HTML-based web sources, and also some capabilities for information
extraction from Microsoft Word, Microsoft Excel and Adobe PDF files.



Graphically, users can select the components required for a specific Web
automation process from a palette. Each component can be linked to
others through information input and output relations. Thus, the result
of a component may be used as input for others. For example, the data
extraction component (the “extractor”) will return a list of results
extracted from a web page that may be used as input for an iteration
component (the “iterator”). This component, in turn, will return in each
iteration one of the elements comprising the list, so further operations
can be performed on each element (such as processing it or returning it
as a result of the wrapper). There are also other components for tasks
such as applying conditions, executing loops or filtering and
transforming the extracted data. A full description of each component in
the form of a reference guide can be found in section :ref:`Appendix C:
Catalog of Components`.



Denodo ITPilot generates a program in JavaScript :doc:`/itpilot/developer/index` based on this
graphical description of components. This program contains the
declaration of each component and their relations. The ITPilot
components related to navigation sequences and information extraction
tasks also use specific ITPilot browsing and extraction languages known
as NSEQL and DEXTL respectively, although
users do not need to use them directly, since ITPilot includes graphical
tools to automatically generate from examples both navigation sequences
and extraction programs.



The ITPilot Wrapper Generation Tool also allows testing and debugging
the generated wrappers (both at the wrapper and component levels) before
deploying it in the run server, as described in this manual.



This manual uses an example to illustrate some of the main features of
the tool. The example is split into two different parts. The first part
provides a small, detailed example of extracting e-mail information from
a Web application. The second part expands on this example to observe
and practice with functions such as the extraction of data dispersed in
different detail pages, browsing through pages of “more results” and
other advanced capacities.



The following section will describe the Generation Environment
installation and configuration process.
