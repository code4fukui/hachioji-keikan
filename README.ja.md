# Hachioji Landscape Resources Map (hachioji-keikan)

このリポジトリは、インタラクティブなウェブベースの市区町村マップを作成するためのテンプレートです。ここでは東京都八王子市の「地域景観資源マップ」としてデモンストレーションを行っていますが、ハザードマップなどさまざまな目的に応用可能です。

**ライブデモ:** ~~https://sankichi92.github.io/hachioji-keikan/~~ *(unavailable)* *(demo unavailable)*

![hachioji-keikanマップアプリケーションのスクリーンショット。八王子市の地図上に、関心のあるポイントを示す色付きのマーカーが表示されています。右上にはレイヤーコントロールパネルがあり、左下にはタイトル付きの画像カードが折りたたみ可能なパネルに表示されています。](https://user-images.githubusercontent.com/873336/163695254-83959954-215c-4861-9878-3806a6442654.png)

## 機能

- **簡単なセットアップ:** このリポジトリをテンプレートとして使用し、新しいマップインスタンスを作成できます。
- **カスタムデータレイヤー:** 緯度・経度データを含むCSVファイルを `/csv` ディレクトリに追加することで、インタラクティブなマップレイヤーを自動生成します。
- **画像表示:** 画像ファイルを `/images` ディレクトリに追加することで、マップ上の折りたたみ可能なパネルに関連する写真を表示できます。
- **設定可能なタイルレイヤー:** シンプルな設定ファイルを通じて、国土地理院のハザードマップポータルサイトなどのオーバーレイタイルレイヤーを追加できます。
- **自動デプロイ:** 事前設定されたGitHub Actionにより、`main` ブランチにプッシュするたびにマップがビルドされ、GitHub Pagesにデプロイされます。
- **モダンな技術スタック:** React、Leaflet、TypeScriptで構築されています。

## 動作原理

このプロジェクトでは、ビルド時に一連のNode.jsスクリプトを使用してReactアプリケーション用のデータを準備します。

1. `hazardmap-config.jsonc` を標準のJSONファイルにパースします。
2. 設定された都道府県と市区町村名に基づいて、[Shikuchoson Boundaries](https://shikuchoson-boundaries.sankichi.app/) APIから市区町村境界のGeoJSONを取得します。
3. `/csv` ディレクトリ内のすべての `.csv` ファイルを単一のGeoJSONファイルに変換します。
4. `/images` ディレクトリ内のすべての画像をソースツリーにコピーし、アプリケーションにバンドルします。

その後、このデータがReactアプリケーションに読み込まれ、マップ、レイヤー、UIコンポーネントがレンダリングされます。

## 使い始め方

1. **新しいリポジトリの作成:** このページの上部にある「Use this template」ボタンをクリックします。
2. **新しいリポジトリのクローン:**
   ```bash
   git clone https://github.com/your-username/your-repository-name.git
   cd your-repository-name
   ```
3. **依存関係のインストール:**
   ```bash
   npm install
   ```
4. **開発サーバーの起動:**
   ```bash
   npm start
   ```
   これによりローカルサーバーが起動し、ブラウザでマップが開きます。

## カスタマイズとデプロイ

1. **市区町村の設定:**
   - `hazardmap-config.jsonc` を開きます。
   - `prefecture` と `shikuchoson` フィールドを対象の市区町村に合わせて設定します。これは正しいマップ境界を取得するために使用されます。

2. **カスタムデータポイントの追加:**
   - `/csv` ディレクトリに1つ以上の `.csv` ファイルを作成します。
   - 各CSVには、座標を示す `lat` と `lon` 列が含まれている必要があります。
   - その他の列（例: `name`、`description`）は、ユーザーがマップ上のマーカーをクリックした際にポップアップに自動的に表示されます。

3. **関連画像の追加:**
   - `.jpg`、`.png`、またはその他のウェブ対応画像ファイルを `/images` ディレクトリに配置します。
   - ファイル名（拡張子を除く）がマップ上の画像のタイトルとして使用されます。

4. **ハザードマップレイヤーの追加（オプション）:**
   - `hazardmap-config.jsonc` の `tiles` 配列にレイヤーオブジェクトを追加します。利用可能なデータは [ハザードマップポータルサイト](https://disaportal.gsi.go.jp/hazardmap/copyright/opendata.html) から確認できます。
   - 洪水レイヤーの例:
     ```jsonc
     "tiles": [
       {
         "name": "洪水浸水想定区域（想定最大規模）",
         "url": "https://disaportal.gsi.go.jp/data/raster/01_flood_l2_shinsuisoutai_data/{z}/{x}/{y}.png",
         "attribution": "<a href='https://disaportal.gsi.go.jp/hazardmap/copyright/opendata.html'>ハザードマップポータルサイト</a>",
         "checked": true
       }
     ]
     ```

5. **デプロイ:**
   - 変更をコミットし、`main` ブランチにプッシュします。
   - GitHub Actionが自動的にサイトをビルドし、デプロイします。
   - リポジトリの設定から「Pages」に移動し、ソースが `gh-pages` ブランチと `/ (root)` ディレクトリに設定されていることを確認します。提供されたURLでマップが公開されます。

## データソース

このプロジェクトでは、以下のオープンデータソースとAPIを利用しています。

- **ベースマップ:** 国土地理院の [地理院タイル（淡色地図）](https://maps.gsi.go.jp/development/ichiran.html)
- **境界データ:** 市区町村境界のGeoJSONを取得するための [Shikuchoson Boundaries](https://shikuchoson-boundaries.sankichi.app/)
- **ハザードデータ（オプション）:** 公式のハザードマップタイルデータを取得するための [ハザードマップポータルサイト](https://disaportal.gsi.go.jp/)

## ライセンス

MIT License — 詳細は [LICENSE](LICENSE) を参照してください。
