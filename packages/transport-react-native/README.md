# @fedimint/transport-react-native

Transport layer for React Native that bridges `@fedimint/core` with the native Fedimint client via UniFFI.

## Overview

This package provides the transport layer that allows `@fedimint/core` to communicate with the Fedimint Rust client in React Native apps. It implements the `Transport` interface and uses UniFFI-generated bindings to call native code.

## Architecture

```
@fedimint/core (TypeScript)
    ↓
TransportClient
    ↓
RNTransport (this package)
    ↓
UniFFI Bindings (@fedimint/react-native)
    ↓
Rust RpcHandler
```

## RPC Interface

The transport uses a callback-based RPC interface instead of promises:

```typescript
interface RpcCallback {
  onResponse(responseJson: string): void
}

interface RpcHandlerInterface {
  rpc(requestJson: string, callback: RpcCallback): void
}
```

This design allows for more efficient native-to-JavaScript communication without the overhead of promise resolution.

## Differences from Web Transport

### Web (`@fedimint/transport-web`)

- Uses a Web Worker for WASM execution
- Asynchronous message passing via `postMessage`
- WASM runs in separate thread
- Promise-based RPC calls

### React Native (this package)

- Direct native calls via UniFFI (no worker needed)
- Native code runs on native threads managed by React Native
- Callback-based RPC calls (no promises)
- Synchronous FFI calls, async handled by Rust callbacks

## License

MIT
