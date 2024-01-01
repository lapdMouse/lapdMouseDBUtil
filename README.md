# lapdMouseDBUtil

**lapdMouseDBUtil** is a command line tool to list, search, and download files
from the
[Lung anatomy + particle deposition (lapd) mouse archive](https://doi.org/10.25820/9arg-9w56).
For more information about the lapdMouse project and available data visit
<https://doi.org/10.25820/9arg-9w56>. Using **lapdMouseDBUtil**
you can for example download all AirwayWallDeposition.vtk files for all datasets
in the data archive using one command:

`python lapdMouseDBUtil.py --pattern=*/*AirwayWallDeposition.vtk --action=download`

## Getting started

**lapdMouseDBUtil** is a [Python](https://www.python.org/download) script. It
does not require installation but it requires Python to be installed on the
user's system.

**Prerequisite - Python**: If Python has not yet been installed on your system, go
to <https://www.python.org/downloads>, download and install a version suitable
for your operating system. **lapdMouseDBUtil** has been tested with Python 3.4.3
and 2.7.6 on Windows 7, Ubuntu 16.04, and MacOS Sierra.

**Download lapdMouseDBUtil**: Go to <http://lapdmouse.iibi.uiowa.edu/software>,
download **lapdMouseDBUtil** and unzip if necessary.

**Test**: Go to the folder containing lapdMouseDBUtil.py and call
`python lapdMouseDBUtil.py`

## Step-by-step usage guide

Using **lapdMouseDBUtil** to obtain files from the lapdMouse data archive involves
the following steps:

  - [List datasets](#listdatasets)
  - [List program options](#listprogramoptions)
  - [Select files by specifying file search pattern](#selectfilesbyspecifyingfilesearchpattern)
  - [Specify local download folder](#specifylocaldownloadfolder)
  - [Download data (or update data)](#downloaddataorupdatedata)
  - [Verify download status](#verifydownloadstatus)

All steps will be explained in detail below.

### List datasets

Calling `lapdMouseDBUtil.py` without any parameters lists all datasets in the
lapdMouse data archive.

```sh
$ python lapdMouseDBUtil.py
DB access status: available
DB query: root=., depth=0, pattern=*
m01 -> ./m01 (folder)
m02 -> ./m02 (folder)
m03 -> ./m03 (folder)
...
Matching files/folders: total=35(0.0 B), downloaded=0, require download=35(0.0 B), require update=0
```

It provides in addition the following information:

  * **Database access status**, whether the tool was able to connect to the
    lapdMouse  database.
  * The **file search pattern** utilized in querying the data archive.
  * The **list of matching** files/folders in the data archive. Each line follows
    the pattern `nameInArchive -> localFileName (download status, size)`,
    where **download status** is one of:
      * `require download`: the file is available on the server but has not yet
        been downloaded
      * `downloaded`: the file has been downloaded and matches the version on the
        server
      * `require update`: the file has been downloaded but a newer version
        is available on the server
      * `folder`: folder, not a file
  * A **summary** of files/folders in the data archive matching the query pattern
    organized by their download status and their total size.

### List program options

To obtain a list of options supported by `lapdMouseDBUtil.py`, call

```sh
$ python lapdMouseDBUtil.py --help
usage: lapdMouseDBUtil.py [-h] [-p [PATTERN]] [-l LOCALDIR]
                         [-a {list,download,update,none}]

lapdMouse data archive access tool to list, search, and download files.

optional arguments:
  -h, --help            show this help message and exit
  -p [PATTERN], --pattern [PATTERN]
                        file pattern
  -l LOCALDIR, --localDir LOCALDIR
                        local base directory for download
  -a {list,download,update,none}, --action {list,download,update,none}
                        action to perform

lapdMouse project webpage including documentation and support:
<https://lapdmouse.iibi.uiowa.edu>
This work was supported by NIH project R01ES023863.
```

### Select files by specifying file search pattern
First, specify a file search pattern (`--pattern`) describing the files you
are interested in. E.g. list all aerosol deposition images `Aerosol*.mha` for
dataset `m01`:

```
$ python lapdMouseDBUtil.py --pattern=m01/*Aerosol*.mha
DB access status: available
DB query: root=m01, depth=0, pattern=*Aerosol*.mha
m01/m01_Aerosol.mha -> ./m01/m01_Aerosol.mha (require download; 25.4 GB)
m01/m01_AerosolDeconv.mha -> ./m01/m01_AerosolDeconv.mha (require download; 25.4 GB)
m01/m01_AerosolDeconvSub2.mha -> ./m01/m01_AerosolDeconvSub2.mha (require download; 3.2 GB)
m01/m01_AerosolDeconvSub4.mha -> ./m01/m01_AerosolDeconvSub4.mha (require download; 405.2 MB)
m01/m01_AerosolNormalized.mha -> ./m01/m01_AerosolNormalized.mha (require download; 25.4 GB)
m01/m01_AerosolNormalizedSub2.mha -> ./m01/m01_AerosolNormalizedSub2.mha (require download; 3.2 GB)
m01/m01_AerosolNormalizedSub4.mha -> ./m01/m01_AerosolNormalizedSub4.mha (require download; 405.2 MB)
m01/m01_AerosolSub2.mha -> ./m01/m01_AerosolSub2.mha (require download; 3.2 GB)
m01/m01_AerosolSub4.mha -> ./m01/m01_AerosolSub4.mha (require download; 405.2 MB)
Matching files/folders: total=9(86.8 GB), downloaded=0, require download=9(86.8 GB), require update=0
```

Common search patterns to specify interesting files include:

  * all datasets: `python lapdMouseDBUtil.py --pattern=*`
  * all top level files for a specific dataset (`m01`): `python lapdMouseDBUtil.py --pattern=m01/*`
  * all aerosol deposition images for a specific dataset (`m01`): `python lapdMouseDBUtil.py --pattern=m01/*Aerosol*.mha`
  * smallest aerosol deposition images for a specific dataset ('m01'): `python lapdMouseDBUtil.py --pattern=m01/*Aerosol*Sub4.mha`
  * all .vtk meshes for a specific dataset (`m01`): `python lapdMouseDBUtil.py --pattern=m01/*.vtk`
  * raw cryomicrotome image data for a specific dataset (`m01`): `python lapdMouseDBUtil.py --pattern=m01/m01_RawCryomicrotomeData/*`
  * Info.md files for **all** datasets: `python lapdMouseDBUtil.py --pattern=*/*Info.md`

### Specify local download folder

After specifying the files you are interested in, select a local output folder
where to download the files to (`--localDir`). The default output
directory is the current working directory.

```sh
$ python lapdMouseDBUtil.py --pattern=m01/*Aerosol*Sub4.mha --localDir=/home/christian/data/lapdMouseDB
DB access status: available
DB query: root=m01, depth=0, pattern=*Aerosol*Sub4.mha
m01/m01_AerosolDeconvSub4.mha -> /home/christian/data/lapdMouseDB/m01/m01_AerosolDeconvSub4.mha (require download; 405.2 MB)
m01/m01_AerosolNormalizedSub4.mha -> /home/christian/data/lapdMouseDB/m01/m01_AerosolNormalizedSub4.mha (require download; 405.2 MB)
m01/m01_AerosolSub4.mha -> ./m01/home/christian/data/lapdMouseDB/m01/m01_AerosolSub4.mha (require download; 405.2 MB)
Matching files/folders: total=3(1.2 GB), downloaded=0, require download=3(1.2 GB), require update=0
```

This file selection contains 3 files with ~1.2GB of data listed as `require download`.

### Download data (or update data)

Once the file selection has been completed and a local output folder specified,
trigger the actual download (`--action=download`):

```sh
$ python lapdMouseDBUtil.py --pattern=m01/*Aerosol[S\.]*.mha --localDir=/home/christian/data/lapdMouseDB --operation=download
DB access status: available
DB query: root=m01, depth=0, pattern=*Aerosol[S.]*.mha
m01/m01_Aerosol.mha -> /home/christian/data/lapdMouseDB/m01/m01_Aerosol.mha (require download; 25.4 GB)
  Downloading ...[DONE] time: 02:31(mm:ss)
m01/m01_AerosolSub2.mha -> /home/christian/data/lapdMouseDB/m01/m01_AerosolSub2.mha (require download; 3.2 GB)
  Downloading ...[DONE] time: 00:21(mm:ss)
m01/m01_AerosolSub4.mha -> /home/christian/data/lapdMouseDB/m01/m01_AerosolSub4.mha (require download; 405.2 MB)
  Downloading ...[DONE] time: 00:03(mm:ss)
Download statistics: files=3(28.9 GB), time=00:02:56(hh:mm:ss), speed=167.8 MB/second
```

The download of the requested 3 files with 28.9 GB took ~3 minutes. To update
files that have been previously downloaded but have a newer version on the
server, use `--action=update`.

### Verify download status

After download, querying the data archive for the same files again, will list the
files with their updated download status (`downloaded` instead of `require
download`):

```sh
$ python lapdMouseDBUtil.py --pattern=m01/*Aerosol*.mha --localDir=/home/christian/data/lapdMouseDB
DB access status: available
DB query: root=m01, depth=0, pattern=*Aerosol*.mha
m01/m01_Aerosol.mha -> /home/christian/data/lapdMouseDB/m01/m01_Aerosol.mha (downloaded; 25.4 GB)
m01/m01_AerosolSub2.mha -> /home/christian/data/lapdMouseDB/m01/m01_AerosolSub2.mha (downloaded; 3.2 GB)
m01/m01_AerosolSub4.mha -> /home/christian/data/lapdMouseDB/m01/m01_AerosolSub4.mha (downloaded; 405.2 MB)
Matching files/folders: total=3(28.9 GB), downloaded=3(28.9 GB), require download=0, require update=0
```

## License

**lapdMouseDBUtil** is distributed under [3-clause BSD license](LICENSE.txt).

## Acknowledgements

This work was supported in part by NIH project R01ES023863.

## Support

For support and further information please visit the
"Lung anatomy + particle deposition (lapd) mouse archive"
at <https://doi.org/10.25820/9arg-9w56>.

