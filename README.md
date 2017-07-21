# Stroom Content

_Stroom Content_ is a repository of the content definition files used to import entities into _Stroom_. Entities are such things as XML Schemas, XSLT translations, data splitter definitions, pipelines, dashboards, visualisations, etc. This repository provides a means to share common _Stroom_ content in a form that can be imported into multiple instances of _Stroom_.

The content is grouped into a number of _content packs_ which can each be built into a zip file that is ready for import directly into _Stroom_.

## Content pack contents

The following content packs are currently available.  Click on the link for details of what is in each content pack.

### [**core-xml-schemas**](./source/core-xml-schemas/README.md) 

The core XML Schemas required by Stroom for basic operation.

#### Compatibility matrix

| Pack version                                                                                        | Stroom 5.0.x   | Stroom 6.0.x  |
| --------------------------------------------------------------------------------------------------- | -------------- | ------------- |
| [v1.0](https://github.com/gchq/stroom-content/releases/tag/core-xml-schemas-v1.0)                   | Y              | Y             |


### [**event-logging-xml-schema**](./source/event-logging-xml-schema/README.md) 

An XML Schema standard for describing audit events.

#### Compatibility matrix

| Pack version                                                                                        | Stroom 5.0.x   | Stroom 6.0.x  |
| --------------------------------------------------------------------------------------------------- | -------------- | ------------- |
| [v3.1.1](https://github.com/gchq/stroom-content/releases/tag/event-logging-xml-schema-v3.1.1)       | Y              | Y             |
| [v1.0](https://github.com/gchq/stroom-content/releases/tag/event-logging-xml-schema-v1.0)           | Y              | Y             |


### [**internal-dashboards**](./source/internal-dashboards/README.md) 

A set of _Dashboard_ entities for displaying various metrics about the state of the _Stroom_ application and the undelying hardware and file systems.

#### Compatibility matrix

| Pack version                                                                                        | Stroom 5.0.x   | Stroom 6.0.x  |
| --------------------------------------------------------------------------------------------------- | -------------- | ------------- |
| [v1.1](https://github.com/gchq/stroom-content/releases/tag/internal-dashboards-v1.1)                | Y              | Y             |
| [v1.0](https://github.com/gchq/stroom-content/releases/tag/internal-dashboards-v1.0)                | Y              | Y             |


### [**internal-statistics-sql**](./source/internal-statistics/README.md) 

A set of _StatisticStore_ entities for representing the internal statistics generated by Stroom and record by the SQL Statistics service. This pack was previously known as 'internal-statistics' so the versions in the matrix below refer to either name.

#### Compatibility matrix

| Pack version                                                                                        | Stroom 5.0.x   | Stroom 6.0.x  |
| --------------------------------------------------------------------------------------------------- | -------------- | ------------- |
| [v2.0](https://github.com/gchq/stroom-content/releases/tag/internal-statistics-sql-v2.0)            | N              | Y             |
| [v1.2](https://github.com/gchq/stroom-content/releases/tag/internal-statistics-v1.2)                | Y              | N             |
| [v1.1](https://github.com/gchq/stroom-content/releases/tag/internal-statistics-v1.1)                | Y              | N             |
| [v1.0](https://github.com/gchq/stroom-content/releases/tag/internal-statistics-v1.0)                | Y              | N             |


### [**internal-statistics-stroom-stats**](./source/internal-statistics/README.md) 

A set of _StroomStatsStore_ entities for representing the internal statistics generated by Stroom and recorded by the Stroom-Stats service.

#### Compatibility matrix

| Pack version                                                                                        | Stroom 5.0.x   | Stroom 6.0.x  |
| --------------------------------------------------------------------------------------------------- | -------------- | ------------- |
| [v2.0](https://github.com/gchq/stroom-content/releases/tag/internal-statistics-stroom-stats-v2.0)   | N              | Y             |


### [**stroom-101**](./source/stroom-101/README.md) 

The entities used in the [quick start guide](https://gchq.github.io/stroom-docs/quick-start-guide/quick-start.html) 

#### Compatibility matrix

| Pack version                                                                                        | Stroom 5.0.x   | Stroom 6.0.x  |
| --------------------------------------------------------------------------------------------------- | -------------- | ------------- |
| [v1.0](https://github.com/gchq/stroom-content/releases/tag/stroom-101-v1.0)                         | Y              | Y             |

## Building the content packs

Each content pack is defined as a directory within _stroom-content-source_ with the name of content pack being the name of the directory.

There are currently two methods to building content packs, a Gradle build or a python script. The former has superseded the latter, though the python script is still useful if you want all packs built into a single zip file. For all other purposes the Gradle build should be favoured.

### Gradle build

The Gradle build will manage the dependencies between content packs, including transitive dependencies, so if you want a pack that has dependencies on other packs then those dependency packs will get built with it. Running the build requires _xmllint_ and _python_.

To run a full build of all packs do:

`./gradlew clean build`

If you just want to validate the pack's source to make sure the UUID references are correct and there are no clashes, then do:

`./gradlew validate`

To build a single pack do something like:

`./gradlew clean :my-pack-name:build`

The build will place the following pairs of zip files in `./build/distributions`:

* `my-pack-name.zip`

* `my-pack-name-all.zip`

The `-all` variant contains all dependency packs and is therefore larger. The other zip just contains the named pack with no dependencies.


### Python script (_deprecated_)

The content packs can be built into the zip files by running the python script _buildContentPacks.py_. You can either build all packs or a list of named packs and you can choose to have them packaged as single combined zip or one per pack.

For example:

```bash
#build all packs into a single zip
./buildContentPacks.py --combine --all 

#build all packs, one zip per pack
./buildContentPacks.py  --all 

#build a named list of packs
./buildContentPacks.py  stroom-101 core-xml-schemas
```

## Released content packs

To see all the pre-built content packs see the [releases page](https://github.com/gchq/stroom-content/releases).

## Importing the content packs

The content pack zip files can be imported into _Stroom_ by selecting _Import_ from the _Tools_ menu.

## Content pack development

### IMPORTANT - A word about UUIDs

Each _Stroom_ entity is identified by a [Universally Unique Identifier](https://en.wikipedia.org/wiki/Universally_unique_identifier).  Where entities have dependencies on other entities, e.g. a _Dashboard_ entity depends on a _StatisticStore_ entity, this dependency relationship is defined by the UUID of the dependency entity. It is possible to import content into _Stroom_ with missing dependencies as this can sometimes be a valid use case, though with missing dependencies the entities will not work correctly until the dependencies are resolved, e.g. by updating the dependency.

All the entities defined in these content packs are defined with hard coded UUIDs and dependencies.  It is therefore critical that the correct UUIDs are used in the entity and dependency definitions in the XML.

Where a Stroom folder is used in more than one content pack, you should ensure that the name of the folder and its UUID matches for all instances.

### Gradle dependencies

The dependencies between content packs are also defined in the Gradle `build.gradle` file in the root of each content pack directory. The `createSkeletonPack.sh` script will create a skeleton `build.gradle` file for you with an example of how to define dependencies on other packs. A pack should define all direct dependencies it has in its entities and not rely on transitive dependencies as this is more explicit.

### Helper scripts

The following helper shell scripts exist to assist in content pack creation.

#### createSkeletonPack.sh

This script will create a skeleton structure for a new content pack.

`./createSkeletonPack.sh my-new-pack`

#### createNewStroomFolder.sh

This script can be run from within an directory to create a new Stroom Folder (XML definition file and directory). This should only be used to create a directory that doesn't already exist in Stroom as it will have a new UUID generated for it.
