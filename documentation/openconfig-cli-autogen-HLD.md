# openconfig-cli - A SONiC Application

sonic-openconfig-cli container is developed to automatically generate CLIs for Openconfig yang model to minimize the development and maintenance effort of supporting Openconfig yang models in SONiC.

## Existing workflow for supporting Openconfig Yang
SONiC has established development guideline for [supporting openconfig YANG](https://github.com/project-arlo/SONiC/blob/e5922bd39823aaeb0a2297f75e051ff5cf1d3186/doc/mgmt/Developer%20Guide.md#23-openconfig-yang):
1. add an openconfig yang to sonic-mgmt-common
2. generate and edit responding annotation yang using sonic-extension annotation. The annotation yang provides the mapping between openconfig yang model and SONiC redis DB, tables and fields.
3. write coresponding sonic yang for configuration validation layer (CVL) which is invoked when writing to redis DB via REST APIs. 
4. write transformer for common app in translib. At this point, REST API for the openconfig yang model is supported.
5. write CLI (xml, actioner and render) based on [klish framework](https://src.libcode.org/pkun/klish/src/master)

Currently, most sonic CLI is implemented in sonic-utility based on [python click library](https://click.palletsprojects.com/en/8.1.x/). So providing click based CLIs is desireable as kish based CLIs are only available inside management-framework docker.
   
## Requierment
- It should be a SONiC compatible Docker image, based on [SONiC application extension mechanism](https://github.com/sonic-net/SONiC/tree/master/doc/sonic-application-extension).
- It can be built stand alone, then installed on a SONiC system at run time. Or it can be built into a sonic image at build time using [sonic-buildimage](https://github.com/sonic-net/sonic-buildimage).
- Automatically generates click based pyhton CLI code from Openconfig yang and its annotation.
- Provide a way of including other manual written CLIs.

## Design 

The following diagram illustrates the context and design of this application.
<img src="../assets/openconfig-cli.png" alt="openconfig-cli application design" style="zoom: 50%;" />

sonic-mgmgt-framework provides tools and infrastructure for adding openconfig yang (step 1-3). With that the REST APIs coresponding to the openconfig yang mode can be supported. However, the sonic yang for CVL (configuration validation layer) at step 3 need to be written and the coresponding CLIs, wether it is klish based or clish based, are still need to be developed.

openconfig-cli improves this process by:
- autogenerates sonic yang for CVL and eliminates the manul effort in step 3
- autogenerates click based CLI by using [SONiC CLI auto gneration tool](https://github.com/sonic-net/SONiC/blob/master/doc/cli_auto_generation/cli_auto_generation.md). As result development CLI becomes fully automated and no manul effort is required.
- Current SONiC cli autogeneration tools only support set and show from `config-db`. [A PR](https://github.com/sonic-net/sonic-utilities/pull/3222) is submitted to enhance cli genertion tool to support show data from `state-db`.

## Implementation
Please see implementation in [this repository](https://github.com/jjin62/sonic-openconfig-cli).
