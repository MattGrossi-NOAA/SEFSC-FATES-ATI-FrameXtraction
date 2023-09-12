# imageXtraction
 Simple tool for extracting frames from stereo videos and saving as images

## Overview
This script a database spreadsheet generated by stereo video annotators an extracts the frame number corresponding to each annotation in each stereo video. Currently, frames are expected to be in columns named ```FrameLeft``` and ```FrameRight``` for left- and right- channel video files listed in columns named ```FilenameLeft``` and ```FilenameRight```, respectively. For example, if annotation *x* occurs in frame 45 in FilenameLeft and frame 50 in FilenameRight, then frame 45 will be extracted from FilenameLeft and frame 50 extracted from FilenameRight. Optionally, a window can be passed to extract a specified number of frames on each side of the specified frame number -- see below for details. Images are written as jpg files following the naming convention:

```
videoFileName_frameN.jpg
```

where N is the zero-padded frame number extracted from video ```videoFileName```.

## Usage

The preferred method of usage is to run the script within its [Docker container](https://www.docker.com/). You will need to [install Docker](https://www.docker.com/) or [Docker Desktop](https://www.docker.com/products/docker-desktop/) (recommended) locally for this to work. The container includes the following components:
1. **frameXtract.py**: Python script
2. **requirements.txt**: Python library dependencies
3. **Dockerfile** and **docker-compose.yml**: Docker container configuration files

### Method 1: Docker container from GitHub (preferred)
1. Install [GitHub Desktop](https://desktop.github.com/) or [GitHub Command Line Interface (CLI)](https://cli.github.com/).
2. [Clone this GitHub repository](https://docs.github.com/en/repositories/creating-and-managing-repositories/cloning-a-repository). Alternatively, download the files listed above from the repository, either separately or as a .zip package, ensuring that they are all co-located in the same directory.
   
    a. By default, this program, the database file, and video files are all expected to be in the same directory. In this case, either download the container files into the same directory as the database and video files, or download the container files into a new directory and then copy or move the database and video files into that directory.
   
    b. If the database file and video files are in separate directories, the ```docker-compose.yml``` file needs to be modified as follows:
      *  Under the 'volumes' heading, replace the "." before the colon (:) for the annotations directory with the full local directory of the database file. For example, if the database file is on a local Windows desktop, the new entry should read:
      ```
      volumes:
        - .:/home/app
        - .:/home/app/videos
        - C:\Users\user.name\Desktop:/home/app/annotations
      ```
      * Replace the "." before the colon (:) for the videos directory with the full local directory containing the video files. For example, if the video files are in a designated directory within Documents, the new entry should read:
      ```
      volumes:
        - .:/home/app
        - C:\Users\user.name\Documents\video:/home/app/videos
        - C:\Users\user.name\Desktop:/home/app/annotations
      ```
    Some things to note:
    1. Docker is platform-agnostic. Mac OS or Linux directory chains can be used here as well.
    2. One can set either the videos directory or the annotations directory or both. Any combination will work. The leading period (.) means "here" and is used to specify the current directory. Thus, if either the videos or the database annotations file are located in the same directory as the program, the volume mapping should retain the default ".".
    3. The first path (```.:/home/app```) should not be altered unless you understand Docker and know what you are doing. Same with the directory chains *after* the colons.
    4. This part of ```docker-compose.yml``` maps local directories to independent directories inside the container. The container will only be able to see the contents of local directories mounted here. If you run into "file not found" errors, look here first.
  
 3. Open a Command Prompt (Windows) or Terminal (Mac) and navigate to the directory containing this program.
 4. Build the Docker container:
    ```shell
    docker-compose build
    ```
 6. To execute, run
    ```shell
    docker-compose run framextract -f databaseFilename.ext
    ```
    where ```databaseFilename.ext``` is the name of the annotations database file. (Do not pass the full directory chain; just the file name.) Additional options are described below.

### Method 2: As a stand-alone Python script
1. [Clone this GitHub repository](https://docs.github.com/en/repositories/creating-and-managing-repositories/cloning-a-repository). Alternatively, download the script file ```frameXtract.py``` and the package dpendency file ```requirements.txt''' from the repository.
2. Download and install [Python]{https://www.python.org/downloads/} if needed. This program was written in Python 3.11.
3. **Highly recommended:** Create a virtual environment and install the package dependencies in ```requirements.txt```.
4. Execute by passing a *full directory path* for the database annotation file to "-f" or "--file". This will tell the script that it is being run stand-alone instead of within a container:
   ```shell
   python framextract.py -f full/path/to/databaseFilename.ext
   ```
6. 
    
If no separator (delimiter) is provided at execution, the script tries to determine the separator from the file extention (e.g., "tsv"==tab-delimited, "csv"==comma-separated) or let the Python parsing engine try to automatically determine it.

This scripts runs in a [Docker container](https://www.docker.com/).  

## To use:
1. [Clone this repository](https://docs.github.com/en/repositories/creating-and-managing-repositories/cloning-a-repository) or download the files listed above.
