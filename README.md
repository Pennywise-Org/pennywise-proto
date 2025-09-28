# PennyWise - Proto Definitions Repository

This repository contains all the shared gRPC protocol buffer (`.proto`) definitions used for inter-service communication within the PennyWise microservices architecture. It serves as a single source of truth for service contracts between independent services.

## Purpose

- Define service contracts and message schemas in a language-agnostic format
- Enable consistent and strongly typed communication via gRPC across services
- Support code generation for client and server implementations in multiple languages (e.g., TypeScript, Go, Python)

## Structure

```
pennywise-proto/
├── proto/
│   ├── user/
│   │   └── user.proto
│   ├── plan/
│   │   └── plan.proto
│   └── common/
│       └── types.proto
├── buf.yaml              # Buf configuration file (optional)
├── buf.gen.yaml          # Code generation targets (optional)
├── README.md
```

## Usage

Each microservice pulls proto files from this repo (e.g., via git submodule or CI sync) and uses the appropriate codegen tool to generate client/server stubs.

### Generate TypeScript Types (via `@grpc/grpc-js` + `@protobuf-ts/plugin`)

From within the service that consumes these protos:

```bash
buf generate $ROOT_PATH_TO_PROTO_DEFINITION_FILE
```

### For Go or Python

Use language-specific plugins with `protoc`.

## Example: user.proto

```proto
syntax = "proto3";

package user;

service UserService {
  rpc GetUserById (GetUserByIdRequest) returns (UserResponse);
  rpc ValidateToken (ValidateTokenRequest) returns (ValidateTokenResponse);
}

message GetUserByIdRequest {
  string id = 1;
}

message UserResponse {
  string id = 1;
  string email = 2;
  string role = 3;
}

message ValidateTokenRequest {
  string token = 1;
}

message ValidateTokenResponse {
  bool valid = 1;
  string user_id = 2;
}
```

## Best Practices

- Group proto files by domain (`user/`, `plan/`, etc.)
- Use versioning if needed (e.g., `user/v1/user.proto`)
- Keep message definitions flat and reusable across services
- Define shared types in `common/` folder

## License

MIT License. Use freely within PennyWise ecosystem.

## Contact

For questions or improvements, open an issue or submit a pull request.
