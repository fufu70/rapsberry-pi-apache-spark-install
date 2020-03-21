# rapsberry-pi-apache-spark-install

Below is a guide on installing the apache spark environment.

Ensure that everything is updated and that java is installed
```
sudo apt-get update
sudo apt-get upgrade
sudo apt update
sudo apt install default-jdk
```

Is our firmware updated as well? If you do this then you must restart your raspberry pi as well.
```
sudo rpi-update
# uname -a
## Output might be something like: "Linux raspberrypi 4.9.80-v7+ #1098 SMP Fri Mar 9 19:11:42 GMT 2018 armv7l GNU/Linux"
# sudo shutdown -r now
## SSH back in to the terminal to check if the update was put through
# uname -a
## Output might be something like: "Linux raspberrypi 4.19.108-v7+ #1298 SMP Fri Mar 6 18:04:35 GMT 2020 armv7l GNU/Linux"
```

Next we want to configure our users for apache spark

```
sudo groupadd -g 5000 spark
sudo useradd -g 5000 -u  5000 -m spark
sudo passwd spark #password was set to "spark"
```

And we want to configure a ssh key for the spark user so that passwords don't need to be utilized
```
ssh-keygen -C "pi" -b 2048 -t rsa
## Copy the keygens to your other hosts
# ssh-copy-id pi@192.168.88.217
# ssh-copy-id pi@192.168.88.218
```

## Installing Spark

Download the version of spark and install it:
```
wget http://d3kbcqa49mib13.cloudfront.net/spark-1.4.0-bin-hadoop2.6.tgz
sudo tar -zxf spark-1.4.0-bin-hadoop2.6.tgz -C /opt/
sudo ln -s /opt/spark-1.4.0-bin-hadoop2.6 /opt/spark
sudo chown -R pi:pi /opt/spark-1.4.0-bin-hadoop2.6
```

Configure master environment for running by adding these lines into your `/opt/spark/conf/spark-env.sh` file
```
SPARK_MASTER_IP=192.168.88.215
SPARK_WORKER_MEMORY=768m
```

Also create a file that contains a list of all the slave ip addresses, name the file `/opt/spark/conf/slaves`. My file looks like this:
```
192.168.88.217
```

Now simply copy over the configurations for apache spark on to the slave nodes and ensure that all installations are propogated as well.

```
for i in `cat /opt/spark/conf/slaves`
do 
    scp /opt/spark/conf/slaves $i:/opt/spark/conf/slaves
done

for i in `cat /opt/spark/conf/slaves`
do 
    scp /opt/spark/conf/spark-env.sh $i:/opt/spark/conf/spark-env.sh
done
```

## Starting Apache Spark

Once everything is configured correctly we want to start the master node and slave nodes:
```
sudo /opt/spark/sbin/start-master.sh
```
To start all of the slave nodes run the script below:
```
/opt/spark/sbin/start-slaves.sh 
```

