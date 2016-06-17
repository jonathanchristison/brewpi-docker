# brewpi-docker
BrewPi Docker TEST repository

This is where we're working on dockerizing BrewPi. These files are constantly under construction, so please don't hesitate to report any issues you encounter!

The containers in this diretory will only run on an x86 or x64 based system, NOT a Raspberry Pi.

In the spirit of Docker, each service (and data store) has been broken out into its own container. As such, running BrewPi will require you to run 4 containers. To simplify this, we have constructed a docker-compose.yml file. To launch a BrewPi instance, use the docker-compose.yml file for your architecture (cd into the x86_64 directory if you're using a 32- or 64-bit system like Ubuntu or Debian), and then run:

*x86_64*
```
docker-compose --x-networking up -d
```

## Description
#### brewpi
- This image contains a configured version of the current brewpi-script repository. Basically, this is the guts of BrewPi.

#### brewpi_nginx
- This image contains an nginx web server configured to run BrewPi. The current web interface files from brewpi-www are loaded.

#### brewpi_fpm
- This image is an instance of php-fpm, configured to run with brewpi_nginx in support of the web interface. 

#### brewpi_data
- This is a data volume container dedicated to housing the user-mutable content- namely, /home/brewpi/data and /home/brewpi/settings in the brewpi container. This is a temporary solution; once docker 1.10 is released a better named volume implementation will be available and will make this less cumbersome, easier for a user to access their data from the host machine, and harder for a user to accidentally delete their data. 

## How to find your datafiles
If you need access to your data files from the brewpi_data container, from the host run
```
docker inspect data|grep -A 1 Source
```
It will return something similar to
```
"Source": "/var/lib/docker/volumes/0f05f286ef4f8b15b28914d497bbc53a4168393f7d34be74a6e56de3e5c1ce1e/_data",
"Destination": "/home/brewpi/settings",
"Source": "/var/lib/docker/volumes/073223c0560f372c87e02daaafa959170cbb86ad662f8e7409f2d55bd44abc43/_data",
"Destination": "/home/brewpi/data",
```
Copy the Source path for the Destination you are interested in, and cd to that path. Your data is stored there on the host for you to do as you wish!
