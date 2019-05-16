## M220P - Course as provided by MongoDB university</h2>

MongoDB for python developers as per the course from MongoDB university - Course Starting 14 May 2019

Please go to [MongoDB University](https://university.mongodb.com/) for all the courses they provide. 

The course was performed in a Windows environment using python virtualenv in order to work around proxy settings regarding Anaconda usage in a company environment.

If you are doing the course, the below will help you set up your environment to perform the course tickets.

If not, the project may be cloned in entirety and the user will only need to ensure that that python3, pip, and virtualenv are installed, an Atlas cluster is set up and the .ini file to connect to the Atlas cluster is changed accordingly to match your cluster url. 

The below assumes that the user has Python3 and pip installed and has the python command set to an environment variable in Windows.

It also assumes some knowledge of MongoDB and that the user has the mongo shell installed to run the mongo command. The course notably also does however assume that the Atlas cluster will be utilised rather than a local MongoDB server instance.

The course README is thorough however some small tips from the below may assist future students with the setup of the environment, these are bolded.

### The only files to alter
- several files in this project should not be altered so as to prevent breaking changes
- mflix/db.py and dotini_windows will require changes

### Setting up virtualenv for the project
- download the handout provided in Chapter 0
- extract to a directory of your choosing naming the folder mflix-python
- open the command prompt
- navigate to the mflix-python folder
- in the command prompt using pip, install virtualenv
  - ``` -pip3 install virtualenv ```
- create the virtual environment, python refers to your environment variable set to python, mflix_venv will be the name of the environment housed in the mflix-python folder
  - ``` virtualenv -p python mflix_venv ```
- **connect to mflix_venv in windows cmd**
  - your\path\to\mflix-python> ```mflix_venv\Scripts\activate ```
- the command prompt will change to show you are in the virtual environment
  - (mflix_venv) your\path\to\mflix-python>
- **within this area you can install the requirements for the project using pip3 and requirements.txt**
  - **NB** use pip3 for installation of requirements, errors occurred when using pip as was also noted by other students 
  - (mflix_venv) your\path\to\mflix-python> ``` pip3 install -r requirements.txt ```
- once requirements are installed, deactivate the virtual environment
  - your\path\to\mflix-python> ```deactivate ```

### Set up an Atlas cluster to use for the project
- the course readme has clear instructions on this and can be found in this repository, named README.rst
- you will need to creat an Atlas account and set up a cluster on the **free tier**
- if you already have an Atlas account from previous courses you can use this and simply add another project called M220P and set up a cluster under this called mflix
  - under context at the top left click **new project**
  - give the project a name eg M220P
  - create a cluster under the free tier and call it mflix
- once created go to the project and whitelist all IP's (not recommended beyond the scope of this course work)
  - Security
  - IP Whitelist
  - Whitelist entry 0.0.0.0/0
  - Confirm
- create a user on the cluster
  - MongoDB users
  - username: m220student
  - password: m220password
  - read and write to any database
  - confirm
- you are now ready to test that you can connect to you Atlas cluster via the mongo shell **(not required)**
  - in Atlas, at your project and cluster, click connect
  - for this specific course choose the **connect your application** link (choosing connect using mongo shell resulted in authetication failure)
  - copy the connection string
  - in command prompt type in ``` mongo your connection string ``` but be sure to change <PASSWORD> to m220password
  - check that the server does in fact connect
  - the cluster is thus most likely set up correctly to be used by the project in a more intuitive manner thanks to the course developers

### Running the project
Now that you have a **working virtualenv (currently deactivated) and working Atlas cluster (not currently connected in the mongo shell)** you're close to running the project.

- firstly, import some the data provided to your cluster but activate the virtual environment first (importing data outside the virtual errors resulted in running into search query errors later)
  - your\path\to\mflix-python> ```mflix_venv\Scripts\activate ```
  - (mflix_venv) your\path\to\mflix-python>```mongorestore --drop --gzip --uri mongodb+srv://m220student:m220password@<YOUR_CLUSTER_URI> data```
- in Windows, using your favourite text editor, locate your mflix-python directory and open the dotini_windows file
- change the file to link your atlas cluster info ``` MFLIX_DB_URI = mongodb+srv://m220student:m220password@YOUR_ATLAS_URL_LINK_HERE ```
- you can use the same for both test and prod
- **rename this file to ```.ini```**
- you can add a super secret key to the prod section, of your own design
- save and close this file
- in command prompt navigate to the mflix-python directory and activate the virtual environment again
  - your\path\to\mflix-python> ```mflix_venv\Scripts\activate ```
- to run the project, from the virtual env:
  - (mflix_venv) your\path\to\mflix-python> ```python run.py ```
  - in cmd you will see the following:
    - Restarting with stat
    - Debugger is active!
    - Debugger PIN: pin here
    - Running on http://127.0.0.1:5000/ (Press CTRL+C to quit)
- in your browser navigate to localhost:5000 and you should see the project
- you can leave it running here, any syntax errors or breaking changes in the db.py should stop the process and provide errors

### Following the course in Jupyter Notebooks
- your\path\to\mflix-python> ```cd notebooks```
- your\path\to\mflix-python>notebooks> ``` Jupyter notebook ```
- this will provide a URL link where you can find how the course material was created and follow along if you wish
- note however that on each section you will need to update the client link to match your cluster link, the sam link you provided in the .ini file
- you can run the queries set up by the course instructors here on your cluster and data and read through the logic as provided in the videos

### To run the unit tests when doing the course
- you can activate a new instance of the virtual environment (rather than using the one that is actively running the project)
- from the new instance you can run the relevant test
  - (mflix_venv) your\path\to\mflix-python> ``` pytest -m LAB_UNIT_TEST_NAME ```