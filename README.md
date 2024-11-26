# gRPC API
[![GitHub Super-Linter](https://github.com/blockjoy/api-proto/workflows/Lint%20Code%20Base/badge.svg)](https://github.com/marketplace/actions/super-linter)

All protobuf definitions for interacting with blockvisor-api.

## API v1

### Node States

```mermaid
stateDiagram
    state "Starting" as S
    state "Running" as R
    state "Updating" as U
    state "Stopped" as Z
    state "Failed" as F
    state "Deleting" as D1
    state "Deleted" as D2

    [*] --> S : start

    S --> R : started
    S --> F : error

    R --> U : update
    U --> R : updated
    U --> F : error

    R --> Z : stop
    Z --> R : start

    R --> F : error

    R --> D1 : delete
    Z --> D1 : delete
    D1 --> D2 : deleted
```

## Compilation

### TypeScript

Run this for the initial setup:
```bash
npm install protobufjs long nice-grpc-common grpc-tools ts-proto
mkdir generated
```

Then each time you want to get new ts files from the changed protos run:
```bash
./node_modules/.bin/grpc_tools_node_protoc \
    --plugin=protoc-gen-ts_proto=./node_modules/.bin/protoc-gen-ts_proto \
    --ts_proto_out=./generated \
    --ts_proto_opt=env=browser,outputServices=nice-grpc,outputServices=generic-definitions,outputJsonMethods=false,useExactTypes=false,esModuleInterop=true \
    --proto_path=./ \
    ./v1/*
```

