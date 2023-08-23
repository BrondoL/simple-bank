# Simple Bank

This repository contains the codes of the [Backend Master Class](https://www.youtube.com/watch?v=rx6CPDK_5mU&list=PLy_6D98if3ULEtXtNSY_2qN21VCKgoQAE&ab_channel=TECHSCHOOL).

In this course, you will learn step-by-step how to design, develop and deploy a backend web service from scratch. I believe the best way to learn programming is to build a real application. Therefore, throughout the course, you will learn how to build a backend web service for a simple bank. It will provide APIs for the frontend to do the following things:

- Create and manage bank accounts.
- Record all balance changes to each of the accounts.
- Perform a money transfer between 2 accounts.

The programming language we will use to develop the service is Golang, but the course is not just about coding in Go. You will learn a lot of different topics regarding backend web development. They are presented in 6 sections:

1. In the 1st section, you will learn deeply about how to design the database, generate codes to talk to the DB in a consistent and reliable way using transactions, understand the DB isolation levels, and how to use it correctly in production. Besides the database, you will also learn how to use docker for local development, how to use Git to manage your codes, and how to use GitHub Action to run unit tests automatically.

2. In the 2nd section, you will learn how to build a set of RESTful HTTP APIs using Gin - one of the most popular Golang frameworks for building web services. This includes everything from loading app configs, mocking DB for more robust unit tests, handling errors, authenticating users, and securing the APIs with JWT and PASETO access tokens. 

3. In the 3rd section, you will learn how to build your app with Docker and deploy it to a production Kubernetes cluster on AWS. The lectures are very detailed with a step-by-step guide, from how to build a minimal docker image, set up a free-tier AWS account, create a production database, store and retrieve production secrets, create a Kubernetes cluster with EKS, use GitHub Action to automatically build and deploy the image to the EKS cluster, buy a domain name and route the traffics to the service, secure the connection with HTTPS and auto-renew TLS certificate from Let's Encrypt.

4. In the 4th section, we will discuss several advanced backend topics such as managing user sessions, building gRPC APIs, using gRPC gateway to serve both gRPC and HTTP requests at the same time, embedding Swagger documentation as part of the backend service, partially updating a record using optional parameters, and writing structured logger HTTP middlewares and gRPC interceptors.

5. Then the 5th section will introduce you to asynchronous processing in Golang using background workers and Redis as its message queue. We'll also learn how to create and send emails to users via Gmail SMTP server. Along the way, we'll learn more about writing unit tests for our gRPC services that might involve mocking multiple dependencies at once.

6. The final section 6th concludes the course with lectures about how to improve the stability and security of the server. We'll keep updating dependency packages to the latest version, use Cookies to make the refresh token more secure, and learn how to gracefully shut down the server to protect the processing resources. As this part is still a work in progress, we will keep making and uploading new videos about new topics in the future. So please come back here to check them out from time to time.

This course is designed with a lot of details, so that everyone, even with very little programming experience can understand and do it by themselves. I strongly believe that after the course, you would be able to work much more confidently and effectively on your projects.

## Simple bank service

The service that we’re going to build is a simple bank. It will provide APIs for the frontend to do following things:

1. Create and manage bank accounts, which are composed of owner’s name, balance, and currency.
2. Record all balance changes to each of the account. So every time some money is added to or subtracted from the account, an account entry record will be created.
3. Perform a money transfer between 2 accounts. This should happen within a transaction, so that either both accounts’ balance are updated successfully or none of them are.

## Setup local development

### Install tools

- [Docker desktop](https://www.docker.com/products/docker-desktop)
- [TablePlus](https://tableplus.com/)
- [Golang](https://golang.org/)
- [Homebrew](https://brew.sh/)
- [Migrate](https://github.com/golang-migrate/migrate/tree/master/cmd/migrate)

    ```bash
    brew install golang-migrate
    ```

- [DB Docs](https://dbdocs.io/docs)

    ```bash
    npm install -g dbdocs
    dbdocs login
    ```

- [DBML CLI](https://www.dbml.org/cli/#installation)

    ```bash
    npm install -g @dbml/cli
    dbml2sql --version
    ```

- [Sqlc](https://github.com/kyleconroy/sqlc#installation)

    ```bash
    brew install sqlc
    ```

- [Gomock](https://github.com/golang/mock)

    ``` bash
    go install github.com/golang/mock/mockgen@v1.6.0
    ```

### Setup infrastructure

- Create the bank-network

    ``` bash
    make network
    ```

- Start postgres container:

    ```bash
    make postgres
    ```

- Create simple_bank database:

    ```bash
    make createdb
    ```

- Run db migration up all versions:

    ```bash
    make migrateup
    ```

- Run db migration up 1 version:

    ```bash
    make migrateup1
    ```

- Run db migration down all versions:

    ```bash
    make migratedown
    ```

- Run db migration down 1 version:

    ```bash
    make migratedown1
    ```

### Documentation

- Generate DB documentation:

    ```bash
    make db_docs
    ```

- Access the DB documentation at [this address](https://dbdocs.io/techschool.guru/simple_bank). Password: `secret`

### How to generate code

- Generate schema SQL file with DBML:

    ```bash
    make db_schema
    ```

- Generate SQL CRUD with sqlc:

    ```bash
    make sqlc
    ```

- Generate DB mock with gomock:

    ```bash
    make mock
    ```

- Create a new db migration:

    ```bash
    make new_migration name=<migration_name>
    ```

### How to run

- Run server:

    ```bash
    make server
    ```

- Run test:

    ```bash
    make test
    ```

## Deploy to kubernetes cluster

- [Install nginx ingress controller](https://kubernetes.github.io/ingress-nginx/deploy/#aws):

    ```bash
    kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/controller-v0.48.1/deploy/static/provider/aws/deploy.yaml
    ```

- [Install cert-manager](https://cert-manager.io/docs/installation/kubernetes/):

    ```bash
    kubectl apply -f https://github.com/jetstack/cert-manager/releases/download/v1.4.0/cert-manager.yaml
    ```