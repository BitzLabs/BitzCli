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

## このライブラリの位置づけ

`BitzCli`は、BitzLabsエコシステムのメインエントリーポイントです。開発者はこのCLIツールを使うことで、BitzLabsの強力なドキュメント生成・変換機能を簡単に利用できます。

将来的には、このリポジトリをベースにVS Codeなどのエディタ拡張機能も開発される予定です。

### 依存関係

-   `BitzDoc`
-   `BitzPlot`, `BitzFlow`, `BitzGraph`, `BitzMath`, `BitzUi`, `BitzCode`
-   `BitzRenderers`
-   (外部ライブラリ) `System.CommandLine`など、CLI引数解析ライブラリ。

## **✅ 初期開発ToDoリスト**

1.  **CLIフレームワークの選定と導入**:
    *   C#で本格的なCLIツールを構築するために、`System.CommandLine`ライブラリをプロジェクトに追加します。これにより、コマンド、引数、オプションの定義が非常に簡単になります。

2.  **メインコマンドの定義**:
    *   `build`コマンドを定義します。
    *   `--output` (`-o`) オプション（出力ファイルパス）と `--format` (`-f`) オプション（出力形式）を定義します。
    *   入力ファイルパスを引数として受け取れるようにします。

3.  **パイプライン実行エンジンの実装**:
    *   `PipelineEngine` クラスを作成。
    *   `public void Execute(string inputFile, string outputFile, string format)` のようなメソッドを持つ。
    *   **内部ロジック**:
        1.  入力ファイルの拡張子（`.md`, `.plot.yaml`, `.dot`など）を見て、どのパーサーを最初に呼び出すべきかを判断する**ディスパッチャ**を実装する。
        2.  **Markdownの場合 (`.md`)**:
            *   `BitzDoc.MarkdownParser`を呼び出して`DocAST`を生成。
            *   `BitzDoc.HtmlCompiler`を呼び出してHTML文字列を生成。
            *   結果をファイルに出力。
        3.  **専門DSLの場合 (`.plot.yaml`など)**:
            *   対応する専門パーサー（`BitzPlot.PlotParser`）を呼び出して専門AST（`PlotAST`）を生成。
            *   対応するレイアウトエンジン（`BitzPlot.PlotLayoutEngine`）を呼び出して`LayoutAST`を生成。
            *   `BitzRenderers.SvgRenderer`を呼び出してSVG文字列を生成。
            *   結果をファイルに出力。

4.  **設定ファイルの読み込み (将来の機能)**:
    *   プロジェクトルートにある`bitz.config.json`のような設定ファイルを読み込む機能のインターフェースを設計しておく。

