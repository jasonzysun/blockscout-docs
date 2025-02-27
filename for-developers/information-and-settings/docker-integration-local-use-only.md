---
description: For local builds
---

# Docker Integration

{% hint style="success" %}
Please see [https://github.com/blockscout/blockscout/tree/master/docker-compose](https://github.com/blockscout/blockscout/tree/master/docker-compose) for all required information.
{% endhint %}

### Prerequisites

- Docker v20.10+
- Docker-compose 2.x.x+
- Running Ethereum JSON RPC client

### Building Docker containers from source

```bash
docker-compose up --build
```

This command uses by-default `docker-compose.yml`, which builds the explorer into the Docker image and runs 6 Docker containers:

- Postgres 14.x database, which will be available at port 7432 on localhost.
- Redis database of latest version, which will be available at port 6379 on localhost.
- Blockscout explorer at http://localhost:4000.

and 3 Rust microservices:

- [Smart-contract-verifier](https://github.com/blockscout/blockscout-rs/tree/main/smart-contract-verifier) service, which will be available at port 8150 on the host machine.
- [Sig-provider](https://github.com/blockscout/blockscout-rs/tree/main/sig-provider) service, which will be available at port 8151 on the host machine.
- [Sol2UML visualizer](https://github.com/blockscout/blockscout-rs/tree/main/visualizer) service, which will be available at port 8152 on the host machine.

### Configs for different Ethereum clients

The repo above contains built-in configs for different clients without needing to build the image.

{% hint style="warning" %}
**Note for Linux users**: Linux users who run Blockscout in docker with a local node need to run the node on [http://0.0.0.0/](http://0.0.0.0/) rather than [http://127.0.0.1/](http://127.0.0.1/)
{% endhint %}

* Erigon: `docker-compose -f docker-compose-no-build-erigon.yml up -d`
* Geth: `docker-compose -f docker-compose-no-build-geth.yml up -d`
* Nethermind, OpenEthereum: `docker-compose -f docker-compose-no-build-nethermind up -d`
* Ganache: `docker-compose -f docker-compose-no-build-ganache.yml up -d`
* HardHat network: `docker-compose -f docker-compose-no-build-hardhat-network.yml up -d`
* Running only explorer without DB: `docker-compose -f docker-compose-no-build-no-db-container.yml up -d`. In this case, one container is created - for the explorer itself. And it assumes that the DB credentials are provided through `DATABASE_URL` environment variable.

All of the configs assume the Ethereum JSON RPC is running at [http://localhost:8545](http://localhost:8545/).

In order to stop launched containers, run `docker-compose -d -f config_file.yml down`, replacing `config_file.yml` with the file name of the config which was previously launched.

You can adjust BlockScout environment variables from `./envs/common-blockscout.env`. Descriptions of the ENVs are available in [the docs](https://docs.blockscout.com/for-developers/information-and-settings/env-variables).

