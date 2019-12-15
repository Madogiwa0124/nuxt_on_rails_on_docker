# Nuxt.js On Rails On Docker

build nuxt.js X Ruby On Rails enviroment.

## Composition

| container name | memo                      |
|----------------|---------------------------|
| db             | posgresql:latest          |
| nuxt           | Nuxt.js from Node 12.13.1 |
| rails          | Rails from Ruby 2.6.5     |

## Usage

1. Clone this repository and change directory.

``` sh
$ git clone https://github.com/Madogiwa0124/nuxt_on_rails_on_docker.git
$ cd nuxt_on_rails_on_docker
```

2. Clone Nuxt.js and Rails application repository.

``` sh
$ cd repos
$ git clone nuxt-app-repository
$ git clone rails-app-repository
```

3. Change Nuxt.js and Rails application names in args and volumes.

``` yml
nuxt:
  build:
    context: .
    dockerfile: ./docker/nuxt/Dockerfile
    args:
      - APP_NAME=nuxt-app-name
    volumes:
      - ./repos/nuxt-app-name:/nuxt-app-name
rails:
  build:
    context: .
    dockerfile: ./docker/rails/Dockerfile
    args:
      - APP_NAME=rails-app-name
    volumes:
      - ./repos/rails-app-name:/rails-app-name
```

4. build containers by docker-compose.

``` sh
$ docker-compose up -d
       Name                     Command               State            Ports
-------------------------------------------------------------------------------------
nuxt_rails_db_1      docker-entrypoint.sh postgres    Up      0.0.0.0:32839->5432/tcp
nuxt_rails_nuxt_1    docker-entrypoint.sh npm r ...   Up      0.0.0.0:8080->3000/tcp
nuxt_rails_rails_1   bin/rails s -b 0.0.0.0 -p 3000   Up      0.0.0.0:3000->3000/tcp
```

* Rails will be displayed when you access `http://localhost:8080`
* Nuxt.js will be displayed when you access `http://localhost:3000`
