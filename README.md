# What is this?

Development environment for kuma.

Including...

* MySQL
* Redis
* Elasticsearch
* Kibana

# Getting started.

## Install Docker for Mac.

https://docs.docker.com/docker-for-mac/

## Install Xcode command line tools.

```
xcode-select --install
```

## Install local dependencies.

```
brew install yarn mysql
```

## Run containers.

Run setup commands when you run first time. This commands run as foreground process.

```sh
make
```

# Setting up kuma.

Move to the repository of Kuma first. Keep containers are running.

## Install mecab.

```
brew install mecab mecab-ipadic
```

## Install nodenv(includes node-build).

https://github.com/nodenv/nodenv

Install node versions configured at `.node-version`.

```
nodenv install $(cat .node-version)
```

## Install rbenv(with ruby-build recommended).

https://github.com/rbenv/rbenv

Install ruby versions configured at `.ruby-version`.

```
rbenv install $(cat .ruby-version)
```

## Install Bundler.

```
gem install bundler
```

## Setting up credential values.

```
cp config/application.yml.sample config/application.yml

> db_username: "root"
> db_password: "root"
> db_host: 127.0.0.1
```

## Make up local environment.

```
yarn
bundle install
bundle exec rake db:setup
EXTRA_SEED=1 bundle exec rake db:seed
```

## Make Procfile.

```
web: rails server -b 0.0.0.0
resque: QUEUE=* rake environment resque:work
```

## Run as a development mode.

```
foreman start
# Go http://lvh.me:3000/
```

