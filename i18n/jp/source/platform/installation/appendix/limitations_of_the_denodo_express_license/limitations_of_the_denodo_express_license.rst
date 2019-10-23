=========================================
Limitations of the Denodo Express License
=========================================

The Denodo Express license has some limitation compared to a full
license. The main one is that it does not allow using ITPilot.

It also imposes other limitations over the Virtual DataPort and
Scheduler modules:

.. table:: Limitations of the Denodo Express license (Virtual DataPort and Scheduler)
   :name: Limitations of the Denodo Express license (Virtual DataPort and Scheduler)

   +-------------------------------------+-------------------------------------+
   | Limitations of Virtual DataPort with Denodo Express                       |
   +=====================================+=====================================+
   | Number of user accounts             | 1                                   |
   |                                     |                                     |
   |                                     | Virtual DataPort includes a default | 
   |                                     | administrator account ("admin"). As |
   |                                     | the license only allows you to have |
   |                                     | one user account, you will always   |
   |                                     | have to use "admin".                |
   +-------------------------------------+-------------------------------------+
   | Maximum number of simultaneous      | 3                                   |
   | requests                            |                                     |
   +-------------------------------------+-------------------------------------+
   | Maximum number of rows returned by  | 10,000                              |
   | a query                             |                                     |
   +-------------------------------------+-------------------------------------+
   | ODBC adapters allowed               | Microsoft Access and Excel          |
   +-------------------------------------+-------------------------------------+
   | Sources not allowed                 | Multidimensional databases: SAP BI, |
   |                                     | SAP BW, Mondrian, Microsoft         |
   |                                     | Analytical Services and Oracle      |
   |                                     | Essbase.                            |
   |                                     |                                     |
   |                                     | Google Search                       |
   |                                     |                                     |
   |                                     | Custom wrappers                     |
   |                                     |                                     |
   |                                     | The Denodo Browser client cannot be |
   |                                     | used to retrieve data from CSV,     |
   |                                     | JSON or XML files.                  |   
   +-------------------------------------+-------------------------------------+
   | View parameters                     | You cannot set view parameters in a |
   |                                     | derived view.                       |
   +-------------------------------------+-------------------------------------+
   | Importing extensions (jars) is not  | This implies not being able to      |
   | allowed                             | develop custom connectors to        |
   |                                     | sources, custom policies or custom  |
   |                                     | functions.                          |
   +-------------------------------------+-------------------------------------+
   | Version Control System              | Disabled. With Denodo Express you   |
   |                                     | cannot store your work in a Version |
   |                                     | Control System such as Subversion,  |
   |                                     | GIT, etc.                           |
   +-------------------------------------+-------------------------------------+
   | JMS listeners cannot be created     |                                     |
   +-------------------------------------+-------------------------------------+
   | **Limitations of Scheduler with Denodo Express**                          |
   +-------------------------------------+-------------------------------------+
   | Maximum number of jobs              | 1                                   |
   +-------------------------------------+-------------------------------------+

