# Load Generator to reproduce AcaPy "Revocation registry metadata not found" Errors

This repository is a fork of the [Aries Cloud Agent Load Generator](https://github.com/My-DIGI-ID/aries-cloudagent-loadgenerator). It configures the load generator specifically to reproduce AcaPy "Revocation registry metadata not found" Errors.

## Install Dependencies
You need to have [Docker](https://docs.docker.com/get-docker/)
and [Docker-Compose](https://docs.docker.com/compose/install/) installed.

Further, you need to ensure to not use [Docker Compose V2](https://docs.docker.com/compose/cli-command/) as it is
incompatible to the setup used by the load generator. To deactivate Compose V2 run `docker-compose disable-v2`.

## Run the Load Generator
```
./setup/manage.sh start
```

## View the Test Results
1. Open http://localhost:3000/d/0Pe9llbnz/test-results to the test results visualized in a Grafana dashboard.
2. Scroll down to: ![](dashboard-errors.png)

## Sample Test Results with "Revocation registry metadata not found" errors
- https://github.com/lissi-id/acapy-load-test-results/tree/main/Full%20Flow%20Increasing%20Load/08%20AcaPy%200_7_3%20askar_wallet
- https://github.com/lissi-id/acapy-load-test-results/tree/main/Full%20Flow%20Constant%20Load/08%20AcaPy%200_7_3%20askar_wallet

## Debug AcaPy

In case you want to debug the AcaPy while running the load-tests you can
set `ISSUER_VERIFIER_AGENT_ENABLE_DEBUGGING=true` in the `.env`. Afterwards, you can start the test environment
using `./setup/manage.sh debug`. This will build an AcaPy docker image based on the AcaPy version currently checked out
under `./setup/agents/acapy` and includes a Python debugger into the docker image.

Once the test environment started the issuer-verifier-acapy will state `=== Waiting for debugger to attach ===`. To
attach a debugger open `./setup/agents/acapy` in VS Code, add the following debug configuration to the `launch.json`,
and start the debugging.

```
{
  "version": "0.2.0",
  "configurations": [
    
    {
      "name": "Python: Remote Attach",
      "type": "python",
      "request": "attach",
      "connect": {
        "host": "localhost",
        "port": 5678
      },
      "pathMappings": [
        {
          "localRoot": "${workspaceFolder}",
          "remoteRoot": "."
        }
      ]
    }
  ]
}
```

Finally, you can start the load-generator from the IDE or by running `./mvnw spring-boot:run`.
