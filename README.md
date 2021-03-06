# plannen-dev

## deployment of dev environment for the 'plannen' app (a.k.a beheersplannen)

### build instructions

#### assumes
- access to OE private repos
- osx/linux environment
- docker installed
- python installed
- git
- your wildcards-private.ini files
- pycharm enterprise edition

#### general setup
```
# assumes you have access to OE private repos
git clone --recursive https://github.com/cecemel/plannen-dev.git
cd plannen-dev
# now, you have to add the development-private.ini and wildcards files on the right place,
ask your colleagues for help if you don't know
```

#### building the frontend
```
TODO :-/ (in docker)
# manually
cd ./plannen/plannen/static
npm insall; bower install
```

### building, migrating & init elastic, dummy data etc...
```
# assumes you are in folder plannen-dev
docker-compose stop; docker-compose rm -f; #not required, but cleans your working environment
# assumes you are in plannen-dev
python build_images.py [GITHUB_USER] [GITHUB_PASS];
python migrate_dbs.py;
python init_data.py;
```

#### reset backend one liner
```
docker-compose stop; docker-compose rm -f; rm -rf data/*; python build_images.py [GITHUB_USER] [GITHUB_PASS]; python migrate_dbs.py; python init_data.py;
```

### running in pycharm
see e.g.
https://blog.jetbrains.com/pycharm/2017/03/docker-compose-getting-flask-up-and-running/

### rebuilding and running a dependent service
```
# e.g.
python build_images.py [GITHUB_USER] [GITHUB_PASS] storageprovider
```

### some git submodule tricks
- tutorial: https://git-scm.com/book/en/v2/Git-Tools-Submodules
- some good submodule SO: https://stackoverflow.com/questions/1030169/easy-way-to-pull-latest-of-all-git-submodules
- bringing *-dev repo up to data: git pull; git submodule update --init --recursive

### caveats-todos
- on slow networks, you'll might have to build a couple of times (2,3) again, because some scripts are not robust. needs fix
- if you modify the production.ini, you will have to build the image again
- docs need to be build automatically
- currently, only dummy data is imported, if you want data from the db.dump -> you will need to fiddle a little with the docker files...
- scripts contain some hard coded parameters and should be cleaned
- currently, if you change code or config in dependent services, you will have to build and run every time again
- if you add a new dependent service and it needs a database, you will have to remove the postgres folder, and start the build from scratch
- working with private pypi server is still a hack, needs a fix
- a generic base image should be extract to speed up image build
- clean-up scripts, docker-compose should be sufficient for all migrations
