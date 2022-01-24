# Файл манифеста

Файл манифеста `project.yaml` можно рассматривать как входную точку вашего проекта, и он определяет большую часть деталей о том, как SubQuery будет индексировать и преобразовывать данные цепочки.

Манифест может быть в формате YAML или JSON. В этом документе мы будем использовать YAML во всех примерах. Ниже приведен стандартный пример базового файла Манифест `project.yaml`.

<CodeGroup> <CodeGroupItem title="v0.2.0" active> ``` yml specVersion: 0.2.0 name: example-project # Provide the project name version: 1.0.0  # Project version description: '' # Description of your project repository: 'https://github.com/subquery/subql-starter' # Git repository address of your project schema: file: ./schema.graphql # The location of your GraphQL schema file network: genesisHash: '0x91b171bb158e2d3848fa23a9f1c25182fb8e20313b2c1eb49219da7a70ce90c3' # Genesis hash of the network endpoint: 'wss://polkadot.api.onfinality.io/public-ws' # Optionally provide the HTTP endpoint of a full chain dictionary to speed up processing dictionary: 'https://api.subquery.network/sq/subquery/dictionary-polkadot' dataSources: - kind: substrate/Runtime startBlock: 1 # This changes your indexing start block, set this higher to skip initial blocks with less data mapping: file: "./dist/index.js" handlers: - handler: handleBlock kind: substrate/BlockHandler - handler: handleEvent kind: substrate/EventHandler filter: #Filter is optional module: balances method: Deposit - handler: handleCall kind: substrate/CallHandler ```` </CodeGroupItem> <CodeGroupItem title="v0.0.1"> ``` yml specVersion: "0.0.1" description: '' # Description of your project repository: 'https://github.com/subquery/subql-starter' # Git repository address of your project schema: ./schema.graphql # The location of your GraphQL schema file network: endpoint: 'wss://polkadot.api.onfinality.io/public-ws' # Optionally provide the HTTP endpoint of a full chain dictionary to speed up processing dictionary: 'https://api.subquery.network/sq/subquery/dictionary-polkadot' dataSources: - name: main kind: substrate/Runtime startBlock: 1 # This changes your indexing start block, set this higher to skip initial blocks with less data mapping: handlers: - handler: handleBlock kind: substrate/BlockHandler - handler: handleEvent kind: substrate/EventHandler filter: #Filter is optional but suggested to speed up event processing module: balances method: Deposit - handler: handleCall kind: substrate/CallHandler ```` </CodeGroupItem> </CodeGroup>

## Migrating from v0.0.1 to v0.2.0 <Badge text="upgrade" type="warning"/>

**If you have a project with specVersion v0.0.1, you can use `subql migrate` to quickly upgrade. [Смотрите здесь](#cli-options) для получения дополнительной информации**

В разделе network

- –Появилось новое обязательное поле genesisHash, которое помогает идентифицировать используемую цепочку.
- В версии 0.2.0 и выше, вы можете ссылаться на внешний [chaintype file](#custom-chains), если вы ссылаетесь на пользовательскую цепь.

В разделе dataSources

- Возможность напрямую связать `index.js` точку входа для обработчиков сопоставления. По умолчанию этот index.js будет сгенерирован из index.ts во время процесса сборки.
- Источники данных теперь могут быть как обычным источником данных во время выполнения, так и пользовательским источником данных.

### Опции CLI

By default the CLI will generate SubQuery projects for spec verison v0.2.0. This behaviour can be overridden by running `subql init --specVersion 0.0.1 PROJECT_NAME`, although this is not recommended as the project will not be supported by the SubQuery hosted service in the future

subql migrate функцию можно запустить в существующем проекте, чтобы мигрировать файл манифест проекта на последнюю версию.

USAGE $ subql init [PROJECTNAME]

ARGUMENTS PROJECTNAME  Give the starter project name

| Параметры               | Описание                                                                     |
| ----------------------- | ---------------------------------------------------------------------------- |
| f, --force              |                                                                              |
| -l, --location=location | local folder to create the project in                                        |
| --install-dependencies  | Install dependencies as well                                                 |
| --npm                   | Force using NPM instead of yarn, only works with `install-dependencies` flag |
| --specVersion=0.0.1     | 0.2.0  [default: 0.2.0] | The spec version to be used by the project         |

## Обзор

### Спецификации верхнего уровня

| Поле                    | v0.0.1                              | v0.2.0                      | Описание                                              |
| ----------------------- | ----------------------------------- | --------------------------- | ----------------------------------------------------- |
| **спецификация версии** | String                              | String                      | 0.0.1 или 0.2.0 - версия спецификации файла манифеста |
| **имя**                 | 𐄂                                   | String                      | Название вашего проекта                               |
| **версия**              | 𐄂                                   | String                      | Версия вашего проекта                                 |
| **описание**            | String                              | String                      | Описание вашего проекта                               |
| **репозиторий**         | String                              | String                      | Адрес Git-репозитория вашего проекта                  |
| **схема**               | String                              | [Schema Spec](#schema-spec) | Расположение вашего файла схемы GraphQL               |
| **сеть**                | [Network Spec](#network-spec)       | Network Spec                | Деталь сети, подлежащей индексированию                |
| **источники данных**    | [DataSource Spec](#datasource-spec) | DataSource Spec             |                                                       |

### Спецификация схемы

| Поле      | v0.0.1 | v0.2.0 | Описание                                |
| --------- | ------ | ------ | --------------------------------------- |
| **файла** | 𐄂      | String | Расположение вашего файла схемы GraphQL |

### Спецификация сети

| Поле               | v0.0.1 | v0.2.0        | Описание                                                                                                                                                                               |
| ------------------ | ------ | ------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **genesisHash**    | 𐄂      | String        | genesis hash сети                                                                                                                                                                      |
| **конечная точка** | String | String        | Определяет конечную точку wss или ws блокчейна для индексирования - Это должен быть узел полного архива. В OnFinality, Вы можете бесплатно получить конечные точки для всех парачейнов |
| **словарь**        | String | String        | Для ускорения обработки рекомендуется предоставлять HTTP конечную точку полного словаря цепочки - смотрите, как работает SubQuery Dictionary.                                          |
| **типы цепей**     | 𐄂      | {file:String} | Путь к файлу с типами цепей, принимается формат .json или .yaml                                                                                                                        |

### Спецификация источника данных

Defines the data that will be filtered and extracted and the location of the mapping function handler for the data transformation to be applied.
| Поле           | v0.0.1                                                    | v0.2.0                                                                           | Описание                                                                                                                                                                              |
| -------------- | --------------------------------------------------------- | -------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **имя**        | String                                                    | 𐄂                                                                                | Name of the data source                                                                                                                                                               |
| **вид**        | [substrate/Runtime](./manifest/#data-sources-and-mapping) | substrate/Runtime, [substrate/CustomDataSource](./manifest/#custom-data-sources) | We supports data type from default substrate runtime such as block, event and extrinsic(call). <br /> From v0.2.0, we support data from custom runtime, such as smart contract. |
| **startBlock** | Integer                                                   | Integer                                                                          | This changes your indexing start block, set this higher to skip initial blocks with less data                                                                                         |
| **mapping**    | Mapping Spec                                              | Mapping Spec                                                                     |                                                                                                                                                                                       |
| **фильтр**     | [network-filters](./manifest/#network-filters)            | 𐄂                                                                                | Filter the data source to execute by the network endpoint spec name                                                                                                                   |

### Mapping Spec

| Поле                      | v0.0.1                                                                         | v0.2.0                                                                | Описание                                                                                                                                                                                                                                                                                           |
| ------------------------- | ------------------------------------------------------------------------------ | --------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **файла**                 | String                                                                         | 𐄂                                                                     | Путь к записи сопоставления                                                                                                                                                                                                                                                                        |
| **обработчики и фильтры** | [Обработчики и фильтры по умолчанию](./manifest/#mapping-handlers-and-filters) | Базовые обработчики и фильтры, Пользовательские обработчики и фильтры | Перечислите все [функции сопоставления](./mapping.md) и их соответствующие типы обработчиков, с дополнительными фильтрами сопоставления. Для получения информации о пользовательских обработчиках отображения времени выполнения, пожалуйста, просмотрите раздел Пользовательские источники данных |

## Источники данных и картирование

In this section, we will talk about the default substrate runtime and its mapping. Here is an example:

```yaml
dataSources:
  - kind: substrate/Runtime # Указывает, что это время выполнения по умолчанию
    startBlock: 1 # Это изменяет начальный блок индексирования, установите его выше, чтобы пропускать начальные блоки с меньшим количеством данных
    mapping:
      file: dist/index.js # Путь входа для этого отображения
```

### Обработчики и фильтры по умолчанию

The following table explains filters supported by different handlers.

**Your SubQuery project will be much more efficient when you only use event and call handlers with appropriate mapping filters**

| Обработчик                                       | Поддерживаемый фильтр |
| ------------------------------------------------ | --------------------- |
| [Обработчик блоков](./mapping.md#block-handler)  | `спецификация версии` |
| [Обработчик событий](./mapping.md#event-handler) | модуль,метод          |
| [Обработчик вызовов](./mapping.md#call-handler)  | модуль,метод ,успех   |

Default runtime mapping filters are an extremely useful feature to decide what block, event, or extrinsic will trigger a mapping handler.

Only incoming data that satisfy the filter conditions will be processed by the mapping functions. Mapping filters are optional but are highly recommended as they significantly reduce the amount of data processed by your SubQuery project and will improve indexing performance.

```yaml
# Пример фильтра из обработчика вызовов
filter: 
   module: balances
   method: Deposit
   success: true
```

- Фильтры модулей и методов поддерживаются на любой цепи субстрата.
- Фильтр success принимает логическое значение и используется для фильтрации по статусу успеха.
- Фильтр по спецификации  определяет диапазон версии спецификации для блока субстрата. В следующих примерах описано, как выставить диапазоны версий.

```yaml
filter:
  specVersion: [23, 24] #Index блок с specVersion в диапазоне от 23 до 24 (включительно).
  specVersion: [100]      #Index блок со спецификацией больше или равно 100.
  specVersion: [null, 23] #Индекс блок со специализацией равной или менее 23.
```

## Пользовательские цепочки

### Спецификация сети

When connecting to a different Polkadot parachain or even a custom substrate chain, you'll need to edit the [Network Spec](#network-spec) section of this manifest.

The `genesisHash` must always be the hash of the first block of the custom network. You can retireve this easily by going to [PolkadotJS](https://polkadot.js.org/apps/?rpc=wss%3A%2F%2Fkusama.api.onfinality.io%2Fpublic-ws#/explorer/query/0) and looking for the hash on **block 0** (see the image below).

![Genesis Hash](/assets/img/genesis-hash.jpg)

Additionally you will need to update the `endpoint`. This defines the wss endpoint of the blockchain to be indexed - **This must be a full archive node**. В OnFinality, Вы можете бесплатно получить конечные точки для всех парачейнов

### Типы цепи

You can index data from custom chains by also including chain types in the manifest.

We support the additional types used by substrate runtime modules, `typesAlias`, `typesBundle`, `typesChain`, and `typesSpec` are also supported.

In the v0.2.0 example below, the `network.chaintypes` are pointing to a file that has all the custom types included, This is a standard chainspec file that declares the specific types supported by this blockchain in either `.json`, `.yaml` or `.js` format.

<CodeGroup> <CodeGroupItem title="v0.2.0" active> ``` yml network: genesisHash: '0x91b171bb158e2d3848fa23a9f1c25182fb8e20313b2c1eb49219da7a70ce90c3' endpoint: 'ws://host.kittychain.io/public-ws' chaintypes: file: ./types.json # The relative filepath to where custom types are stored ... ``` </CodeGroupItem>
<CodeGroupItem title="v0.0.1"> ``` yml ... network: endpoint: "ws://host.kittychain.io/public-ws" types: { "KittyIndex": "u32", "Kitty": "[u8; 16]" } # typesChain: { chain: { Type5: 'example' } } # typesSpec: { spec: { Type6: 'example' } } dataSources: - name: runtime kind: substrate/Runtime startBlock: 1 filter:  #Optional specName: kitty-chain mapping: handlers: - handler: handleKittyBred kind: substrate/CallHandler filter: module: kitties method: breed success: true ``` </CodeGroupItem> </CodeGroup>

To use typescript for your chain types file include it in the `src` folder (e.g. `./src/types.ts`), run `yarn build` and then point to the generated js file located in the `dist` folder.

```yml
network:
  chaintypes:
    file: ./dist/types.js # Будет сгенерирован после yarn run build
...
```

Things to note about using the chain types file with extension `.ts` or `.js`:

- Версия вашего manifest должна быть v0.2.0 или выше.
- При выборке блоков только экспорт по умолчанию будет включен в polkadot api.

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
