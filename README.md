## A Shiny web-app for visualizing trait data in BETYdb

Time series box, line plots and heatmaps for every available season

## Installation

### Easy deploy with Docker

```sh
git clone https://github.com/terraref/traitvis-webapp
cd traitvis-webapp

docker build -t shiny-traits .
## after deploying app, launch so it can be accessed by browser at localhost:3838
docker run --rm -t -i -p 3838:3838 shiny-traits
```

To set one of the environment variables (see "Setup and Notes" section), you can use the `-e` flag, for example to connect to the TERRA REF trait database (bety6), run:

```sh
docker run --rm -t -i -e bety_host=bety6 -p 3838:3838 shiny-traits
```

To enter the container bash shell as root, e.g. for development, testing, reviewing logs:

```sh
docker run --rm -t -i shiny-traits /bin/bash
```

### Database Connection

This application requires a connection to a BETYdb database. It requires a BETYdb database that uses the experiments table to associate time ranges and sites or plots with specific experiments or seasons.

Connection parameters can be set as environment variables. By default they are:

```
bety_dbname=bety
bety_password=bety
bety_host=localhost
bety_port=5432
bety_user=bety
```

There are three methods that you can use access to the TERRAREF instance of BETYdb:

1. If running on the NCSA Nebula cluster within the terraref domain, can be accessed by setting the environment variable `bety_host=bety6`
    1. This can be done within the NDS Workbench at terraref.ndslabs.org
2. Otherwise, use an ssh tunnel. Developers can get ssh access to the TERRA REF database server, you can [request here](https://identity.ncsa.illinois.edu/join/TU49BUUEDM) and the connect with
    ```sh
    ssh -Nf -L 5432:localhost:5432 bety6.ncsa.illinois.edu
    ```    
3. Install a local instance of BETYdb
    ```sh
    load.bety.sh -c -u -m 99 -r 0 -w https://terraref.ncsa.illinois.edu/bety/dump/bety0/bety.tar.gz
    load.bety.sh -m 99 -r 6 -w https://terraref.ncsa.illinois.edu/bety/dump/bety6/bety.tar.gz
    ```


### Configuring a Server

This is the manual version of what is automated using Docker.

#### Dependencies


#### R packages

- shiny
- shinythemes
- lubridate
- dplyr
- ggplot2
- timevis
- rgeos
- leaflet
- cronR

```r
lapply(list('shiny', 'shinythemes', 'lubridate', 'dplyr', 'ggplot2', 'timevis', 'rgeos', 'leaflet', 'cronR'),
       install.packages)
```

#### System Programs 

cron (Ubuntu)

```sh
apt-get update
apt-get install cron
```

Shiny server: see https://www.rstudio.com/products/shiny/download-server/

#### Deploy

```r
shiny::runGitHub(repo = 'reference-data', 
                 username= 'terraref', 
                 subdir = 'experiment-trait-data-visualizer',
                 launch.browser = FALSE
                 )
```

