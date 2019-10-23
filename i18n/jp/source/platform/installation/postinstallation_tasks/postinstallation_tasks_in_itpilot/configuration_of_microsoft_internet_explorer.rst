============================================
Configuration of Microsoft Internet Explorer
============================================

To run automated browser sequences, ITPilot uses a Browser Pool that
will spawn browsers based on Microsoft Internet Explorer or Denodo
Browser.

To be able to use Internet Explorer, you have to configure it
appropriately.

It is *very important* that you perform these steps using the same user
account that you will use to launch the Denodo Control Center *and*
under which the Denodo Windows services will run.

#. Open Internet Explorer and configure it with the settings required by
   your environment: proxy options, security level, cookies, etc. The
   reason is that the browsers opened by the pool will use this
   configuration.

#. Make sure that “Active Scripting” is enabled (this option is disabled by
   default in some Windows Server versions). To do this, follow these
   steps:

   a. On the menu **Tools** of Internet Explorer, click **Internet
      options**.
   b. Click the tab **Security**.
   c. Click **Custom level**.
   d. Look for the category **Active scripting**, inside the category
      **Scripting**, and click **Enable**.

#. If using Internet Explorer 10 and 11, follow these steps:

   -  On 32-bit Windows, follow these steps:

      i.   Execute ``regedit.exe``.
      ii.  Browse to HKEY\_LOCAL\_MACHINE > SOFTWARE > Microsoft > Internet
           Explorer > Main.
      iii. Add a DWORD value called ``TabProcGrowth`` with value ``0``.

   -  On 64-bit Windows, follow these steps:

      i.   Execute the 64-bit version of ``regedit.exe`` (this is the version
           included in the PATH by default).
      ii.  Browse to HKEY\_LOCAL\_MACHINE > SOFTWARE > Microsoft > Internet
           Explorer > Main.
      iii. Add a DWORD value called ``TabProcGrowth`` with value ``0``.
      iv.  Browse to HKEY\_LOCAL\_MACHINE > SOFTWARE -> Wow6432Node >
           Microsoft > Internet Explorer > Main.
      v.   Add a DWORD value called ``TabProcGrowth`` with value 0.
   
   You have to do this because Internet Explorer, in the versions 10 and
   11, spawns each tab as a separate process of the operating system. To
   record navigation sequences (NSEQL sequences), ITPilot requires Internet
   Explorer to use the same process for all tabs.
   
   .. note:: If the property ``TabProcGrowth`` already exists, modify its type and value to match type ``DWORD`` with value ``0``.
