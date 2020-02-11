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
vi config/application.yml

# Comment out existing items and add:
> db_username: "root"
> db_password: "root"
> db_host: 127.0.0.1

cp .env.development .env.development.local

# Delete all the contents of the file and change to the following
> YAML_VAULT_PASSPHRASE='UV[manh+p4W3J#H.0h$u'
> DATABASE_URL='mysql2://root:root@lvh.me:3306/kuma_development'
> ELASTICSEARCH_URL='http://lvh.me:9200'
> REDIS_URL='redis://lvh.me:6379'
```

## Reset rails settings
```
bin/rails db:migrate:reset
```

## Make up local environment.

```
yarn
bundle config --local build.mysql2 "--with-ldflags=-L/usr/local/opt/openssl/lib"
bundle install
bundle exec rake db:setup
EXTRA_SEED=1 bundle exec rake db:seed
```

## Make Procfile.

```
web: rails server -b 0.0.0.0 -p 3000
frontend: bin/webpack-dev-server
```

## Run as a development mode.

```
foreman start
# Go http://lvh.me:3000/
```

## How to see the sing-up e-mail.

```
# Go http://lvh.me:3000/letter_opener
```

### If it dosen't work.

```
# Open another terminal
bin/rails c
User.last.confirm
# Then the last Sign-up is confirmed
```
## How to only deploy to front-end
```
RAILS_ENV=production NODE_ENV=production bin/webpack
```
