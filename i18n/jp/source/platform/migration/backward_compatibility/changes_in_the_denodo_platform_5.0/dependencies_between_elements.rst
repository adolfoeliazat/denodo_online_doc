=============================
Dependencies Between Elements
=============================

Starting with Denodo 5.0, Denodo establishes a **dependency** between
the custom maps you create and the views that use them. It also
establishes a dependency between the jar files imported into the
Server and the “Denodo stored procedures” and “Custom Wrappers” that
use the Java classes provided by these jars.

If a user executes the statements ``DROP MAP 18N`` to delete a custom
i18n map, ``DROP MAP SIMPLE`` to delete a simple map or ``DROP JAR``
to delete a jar and they have a dependency, the statement will fail.
In this case, you can execute a ``DROP MAP I18N <name> CASCADE``,
``DROP MAP SIMPLE <name> CASCADE`` or ``DROP JAR <name> CASCADE`` to
delete the custom i18n map, the custom simple map or the jar file
respectively. These statements delete the custom i18n map, the custom
simple map or the jar file **and** the elements that depend on them.

The previous versions of Virtual DataPort do not establish
dependencies between custom maps or jar files and the elements that
use them. Therefore, *in the previous versions*, the statements
``DROP MAP I18N`` or ``DROP JAR`` never fail and they do not delete
the elements that use the custom i18n map or the jar.
