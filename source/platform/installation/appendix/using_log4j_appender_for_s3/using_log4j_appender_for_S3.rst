===================================================================================================
How to Store the Log files of Denodo in Amazon AWS S3
===================================================================================================

By default, the Denodo components store the log files on the local filesystem. However, you can configure them to store the log files on Amazon AWS S3 as well. This is useful for Denodo deployments that run on AWS and that you plan on switching off when you no longer need them. With this feature, you make sure that the logging information is saved even if the instance is deleted.

Before enabling this feature, you need to create or have access to an S3 bucket where the logs will be stored.

To enable this feature for Virtual DataPort, follow these steps:

#. Stop the Virtual DataPort server.

#. Edit :file:`{<DENODO_HOME}/conf/vdp/log4j2.xml` 

#. In this file, search for ``<Root level="error">`` and inside this element, add ``<AppenderRef ref="S3Appender" />``. You have to end up with something like this:

   .. code-block:: xml

      <Root level="error">
          <AppenderRef ref="FILEOUT" />
          <AppenderRef ref="S3Appender" />
      </Root>
      
   With this change, the log files to be stored in the local file system *and* in S3. If you only want to store them on S3, remove the line ``<AppenderRef ref="FILEOUT" />``.

#. In this file, add the following **inside** the element ``<Appenders>``:

   .. code-block:: xml

      <S3Appender name="S3Appender">
          <PatternLayout pattern="%-4r [%t] %-5p %d{yyyy-MM-dd'T'HH:mm:ss.SSS} %c %x - %m  %n"/>
          <stagingBufferSize>100</stagingBufferSize>
          <stagingBufferAge>1</stagingBufferAge>
          <s3Bucket>bucket_denodo_logs</s3Bucket>
          <s3Path>%instanceId/vdp_logs/</s3Path>
          <s3Region>eu-central-1</s3Region>
      </S3Appender>
      
   You have to end up with something like this:
   
   .. code-block:: xml
      :linenos:
      :emphasize-lines: 1
   
      <Appenders>

          <S3Appender name="S3Appender">
              <PatternLayout pattern="%-4r [%t] %-5p %d{yyyy-MM-dd'T'HH:mm:ss.SSS} %c %x - %m  %n"/>
              <stagingBufferSize>100</stagingBufferSize>
              <stagingBufferAge>1</stagingBufferAge>
              <s3Bucket>bucket_denodo_logs</s3Bucket>
              <s3Path>%instanceId/vdpLogs/</s3Path>
              <s3Region>eu-central-1</s3Region>
          </S3Appender>

5. In the XML fragment added in the previous step, modify these values:

   -  ``s3Bucket``: name of the S3 bucket where the log files will be stored.
   -  ``s3Path``: path within the bucket where the log files will be stored. **Do not** remove ``%instanceId`` from the value of this parameter. At runtime, this will be replaced with the id of the AWS instance where this Denodo server is running.
   -  ``s3Region``: the region of the S3 bucket.
  
   You can also modify these values related to how often the log information is transferred to the S3 bucket:
   -  ``stagingBufferSize``: number of log messages after which the content is written to the S3 bucket.
   -  ``stagingBufferAge``: number of minutes after which the content is written to the S3 bucket. If this property is defined, ``stagingBufferSize`` is ignored. 

|

If you want to enable this feature for other components of Denodo, modify their logging configuration:

-  Scheduler: edit :file:`{<DENODO_HOME>}/conf/scheduler/log4j2.xml`.
-  Scheduler Index: edit :file:`{<DENODO_HOME>}/conf/arn-index/log4j2.xml`.
-  Solution Manager: edit :file:`{<SOLUTION_MANAGER_HOME>}/conf/solution-manager/log4j2.xml`.
-  License Manager: edit :file:`{<SOLUTION_MANAGER_HOME>}/conf/license-manager/log4j2.xml`
-  Virtual DataPort of the Solution Manager: edit :file:`{<SOLUTION_MANAGER_HOME>}/conf/vdp/log4j2.xml`

It is not possible to enable this for the Denodo web container.