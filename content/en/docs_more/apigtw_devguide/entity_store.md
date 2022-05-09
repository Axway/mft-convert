{
"title": "Entity Store",
"linkTitle": "Entity Store",
"weight":"70",
"date": "2019-11-27",
"description": "Learn about the Entity Store (ES) and the ES Explorer tool."
}

Configuration data for API Gateway is stored in the Entity Store (ES). The Entity Store is an XML-based store that holds all configuration data required to run API Gateway. It contains policies, filters, certificates, resources, and so on. The Entity Store for a group of API Gateways can be found at the following location:

```
INSTALL_DIR/apigateway/groups/GROUP_ID/conf/CONFIG_UID
```

API Gateway runs with a number of separate stores that are combined as a federated entity store for the API Gateway’s configuration.

The federated entity store is made up of component stores. Each component store is responsible for one or more *branch points* in the configuration tree. Each component store must be consistent in its own right:

* It must have all the entity types required to describe its component entities.
* It must also have valid hard references within. Hard references are fields that refer to other entities via their real primary keys (PKs). Soft references allow an entity in one store to reference an entity in another store. This is done via Portable ESPKs, which the federated entity store has the added ability, above other store flavors, to resolve to the correct entity when calling `getEntity(ESPK pk)`.

The following table lists the XML-based stores:

| File name               | Description                                                                                                                  |
|-------------------------|------------------------------------------------------------------------------------------------------------------------------|
| ```CertStore.xml```     | Contains certificates and private keys. Private keys are encrypted with the API Gateway passphrase.|
| ```configs.xml```        | The federated store which imports all other component stores.                                                                |
| ```EnvSettingsStore.xml```                      | Contains environment settings for the configuration.                                                                         |
| ```ExtConnsStore.xml```                      | Contains the external connection information (for example, database, LDAP, and so on) that the runtime might connect to.     |
| ```ListenersStore.xml```                      | Contains the configuration for the HTTP ports and protocols that API Gateway listens on to receive messages to be processed. |
| ```PrimaryStore.xml```                      | Contains the policies and filters to be applied to messages received by API Gateway.                                         |
| ```UserStore.xml```                      | User store containing user names and passwords and associated user roles.                                                    |
| ```ResourcesRepository.xml```                      | Contains the resource information.                                                                                           |

## Entity types

An *entity type* is a description of an entity in the Entity Store. An entity type defines the following properties for a given entity:

* Constant fields, specifically name and value.
* Fields, specifically name, data type, and cardinality.

The entity type is known by its name, and is located in its inheritance tree by its parent type. All entity types are defined within the top-level `<entityStoreData>` element.

The following are additional properties of entity types:

* Each entity is an instance of an `EntityType`.
* Each entity is of exactly one entity type, and has a link to its defining type.
* Entity types can inherit properties from zero or one other entity types.
* The root `EntityType` is called `Entity`, so a basic entity is of type `Entity`.

A field in an entity can be of the following type:

* boolean
* string
* integer
* long
* utctime
* binary
* encrypted
* reference to another entity (either in the same component entity store or across a component store in the federated store)

A field must be assigned a cardinality. The possible cardinalities are:

* zero or one (?)
* zero or many (*)
* one or many (+)
* one (1)

The following shows an example.

```xml
<field name="name" type="string" cardinality="1" isKey="true" />
<field name="isEnabled" type="boolean" cardinality="1" default="true"/>
<field name="hosts" type="string" cardinality="*"/>
<field name="timeout" type="integer" cardinality="?" default="30000" />
<field name="certificates" type="^Certificate" cardinality="*" />
<field name="nextEntity" type="@Entity"/>
```

### References to other entities

A field that refers to another entity within the same component entity store is called a *hard reference*. For example:

```xml
<field cardinality="1" name="category" type="@Category">
```

A field that refers to another entity in another component entity store is called a *soft reference*. For example:

```xml
<field cardinality="1" name="repository" type="^AuthnRepositoryBase"/>
```

### Entity type definitions

Each configurable item has an entity type definition. The entity type definition is defined in an XML file known as the *TypeDoc*.

Entity types are analogous to class definitions in an object-oriented programming language. In the same way that instances of a class can be created in the form of objects, an instance of an entity type can also be created. Therefore it is useful to think of the entity type defined in a TypeDoc as a header file, and the entity itself as a class instance. All entities and their entity type definitions are stored in the Entity Store.

Every filter requires specific configuration data to perform its processing on the message. For example, a filter that extracts the values of two elements from a SOAP message, and adds them together, must be primed with the names and namespaces of those two elements.

Because a filter is a configurable item, it requires a new XML TypeDoc to be written containing an entity type definition for it. The entity type for a filter contains a set of configuration parameters and their associated data types and default values.

When an instance of the filter is added to a policy using Policy Studio, a corresponding entity instance is created and stored in the Entity Store. Whenever the filter instance is invoked, its configuration data is read from the entity instance in the Entity Store.

## Use the ES Explorer

ES Explorer is a registry editor type tool which allows you to connect directly to an Entity Store (ES). Within the ES explorer you can perform various create, read, update, delete (CRUD) operations on the entities, and view the available types in the Entity Store. To open a federated entity store in ES Explorer, load the `configs.xml` file. Alternatively, to open a component entity store, load the XML file (for example, `PrimaryStore.xml`).

To start the ES Explorer, run this command:

```
INSTALL_DIR/apigateway/posix/bin/esexplorer
```

When ES Explorer is started you can load an entity store by selecting **Store > Connect** from the menu. In the **Connect to EntityStore** dialog browse to either the federated or component entity store to be opened.

### Load a type definition

You can import a type definition or entity instances using ES Explorer by using a catalog called a `typeset`. The typeset is essentially a list of references to other files, each of which can contain type definitions or previously exported entity definitions. While you can import entity instances into the federated store, you must be careful when adding types to ensure that they get added to the appropriate component store, as each component store is responsible for only a subset of types visible from the federated view.

For example, if your TypeDoc is describing an entity type that defines a custom filter then you should add this to the Primary Store (`PrimaryStore.xml`), which is responsible for storing policies and filters. If your TypeDoc describes a listener for messages then you would add it to the Listener Store (`ListenersStore.xml`).

If you have followed the preceding steps to connect to a component store, you can add a typeset by right-clicking on the component store icon and selecting **Load a Type Set**. In the dialog you can specify the location of the `typeset.xml` file to add. This adds all types referenced by the typeset into the component store.

For an example of how to build up a typeset and have it indirect to a type definition for import, see the sample script `publish.py`, which is available in the `INSTALL_DIR/apigateway/samples/scripts/publish` directory. By default the `typeset.xml` file includes a `SimpleFilter.xml` file which contains a **SimpleFilter** entity type definition.

### Locate entities using shorthand keys

Entities in the Entity Store can be located using shorthand keys. In the ES Explorer, you can right-click on a node and select **Print PortableESPK (shorthand)** to print out the shorthand key for it. For example, the following is the shorthand key for the **Set Message** filter in the Health Check policy:

```
/[CircuitContainer]name=Policy Library/[FilterCircuit]name=Health Check/[ChangeMessageFilter]name=Set Message
```

You can then use the **Find an Entity** action to use the key to find that entity, or you can fashion a more general search string to find entities at a given level.

The following table shows some examples of shorthand keys:

| Shorthand key                                                 | Description                                                                                     |
|---------------------------------------------------------------|-------------------------------------------------------------------------------------------------|
| ```/[CircuitContainer]**/[FilterCircuit]```                                                            | Get all filter circuits at all levels.                                                          |
| ```/[CircuitContainer]**/[FilterCircuit]/[Reflector]```                                                            | Get all filters of type Reflector.                                                              |
| ```/[CircuitContainer]**/[FilterCircuit]/[Reflector]name=Reflect```| Get all filters of type Reflector, but restricted to Reflector filters with the name `Reflect`. |
| ```/[CircuitContainer]**/[FilterCircuit]/[FilterCircuit]```                                                            | Get all policies in a configuration (you need two shorthand keys).                              |

{{< alert title="Note" color="primary" >}}
Forward slashes must be escaped, for example:

```
/[NetService]name=Service/[HTTP]name=Management Services/[ServletApplication]uriprefix=\/manager\//[Servlet]uri=webManager
```

{{< /alert >}}
