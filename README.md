# Vault hook script for [Dehydrated](https://dehydrated.de)

This hook script for [Dehydrated](https://dehydrated.de) stores the
certificates in [Vault](https://www.vaultproject.io/).

## Layout

This script uses an AppRole in Vault to request a token and deploys the
certificate, chain and key to a secret in Vault.

It uses the following scheme for storage:

```
$SECRET_BASE/domain.tld/host
```
For example

```
secret/ca/example.com/gitlab
```

This enables you to define two (or more) seperate policies for security
isolation:

- ca, with ```update``` permissions on ```secret/ca/*```
- gitlab, with ```read``` permissions on ```secret/ca/example.com/gitlab```

In the examples directory there are two hcl files you could use as reference.


## Dependencies

This script is only tested in Bash and requires:

- curl
- jq

For a Ubuntu/Debian based Linux system:

```bash
sudo apt install curl jq
```

And ofcourse a fully set-up Vault installation. See the [Vault documentation](https://www.vaultproject.io/docs/index.html)
on how to do this.

## Configuration

Create a configuration file as ```/etc/dehydrated/vault.inc```. Set the ROLE_ID and
SECRET_ID to the values for the ```ca``` AppRole.

 It should look like the following:

```bash
VAULT_ROLE_ID="xxxxxxx-yyyy-zzzz-aaaa-bbbbbbbbbbbb"
VAULT_SECRET_ID="cccccccc-dddd-eeee-ffff-ggggggggggggg"

VAULT_ADDRESS="https://127.0.0.1:8200"
VAULT_SECRET_BASE="secret/ca"
```

## Usage

This script can be used in two ways:

### Stand-alone

Set this script as your HOOK script in Dehydrated.

### Combining of multiple hooks

Since the Vault only allows for storage it only acts on the
```deploy_cert``` and ```unchanged_cert``` hooks. To verify your 
certificates you will need another script that does that.

Read [Example: Using multiple hooks](https://github.com/lukas2511/dehydrated/wiki/Example:-Using-multiple-hooks) on how to do this.

A nice example would be the [dehydrated-vultr-hook](https://github.com/ttalle/dehydrated-vultr-hook) script that uses the Vultr DNS
service for dns-01 verification.

## Next

Now you need a simple script to read the certificates from Vault and
store them in the right place. This script will be released soon.
