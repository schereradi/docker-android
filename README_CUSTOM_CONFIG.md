VNC pass
--------

Passing ```VNC_PASSWORD="your_pass_here"``` will secure your vnc connection. 

Proxy
-----

You can enable proxy inside container and Android emulator by passing following environment variables:

- HTTP_PROXY="http://\<docker\_bridge\_ip>:<port>"
- HTTPS_PROXY=""http://\<docker\_bridge\_ip>:<port>"
- NO_PROXY="localhost"
- ENABLE_PROXY_ON_EMULATOR=true

Proxy with authentication
----

You can set proxy with authentication by passing following environment variable:

- HTTP_PROXY_USER="\<username>"
- HTTPS_PROXY_PASSWORD="\<password>"


Language
--------

You can change the language setting of Android Emulator on the fly by passing following environment variable:

- LANGUAGE="\<language>"
- COUNTRY="\<country>"

Data partition size
-------------------

The size of the data partition can be set by passing the following environment variable:

- DATAPARTITION="\<size>"

The value can be specified in the same format that is used by the emulator config file (`disk.dataPartition.size`), e.g. `800m`.

Camera
------

Passing following environment variable to be able to connect laptop / pc camera to Android emulator:

- EMULATOR_ARGS="-camera-back webcam0"

Custom Avd name Arguments
-------------------------

Passing following environment variable to set a custom avd name

- AVD_NAME="customName"


Custom Emulator Arguments
-------------------------

If you want to add more arguments for running emulator, you can ***pass an environment variable EMULATOR_ARGS*** while running docker command.

```bash
docker run --privileged -d -p 6080:6080 -p 4723:4723 -p 5554:5554 -p 5555:5555 -e DEVICE="Samsung Galaxy S6" -e EMULATOR_ARGS="-no-snapshot-load -partition-size 512" --name android-container budtmo/docker-android-x86-8.1
```

Add entries to emulator hosts file
-------------------------

You can add other hosts into the emulator's `/etc/hosts` file by using the `ADD_HOSTS` flag. The following example adds two host to IP mapping within the emulator. This flag can be usefull when you need to connect from the emulator to an external host.

```bash
docker run --privileged -d -p 6080:6080 -p 4723:4723 -p 5554:5554 -p 5555:5555 -e DEVICE="Samsung Galaxy S6" -e ADD_HOSTS="10.0.2.2:foo.com,10.0.2.3:bar.com" --name android-container budtmo/docker-android-x86-8.1
```

Currently the `ADD_HOSTS` flag works only on Android API level 28 or lower.

Appium Test Distribution (ATD)
------------------------------

You can enable [ATD](https://github.com/AppiumTestDistribution/AppiumTestDistribution) by passing environment variable ATD=true and bind the port to the host, e.g. -p 4567:4567

SaltStack
---------

You can enable [SaltStack](https://github.com/saltstack/salt) to control running containers by passing environment variable SALT_MASTER=<ip_address_of_salt_master>.

Back & Restore
--------------

If you want to backup/reuse the avds created with furture upgrades or for replication, run the container with two extra mounts

- -v local_backup/.android:/root/.android
- -v local_backup/android_emulator:/root/android_emulator

```bash
docker run --privileged -d -p 6080:6080 -p 4723:4723 -p 5554:5554 -p 5555:5555 -v local_backup/.android:/root/.android -v local_backup/android_emulator:/root/android_emulator -e DEVICE="Nexus 5" --name android-container budtmo/docker-android-x86-8.1
```

For the first run, this will create a new avd and all the changes will be accessible in the `local_backup` directory. Now for all future runs, it will reuse the avds. Even this should work with new releases of `docker-android`

Nginx
-----

Sample nginx configuration can be found [here](nginx)
