# Iroha 2 Explorer Backend

This is a backend service for
the [Iroha 2 Block Explorer Web application](https://github.com/soramitsu/iroha2-block-explorer-web).
It is written in Rust and provides a classic HTTP-API way to observe data
in [Iroha 2](https://github.com/hyperledger/iroha).

Note that the current implementation is more of a draft and is not production ready. This implementation maintains an
in-memory normalised SQLite database that reflects the history of blocks (transactions, instructions) in Iroha and its
current world state (domains, accounts, assets). The database is re-created from scratch on each update in Iroha, which
is not suitable for a large scale. The rationale of using an SQL-based solution is that currently, Iroha Query API is
limited and cannot provide some features that are necessary for efficient Explorer implementation (e.g. selects and
joins). However, having an SQL-based querying allows creating a set of endpoints to serve the frontend needs
efficiently, allowing at least the frontend implementation _as desired_.

## Usage

Prerequisites: [Rust toolchain](https://rustup.rs/).

Install from this repo remotely:

```shell
cargo install --git https://github.com/soramitsu/iroha2-block-explorer-backend

iroha_explorer help
```
adapted to DiCl's internal project:

```shell
cargo install --git https://github.com/DigitClopedia2024/iroha2-block-explorer-backend-rc2-frozen --rev 4de8dd2b 

iroha_explorer help
```

Or build and run locally from the repo with:

```shell
cargo run --release -- help
```

Use `help` for detailed usage documentation. In short:

- `serve` to run the server
- `scan` to scan Iroha ones and save into a database (for troubleshooting)
- pass `--torii-url`, `--account`, and `--account-private-key` options to configure connection to Iroha

For example:

```shell
# via ENVs; also could be passed as CLI args
export ACCOUNT=<account id>
export ACCOUNT_PRIVATE_KEY=<acount private key>
export TORII_URL=http://localhost:8080

iroha_explorer serve --port 4123
iroha_explorer scan ./scanned.sqlite
```

With the running server, open `/api/docs` path (e.g. `http://localhost:4123/api/docs`) for API documentation.

To configure **logging**, use `RUST_LOG` env var. For example:

```shell
RUST_LOG=iroha_explorer=debug,sqlx=debug
```

### Health check

```shell
test "$(curl -fsSL localhost:4000/api/health)" = "healthy" && echo OK || echo FAIL
```

### Serve with test data

_Only available in debug builds._

Serve test data without connecting to Iroha:

```shell
cargo run -- serve-test
```

> `/status` endpoint would not work in this case

---

## 🔒 **Stable Deployment Documentation - Frozen version**  
*This fork is pinned to a verified commit for internal projects. Do not upgrade without auditing.*  

### **Version Lock v0.3.0**  
- **Commit**: [`4de8dd2`](https://github.com/soramitsu/iroha2-block-explorer-backend/tree/4de8dd2b993081645e04469b76b3552acd7cc62f)  
- **Purpose**: Compatibility with blockchain Iroha 2.0.0-rc.2. (https://github.com/hyperledger/iroha)  

### **Install from This Fork**  
```shell
cargo install --git https://github.com/DigitClopedia2024/iroha2-block-explorer-backend-rc2-frozen --rev 4de8dd2b993081645e04469b76b3552acd7cc62f
```
### **Recovery Protocol**

1. Local backup with command:

```shell
git clone --mirror https://github.com/DigitClopedia2024/iroha2-block-explorer-backend-rc2-frozen
```
2. Reinstall with command:

```shell
cargo install --path /path/to/your/local/clone
```

### **License**

Apache 2.0 (added to this fork; original had no license).

*Frozen on 2024-03-15*

