# 医学教育モデル・コア・カリキュラム

## このリポジトリで公開されているもの

[医学教育モデル・コア・カリキュラム（令和4年度改訂版）](https://www.mext.go.jp/b_menu/shingi/chousa/koutou/116/toushin/mext_01280.html)(以下コアカリ)として公開されているpdf(pdf版)の内容を、csvやマークダウンといったデータとして活用しやすい形式(データ版)で配布しています。

## なぜこのプロジェクトが必要か?

pdfは、再現性が高く可読性が高い印刷物を生成するためのデータ形式として優れています。一方で、統計処理・研究に用いる場合にpdfからデータを抽出することは困難です。

コアカリを以下のような領域で活用するためには、よりデータとしての処理が容易なファイル形式が必要となります。csvやマークダウンと言ったデータ処理と親和性のある形式で配布することで以下のような活用が可能になります

- 各大学での教育プログラムの情報の収集および解析([IR](https://doi.org/10.24489/jjphe.2018-012))
- 電子シラバスや評価システムの開発
- コアカリ普及のための電子書籍・webサイトの作成


## ファイル形式

このリポジトリでのデータは主に以下のファイル形式で配布されています。文字コードは特に断りがなければUTF-8(BOMなし)ですが、csvのみExcelとの互換性のためUTF-8 (BOMあり)としています。

|  ファイル形式 |  文字コード |   内容 |
|-------|----|----|
| csv   |  UTF-8(BOMあり)   |表データ(主に第1章/第2章)  |
| マークダウン(.md) | UTF-8(BOMなし) |ドキュメント形式データ(上記以外) |
| biblatex(.bib) | UTF-8(BOMなし) | 参考文献 |

## pdf版との違いについて

pdf版とこのリポジトリのデータ(データ版)には以下の違いがあります

- csvの列名は全て英数字と一部の記号で構成されています
- pdfには記載されていない[id](#インデックスとid)がすべての項目に付されています
    - それぞれの項目を一意に特定するため
    - 項目同士の関連性を厳密に示すため
- データ形式の違いから以下のような変更がされています
    - csvにおいて、斜線・太字などはマークダウンの書式を用いて表現しています
    - 「第2章 資質・能力」において、表への参照は[pandoc markdown形式](https://pandoc-doc-ja.readthedocs.io/ja/latest/users-guide.html)を用いて記載されています
    - 参考文献は直接文書中に記載されずcitations.bibファイルの内容を参照するようになっています
    - マークダウンにおいて四角囲いなど該当する表現ができない部分は代替の形式に変換されています

これは、以下のような理由に基づくものです。

- データとしての厳密性を持たせる
- 英語版と日本語版でデータの構造を同じものとする
- 一部のアプリケーションで起こる日本語によるエラーを防ぐ
- データ形式による違い


## インデックスとid

pdf版では第1章・第2章の資質・能力の先頭にPR-01-02、GE-02-01-01といった**インデックス**が付けられています。インデックスはそれぞれの項目毎に連番で付けられており、本でいえばページにあたります。

データ版では、第1章・第2章の資質・能力および別表や各文書に**id**が割り振られております。idは項目作成時のUnixタイムスタンプ(ミリ秒)より生成されたa-zA-Z_-のurlセーフな文字列で表現される一意な識別子です。

インデックスは連番であるため比較的わかりやすい形式ですが、今後もし項目の削除・追加があった場合にはインデックスが指し示す項目は変わってしまう可能性があります。idは今後もし項目の削除・追加があった場合でも常に同じ項目を指します。厳密なデータ分析やシステム作成の際にはインデックスではなくidを用いることを推奨します。

| |  インデックス | id |
|--|--|--|
| pdf版への記載 | あり | なし |
| 付与されている項目 | 資質・能力 | 資質・能力, 別表, 文書 |
| 改訂により変化する可能性 | あり | なし |
| 読みやすさ | ○ | × |


## データ形式について

### 資質・能力(第1章・第2章) 

コアカリでは、資質・能力を最も大きな第1層から最も小さく詳細な第4層に階層化して表現しています。資質・能力は[2022/ja/outcomes](2022/ja/outcomes)に格納されています。

|ファイル|内容 |
|-|-|
| [layer1.csv](2022/ja/outcomes/layer1.csv) | 第1層の資質・能力 |
| [layer2.csv](2022/ja/outcomes/layer2.csv) | 第2層の資質・能力 |
| [layer3.csv](2022/ja/outcomes/layer3.csv) | 第3層の資質・能力 |
| [layer4.csv](2022/ja/outcomes/layer4.csv) | 第4層の資質・能力 |

また各csvファイルの列は以下の通りです

|列| 存在する階層 |内容 |
|-|-|-|
| index | 第1-4層 | pdf版と同じ[インデックス](#インデックスとid) |
| id | 第1-4層 | 項目固有の[id](#インデックスとid) | 
| spell | 第1層 | アルファベット2 文字と資質・能力名の対応 |
| item | 第1-4層 | 該当項目の内容 |
| description | 第1-2層 | 資質・能力の概要 |
| parent | 第2-3層 | 親階層(第2層なら第1層, 第3層なら第2層)の[id](#インデックスとid) |

### 別表(第1章・第2章) 

第1章・第2章では、内容の一部を別表として切り出しています。別表は[layer3.csv](2022/ja/outcomes/layer3.csv)もしくは[layer3.csv](2022/ja/outcomes/layer3.csv)の中で[@TBL:TBLxxxx]という形式で参照されています。別表のデータは[2022/ja/tables](2022/ja/tables)に格納されています。

別表の一覧とそれぞれの表自体についてのデータは上記フォルダの[index.csv](2022/ja/tables/index.csv)の中で以下のように表現されています

|列| 内容 |
|-|-|
| id | 表自体の[id](#インデックスとid) |
| index |  表自体の[インデックス](#インデックスとid)(pdf版には記載なし) |
| name | 表の名前 |
| file | 表のデータが格納されているファイル名 |
| legend | 表の解説文 |
| number | 表の番号 |
| columns | pdf版の列名との対応関係 |
| main | 表の中で主となる項目の列名 | 

別表のデータは[2022/ja/tables](2022/ja/tables)のfile名に格納されています。pdf版と違い、表の1つ1つの項目にも[インデックス](#インデックスとid)と[id](#インデックスとid)が付されています。表の列名は英数字となっています。pdf版の列名との対応関係はcolumns列に記載されています。例えば[TBL0202.csv](2022/ja/tables/TBL0202.csv)では、indexとidの他にcategory,itemという列があります。[index.csv](2022/ja/tables/index.csv)のcolumnsには"category:分類,item:項目名"と記載されているため、categoryはpdf版の分類に、itemはpdf版の項目名に該当することが分かります。

### その他のデータ

- [2016年(平成28年)版コアカリ](2016)
- [2016年版コアカリと2022年(令和4年)版コアカリの対応](relations/y2016_to_y2022/)

## ライセンス

このリポジトリの内容を利用する場合は、「文部科学省ウェブサイト利用規約」( https://www.mext.go.jp/b_menu/1351168.htm )に従って、御利用ください。