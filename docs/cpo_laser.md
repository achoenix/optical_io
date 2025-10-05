
{% include nav.md %}

## [License & Citation](index.md#license)

<br>

# External Light Source for CPO


## Navigation

- [Standards](#standards)
    - [CW-WDM](#cw-wdm)
    - [OIF-ELSFP](#oif-elsfp)
    - [IPEC-PELS](#ipec-pels)
- [Suppliers](#suppliers)

<br>

## Standards

### CW-WDM

[Website](https://cw-wdm.org/)

*Version: June 4th, 2021*

**Wavelength grid specifications**

Table 2–5 Grid specifications

| Description      | Wavelength span (nm) | Number of channels | Channel spacing (GHz) | Channel bandwidth (GHz) |
|------------------|----------------------|--------------------|------------------------|--------------------------|
| Grid definition  | 9                    | 8+1                | 200                    | 100                      |
|                  | 9                    | 16+1               | 100                    | 50                       |
|                  | 18                   | 8+1                | 400                    | 200                      |
|                  | 18                   | 16+1               | 200                    | 100                      |
|                  | 18                   | 32+1               | 100                    | 50                       |
|                  | 36                   | 8+1                | 800                    | 400                      |
|                  | 36                   | 16+1               | 400                    | 200                      |
|                  | 36                   | 32+1               | 200                    | 100                      |

<br>

**Maximum output power classes**

| Output power class | 9 nm span  | 18 nm span  | 36 nm span | Units |
|--------------------|----------------------------------|----------------------------------|----------------------------------|--------|
| Type 1             | 20                               | 20                               | 14                               | dBm    |
| Type 2             | 14                               | 14                               | 8                                | dBm    |
| Type 3             | 26                               | 26                               | 20                               | dBm    |

<br>



### OIF-ELSFP

[Website](https://www.oiforum.com/technical-work/implementation-agreements-ias/)

*Version: 02.0 (January 8, 2025)*

<br>

**ELSFP module example**

<img src="img/elsfp_module.png" width="300" />

<br>

**ELSFP application in CPO systems**

<img src="img/elsfp_app.png" width="600" />

<br>

**ELSFP differnet solutions**

<img src="img/elsfp_solution.png" width="800" />

<br>

**Optical power classes**

| Optical Power Classes      | Power/λ/Core (+/- 1.5dB) |
|-----------------------------|--------------------------|
| Low Power - LP              | 11 dBm                  |
| Medium Power - MP           | 14 dBm                  |
| High Power - HP             | 17 dBm                  |
| Very High Power - VHP       | 20 dBm                  |
| Ultra High Power - UHP      | 23 dBm                  |
| Super High Power - SHP      | 26 dBm                  |

<br>

**Selected Electrical power specifications**

| Parameter                                                                 | Symbol    | Minimum | Nominal | Maximum | Unit |
|---------------------------------------------------------------------------|-----------|----------|----------|----------|------|
| ELSFP power supply voltage | Vcc       | 3.135    | 3.3      | 3.465    | V    |
| Total VCC Supply Current*                                                 | Icc       |          |          | 9        | A    |

*: Utilization of the maximum ELSFP electrical power rating requires thermal design and validation at the system level to ensure the maximum connector temperature is not exceeded. An informative design practice is to heatsink the host board power pin pads with multiple vias to a thick copper power plane for conductive cooling.

<br>


### IPEC-PELS

**Optical power classes and power consumption**

| Parameter                                   | Min  | Max  | Unit | Power consumption |
|---------------------------------------------|------|------|------|-------------------|
| Optical power max per fiber @12ch            | 17.5 | 19   | dBm  | <9W               |
| Optical power max per fiber 1:4 [DR4]@8ch    | 19   | 21   | dBm  | <15W              |
| Optical power max per fiber 1:4 [FR4]@8ch    | 20   | 22   | dBm  | <15W              |
| Optical power max per fiber 1:8 [DR4/FR4]    | TBD  | TBD  | dBm  | TBD               |
| Optical power max per fiber 1:4 [DR4]@16ch   | TBD  | TBD  | dBm  | TBD               |
| Optical power max per fiber 1:4 [FR4]@16ch   | TBD  | TBD  | dBm  | TBD               |


<br>


---

## Suppliers

<br>

### O-Net

#### Product

**[Product Page](http://n.o-netcom.com/m/prod_view.aspx?TypeId=106&Id=322&Fid=t3:106:3)**
- Class VHP (20dBm/λ/fiber) (once marketing a 25dBm version)
- 4 or 8 channels
- Cooled or Uncooled laser (1311nm)
- 2x DR4 or 2x FR4 configurations

#### Partnership

**Siver**
- [Press Release (2025/04)](https://www.sivers-semiconductors.com/press/sivers-semiconductors-announces-strategic-oem-partnership-with-o-net-technologies-to-deliver-next-generation-external-laser-sources-for-co-packaged-optics/)
- Siver: DFB laser provider
- O-Net: OEM module manufacturer

**Lumentum**
- [Press Release (2023/04)](https://www.lumentum.com/en/media-room/news-releases/scaling-photonics-new-heights-lumentum-ofc-2023)
- Lumentum: ultra high power 1310nm DFB provider
- O-Net: (active) ELSFP module provider

