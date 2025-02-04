# Cờ dòng lệnh

## subql-node

### --help

Cờ hiển thị các tùy chọn trợ giúp.

```shell
> subql-node --help
Options:
      --help                Show help                                  [boolean]
      --version             Show version number                        [boolean]
  -f, --subquery            Local path of the subquery project          [string]
      --subquery-name       Name of the subquery project                [string]
  -c, --config              Specify configuration file                  [string]
      --local               Use local mode                             [boolean]
      --force-clean         Force clean the database, dropping project schemas
                            and tables                                 [boolean]
      --batch-size          Batch size of blocks to fetch in one round  [number]
      --timeout             Timeout for indexer sandbox to execute the mapping
                            functions                                   [number]
      --debug               Show debug information to console output. will
                            forcefully set log level to debug
                                                      [boolean] [default: false]
      --profiler            Show profiler information to console output
                                                      [boolean] [default: false]
      --network-endpoint    Blockchain network endpoint to connect      [string]
      --output-fmt          Print log as json or plain text
                                           [string] [choices: "json", "colored"]
      --log-level           Specify log level to print. Ignored when --debug is
                            used
          [string] [choices: "fatal", "error", "warn", "info", "debug", "trace",
                                                                       "silent"]
      --migrate             Migrate db schema (for management tables only)
                                                      [boolean] [default: false]
      --timestamp-field     Enable/disable created_at and updated_at in schema
                                                       [boolean] [default: true]
  -d, --network-dictionary  Specify the dictionary api for this network [string]
  -m, --mmr-path            Local path of the merkle mountain range (.mmr) file
                                                                        [string]
      --proof-of-index      Enable/disable proof of index
                                                      [boolean] [default: false]
  -p, --port                The port the service will bind to
                                                        [number] [default: 3000]
```

### --version

Cờ sẽ hiển thị phiên bản hiện tại.

```shell
> subql-node --version
0.19.1
```

### -f, --subquery

Sử dụng cờ này để bắt đầu dự án SubQuery.

```shell
subql-node -f . // Hoặc
subql-node --subquery .
```

### --subquery-name

Cờ này cho phép bạn cung cấp tên cho dự án của mình, tên này hoạt động như thể nó tạo ra một phiên bản của dự án của bạn. Sau khi cung cấp một tên mới, một lược đồ cơ sở dữ liệu mới được tạo và đồng bộ hóa khối bắt đầu từ số không.

```shell
subql-node -f . --subquery-name=test2
```

### -c, --config

Tất cả các cấu hình khác nhau này có thể được đặt vào tệp .yml hoặc .json và sau đó được tham chiếu với cờ cấu hình.

Tệp subquery_config.yml mẫu:

```shell
subquery: . // Mandatory. This is the local path of the project. The period here means the current local directory.
subqueryName: hello // Optional name
batchSize: 55 // Optional config
```

Đặt tệp này trong cùng thư mục với dự án. Sau đó, trong thư mục dự án hiện tại, hãy chạy:

```shell
> subql-node -c ./subquery_config.yml
```

### --local

Cờ này chủ yếu được sử dụng cho mục đích gỡ lỗi trong đó nó tạo bảng starter_entity mặc định trong lược đồ "postgres" mặc định.

```shell
subql-node -f . --local
```

Lưu ý rằng một khi bạn sử dụng cờ này, việc loại bỏ nó sẽ không có nghĩa là nó sẽ trỏ đến một cơ sở dữ liệu khác. Để đặt lại cơ sở dữ liệu khác, bạn sẽ phải tạo một cơ sở dữ liệu MỚI và thay đổi cài đặt env cho cơ sở dữ liệu mới này. Nói cách khác, "export DB_DATABASE =<new_db_here>"

### --force-clean

Cờ này buộc các lược đồ và bảng của dự án phải được tạo lại, hữu ích để sử dụng khi phát triển lặp đi lặp lại các lược đồ graphql sao cho các lần chạy mới của dự án luôn hoạt động ở trạng thái sạch. Lưu ý rằng cờ này cũng sẽ xóa tất cả dữ liệu được lập chỉ mục.

### --batch-size

Cờ này cho phép bạn đặt kích thước lô trong dòng lệnh. Nếu kích thước lô cũng được đặt trong tệp cấu hình, nó sẽ được ưu tiên.

```shell
> subql-node -f . --batch-size=20
2021-08-09T23:24:43.775Z <fetch> INFO fetch block [6601,6620], total 20 blocks
2021-08-09T23:24:45.606Z <fetch> INFO fetch block [6621,6640], total 20 blocks
2021-08-09T23:24:47.415Z <fetch> INFO fetch block [6641,6660], total 20 blocks
2021-08-09T23:24:49.235Z <fetch> INFO fetch block [6661,6680], total 20 blocks
```

<!-- ### --timeout -->

### --debug

Xuất thông tin gỡ lỗi đến đầu ra bảng điều khiển và cài đặt cấp độ nhật ký để gỡ lỗi một cách mạnh mẽ.

```shell
> subql-node -f . --debug
2021-08-10T11:45:39.471Z <db> DEBUG Executing (1b0d0c23-d7c7-4adb-a703-e4e5c414e035): INSERT INTO "subquery_1"."starter_entities" ("id","block_height","created_at","updated_at") VALUES ($1,$2,$3,$4) ON CONFLICT ("id") DO UPDATE SET "id"=EXCLUDED."id","block_height"=EXCLUDED."block_height","updated_at"=EXCLUDED."updated_at" RETURNING "id","block_height","created_at","updated_at";
2021-08-10T11:45:39.472Z <db> DEBUG Executing (default): UPDATE "subqueries" SET "next_block_height"=$1,"updated_at"=$2 WHERE "id" = $3
2021-08-10T11:45:39.472Z <db> DEBUG Executing (1b0d0c23-d7c7-4adb-a703-e4e5c414e035): COMMIT;
```

### --profiler

Hiển thị thông tin hồ sơ.

```shell
subql-node -f . --local --profiler
2021-08-10T10:57:07.234Z <profiler> INFO FetchService, fetchMeta, 3876 ms
2021-08-10T10:57:08.095Z <profiler> INFO FetchService, fetchMeta, 774 ms
2021-08-10T10:57:10.361Z <profiler> INFO SubstrateUtil, fetchBlocksBatches, 2265 ms
2021-08-10T10:57:10.361Z <fetch> INFO fetch block [3801,3900], total 100 blocks
```

### --network-endpoint

Cờ này cho phép người dùng ghi đè cấu hình điểm cuối mạng từ tệp kê khai.

```shell
subql-node -f . --network-endpoint="wss://polkadot.api.onfinality.io/public-ws"
```

Lưu ý rằng đoạn này cũng phải được đặt trong tệp kê khai, nếu không bạn sẽ nhận được:

```shell
ERROR Create Subquery project from given path failed! Error: failed to parse project.yaml.
An instance of ProjectManifestImpl has failed the validation:
 - property network has failed the following constraints: isObject
 - property network.network has failed the following constraints: nestedValidation
```

### --output-fmt

Có hai định dạng đầu ra khác nhau. JSON hoặc colored. Colored là mặc định và chứa văn bản được bôi màu.

```shell
> subql-node -f . --output-fmt=json
{"level":"info","timestamp":"2021-08-10T11:58:18.087Z","pid":24714,"hostname":"P.local","category":"fetch","message":"fetch block [10501,10600], total 100 blocks"}
```

```shell
> subql-node -f . --output-fmt=colored
2021-08-10T11:57:41.480Z <subql-node> INFO node started
(node:24707) [PINODEP007] Warning: bindings.level is deprecated, use options.level option instead
2021-08-10T11:57:48.981Z <fetch> INFO fetch block [10201,10300], total 100 blocks
2021-08-10T11:57:51.862Z <fetch> INFO fetch block [10301,10400], total 100 blocks
```

### --log-level

Có 7 tùy chọn để lựa chọn. “fatal”, “error”, “warn”, “info”, “debug”, “trace”, “silent”. Ví dụ dưới đây cho thấy sự im lặng. Không có gì sẽ được in trong thiết bị đầu cuối vì vậy cách duy nhất để biết nút có hoạt động hay không là truy vấn cơ sở dữ liệu về số hàng (select count(\*) from subquery_1.starter_entities) hoặc truy vấn chiều cao khối.

```shell
> subql-node -f . --log-level=silent
(node:24686) [PINODEP007] Warning: bindings.level is deprecated, use options.level option instead
(Use `node --trace-warnings ...` to show where the warning was created)
(node:24686) [PINODEP007] Warning: bindings.level is deprecated, use options.level option instead
(node:24686) [PINODEP007] Warning: bindings.level is deprecated, use options.level option instead
(node:24686) [PINODEP007] Warning: bindings.level is deprecated, use options.level option instead
(node:24686) [PINODEP007] Warning: bindings.level is deprecated, use options.level option instead
(node:24686) [PINODEP007] Warning: bindings.level is deprecated, use options.level option instead
(node:24686) [PINODEP007] Warning: bindings.level is deprecated, use options.level option instead
(node:24686) [PINODEP007] Warning: bindings.level is deprecated, use options.level option instead
(node:24686) [PINODEP007] Warning: bindings.level is deprecated, use options.level option instead
(node:24686) [DEP0152] DeprecationWarning: Custom PerformanceEntry accessors are deprecated. Please use the detail property.
(node:24686) [PINODEP007] Warning: bindings.level is deprecated, use options.level option instead
```

<!-- ### --migrate TBA -->

### --timestamp-field

Mặc định là true. khi được đặt thành false với:

```shell
> subql-node -f . –timestamp-field=false
```

Thao tác này sẽ xóa các cột created_at và updated_at trong bảng starter_entities.

### -d, --network-dictionary

Điều này cho phép bạn chỉ định điểm cuối từ điển là dịch vụ miễn phí được cung cấp và lưu trữ tại: [https://explorer.subquery.network/](https://explorer.subquery.network/) (tìm kiếm từ điển) và trình bày điểm cuối API là: https://api.subquery.network/sq/subquery/dictionary-polkadot

Thông thường, nó sẽ được đặt trong tệp manifest của bạn nhưng bên dưới cho thấy một ví dụ về việc sử dụng nó làm đối số trong dòng lệnh.

```shell
subql-node -f . -d "https://api.subquery.network/sq/subquery/dictionary-polkadot"
```

[ Đọc thêm về cách thức hoạt động của Từ điển SubQuery](../tutorials_examples/dictionary.md).

## subql-query

### --help

Cờ hiển thị các tùy chọn trợ giúp.

```shell
Options:
      --help        Show help                                          [boolean]
      --version     Show version number                                [boolean]
  -n, --name        Project name                             [string] [required]
      --playground  Enable graphql playground                          [boolean]
      --output-fmt  Print log as json or plain text
                      [string] [choices: "json", "colored"] [default: "colored"]
      --log-level   Specify log level to print.
          [string] [choices: "fatal", "error", "warn", "info", "debug", "trace",
                                                     "silent"] [default: "info"]
      --indexer     Url that allow query to access indexer metadata     [string]
```

### --version

Cờ sẽ hiển thị phiên bản hiện tại.

```shell
> subql-node --version
0.7.0
```

### -n, --name

Cờ này được sử dụng để bắt đầu dịch vụ truy vấn. Nếu cờ - subquery-name không được cung cấp khi chạy trình lập chỉ mục, thì tên ở đây sẽ tham chiếu đến tên dự án mặc định. Nếu - subquery-name được đặt, thì tên ở đây phải khớp với những gì đã được đặt.

```shell
> subql-node -f . // --subquery-name not set

> subql-query -n subql-helloworld  --playground // the name defaults to the project directory name
```

```shell
> subql-node -f . --subquery-name=hiworld // --subquery-name set

> subql-query -n hiworld --playground  // the name points to the subql-helloworld project but with the name of hiworld
```

### --playground

Cờ này cho phép sân chơi graphql hoạt động, vì vậy nó luôn được thêm vào theo mặc định để sử dụng.

### --output-fmt

Xem [--output-fmt](https://doc.subquery.network/references/references.html#output-fmt)

### --log-level

Xem [--log-level](https://doc.subquery.network/references/references.html#log-level)

<!-- ### --indexer TBA -->
