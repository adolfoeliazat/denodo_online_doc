================
Serving Metadata
================

The OData service generates two types of documents with metadata about the Denodo databases:

1. The "Service Document": lists all the entities offered by the data service. It is
   available at the root URI of the service, specifying the database name where  
   we are going to get information: ``/denodo-odata.svc/<database name>``.

   Below, there is an example where the accessible collections of the ``movies`` 
   database are ``actor``, ``address``, ``city``, ``country``, ``film`` and 
   ``film_actor``.

   ``http://localhost:9090/denodo-odata4-service/denodo-odata.svc/movies``

   .. code-block:: json
   
       {
         "@odata.context": "/denodo-odata.svc/movies/$metadata",
         "value": [
           {
             "name": "actor",
             "url": "actor"
           },
           {
             "name": "address",
             "url": "address"
           },
           {
             "name": "city",
             "url": "city"
           },
           {
             "name": "country",
             "url": "country"
           },
           {
             "name": "film",
             "url": "film"
           },
           {
             "name": "film_actor",
             "url": "film_actor"
           }
         ]
       }

#. The "Service Metadata Document" (also called Entity Data Model (EDM): XML 
   representation of the data model exposed by the service. It is available at 
   ``.../$metadata``.
   
   For example:
   
   ``http://localhost:9090/denodo-odata4-service/denodo-odata.svc/movies/$metadata``

   Example of an entity country that has a relationship with zero or more city
   elements:

   .. code-block:: xml
      :emphasize-lines: 11-16

         <EntityType Name="country">
            <Key>
                <PropertyRef Name="country_id"/>
            </Key>
            <Property Name="country_id" Type="Edm.Int16"  
                      Nullable="false"/>
            <Property Name="country" Type="Edm.String" 
                      MaxLength="50"/>
            <Property Name="last_update" Type="Edm.DateTimeOffset" 
                      Precision="19"/>
            <NavigationProperty Name="cities"                                   
                         Type="Collection(com.denodo.odata4.city)"         
                         Partner="country">
                <ReferentialConstraint Property="country_id"                                                              
                         ReferencedProperty="country_id"/>
            </NavigationProperty>
         </EntityType>

The service maps these OData structures to Virtual DataPort concepts like this:

+----------------------------------------+--------------------------------+
| Denodo OData Service                   | Virtual DataPort               |
+========================================+================================+
| Entity Type                            | View Definition                |
+----------------------------------------+--------------------------------+
| Entity Type > Property                 | View Column                    |
+----------------------------------------+--------------------------------+
| Entity Type > Navigation Property      | Association Role               |
+----------------------------------------+--------------------------------+
| Relationship                           | Association Definition         |
+----------------------------------------+--------------------------------+
| Entity Set                             | View Data                      |
+----------------------------------------+--------------------------------+
| Imported Functions                     | \-                             |
+----------------------------------------+--------------------------------+
