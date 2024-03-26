# Apache-Airflow-Helm




## Install Apache Airflow on Ubuntu 

1. Installation of pip on Ubuntu
To set up a virtual environment, we need to install a python package named virtualenv

```
sudo apt install python3-pip

```

2. Installing & Setting Up a Virtual Environment
After successfully installing pip, we will now install the virtualenv package using the following command:

```
sudo pip3 install virtualenv

```

3. To create a virtual environment directory as "airflow_env" inside the "airflow_workspace" directory, execute the following command:

```
virtualenv airflow_env
```

4. To activate the environment use the following command:

```
source airflow_env/bin/activate
```

5. Next, we will install airflow and some additional libraries using the following command:

```
pip3 install apache-airflow[gcp,sentry,statsd]
```

6. Initialization of Airflow Database
Now we will go to the airflow directory and initialize the airflow database using the following commands:

```
airflow db init
```

7. Airflow Directory after the 'airflow db init' command
It is time to create a dags folder. All the future dags will be stored here and accessed by the airflow components.

```
mkdir dags
```

8. Creating a New Airflow User
To create a new user with a username as admin with Admin role, we can run the following code:

```
airflow users create --username admin --password admin1 --firstname admin --lastname admin --role Admin --email anand_rj91@yahoo.co.in
```

_______

9. Running of the Airflow Scheduler and Webserver
Now we will start the airflow scheduler using the airflow scheduler command after activating the virtual environment:

airflow scheduler
Open a new terminal, activate the virtual environment, go to the airflow directory, and start the web server.
username@desktop_name:~/airflow_workspace$ source airflow_env/bin/activate
(airflow_env) username@desktop_name:~/airflow_workspace$ cd airflow
(airflow_env) username@desktop_name:~/airflow_workspace/airflow$ airflow webserver
Once the scheduler and webserver get initialized, open any browser and go to http://localhost:8080/.
Port 8080 should be the default port for Airflow, and you see the following page:
