{
"title": "Entity types in YAML configuration",
"linkTitle": "Entity types in YAML configuration",
"weight":"110",
"date": "2020-09-25",
"description": "Learn how entity types are described in a YAML configuration."
}

The `types` directory located under the `META-INF` directory of a YAML configuration contains the definition of all the entity types in the Entity store model, and its subdirectories are organized in a hierarchical tree structure that represents the inheritance relationships between types.

An [entity type](/docs/apigtw_devguide/entity_store/#entity-types) is a description of an entity in the Entity store.

The YAML Entity store supports all entity types and custom types.

Please refer to dedicated page for the [Entity Type files schema](/docs/apim_yamles/apim_yamles_references/yamles_yaml_schema/#entity-type-files)

## Simple type

```yaml
name: JMSSession                       # name used in YAML entity file
version: 5
class: com.vordel.dwe.jms.JMSSession
constants:
  descriptorClass:
    type: string
    value: com.vordel.client.manager.filter.jms.JMSTransportDescriptor
fields:
  cloneCount:
    type: integer
    defaultValues:
    - data: 1                          # an example a defaulted field (mandatory but having a default value)
    cardinality: 1
  duplicatesOK:
    type: boolean
    defaultValues:                     # this is an optional field.
    - {}
    cardinality: '?'
  messageRemovalPolicy:
    type: string
    defaultValues:
    - data: UNLESS_EXCEPTION           # if you do specify this field in you YAML file, value will be 'UNLESS_EXCEPTION'
    cardinality: 1
  messageRemovalProperty:
    type: string
    defaultValues:
    - data: jms.message.remove
    cardinality: '?'
  name:                                # this one a mandatory field -- it is actually a key field
    type: string
    defaultValues:
    - {}
    cardinality: 1
  servicePK:                           # this fields must contain a reference to another entity of type 'JMSService'
    type: '@JMSService'                # '@' char tells it is a reference
    cardinality: 1
components:
  JMSConsumer: '?'                     # an entity of this type JSMSession can have 1 children of type JMSConsumer
keyFields:
- name
loadorder: 1000100
```

## Types with inheritance

Consider the following types: `Process`, `JavaProcess` and `NetService`:

```yaml
name: Process
version: 0
fields:
  name:
    type: string
    defaultValues:
    - {}
    cardinality: 1
keyFields:
- name
abstract: true
```

```yaml
name: JavaProcess
version: 0
abstract: true
```

```yaml
name: NetService
version: 5
constants:
  executableImage:
    type: string
    value: vshell
components:
  LoadableModule: '?'
  ClassLoader: '?'
```

and, the following directory structure:

![types example](/Images/apim_yamles/yamles_types_example.png)

We can say that:

* As `JavaProcess.yaml` file is contained within `Process` directory, then `JavaProcess` is a child type of `Process` type.
* As `NetService.yaml` file is contained within `JavaProcess` directory, then `NetService` is a child type of `JavaProcess` type.

## Cardinality

The following table shows the meaning of the cardinality symbol found in entity type definitions located in the `META-INF/types` directory:

| Symbol | Min | Max | Mandatory |
|:------:|:---:|:---:|:---------:|
|   1    |  1  |  1  |    Yes (if no default value) |
|   +    |  1  |  ∞  |    Yes (if no default value) |
|   ?    |  0  |  1  |    No     |
|   *    |  0  |  ∞  |    No     |

## YAML custom types

You can add custom types by creating a YAML file definition of an entity type, and placing the file in the correct subdirectory, under `META-INF/types`.

### Filter type

This section extends the [Create the TypeDoc](/docs/apigtw_devguide/custom_filter_extension_kit/#create-the-typedoc) section to add a new filter when using YAML.

For a custom filter to be usable, it must inherit from `Filter` or a subtype of `Filter`. To add a new filter, create a YAML file within `META-INF/types/Entity/Filter/`. Note that if you place the YAML file under `META-INF/types/Entity/Filter/AWSFilter`, it will inherit from `AWSFilter` fields.

The following is an example that takes a value from the query string or a HTTP header, and echoes it back using a size threshold and an extended MIME type.

```yaml
---
name: EchoFilter
version: 0
class: com.acme.apim.filters.EchoFilterImpl
fields:
  from:
    type: string
    defaultValues:
    - queryString
    cardinality: 1
  paramName:
    type: string
    defaultValues:
    - echo
    cardinality: 1
components:
  ThresholdRange: '?'
  ExtendedMimeType: '?'
```

After you save the file, for example, `META-INF/types/Entity/Filter/EchoFilter.yaml`, proceed as usual to add the implementation of this custom filter.

## Other custom types

In the [Filter Type](#filter-type) example, the `EchoFilter` filter has child entities of type `ThresholdRange` and `ExtendedMimeType` that can be set. You must add the type definitions for these types to the `types` directory in the same way.

A type definition for child type `ThresholdRange` can be added to `META-INF/types/Entity/RootChild/ThresholdRange.yaml` as shown below. This example entity can be used as independent entity elsewhere.

```yaml
---
name: ThresholdRange
version: 0
fields:
  minThreshold:
    type: integer
    defaultValues:
    - data: 100
    cardinality: 1
  maxThreshold:
    type: integer
    defaultValues:
    - data: 1000
    cardinality: 1
  description:
    type: string
    defaultValues:
    - data: {}
    cardinality: '?'
keyFields:
- minThreshold
- maxThreshold
```

The child entity type definition for `ExtendedMimeType` could be added to `META-INF/types/Entity/RootChild/LoadableModule/MimeType/ExtendedMimeType.yaml` which means that it extends the `MimeType` definition. The example below shows that a `description` field has been added.

```yaml
---
name: ExtendedMimeType
version: 0
fields:
  description:
    type: string
    defaultValues:
    - {}
    cardinality: '?'
```

### Example of how to use custom filters in a policy

The following is a complete example of how you can use a custom filter in a policy when all of the type definitions are setup in the `types` directory.

```yaml
---
type: FilterCircuit
fields:
  name: Custom policy
  start: ./echo
  description: Test of my new shiny filter
children:
- type: EchoFilter
  fields:
    name: echo                            # Name field is inherited from
    from: header
    paramName: X-ACME-ECHO
  logging:                                # Same form logging
    success: Successfully echoed from header X-ECHO                         
  children:
  - type: ThresholdRange
    fields:
      minThreshold: 0
      description: From 0 to 1000       # max uses the default value (1000)
  - type: ExtendedMimeType
    fields:
      mimeType: txt
      description: Text based echo
    
```
