 
## Sphinx Prequisites
Install the following packages:

```sh
$ sudo apt install build-essential python3 python3-pip doxygen
```

Then, install the Python dependencies with the following command:

```sh
$cd uwc-docs
$ sudo -H -E pip3 install -r requirements.txt
```

## Directory Details
 
* source: This folder contains all  the .rst files that we want to convert into HTML format to view the documentation.
  This directory also contains the default conf.py file with the most useful configuration values.
  We can update this conf.py file to fit our requirements.

* Pages: This folder contains all the .rst files of the document that has to be converted. 
  After updating the content in any .rst file, we need to build the project to see the updated output.
  When we add a new .rst file, make sure to mention the path of file in the content of the .. toctree:: directive in the index.rst file.

* Doc_Images: This folder is available in the source directory. The Doc_Images folder containes all the images and tables that are used
  in the UWC documentation. To add new images or tables, keep the .png or .jpg files of the images or tables in the Doc_Images folder and 
  then add this image using the proper syntax in the .rst file wherever required.

* docs: This is the output folder. This folder containes all the created html files after building the project.
  To see the created documentation, open the index.html file.

* requirements.txt: Has the python packages to be "pip" installed.  

* Use uniform underlines for headings in all files and keep track of indentations, whitespace and blank lines in the RST file.


## Sphinx build steps

Make the necessary changes to the RST files at "source/Pages/*.rst" & run the makefile which builds the sphinx documentation locally in "build" directory and copies the necessary files into "docs/" directory. POst the successful build it hosts the documentation 
on the same machine at port "8652", which can be access at http://localhost:8652

This is for local previewing the changes before checking in the code. Once the preview is done, we need to do git add, commit & push the changes in a PR (Pull Request).

```sh
cd uwc-docs
make html
```
While checking in these files back to remote git repo, make sure to git add all the files in untracked section but not the "build" directory as it's generated at runtime & doesn't need to be checked in.
