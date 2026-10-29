# mtk-log-util
My personal mtkclient wrapper for getting bootloader or kernel logs from a mediatek device

This script extracts `expdb` partition/`pstore` memory region and extracts ASCII strings from them.

## Installing
This project has to be cloned with `--recurse-submodules` in order to fetch the [mtkclient](https://github.com/bkerler/mtkclient) repo
```shell
git clone https://github.com/chkndrp/mtk-log-util --recurse-submodules
```

There are basically no python requirements besides the ones coming from [mtkclient](https://github.com/bkerler/mtkclient/blob/main/requirements.txt)
```shell
# requirements.txt is a symlink to mtkclient/...
pip install -r requirements.txt
```

If you haven't already, i recommend following the [mtkclient setup instructions](https://github.com/bkerler/mtkclient/blob/main/README-INSTALL.md) first, otherwise you won't get far with this project

## Usage
```
mtk-log-util - mtkclient wrapper for getting bootloader or kernel logs from a mediatek device

positional arguments:
  {expdb,pstore}        Choose extraction method: expdb (partition) or pstore (memory)
  filename              Output file to save ASCII strings

options:
  -h, --help            show this help message and exit
  --mtkclient-args MTKCLIENT_ARGS
                        Extra mtkclient arguments (default: "")
  --pstore-address PSTORE_ADDRESS
                        Pstore memory address (default: 0x48090000)
  --pstore-size PSTORE_SIZE
                        Pstore memory size (default: 0xe0000)
  --auto-detect-pstore  Auto-detect pstore address and size from expdb partition (pstore only)
  --dont-peek-via-da    Don't do a peek via Download Agent (use in case of issues) (pstore only)
```

## Basic Examples
Extracting `pstore` memory region. Will work only if DRAM was not cleared (eg. after panic)
```shell
python logutil.py pstore
```

Extracting `preloader`/`tee`/`lk` logs. Will work only if the partition exists
```shell
python logutil.py expdb
```

## Notes
This script does not take `pstore` compression into account, 
and might result in the first half of the data being corrupted if enabled.

## Requirements
- Python 3.10 or newer
- Cloning with `--recurse-submodules` to fetch [mtkclient](https://github.com/bkerler/mtkclient) repo
- Installed [mtkclient requirements](https://github.com/bkerler/mtkclient/blob/main/requirements.txt) 
and followed the [setup instructions](https://github.com/bkerler/mtkclient/blob/main/README-INSTALL.md)
