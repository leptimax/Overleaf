# Overleaf

This repository contains the overleaf Toolkik, the standard tools for running a local instances of Overleaf. This toolkit will help you to set up and administer both Overleaf Community Edition (free to use, and community supported)

## Getting Started

Clone this repository locally:

```sh
git clone https://github.com/leptimax/Overleaf.git
```

1. Initiate Configuration
     
     Let's create our local configuration by running:

     ```sh
     cp .env.example .env
     ```

2. Starting Up

     The Overleaf toolkit uses `docker-compose` to manage the overleaf docker containers. To launch the project, you have just to run:

     ```sh
     docker-compose up -d mongo

     docker-compose exec -T mongo sh -c '
    while ! mongo --eval "db.version()" > /dev/null; do
      echo "Waiting for Mongo..."
      sleep 1
    done
    mongo --eval "rs.initiate({ _id: \"overleaf\", members: [ { _id: 0, host: \"mongo:27017\" } ] })" > /dev/null'

    docker-compose up -d
     ```

3. Create first admin count

     In a browser, open [http://localhost/launchpad](http://localhost/launchpad). You should see a form with email and password fields.

     Then click the link to go to the login page ([http://localhost/login](http://localhost/login)). Enter the credentials. Once you are logged in, you will be taken to a welcome page.

     Click the green button at the bottom of the page to start using Overleaf.

4. Create your first project

     On the [http://localhost/project](http://localhost/project) page you will see a button prompting you to create your first project. Click the button and follow instructions.

     You should thenbe taken to the new project, where you will see a text editor and a PDF preview

## Add packages

The sharelatex image only comes with minimal install of [TeXLive](https://www.tug.org/texlive/). If you want upgrade your image to a complete TeXLive installation, run :

```sh
docker exec sharelatex tlmgr install scheme-full
docker exec sharelatex tlmgr path add

#Then save your new image to make upagre persistent
docker commit sharelatex sharelatex/sharelatex:4.0.2-full
```

**Note** ; if you want just add a package, change `scheme-full` with the package name

Then do `docker-compose down`, update your `docker-compose.yml` file to use these image

```yml
#...
services:
    sharelatex:
        restart: always
        image: sharelatex/sharelatex:4.0.2-full
#...
```

## Disclaimer

This repository is a personal rework of the official [overleaf repository](https://github.com/overleaf/overleaf). 


