# Finnish building stock default structural data

This dataset contains default data describing the variety of structures used in the Finnish building stock, as well as ventilation properties.
The main data contains structural layer properties like type, thickness, and material by `source` and `structure`.
The data in this dataset is based on a variety of public sources, and a lot of subjective guesstimation.

The main source is the [appendix of the Finnish energy certificate guide](https://www.ymparisto.fi/download/noname/%7BA6558C5F-9B2E-40E5-B261-605118163F03%7D/141252),
but others are included as well.


## Contents

Here's a quick overview of the important contents of this dataset,
and a brief description of each file.
For a detailed description of this dataset, see the end of the README.

1. `datapackage.json`, the [Data Package](https://specs.frictionlessdata.io/data-package/) definition.
2. `import_finnish_structural_data.json`, the [Spine Toolbox](https://github.com/Spine-project/Spine-Toolbox) importer specification.
3. `data/fenestration.csv`, contains properties of fenestration for different building types and age categories.
4. `data/materials.csv`, lists all the different materials and their properties that are relevant for the structures.
5. `data/sources.csv`, lists the different `sources` for the structural data used in this dataset.
6. `data/structure_descriptions.csv`,  lists each `structure` by `source`, and their overall properties and descriptions.
7. `data/structural_layers.csv`, contains the detailed structural data down to individual layers.
8. `data/types.csv`, contains surface resistances for different types of structures.
9. `data/ventilation.csv`, contains ventilation properties for different building types and age categories.
10. `data/ventilation_spaces.csv`, contains thermal resistances for ventilation spaces according to Finnish building code C4 2003.


## Usage

The dataset is formatted as a [Data Package](https://specs.frictionlessdata.io/data-package/),
hopefully facilitating interoperability and reuse.
However, this dataset was originally designed to be used via [Spine Toolbox](https://github.com/Spine-project/Spine-Toolbox),
with the following steps in mind:

1. Add a *Data Connection* item, and refer it to the `datapackage.json` file.
2. Use the *Add specification from a file* option from the toolbar, and select the `import_finnish_structural_data.json` file to add the necessary *Importer* definition.
3. Add an *Importer* item, connect the above *Data Connection* to it, and select the above importer specification for it.
4. Connect the *Importer* to the desired *Spine Datastore* where you want the data to be imported to.


## License

Copyright © 2020 Topi Rasku <topi.rasku@vtt.fi> and [VTT Technological Research Centre of Finland Ltd.](https://www.vttresearch.com/en)
This dataset is licensed under the [Creative Commons Attribution 4.0 International (CC BY 4.0)](https://creativecommons.org/licenses/by/4.0/).
See `LICENSE` for more information.


## Acknowledgements

<center>
<center>
<table width=500px frame="none">
<tr>
<td valign="middle" width=100px>
<img src=https://www.aka.fi/globalassets/aka_en_vaaka_valkoinen.svg alt="AKA emblem" width=100%></td>
<td valign="middle">
This dataset was built for the Research Council of Finland project "Integration of building flexibility into future energy systems (FlexiB)" under grant agreement No 332421.
</td>
</table>
</center>

<center>
<table width=500px frame="none">
<tr>
<td valign="middle" width=100px>
<img src=https://csc.fi/o/csc-theme/images/csc-logo-teksti-fi.png alt="CSC emblem" width=100%></td>
<td valign="middle">
CSC – IT Center for Science, Finland, provides the Fairdata.fi service for easy research metadata publication.
</td>
</table>
</center>


# Dataset description

This section hopes to describe the raw data in enough detail to give you a better idea
of what is and isn't included in the dataset.
The format of the raw data can be a bit confusing at times,
as it was originally designed for the [`finnish_RT_structural_data`](https://vttgit.vtt.fi/flexib/finnish_RT_structural_data) dataset,
and doesn't necessarily fit the non-structural data so well.

Currently, the weights of each structure for each building type in `data/structure_descriptions.csv` is based solely on guessimation.
It might be possible to refine these values based on data from e.g. Statistics Finland, but I've yet to try and do so.


## `data/fenestration.csv`

This file contains the following fenestration properties: `U_value_W_m2K`, `solar_energy_transmittance`, `frame_area_fraction`, and `notes`
for each `source` and `building_type`.
The `source` basically represents the time period the windows in question were in use after.

Sources for the raw data include:
- The [appendix of the Finnish energy certificate guide](https://www.ymparisto.fi/download/noname/%7BA6558C5F-9B2E-40E5-B261-605118163F03%7D/141252)
- [Puuikkunan historia](https://urn.fi/URN:NBN:fi:amk-2012120217741) by Pauliina Kamarainen.
- [Vanhan puuikkunan energiakunnostus](https://urn.fi/URN:ISBN:978-952-5863-80-2) by Janne Jokelainen.
- [Ikkunoiden energiatalous](https://urn.fi/URN:NBN:fi:amk-201703283772) by Ilpo Kaikkonen.


## `data/materials.csv`

This file defines the different `structure_materials`, links them to the appropriate `frame_material`,
as well as contains their necessary thermal properties.
Minimum and maximum values found in the literature are reported for density (kg/m3), specific heat (J/kgK), and thermal conductivity (W/mK).

Sources include:
- Primary source [`Rakennusmateriaalisen rakennusfysikaaliset ominaisuudet`](https://urn.fi/URN:NBN:fi:tty-201505131277) by Katariina Laine 2010.
- [Finnish Building Code C4 2003](http://www.finlex.fi/data/normit/1931-C4s.pdf).
- Missing values from [EngineeringToolbox](https://www.engineeringtoolbox.com/), or simply guesstimated.


## `data/sources.csv`

This file defines the different `sources` of data,
and contains the `source_year` and `source_description` parameters for each `source`.
Essentially, each `source` corresponds to a different time period (`source_year`),
after which the structures, fenestration, or ventilation solutions can be assumed to have been in use.
The `source_description` parameter contains a brief description of the contents of each `source`.


### `data/structure_descriptions.csv`

This file defines each `structure` by `source`,
as well as a pivot table for guesstimated `weights` of each `structure` by `building_type`.
Furthermore, the `design_U_value_W/m2K` and a brief `description` of each `structure` is included,
as are `structure_notes` about the assumption regarding the assumed `weights`.

Sources include:
- The [appendix of the Finnish energy certificate guide](https://www.ymparisto.fi/download/noname/%7BA6558C5F-9B2E-40E5-B261-605118163F03%7D/141252)


## `data/structural_layers.csv`

This file defines each the properties of individual structural layers, represented using a `layer_id`.
Each `layer_id` is attributed to a specific structure indicated by the `(source, structure)` pair,
and each structure is assigned a `structure_type` from
`[roof, exterior_wall, base_floor, separating_floor, partition_wall]`
*(this really should be a property in `structure_descritions.csv`, but is currently defined here, unfortunately)*.
Furthermore, each `layer_id` is attributed to a `structure_material`,
which determines the material properties for that layer.
The rest of the columns are layer-specific parameters, described below:

- `layer_weight`: Describes the approximated share of the total volume in the case of overlapping `layers`, e.g. different types of furring with space in between the material.
- `layer_number`: Determines the order of the `layers` in the `structure`, explained in detail further down.
- `layer_minimum_thickness_mm`: Determines the minimum possible thickness of the `layer` in millimetres.
- `layer_load_bearing_thickness_mm`: Determines the minimum thickness of the `layer` in millimetres, when the `structure` is load-bearing.
- `layer_tag`: Describes the primary purpose of the `layer`.
- `notes`: Contains additional information about possible assumptions, dimensioning, furring, etc.

All the structures have their layers enumerated using the `layer_number` to indicate their order in the structure.
For envelope structures, the ordering of the layers is relative to the thermal insulation layer, with the thermal insulation layer being the zeroth layer.
Structural layers towards the exterior of increase into positive numbers, and structural layers towards the interior decrease into negative numbers.
For separating floors and partition walls, the ordering of the layers is relative to the load-bearing layer.
Numbering of structural layers of separating floors and partition walls is based on their order in the RT card, with the topmost structure being the positive extreme.

Sources include (but not limited to):
- Primary source: the [appendix of the Finnish energy certificate guide](https://www.ymparisto.fi/download/noname/%7BA6558C5F-9B2E-40E5-B261-605118163F03%7D/141252)
- [Asuinkerrostalojen välipohjarakenteet 1890-1960 ja niiden korjaaminen](http://urn.fi/URN:NBN:fi:aalto-201705114678) by Suvi Takko.
- [1900-luvun kerrostalojen yleisimmät rakenteet ja niissä ilmeneviä ongelmia](https://urn.fi/URN:NBN:fi:amk-201505209183) by Tomi Paajanen.
- [Korjausrakentamisen suunnitteluratkaisuja 1800 – 1950-luvuilla rakennettuihin rakennuksiin Suomessa. Suunnittelijan ohje.](https://urn.fi/URN:NBN:fi:amk-201704215078) by Tiina Säily.
- [Puurunkoisen pientalon energiatehokkuuden kehitys](https://urn.fi/URN:NBN:fi:amk-2010111014307) by Janne Taskinen.


## `data/types.csv`

This file contains the following parameters: `interior_resistance_m2K_W`, `interior_resistance_m2K_W`,
`linear_thermal_bridge_W_mK`, `is_internal`, `ventilation_space_heat_flow_direction`, and `structure_type_notes`,
for each `structure_type`: `base_floor`, `exterior_wall`, `partition_wall`, `roof`, `separating_floor`.
The values are based on the Finnish building code `Suomen rakentamismääräyskokoelma C4: Lämmöneristys, ohjeet 2003`, but the 2013 draft has the exact same values.
The linear thermal bridges are based on `Energiatehokkuus - Rakennuksen energiankulutuksen ja lämmitystehotarpeen laskenta (2018)`, which is also a part of the Finnish building code.


## `data/ventilation.csv`

This file contains the minimum and maximum ventilation rate (1/h), n50 infiltration rate (1/h),
infiltration factor influenced by the height of the building, as well as the heat recovery unit (HRU) efficiency,
for each `source` and `building_type`.
A `notes` column is also included for notes about data sources and data quality.

Sources include:
- [Asuinrakennusten ilmanpitävyys, sisäilmasto ja energiatalous](https://urn.fi/URN:NBN:fi:tty-2011122914971) and [Puurunkoisten pientalojen kosteus- ja lämpötilaolosuhteet, ilmanvaihto ja ilmatiiviys](https://urn.fi/URN:NBN:fi:tty-2011041510587), both reports by Juha Vinha et al. at Tampere University of Technology.
- [Ilmatiiveys ja vuotokohdat pientaloissa](http://urn.fi/URN:NBN:fi:aalto-201505132633) by Heikki Jussila.
- [Energiatehokkuus - Rakennuksen energiankulutuksen ja lämmitystehotarpeen laskenta](https://ym.fi/documents/1410903/38439968/Ohje---Rakennuksen-energiankulutuksen-ja-lammitystehontarpeen-laskenta-20-12-2017-4332AA81_75E1_4CA0_B208_B0ACB60A267F-133692.pdf/277c79e7-2a12-5052-ba33-cb2e2c8709ab/Ohje---Rakennuksen-energiankulutuksen-ja-lammitystehontarpeen-laskenta-20-12-2017-4332AA81_75E1_4CA0_B208_B0ACB60A267F-133692.pdf?t=1603260201597) by Ympäristöministeriö.
- [Puurunkoisen pientalon energiatehokkuuden kehitys](https://urn.fi/URN:NBN:fi:amk-2010111014307) by Janne Taskinen.
- [Rakennusten sisäilmasto ja ilmanvaihto D2 (2012)](https://ymparisto.fi/download/noname/%7B5EB5B5EC-4DF9-44B9-A315-94D1E6B84AFE%7D/134437) by Ympäristöministeriö.


## `data/ventilation_spaces.csv`

This file contains the thermal resistance of ventilation spaces depending on thickness and heat flow direction according to the Finnish building code C4 2003.
