# Cert Package

[![Go Reference](https://pkg.go.dev/badge/github.com/NodePassProject/cert.svg)](https://pkg.go.dev/github.com/NodePassProject/cert)
[![License](https://img.shields.io/badge/License-BSD_3--Clause-blue.svg)](https://opensource.org/licenses/BSD-3-Clause)

A simple TLS configuration generator using ECDSA keys and self-signed certificates for Go applications.

## Features

- Easy-to-use TLS configuration generator
- Uses ECDSA keys with P-256 curve
- Self-signed certificate generation
- One-year validity period
- Custom organization name
- Automatic random serial number generation

## Installation

```bash
go get github.com/NodePassProject/cert
```

## Usage

### Basic Usage

```go
package main

import (
    "github.com/NodePassProject/cert"
    "log"
    "net/http"
)

func main() {
    // Create a new TLS configuration with organization name
    tlsConfig, err := cert.NewTLSConfig("My Application")
    if err != nil {
        log.Fatalf("Failed to create TLS config: %v", err)
    }
    
    // Create a new HTTP server with TLS
    server := &http.Server{
        Addr:      ":8443",
        TLSConfig: tlsConfig,
        Handler:   yourHandler(),
    }
    
    // Start the server with TLS
    log.Printf("Starting secure server on https://localhost:8443")
    log.Fatal(server.ListenAndServeTLS("", ""))
}

func yourHandler() http.Handler {
    // Your HTTP handler implementation
    return http.HandlerFunc(func(w http.ResponseWriter, r *http.Request) {
        w.Write([]byte("Secure server is running!"))
    })
}
```

### Advanced Configuration

The TLS configuration returned by `NewTLSConfig` is a standard `*tls.Config` object that can be further customized:

```go
tlsConfig, err := cert.NewTLSConfig("My Application")
if err != nil {
    log.Fatalf("Failed to create TLS config: %v", err)
}

// Customize the TLS configuration
tlsConfig.MinVersion = tls.VersionTLS12
tlsConfig.CipherSuites = []uint16{
    tls.TLS_ECDHE_ECDSA_WITH_AES_256_GCM_SHA384,
    tls.TLS_ECDHE_ECDSA_WITH_AES_128_GCM_SHA256,
}
```

## License

Copyright (c) 2025, NodePassProject. Licensed under the BSD 3-Clause License.
See the [LICENSE](./LICENSE) file for details.