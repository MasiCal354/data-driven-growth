# Data-Driven Growth Requirements Installation
This is a quickstart of installing jupyter lab on Ubuntu 18.04 Server. For more detail on installation and configuration, [Here](https://jupyter-notebook.readthedocs.io/en/stable/index.html) is the documentation of Jupyter.
### 1. Download Miniconda
``` bash
wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh
```
### 2. Install miniconda using bash command
``` bash
bash Miniconda3-latest-Linux-x86_64.sh
```
### 3. Follow Instruction (Just Yes to All)
### 4. Go to interactive bash (CTRL+D or run command below)
``` bash 
. ~/.bashrc
```
### 5. Add conda forge channel to get latest update of conda packages
``` bash
conda config --add channels conda-forge
```
### 6. Update all packages in base environment
``` bash
conda update --all -y
```
### 7. Create new conda environment
``` bash
conda create -n [environment-name]
```
for example:
``` bash
conda create -n growth -y
```
### 8. Activate newly created conda environment
``` bash
conda activate [environment-name]
```
for example:
``` bash
conda activate growth
```
### 9. Install neccessary packages for data driven growth
* Numpy support for large, multi-dimensional arrays and matrices, along with a large collection of high-level mathematical functions to operate on these arrays.
* Pandas for data manipulation, data analysis, data structures and operations for manipulating numerical tables and time series.
* TQDM for making progress bar and to track processes. 
* Google Cloud BigQuery for connecting python with data in BigQuery
* Pandas GBQ to read data in BigQuery directly to pandas data frame
* Matplotlib for plotting data.
* Seaborn for high-level interface for plotting based on matplotlib.
* Plotly for interactive plotting.
* Scikit Learn for machine learning and data mining.
* Tensorflow for machine learning framework.
* Keras for deep learning framework based on tensorflow.
* XGBoost for Extreme Gradient Boosting Optimization on machine learning model.
* Jupyter Lab for web based experimental data science and machine learning environment.

Here's the command
``` bash
conda install numpy pandas tqdm google-cloud-bigquery pandas-gbq matplotlib seaborn plotly scikit-learn tensorflow keras xgboost jupyterlab -y
```
### 10. Generate config file for jupyter lab
``` bash
jupyter notebook --generate-config
```
### 11. Generate Password for Jupyter Lab
``` bash
python
```
``` python
from notebook.auth import passwd
passwd('[your-password]')
```
Or
``` python
from notebook.auth import passwd
passwd()
```
it will prompt to set password and confirm it.
The output of above command is hashed password with SHA1.
Copy that hashed password to be pasted on jupyter lab config file.
``` python
quit()
```
### 12. Open jupyter lab config file
``` bash
[text-editor-command-call] ~/.jupyter/jupyter_notebook_config.py
```
For example:
``` bash
nano ~/.jupyter/jupyter_notebook_config.py
```
### 13. Add configuration into configuration file
Add following script to the top most line of the configuration file
``` python
c.NotebookApp.ip = '*'
c.NotebookApp.password = u'[sha1:hashed-password]'
c.NotebookApp.port = 8888
c.NotebookApp.open_browser = False
```
### 14. (Optional) Add SSL certificate to config file
- Create and access certs folder
``` bash
mkdir certs
cd certs
```
- Generate SSL Certificate (Self-Signed)
``` bash
openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout [key-file].key -out [cert-file].pem
```
Self-signed certificate is insecure or unrecognized. Try to use letsencrypt to have a fully compliant self-signed certificate that will not raise warnings. To create letsencrypt SSL certificate follow [this tutorial](https://letsencrypt.org/getting-started/).
- Follow prompt questions
- Back to main directory and open jupyter lab config file
``` bash
cd ~
nano ~/.jupyter/jupyter_notebook_config.py
```
- Add SSL certificate file to config
``` python
c.NotebookApp.certfile = '/home/[username]/certs/[cert-name].pem'
c.NotebookApp.keyfile = '/home/[username]/certs/[key-name].key'
```
### 15. Run Jupyter Lab
``` bash
jupyter lab
```
### 16. Open jupyter lab on https://[your-domain]:8888

### 17. Access without specifiying port
Unspecified port in common web server are 80 (for http) and 443 (for https), commonly both port are restricted for non-root user. We can use authbind and enabling non-root user to access port 80 or 443.
``` bash
sudo apt-get install authbind
sudo touch /etc/authbind/byport/80
sudo touch /etc/authbind/byport/443
sudo chmod 500 /etc/authbind/byport/80
sudo chmod 500 /etc/authbind/byport/443
sudo chown [username] /etc/authbind/byport/80
sudo chown [username] /etc/authbind/byport/443
```
### 18. Change port number on config file
``` bash
nano ~/.jupyter/jupyter_notebook_config.py
```

Replace
``` python
c.NotebookApp.port = 8888
```
into
``` python
c.NotebookApp.port = 80
```
or
``` python
c.NotebookApp.port = 443
```
### 19. Run jupyter lab
``` bash
authbind --deep jupyter lab
```

### 20. Open jupyter lab on http://[your-domain] or https://[your-domain]

### Notes about Dataset uses in this repo
The datasets used in this repo are credential data which accessed from BigQuery. Most of the data are already cleaned, denormalized and preprocessed using Google DataPrep in order to simplify the query processing. The credentials generated using service account on Google Cloud Platform.
