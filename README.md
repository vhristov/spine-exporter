# Spine Exporter

Spine exporter is an attempt to automate [Esoteric Software](http://esotericsoftware.com/)'s Spine export functionality. It finds all spine files in a source folder (called **GRAPHICS_PATH**) and exports them preserving the tree to a destination folder (called **EXPORT_PATH**).
The final goal of this script is to enable automatic bulk export of multiple spine projects with as little manual configuration as possible, while keeping as much flexibility to fine tune different settings as possible.
It shall be noted that at this initial state there are currently more [Known issues and limitations](#known-issues-and-limitations) than there are [configuration option](#Configuration-options) of the output (pull requests are welcome).

Note that in order to use this script you need a to have a valid spine license and installed version of the spine launcher.

## Supported features
Spine export script supports the following features:
 - Export using a specific Spine version.
 - Export every project to preserving the path from source to destination folder
 - Configuration of the source and destination paths
 - Having global and per spine export settings json file
 - Per spine export settings json file automatic detection

## Configuration options
There are two type of configuration settings. One for the script presented in this repository and one for the spine
### Script settings
Script settings is configured using environment variables. Each of the environment variables have as sane as possible default that shall enable running the export script in most common cases.
| Variable          | Description                                     |          Default|
|-------------------|-------------------------------------------------|-----------------|
| GRAPHICS_PATH     | Source location to search for spine projects    | Current path    |
| EXPORT_PATH       | Destination location to export spine project to | ../SpineExport/ |
| SPINE_VERSION     | Version to use for the export | 4.0.08 |
| EXPORT_SETTINGS   | Global settings file location. If the specified file does not exists, then it defaults to "json+pack" option as a failback. |  SpineExportDefault.export.json (located in Graphics path)|
| SPINE_EXE         | Path to use for running spine. By default it tries to detect a few common spine install locations on Windows, Linux and Mac. This Variable shall be set only in case the auto detection is not working (for example when installed in custom location) | |

### Spine settings
There are two different sets of settings that can be provided to spine. One is on a global level and one is on per .spine file. Both of them are in the spine own export settings format that can are used in Load and Save settings in [spine's export window](http://esotericsoftware.com/spine-export#JSON)

The global export settings is taken from **EXPORT_SETTINGS** environment variable.

The per project file needs to be named the same way as the spine project file, but shall have an extension of ".export.json" instead of ".spine" and it needs to be located at the same location. For example for SpineBoy.spine a custom settings file shall be named SpineBoy.export.json

## Usage example
This example assumes there are two separate git repos - one for the code project (for example unity) and one for the spine source projects cloned in the same folder:
```console
example/
├── graphics
│   ├── SpineProject1
│   └── SpineProject2
└── unity
    └── Assets
        └── Graphics
            ├── SpineProject1
            └── SpineProject2
```
The easiest way to use the spine exporter would be to clone this git repository next to them:
```console
 $ git clone https://github.com/vhristov/spine-exporter.git
```
Create a minimal configuration script in **graphics** folder that runs the **spine_export.sh**:
```
#!/bin/bash

export GRAPHICS_PATH=${GRAPHICS_PATH:-"."}
export EXPORT_PATH=${EXPORT_PATH:-"../unity/Assets/Graphics/"}
export SPINE_VERSION=${SPINE_VERSION:-"4.0.08"}

../spine-exporter/spine_export.sh
```
Run the above script from graphics folder.

The script was tested on Windows 10 using git bash and linux.

##  Known issues and limitations

 - Output path cannot be configured per spine project and is currently expected to match between source and destination folders
 - Paths containing spaces will break the script. All attempts to quote the paths so far resulted in spine not finding them.
 - It is possible if there are too many spine projects that the bash's number of arguments limit might be hit
