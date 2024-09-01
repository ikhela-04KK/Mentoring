## Link for design file 
[Mentoring App](https://www.figma.com/design/jQJ2jGnbUXLjHtIuHQtpmD/Untitled?node-id=0-1&node-type=CANVAS&t=YxDjJAhQYLT0GZ1S-0)

# Turborepo Docker starter

This is an official Docker starter Turborepo.

## Using this example

Run the following command:

```sh
npx create-turbo@latest -e with-docker
```

## What's inside?

This turborepo uses [Yarn](https://classic.yarnpkg.com/lang/en/) as a package manager. It includes the following packages/apps:

### Apps and Packages

- `@repo/web`: a [Next.js](https://nextjs.org/) app
- `@repo/api`: an [Express](https://expressjs.com/) server
- `@repo/ui`: ui: a React component library
- `@repo/eslint-config-custom`: `eslint` configurations for client side applications (includes `eslint-config-next` and `eslint-config-prettier`)
- `@repo/eslint-config-custom-server`: `eslint` configurations for server side applications (includes `eslint-config-next` and `eslint-config-prettier`)
- `scripts`: Jest configurations
- `@repo/logger`: Isomorphic logger (a small wrapper around console.log)
- `@repo/typescript-config`: tsconfig.json's used throughout the monorepo

Each package/app is 100% [TypeScript](https://www.typescriptlang.org/).

### Docker

This repo is configured to be built with Docker, and Docker compose. To build all apps in this repo:

```
# Create a network, which allows containers to communicate
# with each other, by using their container name as a hostname
docker network create app_network

# Build prod using new BuildKit engine
COMPOSE_DOCKER_CLI_BUILD=1 DOCKER_BUILDKIT=1 docker-compose -f docker-compose.yml build

# Start prod in detached mode
docker-compose -f docker-compose.yml up -d
```

Open http://localhost:3000.

To shutdown all running containers:

```
# Stop all running containers
docker kill $(docker ps -q) && docker rm $(docker ps -a -q)
```

### Remote Caching

This example includes optional remote caching. In the Dockerfiles of the apps, uncomment the build arguments for `TURBO_TEAM` and `TURBO_TOKEN`. Then, pass these build arguments to your Docker build.

You can test this behavior using a command like:

`docker build -f apps/web/Dockerfile . --build-arg TURBO_TEAM=“your-team-name” --build-arg TURBO_TOKEN=“your-token“ --no-cache`

### Utilities

This Turborepo has some additional tools already setup for you:

- [TypeScript](https://www.typescriptlang.org/) for static type checking
- [ESLint](https://eslint.org/) for code linting
- [Jest](https://jestjs.io) test runner for all things JavaScript
- [Prettier](https://prettier.io) for code formatting

Run npm i --save-dev webpack-node-externals run-script-webpack-plugin webpack
Add the following npm script to dockerized-full-stack-environment/nestjs-app/package.json file:

  "build:back": "docker-compose build nestjs-app",
    "build:front": "docker-compose build nextjs-app",
    "start:db": "docker-compose up mysql-db",
    "start:back": "docker-compose up nestjs-app --renew-anon-volumes",
    "start:front": "docker-compose up nextjs-app --renew-anon-volumes",
    "clean": "docker-compose down -v"



## for jenkins container

Construction de Conteneurs : Jenkins peut être utilisé pour automatiser le processus de construction des images Docker. En montant le socket Docker, Jenkins peut utiliser le client Docker à l'intérieur du conteneur pour lancer des constructions d'images Docker.

Déploiement de Conteneurs : Si votre processus de déploiement implique le déploiement de conteneurs Docker, Jenkins peut utiliser le client Docker dans le conteneur pour gérer ces déploiements. Cela peut être particulièrement utile dans un environnement où vous utilisez des conteneurs pour l'orchestration de services.

Tests dans des Conteneurs : Si vous effectuez des tests dans des conteneurs Docker, Jenkins peut utiliser le client Docker pour créer, démarrer, arrêter et supprimer des conteneurs de test.

Accès aux Fonctionnalités Docker : Le montage du socket Docker permet à Jenkins d'accéder à toutes les fonctionnalités du client Docker, ce qui peut être essentiel si vous avez des scénarios d'utilisation spécifiques qui nécessitent un accès direct aux fonctionnalités de Docker.

## 
