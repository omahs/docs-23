---
id: run-local
title: Running Sourcify Locally
slug: /run-locally
---

# Running Sourcify Locally

## Requirements

- Node.js v16

## Clone

First clone the repository

```bash
git clone https://github.com/ethereum/sourcify.git && cd sourcify
```

## Install and Build

Install server dependencies

```
npm install
```

Install all modules with [lerna](https://github.com/lerna/lerna)

```
npx lerna bootstrap
```

And build

```
npx lerna run build
```

## Create .env

Make a copy of the `environments/.env.dev` and rename it to `environments/.env`.

## Running the Server

### Set Environment Variables

Set the variables below for the file paths. You can also change the other optional environment variables.

```bash
REPOSITORY_PATH= # Path on host where verified contracts will be saved e.g. /home/user/sourcify/repository
SOLC_REPO= # Path where Solidity compiler binaries will be saved. e.g. /home/user/solc/linux-amd64
SOLJSON_REPO= # Path where Solidity JS (solc-js) compilers are saved e.g. /home/user/solc/js
```

You also need to provide either Alchemy API keys or your own JSON-RPC node URL for Ethereum networks. If both provided, it will first query the custom JSON-RPC and fall back to Alchemy.

For other networks with Alchemy support such as Polygon, Optimism, Arbitrum etc., you also need to provide an API key. You can check those networks in the [`services/core/src/sourcify-chains.ts`](https://github.com/ethereum/sourcify/blob/master/services/core/src/sourcify-chains.ts#L60). For the chains without an RPC in `sourcify-chains.ts`, the default RPCs from in [`services/core/src/chains.json`](https://github.com/ethereum/sourcify/blob/master/services/core/src/chains.json) will be used.

Infura is needed only for the Palm Network.

```bash
INFURA_ID=xxx
ALCHEMY_ID=xxx
```

For Ethereum chains JSON-RPC URLs:

```bash
# Custom nodes
NODE_ADDRESS=http://localhost
NODE_PORT_MAINNET=8545
NODE_PORT_RINKEBY=
NODE_PORT_GOERLI=
NODE_PORT_SEPOLIA=
```

### Run

Start the server

```bash
npm run server:start
```

## Running the UI

### Set Environment Variables

Create a copy of `ui/.env.development` and name it to `ui/.env.development.local`. Set the environment variables accordingly

```bash
REACT_APP_SERVER_URL=http://localhost:5555
REACT_APP_REPOSITORY_SERVER_URL=https://localhost:10000
REACT_APP_IPNS=repo.staging.sourcify.dev
REACT_APP_TAG=latest
```

### Run

```bash
cd ui/
npm start
```

## Running the Repository

The repository (https://repo.sourcify.dev) is a simple web UI on top of the file directory of the verified contracts. It runs [a fork of h5ai](https://github.com/sourcifyeth/h5ai) and is a submodule under the folder `/h5ai-nginx` in the Sourcify git repo. The origin is at [sourcifyeth/h5ai-nginx](https://github.com/sourcifyeth/h5ai-nginx/).

The easiest way to run the contract repository is to directly run the docker container using the docker-compose file `environments/repository.yaml`.

```
docker-compose -f environments/repository.yaml up
```

This will pull and run the `ethereum/source-verify:repository-${TAG}` docker container from [our Docker hub](https://hub.docker.com/r/ethereum/source-verify) on port `REPOSITORY_SERVER_EXTERNAL_PORT`.

You can also build your own container by changing the `image` of the container to a local container `build` context:

```yaml
# environments/repository.yaml
services:
  repository:
    <<: *project-base
    # Comment/remove this line
    # image: ethereum/source-verify:repository-${TAG}
    # Add build fields like below.
    build:
      context: ../h5ai-nginx # Point to the /h5ai-nginx folder
      dockerfile: Dockerfile
    container_name: repository-${TAG}
    volumes:
```

However you need to pull the `/h5ai-nginx` submodule as the folder will be initially empty:

```
git submodule update --init --recursive
```

## Running the Monitor

To run the monitor:

```
npm run monitor:start
```
