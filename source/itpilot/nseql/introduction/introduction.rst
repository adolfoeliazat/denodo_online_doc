============
Introduction
============

Several useful services are now offered through the technologies that
make up the Worldwide Web (WWW). The great majority of these Web sites
provide access to said services exclusively through HTML interfaces
designed to be used by humans through an Internet browser.



At times, it is desirable to be able to automate some sequences of
operations in Web sites, making them executable by a program instead of
a human user. For example, this can be useful in speeding up queries to
multiple data sources or for automating transaction processes that
involve completing various HTML forms. Although the http protocol offers
some basic support for these operations, it is insufficient in many
sources due to problems such as scripting languages executed in the
Internet browser (JavaScript, JScript, etc.), session tracking in the
server side, certain types of redirections, etc.



NSEQL (Navigation SEQuence Language) is a language designed for
programming action sequences on the Internet browser interface: the
current version supports Microsoft Internet Explorer (MSIE), and Denodo
Browser. NSEQL can be used in a computer program to reproduce any
operation sequence a human user may have conducted through the browser.



It is important to highlight that it is not normally necessary to
create NSEQL programs manually. The ITPilot graphical generation
environment (:doc:`/itpilot/generation_environment/index`) allows NSEQL programs to be created
graphically by simply providing an example of the required sequence
through an Internet browser. This manual provides an exhaustive
description of the language for advanced users that want to manually
create or edit NSEQL programs.
