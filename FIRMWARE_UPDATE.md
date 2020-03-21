# Firmware Updates

Is your nodes firmware updated? 
Before installing your environments packages for Apache Spark ensure that you have the latest version of Raspbian on all of your nodes. You can check your version by running the command `uname -a` to see the year of your current firmware, mine is shown below:

```bash
~$ uname -a
Linux raspberrypi 4.9.80-v7+ #1098 SMP Fri Mar 9 19:11:42 GMT 2018 armv7l GNU/Linux
```

To run the firmware update run the command below. This command will provide `(y/N)?` prompts, accept them by typing `y`.

```bash
~$ sudo rpi-update
```

Once you've updated your firmware you will need to restart your node and check your firmware version to see if everything installed correctly.

```bash
~$ sudo shutdown -r now
Connection to <node-ip-address> closed by remote host.
Connection to <node-ip-address> closed.
~$ ssh pi@<node-ip-address>
~$ uname -a
Linux raspberrypi 4.19.108-v7+ #1298 SMP Fri Mar 6 18:04:35 GMT 2020 armv7l GNU/Linux
```
