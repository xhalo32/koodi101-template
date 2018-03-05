# Koodi101 Project Template

## How to run backend and frontend using docker-compose?

```shell
docker-compose build
docker-compose up
```

What if I want to run the project in the background?

```shell
docker-compose build
docker-compose up -d
```

Nice, how can I now see if the project is already running?

```shell
docker-compose ps
```

Cool, how can I stop the project from running and start from scratch

```shell
docker-compose down --rmi all --remove-orphans
```

This also deletes the built docker images, if you do not want this, you can also just run

```shell
docker-compose down
````

## How to run the backend and frontend without Docker?

Prerequisites
* [nodejs](http://nodejs.org)

### Backend

```shell
    cd backend
    npm install
    npm run dev
```

* Open browser in [http://localhost:9000](http://localhost:9000/api/chats)

### Frontend

```shell
    cd frontend
    npm install
    npm start
```

* Open browser in [http://localhost:8000](http://localhost:8000)