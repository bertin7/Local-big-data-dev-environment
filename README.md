# Local-big-data-dev-environment
This example shows how with a Windows machine, we can set-up a local big data development environment(Kafka-zookeeper, spark-zeppelin-hdfs, mongodb) with: Virtualbox, Vagrant and Docker-compose. Then run as a test a K-means clustering algorithm(Spark-Zeppelin) on a Uber dataset.

## Set up the environment
* Install <a href="https://www.virtualbox.org/">Virtualbox</a>.
* Install Vagrant <a href="https://www.vagrantup.com/">Vagrant</a>.
* Download the project directory
* Open terminal into the project directory and type : `vagrant up <machine-name>`. In this case master or node1..
  - The Vagrantfile is just a sample, you can rename, delete, change or add configurations to your machines, but pay attention to the  `subconfig.vm.provider "virtualbox" do |vb|
    vb.memory = "4426"
 end"`
 configuration. The machine must run at least with 4GB RAM, 8GB RAM is recommended.
* Connect to the vagrant machine `vagrant ssh master`.
* Install <a href="https://docs.docker.com/install/linux/centos/">Docker</a>, in this case the vagrant box is a Centos machine.
* Install Docker-compose:
  - `sudo yum install epel-release`
  - `sudo yum install -y python-pip`
  - `sudo pip install docker-compose`
* Into the vagrant machine, create the directories for docker volumes (See the docker-compose file).
  - `sudo mkdir -m 0777 /home/vagrant/kafka_data`
  - `sudo mkdir -m 0777 /home/vagrant/mongodb_data`
  - `sudo mkdir -m 0777 /home/vagrant/zeppelin_data`
  - `sudo mkdir -m 0777 /home/vagrant/notebooks`
* Your local project directory must be mounted as a "shared folder" and accessible from the vagrant machine into the "/vagrant" directory.
* Move to this directory `cd /vagrant` and launch docker-compose `docker-compose up`.
  - This must take some time.
  - Zeppelin will be running at `http://${VAGRANT_MACHINE-DOCKER_HOST}:8080`.
  - Kafka will be running at `http://${VAGRANT_MACHINE-DOCKER_HOST}:3030`. from this page you will be able to see the zookeeper port "2181" and the broker port "9092"
  - Mongodb will reachable on port 27017.
  
## Run K-means clustering with Spark and Zeppelin
* Download data: <a href="https://www.kaggle.com/fivethirtyeight/uber-pickups-in-new-york-city/data">Uber dataset</a>.
  - Consider loading the dataset into the spark-zeppelin container see the "docker cp" command.
* Upload the ./notebook/Uber_clustering.json file and run the cells.
  - Before producing to kafka or consuming from kafka consider adding the right .jar files into the container, under the directory "/usr/spark-2.2.0/jars".
