 
## Sphinx Prequisites
1. Install IDE on machine e.g., Visual Studio Code, PyCharm, etc.  

2. Install Python
   Ref:  https://www.python.org/downloads/

3. Create project folder and create Python virtual environment in it(Best practice).
   $ python3  –m venv demo-env
   Ref:  https://docs.python.org/3/tutorial/venv.html#creating-virtual-environments

4. Install Sphinx in virtual environment
   $ pip install –U sphinx
   Ref: https://www.sphinx-doc.org/en/master/usage/installation.html#installation-from-pypi

5. Install required Sphinx theme e.g., sphinx_rtd_theme
   $ sudo pip install sphinx_rtd_theme
   Update the name of required theme in the conf.py file.

6. To activate the created virtual environment, go to the project folder and run command:
   $ source demo-env /bin/activate 

7. To start with Sphinx document creation, run this command:
   $ sphinx-quickstart
   This sets up source directory and default conf.py file

## Directory Details

Clone the UWC Sphinx documentation project from Git and import that project in to the IDE that you are using e.g., Visual Studio code, PyCharm.
Make sure the cloned folders will be in the same folder where you have installed all the prequisites.

After cloning the UWC Sphinx documentation project from Git, the following folders will be available:
 
* source: This folder contains all the .rst files that we want to convert into HTML format to view the documentation.
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

* Use uniform underlines for headings in all files and keep track of indentations, whitespace and blank lines in the RST file.

* To build the project after adding new files, images or even after updating the content, use following command:
 
   ```sh
     $ sphinx-build -t html source build
   ```



