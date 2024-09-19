# Oracle xe

#Oracle

  

oracle xe = oracle database 21c express edition

  

## インストール

  

[インストーラー（zip）のダウンロード](https://www.oracle.com/jp/database/technologies/xe-downloads.html "https://www.oracle.com/jp/database/technologies/xe-downloads.html")

パスワードに特殊文字を含める場合は、`⁠""` が必要な文字とそうでない文字があるため注意

  

## データベースへの接続

  

- `⁠sqlplus / as sysdba`⁠ で接続
- `show con_name`⁠で現在の接続先を確認
    - CDBに接続されている場合は、`⁠show pdbs` を実行
    - 一覧にあるPDBに接続（例：`⁠ALTER SESSION SET CONTAINER=XEPDB1;`）
- `show con_name`⁠で再度現在の接続先を確認（PDBに接続されていればOK）

  

### CDBとPDB

  

#### CDB

  

コンテナ・データベースの略で、後述するPDBを管理するコンテナ内の親⽟のような存在

管理が主な⽬的なので実際の業務に使われる重要なデータは保持していない  
「コンテナ」とついているが貨物コンテナのように貨物（業務データ）を入れるわけではなく、貨物コンテナの管理人のようなイメージ

  

#### PDB

  

ラガブル・データベースの略で、実際の業務データが格納される

従来のデータベースに相当する存在で、データ更新や起動停止などを各PDB毎に独立して行うことができる

他のCDB上へ移動させることも可能で、別のサーバ上に簡単に引っ越しすることができる

「貨物コンテナ」にあたる部分

  

## テーブルスペースの作成

  

テーブルスペース＝表領域

テーブルが格納されるファイル、物理領域

  

```
CREATE TABLESPACE demo_space
DATAFILE '~\demo.dbf' SIZE 100M
AUTOEXTEND ON NEXT 500K MAXSIZE 1024M;
```

  

  

## ユーザの作成

  

```
CREATE USER demo
IDENTIFIED BY "パスワード"
DEFAULT TABLESPACE demo_space
TEMPORARY TABLESPACE TEMP
PROFILE DEFAULT;
```

  

  

## tnsnames.oraの編集

  

- 適当なTNS名で接続（例：`tnsping aaa`⁠）
- 表示されたパスのtnsname.oraを編集
    - 恐らく`⁠"~\homes\OraDB21Home1\network\admin\tnsnames.ora"`

```
XE =
  (DESCRIPTION =
    (ADDRESS = (PROTOCOL = TCP)(HOST = host.docker.internal)(PORT = 1521))
    (CONNECT_DATA =
      (SERVER = DEDICATED)
      (SERVICE_NAME = XE)
    )
  )

XEPDB1 =
  (DESCRIPTION=
    (LOAD_BALANCE = off) 
    (ADDRESS = (PROTOCOL = tcp)(HOST = localhost)(PORT = 1521))
    (CONNECT_DATA=
      (SERVICE_NAME = XEPDB1) 
      (FAILOVER_MODE = (TYPE = select)(METHOD = basic))
    )
  )
```

- 正確なTNS名で接続（例：`⁠tnsping XEPDB1`）

  

## SQL Developerで確認

  

![](Files/image.png)  

  

SQL Plus・SQL Developerでインサートした場合は、それぞれコミットしないと反映されない

どちらからでもTBLの操作が可能

  

## 参考

  

[【SE入門編】Oracle Database 21c Express Editionのインストール手順及びSQL Developerの設定手順](https://qiita.com/manabu-s/items/df29732448669b5429eb "https://qiita.com/manabu-s/items/df29732448669b5429eb")  

[Oracle 21c XE 構築手順](https://computing-atman.com/blog/34-install-oracle-xe/ "https://computing-atman.com/blog/34-install-oracle-xe/")  

[いまさら遅い？Oracle Databaseのコンテナ技術](https://www.reqtc.com/blog/oracle-pdb-cdb.html "https://www.reqtc.com/blog/oracle-pdb-cdb.html")