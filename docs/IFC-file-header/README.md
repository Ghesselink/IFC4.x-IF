# IFC file header

Each IFC file (.ifc), using the STEP physical file format (as per ISO10303-21), consists of a **header** section and a **data** section

Each header of IFC files should contain, in this order, the following parts:
- FILE_DESCRIPTION
- FILE_NAME
- FILE_SCHEMA

## Header structure
The table below captures the structure of the header.

| **FILE_DESCRIPTION** | **Explanation**                                                  | **Example**                                   |
|----------------------|------------------------------------------------------------------|-----------------------------------------------|
| description          | formal definition of the underlying view definition              | ViewDefinition [Alignment-basedView]          |
| implementation_level | see ISO10303-21, currently always 2;1 for IFC files              | 2;1                                           |
| **FILE_NAME**        |                                                                  |                                               |
| name                 | file name of the IFC export file                                 | Header example.ifc                            |
| time_stamp           | timestamp of creation of the IFC export file, as per ISO 8601    | 2022-09-16T10:35:07                           |
| author               | user defined field to capture the author/creator of the IFC file | Evandro Alfieri                               |
| organization         | user defined field to capture the organization of the author     | buildingSMART Int.                            |
| preprocessor_version | name of the toolbox used to create the IFC file                  | MyToolboxName Version 1.0                     |
| originating_system   | name of the application (including built number)                 | MyApplicationName Version 22                  |
| authorization        | user defined field to capture the authorizer of the IFC file     | none                                          |
| **FILE_SCHEMA**      |                                                                  |                                               |
| schema_identifiers   | name of the IFC schema                                           | IFC4X3_ADD2                                   |

## Valid and Invalid information
Below some examples of Valid and Invalid information for some of the header's fields listed above. Some of these are just examples, not a comprehensive set of Valid and Invalid entries.


### description

**Valid**:

- **Only** for files having a `IFC2X3`schema identifier
    - `ViewDefinition [CoordinationView_V2.0]`
    - `ViewDefinition [SpaceBoundaryAddonView]`
    - `ViewDefinition [BasicFMHandoverView]`
    - `ViewDefinition [StructuralAnalysisView]`

- **Only** for files having a `IFC4`schema identifier
    - `ViewDefinition [ReferenceView_V1.2]`
    - `ViewDefinition [IFC4Precast]`

- **Only** for files having a `IFC4X3_ADD2`schema identifier
    - `ViewDefinition [ReferenceView]`
    - `ViewDefinition [Alignment-basedView]`

**Invalid**:
- `ViewDefinition [ReferenceView]`, when coupled with a `IFC2X3`or `IFC4` schema identifiers
- `ViewDefinition [ReferenceView_V1.2]`, when coupled with a `IFC2X3`or `IFC4X3_ADD2` schema identifiers
- `ViewDefinition [CoordinationView_V2.0]`, when coupled with a `IFC4`or `IFC4X3_ADD2` schema identifiers
- `ViewDefinition[ReferenceView]`, missing blank space
- `ViewDefinition [CoordinationView_V2.0, QuantityTakeOffAddOnView, SpaceBoundary2ndLevelAddOnView]'`, contains not official, final buildingSMART MVDs
-  `ViewDefinition [Alignment-basedReferenceView]`, not official, buildingSMART MVD


### time_stamp

**Valid**:
- `2016-03-15T16:45:40`
- `2022-11-21T05:33:05-05:00`

**Invalid**:
- `2016-03-15T016:45:40`, ISO 8601 specifies that the hour component in the time portion should be represented by a two-digit number ranging from `00` to `23`. Leading zeros are used if necessary. `016` is a three-digit number and not within the allowed range of `00` to `23`.


### organization

**Valid**:
- `$`
- `Unknown`
- `''`
- `Cool Engineering Firm`

**Invalid**:
- :warning: There are no strictly Invalid entries here. However, **this field shall not be used to indicate the name of the software company developing the tool used for the IFC export**. This information shall be stored in the _originating_system_ filed, as specified below. 


### preprocessor_version

**Valid** (examples):
- `ODA SDAI 24.11`
- `Ifc Engine <1.0.1.1365>`
- `GeometryGymIFCcore v0.2.14.0 by Geometry Gym Pty Ltd built 2023-02-27T22:50:56`
- `BSProLib (Ver:2021.4.0.5)`
- `ST-DEVELOPER v16.13`

**Invalid** (examples):
- `$`, `''`, `Unknown` or anything similar. This field shall be meaningfully provided
- `ODA SDAI`, version not provided
- `IFC Database Version: Work`, not meaningful
- `Tekla Structures Next`, that is probably the _originating_system_, see next paragraph


### originating_system
To be complete and meaningful, this field shall include at least this 3 information
1. _Software Company Name_
2. _Application Name_
3. _Application Version_

**Valid** (examples):
- `Autodesk Revit 26.0.0.0 (ENU) - IFC 26.0.0.0`

**Invalid** (examples):
- `$`, `''`, `Unknown` or anything similar. This field shall be meaningfully provided
- `Civil 3D 2023 IfcInfra Plugin v0.8.0.0`, missing _Software Company Name_
- `Allplan 2022.0 12.10.2021 - 23:11:26`, missing _Software Company Name_
- `Quadri <26.0.35.0>`, missing _Software Company Name_
- `OpenBuildings Designer10.09.01.38`, missing _Software Company Name_
- `22.0.0.0 - Exporter 22.0.0.0 - Alternate UI 22.0.0.0`, missing _Software Company Name_ and _Application Name_
- `Lexocad`, missing _Software Company Name_ and _Application Version_
- `ACCA Software S.p.A. - Edificius usBIM(k)`, missing _Application Version_
- `Windows`, that is probably the originating (operating) system. Not meaningful
- `IFC Export Version Jul 11 2019`, that is probably the preprocessor information.
- `IfcPusPlus`, that is probably the preprocessor information. Even if it is, missing the version
- `IFC file generated by Graphisoft ArchiCAD-64 18.0.0 NED FULL Windows version (IFC2x3 add-on version: 6000 NED FULL)`, a bit too verbose, making it difficult to automate data extraction
- `RailCOMPLETE\X2\00AE\X0\ version 0.12.7.0 with DNA 2023.a (alias 2023.a (alpha 2023-04-18T00:00:00+01:00)) for NO-BN (alias Bane NOR SF) created on 2023-04-18T00:00:00+01:00 by NO.0001 (alias Railcomplete AS)`, too verbose


## Header example
The code snippet below is an example of file header, **extracted from the file [Header example.ifc](./Header%20example.ifc)**

```
HEADER;
FILE_DESCRIPTION(('ViewDefinition [Alignment-basedReferenceView]'),'2;1');
FILE_NAME('Header example.ifc', '2022-09-16T10:35:07', ('Evandro Alfieri'), ('buildingSMART Int.'), 'MyToolboxName Version 1.0', 'MyApplicationName Version 22', 'none');
FILE_SCHEMA(('IFC4X3_ADD1'));
ENDSEC;`
```


*For further information, see the complete [IFC header policy](https://standards.buildingsmart.org/documents/Implementation/ImplementationGuide_IFCHeaderData_Version_1.0.2.pdf)*