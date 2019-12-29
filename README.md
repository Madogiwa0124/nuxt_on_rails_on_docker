# Nuxt.js On Rails On Docker

build nuxt.js X Ruby On Rails with OpenAPI enviroment.

## Composition

| container name    | memo                                                      |
|-------------------|---------------------------------------------------------- |
| db                | posgresql:latest                                          |
| nuxt              | Nuxt.js from Node 12.13.1                                 |
| rails             | Rails from Ruby 2.6.5                                     |
| openapi-ui        | swagger-ui from swaggerapi/swagger-ui                     |
| openapi-generator | openapi-generator from openapitools/openapi-generator-cli |

## Usage

1. Clone this repository and change directory.

``` sh
$ git clone https://github.com/Madogiwa0124/nuxt_on_rails_on_docker.git
$ cd nuxt_on_rails_on_docker
```

2. Clone Nuxt.js and Rails application, schema management by OpenAPI repository.

``` sh
$ cd repos
$ git clone nuxt-app-repository
$ git clone rails-app-repository
$ git clone openapi-repository
```

3. Change Nuxt.js and Rails application, schema management names in args and volumes.

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
openapi-ui:
    image: swaggerapi/swagger-ui
    volumes:
      - ./repos/openapi-repo-name:/openapi-repo-name
    ports:
      - "8000:8080"
    tty: true
    environment:
      SWAGGER_JSON: /openapi-repo-name/openapi.yml
openapi-generator:
  image: openapitools/openapi-generator-cli
  volumes:
    - ./repos/fopenapi-repo-name:/openapi-repo-name
```

4. build containers by docker-compose.

``` sh
$ docker-compose up -d
              Name                             Command               State                Ports             
------------------------------------------------------------------------------------------------------------
foodolist_dev_db_1                  docker-entrypoint.sh postgres    Up       0.0.0.0:32769->5432/tcp       
foodolist_dev_nuxt_1                docker-entrypoint.sh npm r ...   Up       0.0.0.0:8080->3000/tcp        
foodolist_dev_openapi-generator_1   docker-entrypoint.sh help        Exit 0                                 
foodolist_dev_openapi-ui_1          sh /usr/share/nginx/run.sh       Up       80/tcp, 0.0.0.0:8000->8080/tcp
foodolist_dev_rails_1               bin/rails s -b 0.0.0.0 -p 3000   Up       0.0.0.0:3000->3000/tcp 
```

* Rails will be displayed when you access `http://localhost:8080`
* Nuxt.js will be displayed when you access `http://localhost:3000`
* Swagger UI will be displayed when you acces `http://localhost:8000`

# genarate stab app by open-api-genarator

``` sh
$ docker-compse run --rm openapi-generator generate -i /openapi-repo-name/openapi.yml -g ruby-on-rails -o /openapi-repo-name/rails_stab
[main] WARN  o.o.c.ignore.CodegenIgnoreProcessor - Output directory does not exist, or is inaccessible. No file (.openapi-generator-ignore) will be evaluated.
[main] INFO  o.o.codegen.DefaultGenerator - OpenAPI Generator: ruby-on-rails (server)
[main] INFO  o.o.codegen.DefaultGenerator - Generator 'ruby-on-rails' is considered stable.
...
[main] INFO  o.o.codegen.AbstractGenerator - writing file /foodolist-openapi/rails_stab/docker-entrypoint.sh
[main] INFO  o.o.codegen.AbstractGenerator - writing file /foodolist-openapi/rails_stab/.openapi-generator-ignore
[main] INFO  o.o.codegen.AbstractGenerator - writing file /foodolist-openapi/rails_stab/.openapi-generator/VERSION

$ ls repos/openapi-repo-name/rails_stab
Dockerfile           README.md            app                  config               db                   lib                  public               tmp
Gemfile              Rakefile             bin                  config.ru            docker-entrypoint.sh log                  test                 vendor
```
