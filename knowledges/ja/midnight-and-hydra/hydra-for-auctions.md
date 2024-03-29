---
title: オークション用のヒドラ
date: '2020-10-06 08:48:23 +0000'
lastmod: '2020-10-06 08:48:23 +0000'
draft: 'false'
images: []
---

## Cardano の NFT マーケットプレイス

Cardano は、非代替トークン (NFT) の鋳造と送信を安価かつ簡単にします。これは、非 ADA トークンの会計システムが、複雑でバグが発生しやすいカスタム スマート コントラクトではなく台帳自体で (ADA とともに) ホストされているためです。これにより、カルダノ上にアート、音楽、アイデンティティ、不動産、ゲーム、サービスのサブスクリプションなどの活気に満ちたNFTエコシステムが誕生しました。

Cardano には、あらゆる種類の NFT をリスト、表示、購入できる高品質のマーケットプレイスが存在します。これらのNFTマーケットプレイスには、画像/アニメーション、希少性チャート、ロイヤルティ条件、その他のNFTのメタデータをきちんと表示できる非常に使いやすいユーザーインターフェイスがあり、販売者の定価で購入することも、購入者の代替オファーを通じて購入することもできます。

ただし、カルダノでトークン化されるデジタル資産と非デジタル資産の目新しさと、市場規模が比較的小さいため、NFT分野では価格発見が課題となっています。

## Cardano と現在の制約に関する NFT オークション

オークションは、販売される商品が斬新またはユニークな場合（美術品など）、最も効率的な割り当てを事前に決定することが難しい場合（電波スペクトル）、またはインサイダー取引や共謀の懸念がある場合（破産）、価格を発見するための効率的なメカニズムです。投げ売り）。従来のマーケットプレイスでは、売り手が特定の価格で商品を出品し、最初に一致した購入者がその価格で商品を購入できます。売り手の価格設定が低すぎる場合、買い手はすぐにその商品を再出品して販売します。それは裁定利益のためです。オークション市場では、売り手は、競争入札プロセスで価格が落ち着くのを待ってから、最高入札者に商品を販売することができます。

オークションは活発かつ効率的でなければなりません。入札者は限られた時間と注意力の範囲内で (多くの場合偶然に) オークションに参加し、入札を行ったり、他の入札者の入札を観察したりするのに時間がかかりすぎる場合は、オークションから去ります。このビッダー エクスペリエンスを Cardano のメイン ネットワーク (L1) に直接実装するのは困難です。L1 トランザクションは、ブロックに追加されるまでに時間がかかり (約 10 ～ 60 秒)、ロールバックの可能性が十分に低い (数分から数時間) と確認されます。

## Hydra を使用したオークションの実行

ただし、Hydra ヘッド内のトランザクションは確認遅延が短く、即時に完了するため、オークション入札メカニズムは、Hydra テクノロジーを介して L2 で拡張するのに最適な候補です。 Hydra の強みを活用した方法で開発すれば、L2 を利用した魅力的なオークション サービスを Cardano 上に構築できます。

私たちは、Hydra ベースのオークション フレームワークが標準のモジュラー コンポーネントとなり、NFT マーケットプレイス、ゲーム、その他の Web 3.0 アプリケーションがアーキテクチャにプラグインして、製品にデジタル アセット オークションを追加することを想定しています。さらに、ステークプール エコシステムや新たなガバナンス エコシステムと同様に、プロのスケーラビリティ プロバイダーがこれらのアプリケーションに L2 ホスティング サービスを提供するための新しいビジネス エコシステムを刺激します。

## 前進への道

このビジョンを実現するための 1 つの可能なパスは、Hydra とその予想される改善を使用して実装可能であると考えられる次の一連のマイルストーンに沿って進む可能性があります。

- 委任バウチャー招待状
- 委任されたバウチャーがオープンしました
- 委任されたバウチャー オークション用の SDK
- サービスとしてのオークション シングル
- サービスとしてのオークション マルチ
