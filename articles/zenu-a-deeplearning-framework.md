---
title: "[Rust] Zenuという深層学習フレームワークを開発しています"
emoji: "🦀"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["Rust", "DeepLearning"]
published: false
---

https://github.com/bokutotu/zenu

# TL;DR
- zenuは高速で使いやすい新しい深層学習フレームワークを開発しています。
- Rust言語で開発され、安全性が高い
- PyTorchと同等の速度で動作(nvidia gpuの場合のみ)し、使い方も似ている
- 今後、より多くのモデルや分散学習のサポートなどの機能拡張を予定
- フルスクラッチで書かれている(rust-ndarray非依存)

# 名前の由来

名前の由来は、冒頓単于です。
GitHubのユーザー名が [bokutotu](github.com/bokutotu) で、クレート名が[zenu](github.com/bokutotu/zenu)だと、合わせてbokutotuzenu(ぼくとつぜんう)となります。


# zenuの特徴

* GPU(Nvidia)対応 : NvidiaのGPUを使って、高速に学習や予測ができます。PyTorchと同等の速度を実現しています。(cudnnを使用)

* Rust言語で開発 : Rust言語で開発されているため、高い安全性を持っています。メモリリークやスレッドセーフなどの問題が少ないので、安心して使えます。

* PyTorchライクな使い方 : Define-by-Runのスタイルを採用しており、PyTorchのような使い方ができます。初心者でも簡単に使えるので、学習コストが低いです。

* フルスクラッチ : Rustのndarrayライブラリを使用せず、フルスクラッチで書かれています。これにより、cudnnを直接使用できます。

# zenuのアーキテクチャ

zenuは複数のクレートから構成されています。

* `zenu-matrix`: 行列演算を行うためのクレート
* `zenu-autograd`: 自動微分を行うためのクレート
* `zenu-optimizer`: 最適化アルゴリズムを提供するクレート
* `zenu-layer`: ニューラルネットワークの層を提供するクレート
* `zenu` : 上記のクレートを統合したメインクレート

# zenuの使い方

zenuがどんなものか、実際に見てみましょう。ここでは、mnistデータセットを使って、手書き数字の分類を行います。

## モデルの定義

```rust
pub struct SimpleModel<D: Device> {
    pub linear_1: Linear<f32, D>,
    pub linear_2: Linear<f32, D>,
}

impl<D: Device> SimpleModel<D> {
    pub fn new() -> Self {
        let linear_1 = Linear::new(784, 128);
        let linear_2 = Linear::new(128, 10);

        SimpleModel { linear_1, linear_2 }
    }
}
```

## 他のフレームワークとの比較

zenuがどれくらい速いのか、具体的に見てみましょう。有名なPyTorchと比べてみました。

| フレームワーク | 学習時間（1回あたり） | 予測時間（1000枚の画像） |
|----------------|----------------------|--------------------------|
| zenu           | X.XX 秒              | X.XX 秒                  |
| PyTorch        | X.XX 秒              | X.XX 秒                  |

※ 具体的な数字は、実際の測定結果を入れてください。

# これからのzenu

zenuの開発チームは、まだまだ新しい機能を追加する予定です。近い将来、以下のような機能が加わる予定です：

* レイヤーのサポート : 現在、RNN, GRU, LSTM, Attentionなどのレイヤーの実装を行っています。
* エラーハンドリング : エラーハンドリングを強化し、よりユーザーフレンドリーにします。
* PyTorchモデルのサポート : PyTorchで学習したモデルをzenuで読み込めるようにします。
* Onnxのサポート : Onnx形式へのエクスポートをサポートします。
* ドキュメントの充実 : ドキュメントを充実させ、使いやすさを向上させます。
* Cpu実装の高速化 : Cpu実装の高速化を行い、より多くの環境で高速に動作するようにします。

# つかってください🙇

zenuは、まだまだ開発途中ですが、使っていただきたいです。バグや要望など、フィードバックをお待ちしています。ぜひ、GitHubの[Issue](github.com/bokutotu/zenu/issues)や[Pull Request](github.com/bokutotu/zenu/pulls)を送ってください。
あと、Starくれると、非常にやる気が出ます🌟
