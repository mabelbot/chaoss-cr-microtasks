# Microtask 0:
Download and Install Augur and Grimoirelab

## Augur
Download and configure Augur, creating a dev environment using the general cautions noted here:
https://oss-augur.readthedocs.io/en/dev/getting-started/installation.html and the full documentation here:
https://oss-augur.readthedocs.io/en/dev/development-guide/toc.html

- Installation of Augur completed following both quickstart & getting started. Here are just the basic steps done with details omitted for length. 
- Observed logs with the verbose mode.
- Setup done on both OSX and cloud compute instance (Ubuntu 20.04)


Part 1: Quickstart setup on Ubuntu 20.04
1. I was able to install PostgreSQL and create Augur Database and User, configure Git, install Go and the correct version of Python3.
The instructions here to configure the virtual env and install Augur ended up working for me
```
# make sure you are logged in as your own user (i.e. "sean") [note: for Ubuntu 20.04 cloud it was not needed to do an extra login step]
git clone https://github.com/chaoss/augur.git
cd augur/
sudo apt install make
sudo apt-get install python3-venv
python3 -m venv $HOME/.virtualenvs/augur_env
source $HOME/.virtualenvs/augur_env/bin/activate
sudo apt install python-pip-whl
sudo apt install python3-pip
sudo apt install pythonpy
python -m pip install --upgrade pip
make install-dev {Follow prompts. You will need database credentials, a file location for cloned repositories, a GitHub Token, and a GitLab token.}
```
2. I chose option `2` fro the database, with the credentials setup in #1
3. `localhost` and `5432` worked for the compute instance
4. For Facade data collection worker, I chose `1` and put in `/home/ubuntu/augur`
5. Yes to front end dependencies. I had an error `pkg_resources.DistributionNotFound: The 'gyp==0.1' distribution was not found and is required by the application` which did not appear on the local installation, and it worked after following this https://github.com/nodejs/node-gyp/issues/2273.
6. Select "No" for Augur API Key.

Part 2: For Development, setup on Mac OSX Big Sur 11.6
1. Using https://oss-augur.readthedocs.io/en/main/getting-started/toc.html 
2. Installed Postgres.app and configure PATH to use `psql` later
3. I also used my PSequel visualizer for visualizing the data 
4. Downgrading Python Verison 3.9 → 3.8 by using the brew tutorial here but just the first command: https://exerror.com/how-to-downgrade-python-version-from-3-9-to-3-8/
5. Install Go for Mac OS
6. Install `scc`
7. Install OpenMP
8. Install Node and npm
9. Install Vue.js and vue-cli
10. Reinstall geckodriver 
11. Clone augur, create and activate venv
12. Run make install-dev, following options same as before in Part 1. 
<img width="1256" alt="Screen Shot 2022-04-17 at 5 48 02 PM" src="https://user-images.githubusercontent.com/70232089/163738734-4e7447a1-95e2-4124-b7b8-03bf06a9a331.png">

Note: I had an interest in looking at what was inside contributor_repo to track contributor activity across Open Source for fun. So I set the Contributor Breadth Worker to collect data on the next startup, after the other workers had already collected their data.
An example of the data found:
<img width="1363" alt="Screen Shot 2022-04-17 at 5 53 07 PM" src="https://user-images.githubusercontent.com/70232089/163738863-0ab2eba8-520f-4cd0-995a-d9be58eab725.png">


## Grimoirelab
https://chaoss.github.io/grimoirelab-tutorial/

Installation of Grimoirelab was done in two parts.

Part 1: **docker-compose** setup
1. The first method was to follow the [setup instructions](https://chaoss.github.io/grimoirelab-tutorial/docs/getting-started/setup/) on the GrimoireLab tutorial (linked above), meaning set up with **docker-compose**. Checking for the software requirements was a bit different done on OSX.
2. This part was used to explore the general capabilities of the GrimoireLab Dashboard: https://chaoss.github.io/grimoirelab-tutorial/sirmordred/dashboard/


Part 2: Development setup
1. Development setup done by setting up SirMordred, for which I used instructions on this Getting Started file: https://github.com/chaoss/grimoirelab-sirmordred/blob/master/Getting-Started.md. Setup is again undergone on OSX. 
2. First I will need to install the correct ElasticSearch, Kibiter and MySQL versions. I already had ElasticSearch 6.8.23 from before, which I had obtained by the following process. 
```
brew tap elastic/tap 
brew search elasticsearch
brew install elasticsearch@6
```
3. I used the docker-compose without SearchGuard instructions to perform the installation. I already had docker-compose since I have Docker Desktop for Mac. The instructions I followed are here: https://github.com/chaoss/grimoirelab-sirmordred/blob/master/Getting-Started.md#docker-compose-without-searchguard-. Note that the MariaDB service will not start if port 3306 is in use, as was in my case; in this case it may be due to MySQL running, which can be killed via the following:
```
sudo pkill mysql
```
4. Create the virtualenv. I will create grimoirelab virtualenv. (`source grimoirelab/bin/activate`). 
5. Inside this virtualenv, I will install 
```
python3 -m pip install PyGitHub GitPython
```
then run the script
```
$ python3 glab-dev-env-setup.py --create --token xxxx --source sources
```
6. The script was successful. 
7. Now I will install PyCharm. I checked Sha256 checksum `shasum -a 256 [file]`. Pycharm Community 2021.3. 
8. Create a project in the grimoirelab-sirmordred directory. Create project from existing sources was selected. Project interpreter is Python 3.8 (may need to change it later, tbd). 
9. The tutorial to install the packages did not work verbatim, possibly because of different PyCharm versions. I was able to access it using `command` + `,`. 
10. I installed elasticsearch version 6.3.1 (as shown in the tutorial gif).
11. I then installed elasticsearch-dsl version 6.3.1.
12. I then installed all the requirements found in each requirements.txt "excluding the ones concerning the grimoirelab components.", but I used pip instead on a txt file I created with all the requirements. It was to avoid manual work, but we will see if it works. The environment comes with pip, setuptools, and wheel. There were some conflicts with the requirements found in each requirements.txt (e.g. httpretty, I chose >=0.9.6 arbitrarily)
13. Panel can be closed and OK selected after these steps. [Screen Shot 2022-04-04 at 6.37.54 PM] is how my interpreter settings looked like after finishing this step.
14. Then, install the dependencies to the grimoirelab components. For this, I also followed the tutorial in the gif.
15. It still prompted me that package requirements numpy<=1.18.3 scipy and grimoirelab-elk are not installed. 
16. Issue: [Errno 2] No such file or directory: '../aliases.json'. I found one but I had to change it to ../../aliases.json after observing my file structure doesn't match the getting started. 
17. Issue: Retrying (Retry(total=20, connect=21, read=8, redirect=5, status=None)) after connection broken by 'SSLError(SSLError(1, '[SSL: WRONG_VERSION_NUMBER] wrong version number (_ssl.c:1108)'))': /
    - Luckily the issue seems to be documented. Both occurrences of https are changed to http. 
18. The following is the result of running `micro.py` in PyCharm. I have verified that the data has correctly been loaded into the Elasticsearch instance inside Docker Container.
<img width="1446" alt="Screen Shot 2022-04-17 at 5 27 03 PM" src="https://user-images.githubusercontent.com/70232089/163737983-c5195a55-1839-4515-9e83-fe339b9bb594.png">


