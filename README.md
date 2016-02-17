# Blockstack Client

[![Slack](http://slack.blockstack.org/badge.svg)](http://slack.blockstack.org/)

This package contains the reference client library for
[blockstack-server](https://github.com/blockstack/blockstack-server). It talks to the
server and provides an interface for creating and managing names in decentralized namespaces on the blockchain.

## Installation

Installing the command line interface and the client library:

### Debian + Ubuntu

```
$ sudo apt-get update && sudo apt-get install -y python-pip python-dev libssl-dev
$ sudo pip install blockstack
```

### OS X

```
$ ARCHFLAGS=-Wno-error=unused-command-line-argument-hard-error-in-future pip install pycrypto
$ sudo pip install blockstack
```

*Note that the first line is a custom command meant to ensure that pycrypto installs properly.*

### Development Version

The client is under heavy development and you might want to install the latest development version:

```
$ sudo pip install git+https://github.com/blockstack/blockstack-client.git@develop
```

## Command Line Usage 

### Listing All Commands

```
$ blockstack
usage: blockstack [-h]
                  ...

Blockstack cli version 0.0.12.2

positional arguments:
    balance             display the wallet balance
    config              change config settings with --server=x --port=y --advanced=true/false
    cost                <name> | get the cost of a name
    import              display the address with which to receive names sent from outside of the client
    info                check if the server is up and get details about the server
    lookup              <name> | get the data record for a particular name
    names               display the names owned by local addresses
    register            <name> <data> | register a name
    transfer            <name> <address> | transfer a name you own to another address
    update              <name> <data> | update a name record with a certain amount of data
    whois               <name> | get the registration information associated with a name

optional arguments:
  -h, --help            show this help message and exit
```

### Info  (or ping or status or details)

*Note: formerly ping, status, server and consensus*

```
$ blockstack info
```

#### Examples

```
$ blockstack info
{
    "alive": true,
    "advanced_mode": false,
    "cli_version": "0.0.12.2",
    "consensus_hash": "ccf9a90ae7a10dc83a9da7e02213eb20",
    "last_block_processed": 398758,
    "last_block_seen": 398765,
    "server": "server.blockstack.org:6264",
    "server_host": "server.blockstack.org",
    "server_port": 6264,
    "server_version": "0.0.10.1"
}
```

### Config

*Note: formerly server and advanced*

```
$ blockstack config <options>
```

#### Examples

```
$ blockstack config --server=server.blockstack.org --port=6264 --advanced=false
{
  "message": "Configuration settings updated.",
  "error": false
}
```

### Cost

```
$ blockstack cost <name>
```

#### Examples

```
$ blockstack cost fredwilson.id
{
    "fee": 0.00049,
    "registration_fee": 0.00025,
    "transaction_fee": 0.00024
}
```

### Whois

```
$ blockstack whois <name>
```

#### Examples

```
$ blockstack whois fredwilson.id
{
    "block_preordered_at": 374084,
    "block_renewed_at": 374084,
    "owner_address": "1F2nHEDLRJ39XxAvSxwQhJsaVzvS5RHDRM",
    "owner_public_keys": ["0411d88aa37a0eea476a5b63ca4b1cd392ded830865824c27dacef6bde9f9bc53fa13a0926533ef4d20397207e212c2086cbe13db5470fd29616abd35326d33090"],
    "owner_script": "76a91499e7f97f5d2c77b4f32b4ed9ae0f0385c45aa5c788ac",
    "preorder_transaction_id": "2986ec31ec957692d7f5bc58a3b02d2ac2d1a60039e9163365fc954ff51aeb5a",
    "registered": true
}
```

```
$ blockstack whois $(whoami)_$RANDOM.id
{
  "registered": false
}
```

### Lookup

```
$ blockstack lookup <name>
```

#### Examples

```
$ blockstack lookup fredwilson.id
{
  "data": {
    "$origin": "fredwilson.id",
    "$ttl": "3600",
    "cname": [{ "name": "@", "alias": "https://zk9.s3.amazonaws.com" }]
  }
}
```

```
$ blockstack lookup $(whoami)_$RANDOM.id
{
  "data": null
}
```

### Register

```
$ blockstack register <name>
```

#### Example

```
$ blockstack register $(whoami)_$RANDOM.id
Registering ryan_30764.id will cost 0.0003025 BTC. Continue? (y/n): y
{
    "message": "Name queued up for registration. Please expect a few hours for this process to be completed.",
    "error": false
}
```

```
$ blockstack register fredwilson.id
{
  "message": "Name has already been registered.",
  "error": true
}
```

### Update

```
$ blockstack update <name> <data>
```

#### Examples

```
$ blockstack update yeezy.id '{"name": "Kanye West"}'
{
  "message": "Data record updated.",
  "error": false
}
```

```
$ blockstack update fredwilson.id '{}'
{
  "message": "That name is not in your possession.",
  "error": true
}
```

### Transfer

```
$ blockstack transfer <name> <address>
```

#### Examples

```
$ blockstack transfer ryan_30764.id 1Jbcrh9Lkwm73jXyxramFukViEtktwq8gt
{
  "message": "Name queued up for transfer.",
  "error": false
}
```

```
$ blockstack transfer fredwilson.id 1Jbcrh9Lkwm73jXyxramFukViEtktwq8gt
{
  "message": "That name is not in your possession.",
  "error": true
}
```

### Names

```
$ blockstack names
```

#### Examples

```
$ blockstack names
{
    "names_owned": [],
    "addresses": [
      { "address": "1Jbcrh9Lkwm73jXyxramFukViEtktwq8gt", "names": [] }
    ]
}
```

### Balance

```
$ blockstack balance
```

#### Examples

```
$ blockstack balance
{
    "balance": 0.05,
    "addresses": [
      { "address": "1EHgqHVpA1tjn6RhaVj8bx6y5NGvBwoMNS", "balance": 0.05 }
    ]
}
```

### Import

```
$ blockstack import
```

#### Examples

```
$ blockstack import
{
    "address": "1Jbcrh9Lkwm73jXyxramFukViEtktwq8gt"
}
```

## Client Library

You can also import the blockstack client and write your own programs.

Here is some example code to get you started:

```
from blockstack_client import client
from blockstack_client.utils import print_result as pprint

client.session(server_host='127.0.0.1', server_port=6264)
resp = client.ping()
pprint(resp)
```
