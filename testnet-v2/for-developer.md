# Getting Started

### Tabi chain API document

#### **Online Tabi chain API document**

[https://api.testnetv2.tabichain.com/swagger/](https://api.testnetv2.tabichain.com/swagger/)

#### **Local Tabi chain API document of full node**

```shell
~$ cd .tabid/config
~/.tabid/config$ vi app.toml
```

#### Config the \[api] section

```
[api]
# Enable defines if the API server should be enabled
enable = true

# Swagger defines if swagger documentation shold automatically be registered
swagger = true
```

open the link [http://127.0.0.1:1317/swagger/](http://127.0.0.1:1317/swagger/) in your web browser

### EVM Rpc

#### **Online RPC service**

[https://rpc.testnetv2.tabichain.com](https://rpc.testnetv2.tabichain.com)

#### **Local RPC service of full node**

```shell
~$ cd 
~$ cd .tabid
~/.tabid$ vi config/app.toml
```

#### Config the \[json-rpc] section

```
[json-rpc]
# Enable defines if the gRPC server should be enabled.
enable = true
```

access the rpc service:

[http://127.0.0.1:8545](http://127.0.0.1:8545)
