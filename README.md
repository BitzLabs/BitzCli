# BitzCli

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

`BitzCli`は、BitzLabsエコシステムの全機能を統合して実行するための、公式コマンドラインインターフェース(CLI)ツールです。Markdown文書の変換から、各種ダイアグラムの単体生成まで、BitzLabsのパワーをコマンドラインから直接利用できます。

## 主な機能

-   Markdownファイルや各種DSLファイルを入力として受け取ります。
-   設定ファイル (`bitz.config.js`または`bitz.json`) に基づいて、使用するプラグインやオプションをカスタマイズできます。
-   `BitzDoc`や各種専門ライブラリを呼び出して、適切なパイプラインを構築・実行します。
-   結果をHTMLファイル、SVGファイルなど、指定された形式で出力します。

## 使い方 (例)

```bash
# MarkdownファイルをHTMLに変換 (デフォルト)
bitz build my-document.md

# 出力ファイル名を指定
bitz build my-document.md -o output.html

# BitzPlotのDSLファイルをSVGに直接変換
bitz build my-chart.plot.yaml --format svg

# ヘルプを表示
bitz --help
```

## ✅ 初期開発ToDoリスト

1.  **CLIフレームワークの導入**:
    *   `System.CommandLine`ライブラリをプロジェクトに追加し、基本的なコマンド構造を定義。
2.  **`build`コマンドの実装**:
    *   入力ファイルパスを引数として、`-o` (output)と`-f` (format)オプションを定義。
3.  **パイプライン実行エンジンの実装**:
    *   `PipelineEngine`クラスを作成。
    *   入力ファイルの拡張子を見て、最初に呼び出すパーサーを決定するディスパッチャロジックを実装。
    *   **Markdown (`.md`) の処理フロー**: `BitzDoc.MarkdownParser` -> `BitzDoc.HtmlCompiler` -> ファイル出力。
    *   **専門DSL (`.plot.yaml`など) の処理フロー**: `BitzPlot.PlotParser` -> `BitzPlot.PlotLayoutEngine` -> `BitzRenderers.SvgRenderer` -> ファイル出力。
4.  **基本的なファイルI/O**:
    *   入力ファイルの読み込みと、処理結果のファイル書き込みを行うロジックを実装。

## このライブラリの位置づけ

`BitzCli`は、BitzLabsエコシステムのメインエントリーポイントです。開発者はこのCLIツールを使うことで、BitzLabsの強力なドキュメント生成・変換機能を簡単に利用できます。

将来的には、このリポジトリをベースにVS Codeなどのエディタ拡張機能も開発される予定です。

### 依存関係

-   `BitzDoc`
-   (動的に解決) `BitzPlot`, `BitzFlow`, `BitzRenderers`など、ほぼ全てのライブラリ。
