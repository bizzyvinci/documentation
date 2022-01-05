# Manifest File

ไฟล์ Manifest `project.yaml` สามารถเห็นได้จากจุดเริ่มต้นโครงการของคุณ โดยจะใช้กำหนดรายละเอียดเกือบทั้งหมด ว่า SubQuery จะกำหนดดัชนีและแปลงข้อมูลที่อยู่บน Chain ได้อย่างไร

Manifest สามารถอยู่ในรูป YAML หรือ JSON ในคู่มือนี้ เราจะใช้ตัวอย่างเป็น YAML ด้านล่างจะเป็ฯตัวอย่างมาตรฐานของ `project.yaml`

<CodeGroup> <CodeGroupItem title="v0.2.0" active> ``` yml specVersion: 0.2.0 name: example-project # Provide the project name version: 1.0.0  # Project version description: '' # Description of your project repository: 'https://github.com/subquery/subql-starter' # Git repository address of your project schema: file: ./schema.graphql # The location of your GraphQL schema file network: genesisHash: '0x91b171bb158e2d3848fa23a9f1c25182fb8e20313b2c1eb49219da7a70ce90c3' # Genesis hash of the network endpoint: 'wss://polkadot.api.onfinality.io/public-ws' # Optionally provide the HTTP endpoint of a full chain dictionary to speed up processing dictionary: 'https://api.subquery.network/sq/subquery/dictionary-polkadot' dataSources: - kind: substrate/Runtime startBlock: 1 # This changes your indexing start block, set this higher to skip initial blocks with less data mapping: file: "./dist/index.js" handlers: - handler: handleBlock kind: substrate/BlockHandler - handler: handleEvent kind: substrate/EventHandler filter: #Filter is optional module: balances method: Deposit - handler: handleCall kind: substrate/CallHandler ```` </CodeGroupItem> <CodeGroupItem title="v0.0.1"> ``` yml specVersion: "0.0.1" description: '' # Description of your project repository: 'https://github.com/subquery/subql-starter' # Git repository address of your project schema: ./schema.graphql # The location of your GraphQL schema file network: endpoint: 'wss://polkadot.api.onfinality.io/public-ws' # Optionally provide the HTTP endpoint of a full chain dictionary to speed up processing dictionary: 'https://api.subquery.network/sq/subquery/dictionary-polkadot' dataSources: - name: main kind: substrate/Runtime startBlock: 1 # This changes your indexing start block, set this higher to skip initial blocks with less data mapping: handlers: - handler: handleBlock kind: substrate/BlockHandler - handler: handleEvent kind: substrate/EventHandler filter: #Filter is optional but suggested to speed up event processing module: balances method: Deposit - handler: handleCall kind: substrate/CallHandler ```` </CodeGroupItem> </CodeGroup>

## การอัพเกรดเวอชันจาก v0.0.1 สู่ v0.2.0 <Badge text="upgrade" type="warning"/>

**ถ้าคุณมีโปรเจ็กต์ที่ใช้ specVersion v0.0..1 คุณสามารถใช้คำสั่ง `subql migrate` เพื่ออัพเกรดอย่างรวดเร็ว [ดูที่นี่](#cli-options) สำหรับข้อมูลเพิ่มเติม**

ภายใต้ `network`

- นี่คือ field ที่ถูกสร้างขึ้นใหม่ **required** `genesisHash` ซึ่งจะช่วยให้คุณรู้ว่ากำลังใช้ chain ใดอยู่
- สำหรับ v0.2.0 หรือสูงกว่า คุณสามารถอ้างอิงถึงไฟล์ภายนอก [chaintype file](#custom-chains) ถ้าหากคุณกำลังอ้างอิงถึง chain ที่คุณกำหนดเอง

ภายใต้ `dataSources`:

- สามารถเชื่อมไปยังจุดเริ่มต้น `index.js` สำหรับ mapping handlers โดยพื้นฐาน `index.js` จะถูกสร้างขึ้นจาก `index.ts` ในกระบวนการ build
- Data sources สามารถเป็นได้ทั้ง data source ทั่วไป หรือ [custom data source](#custom-data-sources)

### CLI Options

ในขณะ v0.2.0 spec version อยู่ในช่วง beta คุณจำเป็นจำต้องกำหนดในขณะที่คุณสร้างโปรเจ็กต์ ด้วยคำสั่ง `subql init --specVersion 0.2.0 PROJECT_NAME`

`subql migrate` สามารถรันอยู่บนโปรเจ็กต์ที่เกิดขึ้นแล้ว เพื่ออัพเกรดไปสู่ project manifest เวอชันล่าสุดได้

| Options        | คำอธิบาย                                                      |
| -------------- | ------------------------------------------------------------- |
| -f, --force    |                                                               |
| -l, --location | local folder ที่จะใช้ migrate (จำเป็นต้องมีไฟล์ project.yaml) |
| --file=file    | ใช้ระบุ project.yaml ที่ต้องการ migrate                       |

## ภาพรวม

### Top Level Spec

| Field           | v0.0.1                              | v0.2.0                      | คำอธิบาย                                             |
| --------------- | ----------------------------------- | --------------------------- | ---------------------------------------------------- |
| **specVersion** | String                              | String                      | `0.0.1` หรือ `0.2.0` - spec version ของไฟล์ manifest |
| **name**        | 𐄂                                   | String                      | ชื่อโปรเจ็กต์ของคุณ                                  |
| **version**     | 𐄂                                   | String                      | เวอร์ชั่นโปรเจ็กต์ของคุณ                             |
| **description** | String                              | String                      | คำอธิบายโปรเจ็กต์ของคุณ                              |
| **repository**  | String                              | String                      | ตำแหน่ง Git repository ของโปรเจ็กต์คุณ               |
| **schema**      | String                              | [Schema Spec](#schema-spec) | ตำแหน่งไฟล์ GraphQL schema ของคุณ                    |
| **network**     | [Network Spec](#network-spec)       | Network Spec                | รายละเอียดของเครือข่ายที่จะถูกนำมา index             |
| **dataSources** | [DataSource Spec](#datasource-spec) | DataSource Spec             |                                                      |

### Schema Spec

| Field    | v0.0.1 | v0.2.0 | คำอธิบาย                          |
| -------- | ------ | ------ | --------------------------------- |
| **file** | 𐄂      | String | ตำแหน่งไฟล์ GraphQL schema ของคุณ |

### Network Spec

| Field           | v0.0.1 | v0.2.0        | คำอธิบาย                                                                                                                                                                                   |
| --------------- | ------ | ------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| **genesisHash** | 𐄂      | String        | Genesis Hash ของเครือข่าย                                                                                                                                                                  |
| **endpoint**    | String | String        | กำหนด wss หรือ ws ปลายทางของ blockchain ที่ต้องการ index - **จำเป็นต้องเป็น full archive node** คุณสามารถดึงปลายทางได้จากทุก parachains ได้ฟรี จาก [OnFinality](https://app.onfinality.io) |
| **dictionary**  | String | String        | แนะนำให้คุณกำ HTTP เป็นปลายทางของโฟลเดอร์ full chain เพื่อเพิ่มความเร็วในการทำงาน - อ่าน [how a subQuery Dictionary works](../tutorials_examples/dictionary.md)                            |
| **chaintypes**  | 𐄂      | {file:String} | เส้นทางไปยัง chain types file รองรับรูปแบบ `.json` หรือ `.yaml`                                                                                                                            |

### Datasource Spec

กำหนดข้อมูลที่จะถูกกรองหรือดึงออกจากตำแหน่งของ mapping function handler เพื่อนำมาใช้กับข้อมูลที่ถูกแปลง
| Field          | v0.0.1                                                    | v0.2.0                                                                           | คำอธิบาย                                                                                                                                                                              |
| -------------- | --------------------------------------------------------- | -------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **name**       | String                                                    | 𐄂                                                                                | Name of the data source                                                                                                                                                               |
| **kind**       | [substrate/Runtime](./manifest/#data-sources-and-mapping) | substrate/Runtime, [substrate/CustomDataSource](./manifest/#custom-data-sources) | We supports data type from default substrate runtime such as block, event and extrinsic(call). <br /> From v0.2.0, we support data from custom runtime, such as smart contract. |
| **startBlock** | Integer                                                   | Integer                                                                          | This changes your indexing start block, set this higher to skip initial blocks with less data                                                                                         |
| **mapping**    | Mapping Spec                                              | Mapping Spec                                                                     |                                                                                                                                                                                       |
| **filter**     | [network-filters](./manifest/#network-filters)            | 𐄂                                                                                | Filter the data source to execute by the network endpoint spec name                                                                                                                   |

### Mapping Spec

| Field                  | v0.0.1                                                                   | v0.2.0                                                                                        | คำอธิบาย                                                                                                                                                                                                                                     |
| ---------------------- | ------------------------------------------------------------------------ | --------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **file**               | String                                                                   | 𐄂                                                                                             | Path to the mapping entry                                                                                                                                                                                                                    |
| **handlers & filters** | [Default handlers and filters](./manifest/#mapping-handlers-and-filters) | Default handlers and filters, <br />[Custom handlers and filters](#custom-data-sources) | List all the [mapping functions](./mapping.md) and their corresponding handler types, with additional mapping filters. <br /><br /> For custom runtimes mapping handlers please view [Custom data sources](#custom-data-sources) |

## Data Sources and Mapping

In this section, we will talk about the default substrate runtime and its mapping. Here is an example:

```yaml
dataSources:
  - kind: substrate/Runtime # Indicates that this is default runtime
    startBlock: 1 # This changes your indexing start block, set this higher to skip initial blocks with less data
    mapping:
      file: dist/index.js # Entry path for this mapping
```

### Mapping handlers and Filters

The following table explains filters supported by different handlers.

**Your SubQuery project will be much more efficient when you only use event and call handlers with appropriate mapping filters**

| Handler                                    | Supported filter             |
| ------------------------------------------ | ---------------------------- |
| [BlockHandler](./mapping.md#block-handler) | `specVersion`                |
| [EventHandler](./mapping.md#event-handler) | `module`,`method`            |
| [CallHandler](./mapping.md#call-handler)   | `module`,`method` ,`success` |

Default runtime mapping filters are an extremely useful feature to decide what block, event, or extrinsic will trigger a mapping handler.

Only incoming data that satisfy the filter conditions will be processed by the mapping functions. Mapping filters are optional but are highly recommended as they significantly reduce the amount of data processed by your SubQuery project and will improve indexing performance.

```yaml
# Example filter from callHandler
filter:
  module: balances
  method: Deposit
  success: true
```

- Module and method filters are supported on any substrate-based chain.
- The `success` filter takes a boolean value and can be used to filter the extrinsic by its success status.
- The `specVersion` filter specifies the spec version range for a substrate block. The following examples describe how to set version ranges.

```yaml
filter:
  specVersion: [23, 24]   # Index block with specVersion in between 23 and 24 (inclusive).
  specVersion: [100]      # Index block with specVersion greater than or equal 100.
  specVersion: [null, 23] # Index block with specVersion less than or equal 23.
```

## Custom Chains

### Network Spec

When connecting to a different Polkadot parachain or even a custom substrate chain, you'll need to edit the [Network Spec](#network-spec) section of this manifest.

The `genesisHash` must always be the hash of the first block of the custom network. You can retireve this easily by going to [PolkadotJS](https://polkadot.js.org/apps/?rpc=wss%3A%2F%2Fkusama.api.onfinality.io%2Fpublic-ws#/explorer/query/0) and looking for the hash on **block 0** (see the image below).

![Genesis Hash](/assets/img/genesis-hash.jpg)

Additionally you will need to update the `endpoint`. This defines the wss endpoint of the blockchain to be indexed - **This must be a full archive node**. คุณสามารถดึงปลายทางได้จากทุก parachains ได้ฟรี จาก [OnFinality](https://app.onfinality.io)

### Chain Types

You can index data from custom chains by also including chain types in the manifest.

We support the additional types used by substrate runtime modules, `typesAlias`, `typesBundle`, `typesChain`, and `typesSpec` are also supported.

In the v0.2.0 example below, the `network.chaintypes` are pointing to a file that has all the custom types included, This is a standard chainspec file that declares the specific types supported by this blockchain in either `.json`, `.yaml` or `.js` format.

<CodeGroup> <CodeGroupItem title="v0.2.0" active> ``` yml network: genesisHash: '0x91b171bb158e2d3848fa23a9f1c25182fb8e20313b2c1eb49219da7a70ce90c3' endpoint: 'ws://host.kittychain.io/public-ws' chaintypes: file: ./types.json # The relative filepath to where custom types are stored ... ``` </CodeGroupItem> <CodeGroupItem title="v0.0.1"> ``` yml ... network: endpoint: "ws://host.kittychain.io/public-ws" types: { "KittyIndex": "u32", "Kitty": "[u8; 16]" } # typesChain: { chain: { Type5: 'example' } } # typesSpec: { spec: { Type6: 'example' } } dataSources: - name: runtime kind: substrate/Runtime startBlock: 1 filter:  #Optional specName: kitty-chain mapping: handlers: - handler: handleKittyBred kind: substrate/CallHandler filter: module: kitties method: breed success: true ``` </CodeGroupItem> </CodeGroup>

To use typescript for your chain types file include it in the `src` folder (e.g. `./src/types.ts`), run `yarn build` and then point to the generated js file located in the `dist` folder.

```yml
network:
  chaintypes:
    file: ./dist/types.js # Will be generated after yarn run build
...
```

Things to note about using the chain types file with extension `.ts` or `.js`:

- Your manifest version must be v0.2.0 or above.
- Only the default export will be included in the [polkadot api](https://polkadot.js.org/docs/api/start/types.extend/) when fetching blocks.

Here is an example of a `.ts` chain types file:

<CodeGroup> <CodeGroupItem title="types.ts"> ```ts
import { typesBundleDeprecated } from "moonbeam-types-bundle"
export default { typesBundle: typesBundleDeprecated }; ``` </CodeGroupItem> </CodeGroup>

## Custom Data Sources

Custom Data Sources provide network specific functionality that makes dealing with data easier. They act as a middleware that can provide extra filtering and data transformation.

A good example of this is EVM support, having a custom data source processor for EVM means that you can filter at the EVM level (e.g. filter contract methods or logs) and data is transformed into structures farmiliar to the Ethereum ecosystem as well as parsing parameters with ABIs.

Custom Data Sources can be used with normal data sources.

Here is a list of supported custom datasources:

| Kind                                                  | Supported Handlers                                                                                       | Filters                         | Description                                                                      |
| ----------------------------------------------------- | -------------------------------------------------------------------------------------------------------- | ------------------------------- | -------------------------------------------------------------------------------- |
| [substrate/Moonbeam](./moonbeam/#data-source-example) | [substrate/MoonbeamEvent](./moonbeam/#moonbeamevent), [substrate/MoonbeamCall](./moonbeam/#moonbeamcall) | See filters under each handlers | Provides easy interaction with EVM transactions and events on Moonbeams networks |

## Network Filters

**Network filters only applies to manifest spec v0.0.1**.

Usually the user will create a SubQuery and expect to reuse it for both their testnet and mainnet environments (e.g Polkadot and Kusama). Between networks, various options are likely to be different (e.g. index start block). Therefore, we allow users to define different details for each data source which means that one SubQuery project can still be used across multiple networks.

Users can add a `filter` on `dataSources` to decide which data source to run on each network.

Below is an example that shows different data sources for both the Polkadot and Kusama networks.

<CodeGroup> <CodeGroupItem title="v0.0.1"> ```yaml --- network: endpoint: 'wss://polkadot.api.onfinality.io/public-ws' #Create a template to avoid redundancy definitions: mapping: &mymapping handlers: - handler: handleBlock kind: substrate/BlockHandler dataSources: - name: polkadotRuntime kind: substrate/Runtime filter: #Optional specName: polkadot startBlock: 1000 mapping: *mymapping #use template here - name: kusamaRuntime kind: substrate/Runtime filter: specName: kusama startBlock: 12000 mapping: *mymapping # can reuse or change ``` </CodeGroupItem>

</CodeGroup>
