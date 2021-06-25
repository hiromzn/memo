# ER図
- Entity Relation ship Diagram
- DOA(Data Orient Approach)の技法
- EntityとRelationの関係を定義する。

# データモデル

- ER図の状態のこと
- 3つのモデルがある。
- 概念モデル
  - 「物」や「出来事」をEntityとRelationshipとして表現したモデル
- 論理モデル
  -  概念モデルの詳細
  - 属性（Attribute）、主キー（Identifier）、外部キー、要素（Cardinality）を定義する。
  - データ型など物理データベース向けの設計は実施しない。
  - 特定のデータベースに依存しないレベルで具体化したモデル
- 物理モデル
  - 特定の物理データベース向けに変換したモデル 

# 表記方法
- 主な表記方法
  - IDEF1X : Integration Definition 1X
  - IE
- 違いはリレーションシップの表記方法
  - IDEF1X : リレーションの向き先を黒丸で表現する。

# entity
- Resource entity
  - 「もの」を管理するEntity
  - 例：顧客、商品
- Event entity
  - 「業務」を管理するEntity
  - 例：受注、出荷、入金

# Attribute
- エンティティ内で管理する具体的なデータ項目

# Identifire
- エンティティのレコードの識別子となるアトリビュート

# 外部キー
- リレーションシップが設定されている場合、子エンティティに追加する、親エンティティのアイデンティファイアとなるアトリビュート
- 外部キーの属性は属性名の後に（ＦＫ）と付ける

# 多重度（カーディナリティ）
- IDEF1X
  - none : 1
  - P    : >=1
  - Z    : 0 or 1
  - n    : constant value n
  - n-m  : constant value from n to m

# Relationship
- Entity間の関係
- 主語から目的語に向かって線を引く
- 目的語の方に黒丸を付ける。
- 主語：親Entity
- 目的語：子Entity
- 依存Relationship
  - 依存関係がある場合
  - 実線、子Entityの枠は角のある四角
- 非依存Relationship
  - 
- 多対多Relationship
  - 

