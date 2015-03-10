# [ReWP](https://github.com/amekusa/ReWP) <small>リワードプレス</small>
スマートに、あなたの WordPress サイトをローカルに再構築

\* これは [VCCW (vagrant-chef-wordpress)](https://github.com/vccw-team/vccw) のフォークです。

➥ [English](README.md)

---

## 既存の WP サイトのローカル開発環境をセットアップするのは簡単ではない
だからこれを作りました！

## 使い方
1. ローカルに [ReWP](https://github.com/amekusa/ReWP) をクローンあるいはダウンロード
2. ReWP フォルダを好きにリネームする。あなたのサイト名にするのが良い
3. あなたのサイトの**ドキュメントルート**フォルダを ReWP フォルダ以下にコピー
4. あなたのサイトのデータベースを一つのファイル (SQL or XML) にエクスポートし、それを ReWP フォルダ内に `db.sql` または `db.xml` として配置
5. `ReWP/provision/default.yml` を `ReWP/site.yml` としてコピー
6. `site.yml` をサイトの仕様に沿うよう編集
    + `hostname` はサイトの**ローカル版**の URL  
    `hostname_old` は本番の URL
    + `sync_folder` は ReWP フォルダを基準とした、サイトのドキュメントルートへのパス
    + `import_sql` は **true** だと SQL インポートが有効
    + `import_wxr` は **true** だと XML インポートが有効
7. シェルで以下のコマンドをタイプ:

        cd /path/to/your/ReWP
        vagrant up
8. 数分間待つ。
9. エラーが無ければ**成功!** `site.yml` で `hostname` として設定したアドレスをブラウザで確認してください

## 動作条件
+ [WordPress](https://wordpress.org/) 3.5.2+ **[MUST]**
+ [VirtualBox](https://www.virtualbox.org/) 4.3+
+ [Vagrant](https://www.vagrantup.com/) 1.7.1+

---

## Need help?
問題は遠慮なくここ <https://github.com/amekusa/ReWP/issues> にポストしてください。

Thx :)
