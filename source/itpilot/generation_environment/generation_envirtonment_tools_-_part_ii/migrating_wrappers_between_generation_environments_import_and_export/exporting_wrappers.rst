==================
Exporting Wrappers
==================

To export the data select the “File” menu and then the option “Export …
“. The export dialog will appear, showing the following options:



-  Export a single wrapper: select the Single Process radio button, and
   then use the two combo boxes to select both the wrapper and the
   project that contains it.
-  Export a whole project (with all the contained wrappers): select the
   Single Project radio button, and then use the combo box to select the
   project that is going to be exported.
-  Export all the projects (with all the wrappers of the environment):
   select the All radio button.



It is possible to select what additional data to export along with the
selected wrappers:



-  Include scanners and tagsets: by checking this option, all the
   tagsets and scanners used by any of the exported wrappers will be
   included.
-  Include extensions: by checking this option, all the extensions that
   contain functions used by any of the exported wrappers will be
   included.
-  Include custom components: by checking this option, the custom
   components used by any of the exported wrappers will be included.
-  Include configuration: by checking this option, the configuration of
   the Wrapper Generation Tool will be exported, so it is updated when
   importing the data. Note that the configured paths (for example, for
   OpenOffice or Adobe Acrobat Professional) will only be overwritten
   after approval.



.. note:: When exporting all the data of the ITPilot Wrapper Generation
   Tool, the Include scanners, Include extensions and Include custom
   components options will export all the items present in the environment,
   instead of just the items used by all the exported wrappers (i.e.,
   unused elements will be also exported).

After configuring the exportation options, click Ok. A file chooser will
appear where you can select a path for the exported data. After clicking
Save the ITPilot Wrapper Generation Tool will generate a WGT file which
includes all the exported data.
