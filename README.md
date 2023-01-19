# GitHub 運用

## ブランチ

ブランチは基本的に以下のように分けて運用する。

| ブランチ名 | 用途             |
| ---------- | ---------------- |
| main       | 開発             |
| staging    | 検証             |
| master     | 本番             |
| feature    | 機能追加         |
| hotfix     | 致命的なバグ修正 |
| bugfix     | バグ修正         |

### ブランチの種類

#### main

開発用のブランチ。

基本的にこのブランチから feature ブランチを切って開発を行う。

#### staging

検証用のブランチ。

本番に忠実なため、開発者やクライアントが検証するためのブランチ。

#### master

本番用のブランチ。

#### feature

機能追加用のブランチ。

main ブランチから切って開発を行う。

#### hotfix

致命的なバグ修正用のブランチ。

緊急パッチをあてる際使用する。

#### bugfix

バグ修正用のブランチ。

### ブランチの運用

main は開発用のブランチなので、基本的にはこのブランチから feature ブランチを切って開発を行う。
feature ブランチは開発が完了したら main ブランチにマージする。
main ブランチにマージされたら、staging ブランチにマージする。( サーバーへのリリースは GitHub Actions で行う )
staging ブランチにマージされたら、master ブランチにマージする。( サーバーへのリリースは GitHub Actions で行う )

以下の図のようにブランチを運用する。

```mermaid
graph TB
  feature --> main
  main --> feature
  main --> staging
  staging --> master
  bugfix --> main
  hotfix --> main
  hotfix --> master
```

```mermaid
sequenceDiagram
  participant E as bugfix
  participant B as feature
  participant A as main
  participant C as staging
  participant D as master
  participant F as hotfix
  A->>B: feature ブランチを切る
  B->>A: 開発が完了したら main ブランチにマージする
  A->>E: bugfix ブランチを切る
  E->>A: バグ修正が完了したら main ブランチにマージする
  A->>C: main ブランチにマージされたら staging ブランチにマージする
  C->>D: staging ブランチにマージされたら master ブランチにマージする
  D->>F: hotfix ブランチを切る
  F->>D: 緊急パッチをあてる
  F->>A: 緊急パッチが完了したら main ブランチにマージする
```
