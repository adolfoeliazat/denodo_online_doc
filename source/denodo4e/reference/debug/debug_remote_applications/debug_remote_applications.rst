====================================================
Debug Remote Applications
====================================================

Denodo4e was designed to debug extensions running in local Denodo servers. 
Nevertheless, it is possible to debug extensions deployed in remote Denodo servers by following the steps detailed here.

On the Remote Machine
=========================
1.  Enable the debug mode of the Denodo server. You can do this in two ways (in the following examples the port ``8100`` is used as the debug port, 
    but any other free port of the remote machine can be used):

    -  Add the following line, which assigns a value to the variable ``DENODO4E_DEBUG_CONF``, at the beginning of the server startup script:

    .. code-block:: none

       SET DENODO4E_DEBUG_CONF=-Xdebug -Xrunjdwp:transport=dt_socket,server=y,suspend=n,address=8100 (windows)
       DENODO4E_DEBUG_CONF=-Xdebug -Xrunjdwp:transport=dt_socket,server=y,suspend=n,address=8100 (unix/linux)
       
|

     These are the startup scripts of each Denodo server:


      ==================   =======================================   ================================
      **Server**           **Script to change (windows)**            **Script to change (unix/linux)**
      ==================   =======================================   ================================
      VDP/ITP Server       %DENODO_HOME%\\bin\\vqlserver.bat           $DENODO_HOME/bin/vqlserver.sh
      Scheduler Server     %DENODO_HOME%\\bin\\schedulerserver.bat     $DENODO_HOME/bin/schedulerserver.sh
      Aracne Server        %DENODO_HOME%\\bin\\arnserver.bat           $DENODO_HOME/bin/arnserver.sh
      ==================   =======================================   ================================
    

    .. note::
       
       When using this way, only the server whose script is being modified will start in debug mode.


    .. note::
      
       If you want to execute two or more servers in debug mode at the same time, you have to specify different debug ports to each one.

    -  Set the following environment variable:

    .. code-block:: none

       DENODO4E_DEBUG_CONF=-Xdebug -Xrunjdwp:transport=dt_socket,server=y,suspend=n,address=8100
          
    .. note::
       
       This environment variable is shared between all Denodo Platform servers and tools. 
       Therefore, after setting the variable and starting the desired server, if you want to start another server or tool: 
           
           -  If you do not want to start it in debug mode, remove the environment variable.
           -  If you want to start it in debug mode at the same time, set a new value for the environment variable using a different debug port.


2.  Start the server using the Denodo Platform Control Center or its modified startup script. 

On the Local Machine
======================

1.  Deploy the extension you want to debug on the remote server using :ref:`the Deploy Wizard`. 

#.  Click **Run** > **Debug Configurations...** and double-click on **Remote Java Application**. In the Connect tab select **"Standard (Socket Attach)"** 
    as **Connection Type** and enter the host where the remote server is running and the debug port that was configured in the variable ``DENODO4E_DEBUG_CONF``
    before starting the server. In the **Source** tab add the projects that contain the sources of the extensions to debug.

#.  You can set breakpoints on the source code and the server will stop the execution when it reaches these points. 
    You will be able to control the extension execution step by step and perform changes at run time from your local eclipse. 

Final Considerations
=====================

-  It is important to disable the debug mode of the server when debugging capabilities are no longer needed. To do this, depending on the way used to start the server in debug mode, remove or comment the definition of the variable ``DENODO4E_DEBUG_CONF`` from the server startup script, or remove the environment variable ``DENODO4E_DEBUG_CONF``, and then restart the server. 
-  Changes performed in the source code of the extension at run time, while debugging it, are not persistent. They are invalidated when the server is stopped. To make the changes persistent you have to deploy the extension again.
-  To debug extensions on two or more servers simultaneously, you have to specify different debug ports to each server, create a debug configuration in eclipse for each one, and launch these configurations simultaneously. 
