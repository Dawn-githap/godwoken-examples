# Godwoken Examples

## Install dependencies

```bash
yarn run bootstrap
```

## Update Demo Configs

Firstly copy config files.

```bash
cp <your godwoken `scripts-deploy-result.json`> packages/demo/src/configs/scripts-deploy-result.json
cp <your godwoken `config.toml`> packages/demo/src/configs/godwoken-config.json (convert config.toml to json format)
cp packages/demo/src/configs/config.json.sample packages/demo/src/configs/config.json
```

For testnet

```bash
yarn run generate-testnet-configs
```

## Deposit By CLI

Deposit CLI both support `testnet` and `dev chain`.

### Build typescripts

```bash
yarn workspace @godwoken-examples/godwoken tsc 
yarn run build-demo-cli
```

### Deposit

Run `deposit.js --help` to see how to use this command.

```bash
LUMOS_CONFIG_FILE=<your lumos config json file> node ./packages/demo/build-cli/cli/deposit.js --help # for devnet
LUMOS_CONFIG_NAME=AGGRON4 node ./packages/demo/build-cli/cli/deposit.js --help # for testnet
```

Or Deposit SUDT

```bash
LUMOS_CONFIG_FILE=<your lumos config json file> node ./packages/demo/build-cli/cli/deposit_sudt.js --help # for devnet
LUMOS_CONFIG_NAME=AGGRON4 node ./packages/demo/build-cli/cli/deposit_sudt.js --help # for testnet
```

### Withdrawal

```bash
LUMOS_CONFIG_FILE=<your lumos config json file> node ./packages/demo/build-cli/cli/withdraw.js --help # for devnet
LUMOS_CONFIG_NAME=AGGRON4 node ./packages/demo/build-cli/cli/withdraw.js --help # for testnet
```

# Polyjuice

## Create Creator account

### Build typescripts

```bash
yarn run build-tools
```

### Create creator account for polyjuice

Create account id for create polyjuice contract account (the `creator_account_id` config)

```bash
export VALIDATOR_SCRIPT_HASH=<your validator script hash>
$ node packages/tools/lib/polyjuice-cli.js createCreatorAccount <from_id> <sudt_id> <rollup_type_hash> <privkey>
```

You can see `<rollup_type_hash>` when godwoken started.

`<sudt_id>` given `1` is for CKB Token.

`<privkey>` is a `0x` prefixed hex string.

`<from_id>` is your user account, you can get the account id by:

```bash
$ node packages/tools/lib/godwoken-cli.js getAccountIdByScriptHash <script_hash>
```

You can get the script hash when deposition finished.

## Update creator account config

`polyjuice-cli.js createCreatorAccount` will return an `creator_account_id` and write this to `packages/demo/src/configs/config.json` `creator_account_ids`.

And recompile demo

```bash
yarn workspace @godwoken-examples/demo clean && yarn workspace @godwoken-examples/demo build
```

## Deploy an Ethereum contract

Also supported by Demo Page.

```bash
$ node packages/tools/lib/polyjuice-cli.js deploy <creator_account_id> <init_code> <rollup_type_hash> <privkey>
```

This will output the account id.

The `<init_code>` sample, please see [SimpleStorage.bin](packages/tools/sample-contracts/SimpleStorage.bin) .

## Call an ethereum contract

Also supported by Demo Page.

```bash
$ node packages/tools/lib/polyjuice-cli.js call <to_id> <input_data> <rollup_type_hash> <privkey>
```

`<to_id>` is the account created in deploy step.

The `<input_data>` is generated by ethabi , please see [SimpleStorage.calls.md](./packages/tools/sample-contracts/SimpleStorage.calls.md) .

