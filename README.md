# SFS Data Scraper

## Disclaimer: the data is noisy - do not use to train a production model

## Description

This is a set of scripts that allows for an automatic collection of _tens of thousands_ of images for the following (loosely defined) categories to be later used for training an image classifier:
- `drawings` - safe for work drawings (including anime)

Here is what each script (located under `scripts` directory) does:
- `1_get_urls.sh` - iterates through text files under `scripts/source_urls` downloading URLs of images for each of the 5 categories above. The [Ripme](https://github.com/RipMeApp/ripme) application performs all the heavy lifting. The source URLs are mostly links to various subreddits, but could be any website that Ripme supports.
*Note*: I already ran this script for you, and its outputs are located in `raw_data` directory. No need to rerun unless you edit files under `scripts/source_urls`.
- `2_download_from_urls.sh` - downloads actual images for urls found in text files in `raw_data` directory.
- `3_optional_download_drawings.sh` - (optional) script that downloads SFW anime images from the [Danbooru2018](https://www.gwern.net/Danbooru2018) database.
- `4_optional_download_neutral.sh` - (optional) script that downloads SFW neutral images from the [Caltech256](http://www.vision.caltech.edu/Image_Datasets/Caltech256/) dataset
- `5_create_train.sh` - creates `data/train` directory and copy all `*.jpg` and `*.jpeg` files into it from `raw_data`. Also removes corrupted images.
- `6_create_test.sh` - creates `data/test` directory and moves `N=2000` random files for each class from `data/train` to `data/test` (change this number inside the script if you need a different train/test split). Alternatively, you can run it multiple times, each time it will move `N` images for each class from `data/train` to `data/test`.

## Prerequisites

- Java runtime environment:
   - debian and ubuntu:`sudo apt-get install default-jre`
- Linux command line tools: `wget`, `convert` (`imagemagick` suite of tools), `rsync`, `shuf`

### On Windows
- option 1: download a linux distro from windows 10 store and run the scripts there

- option 2
   - download and install git from [here](https://git-scm.com/download/win). Git also installs Bash on your pc
   - download and install wget from [here](http://gnuwin32.sourceforge.net/packages/wget.htm) and add it to PATH
   - run the scripts

### On Mac
The only difference I encountered is that OS X does not have `shuf` command, but has `gshuf` instead that can be installed with `brew install coreutils`.
After installation either create an alias from `gshuf` to `shuf` or rename `shuf` to `gshuf` in `6_create_test.sh`.

## How to run
Change working directory to `scripts` and execute each script in the sequence indicated by the number in the file name, e.g.:
```bash
$ bash 1_get_urls.sh # has already been run
$ find ../raw_data -name "urls_*.txt" -exec sh -c "echo Number of URLs in {}: ; cat {} | wc -l" \;
Number of URLs in ../raw_data/drawings/urls_drawings.txt:
   25732
$ bash 3_optional_download_drawings.sh # optional
```
