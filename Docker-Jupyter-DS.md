# Option 1: Docker Package 
### 1.1 Run without enforcing token
The -i tells docker to attach stdin to the container, the -t tells docker to give us a pseudo-terminal, /bin/bash will run a terminal process in your container, -d runs in detached mode
```
sudo docker run -it -d -p 8888:8888 jupyter/datascience-notebook start-notebook.sh --NotebookApp.token='' /bin/bash
```
### 1.2 Start terminal in Jupyter, download graphlab to local and install 
Nb. download link below should have email and token inserted (can be obtained from https://turi.com/download/academic.html)
```
wget -P $HOME https://get.graphlab.com/GraphLab-Create/2.1/<email>/<token>/GraphLab-Create-License.tar.gz
pip install --upgrade --no-cache-dir GraphLab-Create-License.tar.gz
```
### 1.3 Switch back into Jupyter notebook and attempt graphlab import 
Active product key should be output in Jupyter is graphlabimport  is successful 
```
import graphlab
graphlab.product_key.set_product_key("<graphlab token>")
graphlab.set_runtime_config('GRAPHLAB_DEFAULT_NUM_PYLAMBDA_WORKERS', 4)
graphlab.product_key.get_product_key()
```

### 1.4 Run image in detached mode and without enforcing token
```
sudo docker run -d -p 8888:8888 jupyter/datascience-notebook start-notebook.sh --NotebookApp.token=''
```

#
# Option 2 Install via Anaconda
- attempt based on instructions from Uni of Washington's ML course,  this approach errors on graphlab install, is not dockerised and contains runs on pretty old versions of python/etc.
### 2.1 Download Anaconda
```
wget https://repo.continuum.io/archive/Anaconda2-4.0.0-Linux-x86_64.sh
```

### 2.2 Compare hash
compared to https://docs.anaconda.com/anaconda/install/hashes/Anaconda2-4.0.0-Linux-x86_64.sh-hash
```
sha256sum Anaconda2-4.0.0-Linux-x86_64.sh
```

### 2.3 Install to /opt/anaconda2 
(nb. Anaconda 5 available as at 16/3 but maintaining compatibility with graphlab for first install)
```
sudo bash Anaconda2-4.0.0-Linux-x86_64.sh
```

### 2.4 Select option to prepend installation path to bash profile
```
source ~/.bashrc
```

### 2.5 Verify install
```
conda list
```

### 2.6 Revert to turi installation steps
link: https://turi.com/download/install-graphlab-create-command-line.html

#### 2.6.1 Create a new conda environment with Python 2.7.x
```
conda create -n gl-env python=2.7 anaconda=4.0.0
```

#### 2.6.2 Activate the conda environment
```
source activate gl-env
```

#### 2.6.3 Ensure pip is updated to the latest version

#### 2.6.4 Miniconda users may need to install pip first, using 'conda install pip'
```
conda update pip
```

### 2.7 Install graphlab using registered key
Errors on license validation.. seems to be poorly maintained post graphlab sale to Apple
```
pip install --upgrade --no-cache-dir https://get.graphlab.com/GraphLab-Create/2.1/<email>/<productkey>/GraphLab-Create-License.tar=.gz
```

### 2.8 Uninstall
```
conda uninstall -n gl-env python anaconda
```
