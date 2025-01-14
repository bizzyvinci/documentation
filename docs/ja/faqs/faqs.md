# よくある質問

## SubQueryとは?

SubQueryは開発者がSubstrateチェーンのデータにインデックスを付け、変換し、クエリを実行して、アプリケーションを強化するためのオープンソースプロジェクトです。

また、SubQueryは開発者向けにプロジェクトの高品質のホスティングを無料で提供しているため、インフラ管理の責任を負うことなく、開発者はビルドに最善を尽くす事ができます。

## SubQueryを始めるための最良の方法は何ですか?

SubQueryを始める最良の方法は、 [Hello Worldチュートリアル](../quickstart/helloworld-localhost.md) を試してみることです。 これはスターターテンプレートをダウンロードし、プロジェクトを構築するための簡単な5分です。 次にDocker を使用して、localhost上でノードを実行し、単純なクエリを実行します。

## SubQueryに貢献したりフィードバックを与えたりするにはどうすればいいですか?

私たちはコミュニティからの貢献とフィードバックが大好きです。 コードに貢献するためには、関心のあるリポジトリをフォークして変更を加えます。 次にPRまたはPullリクエストを送信します。 ああ、テストすることを忘れないでください! 私たちの貢献ガイドラインもチェックしてください(近日公開)。

フィードバックをいただくには、hello@subquery.network までお問い合わせいただくか、 [discordチャンネル](https://discord.com/invite/78zg8aBSMG)に参加してください。

## 自分のプロジェクトをSubQuery Projectsで公開するにはどのくらいの費用がかかりますか？

SubQuery Projectsであなたのプロジェクトを公開することは完全に無料です - それはコミュニティに還元する私たちの方法です。 プロジェクトを公開する方法については、 [Hello World (SubQuery Hosted)](../quickstart/helloworld-hosted.md) チュートリアルをご覧ください。

## デプロイスロットとは何ですか？

デプロイスロットは、 [SubQuery Projects](https://project.subquery.network) の開発環境と同等の機能です。 例えば、ソフトウェアを扱う組織では、最低でも通常、開発環境と本番環境が存在します（localhostは無視します）。 一般的には、組織のニーズや開発体制に応じて、ステージング環境やプリプロ環境、さらには本番前環境などの追加環境が含まれます。

SubQuery には現在 2 つのスロットがあります。 ステージングスロットと本番環境スロット。 これにより、開発者はSubQueryをステージング環境にデプロイし、ボタンをクリックするだけで「本番環境への反映」を行うことができます。

## ステージングスロットの利点は何ですか?

ステージングスロットを使用する主な利点は、公開せずに SubQuery プロジェクトの新しいリリースを準備できることです。 本番アプリケーションに影響を与えることなく、ステージングスロットがすべてのデータに対してインデックス再作成するのを待つことができます。

[エクスプローラ](https://explorer.subquery.network/) では、ステージングスロットは一般には表示されず、あなただけに表示される固有のURLを持っています。 もちろん、個別の環境では、プロダクションに影響を与えずに新しいコードをテストすることができます。

## 外部関数とは何ですか?

すでにブロックチェーンの概念に慣れている人は、外部関数をトランザクションに匹敵するものと考えることができます。 より正式には、外部関数とはチェーンの外から来て、ブロックに含まれる情報のことです。 外部関数には3つのカテゴリーがあります。 これらは継承、署名されたトランザクション、および署名されていないトランザクションです。

固有の外部関数とは、署名されておらず、ブロック作成者によってのみブロックに挿入される情報のことです。

署名されたトランザクションの外部関数は、トランザクションを発行したアカウントの署名を含むトランザクションです。 それらは、取引がチェーンに含まれるための手数料を支払うことになります。

署名されてないトランザクションの外部関数 とは、トランザクションを発行したアカウントの署名を含まないトランザクションです。 署名されていないトランザクションの外部関数は、署名されているがゆえに、誰も手数料を支払っていないので、注意して使用する必要があります。 このため、トランザクションキューはスパムを防ぐための経済的ロジックを欠いています。

詳細については、 [ここ](https://substrate.dev/docs/en/knowledgebase/learn-substrate/extrinsics) をクリックしてください。

## Kusamaネットワークのエンドポイントとは何ですか?

Kusama ネットワークのエンドポイントは `wss://kusama.api.onfinality.io/public-ws` です。

## Polkadot メインネット のエンドポイントとは何ですか?

Polkadotネットワークのエンドポイントは `wss://polkadot.api.onfinality.io/public-ws` です。

## How do I iteratively develop my project schema?

A known issue with developing a changing project schema is that when lauching your Subquery node for testing, the previously indexed blocks will be incompatible with your new schema. In order to iteratively develop schemas the indexed blocks stored in the database must be cleared, this can be achieved by launching your node with the `--force-clean` flag. 例

```shell
subql-node -f . --force-clean --subquery-name=<project-name>
```

Note that it is recommended to use `--force-clean` when changing the `startBlock` within the project manifest (`project.yaml`) in order to begin reindexing from the configured block. If `startBlock` is changed without a `--force-clean` of the project then the indexer will continue indexing with the previously configured `startBlock`.