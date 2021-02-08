# kuma-local

1. [What is this?](#What-is-this?)
2. [Setting up kuma](#Setting-up-kuma)
3. [Getting started](#Getting-started)
4. [Troubleshooting](#Troubleshooting)



## What is this?

Development environment for kuma.

Including...

* MySQL
* Redis
* Elasticsearch
* Kibana

## Getting started

### Install Docker for Mac

https://docs.docker.com/docker-for-mac/

### Install Xcode command line tools.

```
xcode-select --install
```

### Install local dependencies.

```
brew install yarn mysql
```

### Change directory to `/kuma-local`.

```
cd /path/to/kuma-loca
```

### Run containers.

Run setup commands when you run first time. This commands run as foreground process.

```sh
make
```

## Setting up kuma.

Move to the repository of Kuma first. Keep containers are running.

### Install mecab.

```
brew install mecab mecab-ipadic
```

### Change directory to `/kuma`

```
cd /path/to/kuma
```

### Install nodenv(includes node-build).

https://github.com/nodenv/nodenv

Install node versions configured at `.node-version`.

```
nodenv install $(cat .node-version)
```

### Install rbenv(with ruby-build recommended).

https://github.com/rbenv/rbenv

Install ruby versions configured at `.ruby-version`.

```
rbenv install $(cat .ruby-version)
```

### Install Bundler.

```
gem install bundler
```

### Setting up credential values.

```
vi /path/to/kuma/config/application.yml

# Comment out existing items and add:
db_username: "root"
db_password: "root"
db_host: 127.0.0.1

cp /path/to/kuma/.env.development .env.development.local

# Delete all the contents of the file and change to the following
# latest: https://github.com/Lang-8/kuma/blob/master/.env.development

YAML_VAULT_PASSPHRASE='UV[manh+p4W3J#H.0h$u'
DATABASE_URL='mysql2://root:root@lvh.me:3306/kuma_development'
ELASTICSEARCH_URL='http://lvh.me:9200'
REDIS_URL='redis://lvh.me:6379'
STRIPE_SIGNING_SECRET='whsec_WWp6LumeW19uY66YUeZppONMdmkXszg0'
ALLOWED_HOSTS='lvh.me,trek.lvh.me,admin.lvh.me,f035eadea904.ngrok.io'

vi /path/to/kuma/Procfile

web: rails server -b 0.0.0.0 -p 3000
frontend: bin/webpack-dev-server
```

### Make up local environment.

```
yarn
bundle config --local build.mysql2 "--with-ldflags=-L/usr/local/opt/openssl/lib"
bundle install
```

### Reset rails settings

```
bin/rails db:migrate:reset
```

### Make up DB of `kuma-local` environment.

```
bundle exec rake db:setup
EXTRA_SEED=1 bundle exec rake db:seed
```

### Run as a development mode.

```
foreman start
# Go http://lvh.me:3000/
```

### If it fails during `bundle install`

#### For the following error

```
ERROR: Can't install RMagick 4.1.1.
Can't find the ImageMagick library or one of the dependent libraries.
Check the mkmf.log file for more detailed information.

// etc...

An error occurred while installing rmagick (4.1.1), and Bundler cannot
continue.
Make sure that `gem install rmagick -v '4.1.1' --source 'https://rubygems.org/'`
succeeds before bundling.
```

Downgrade `Imagemagick` to version of ` 6`.

```
brew uninstall imagemagick
brew install imagemagick@6
```

