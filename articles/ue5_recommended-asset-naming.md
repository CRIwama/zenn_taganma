---
title: "Unreal Engineの命名規則はどうしてる？"
emoji: "🐡"
type: "idea" # tech: 技術記事 / idea: アイデア
topics: ["ue5","unrealengine"]
publication_name: "coyote_tateam"
published: true
---

:::message
UE Version : 5.4.3
:::

「金を掘るより、⛏を作ろう！」プロジェクトではゲーム制作をしながら、ゲーム制作に必要になった知見やツールを発信するプロジェクトです。
今回はゲーム制作開始時にアセット名の意識合わせをする際に発生した問題を共有します。

## スケルタルメッシュのプレフィックスの名前は？

現在開発しているゲームのプレイヤーとなる桃「シャーピー（Sharpea）」のSkeletalMesh（骨が入っている3Dモデル）のFBX「SK_Sheapea.fbx」をインポートすると以下のアセットが生成されます。

| アセット名              | アセットの種類 |
| ----------------------- | -------------- |
| SK_Sharpea              | SkeletalMesh   |
| SK_Sharpea_PhysicsAsset | 物理アセット   |
| SK_Sharpea_Skeleton     | Skeleton       |

![](/images/articles/ue5_recommended-asset-naming/2024-07-24-16-30-13.png)

「Skeleton」のアセットはプロジェクトメンバーからプレフィックスについて質問があったので、アンケートを取ってみました。

![](/images/articles/ue5_recommended-asset-naming/2024-07-24-16-54-22.png)

結果としては以下の２つのケースのどちらかが多いようです。

- SK_Sharpea_Skeleton（名前は変更しない）
- SKEL_Sharpea（プレフィックスを設定し、サフィックスのSkeletonは削除する）

## プレフィックス（Prefix）とサフィックス(Surffix)について

ファイル名の先頭につく名前が「Prefix」、ファイルの一番後ろにつく名前が「Surffix」です。
![](/images/articles/ue5_recommended-asset-naming/2024-07-24-17-08-46.png)

PrefixやSurffixを付ける理由は以下のことが考えられます。

- アセットの種類が判別できるように「Prefix」を設定する
- アセットの種類を分けた後にさらに種類を判別できるように「Surffix」を設定する

### 公式ドキュメントに合わせる

今回質問した「スケルトン（Skeleton）」は公式ドキュメントでは「SKEL」を「Prefix」に設定することを推奨しています。

- [アセットの命名規則に関する推奨事項](https://dev.epicgames.com/documentation/ja-jp/unreal-engine/recommended-asset-naming-conventions-in-unreal-engine-projects)

![](/images/articles/ue5_recommended-asset-naming/2024-07-24-17-12-51.png)

「スケルトン（Skeleton）」から分かれる種類はないので、公式ドキュメントに合わせるのであれば、「SKEL_Sharpea」に設定します。

![](/images/articles/ue5_recommended-asset-naming/2024-07-24-17-16-16.png)

「物理アセット（PhysicsAsset）」も公式ドキュメントでは「PHYS」を「Prefix」に設定することを推奨しています。

![](/images/articles/ue5_recommended-asset-naming/2024-07-24-17-19-46.png)

「物理アセット（PhysicsAsset）」から分かれる種類はないので、公式ドキュメントに合わせるのであれば、「PHYS_Sharpea」に設定します。

![](/images/articles/ue5_recommended-asset-naming/2024-07-24-17-27-16.png)

公式ドキュメントの「Prefix」に合わせると、以下のようなアセット名にするのが最適です。

| アセット名              | アセットの種類 | 公式推奨の命名規則に従う |
| ----------------------- | -------------- | ------------------------ |
| SK_Sharpea              | SkeletalMesh   | SK_Sharpea               |
| SK_Sharpea_PhysicsAsset | 物理アセット   | PHYS_Sharpea             |
| SK_Sharpea_Skeleton     | Skeleton       | SKEL_Sharpea             |

![](/images/articles/ue5_recommended-asset-naming/2024-07-24-17-30-28.png)

### SkeletalMeshの派生アセットとして扱う（デフォルト）

アンケートではデフォルトのままの名前を使う方が多かったです。
デフォルトではスケルタルメッシュから派生するアセットとして、「Suffix」で「物理アセット（PhysicsAsset）」と「スケルトン（Skeleton）」を判別するという方針のようです。

![](/images/articles/ue5_recommended-asset-naming/2024-07-24-17-19-18.png)

「物理アセット（PhysicsAsset）」と「スケルトン（Skeleton）」を特別に何かツールなどで処理することも少ないので、公式にあわせずにデフォルトのまま使うという選択も問題ないと考えています。

プロジェクトの方針に従って、名前を設定しましょう。

## 大文字から始めるか小文字で始めるか

::: message
2024/08/28 プレフィックスとサーフィックス問題が解決したら次は単語の分け方について決めることになりました。
:::

単語の先頭は大文字にしました。

SM_Rock

単語ごとに「_」で区切るようにしました。

- 〇：SM_PET_Bottle
- ✖：SM_pet_bottle
- ✖：SM_petbottle

## 命名規則に困った時の対応方法

命名規則はプロジェクトの方針によって決まります。
プロジェクトの方針を決めるときには、以下の内容を参考にしています。

### 公式ドキュメントや「UE5 Style Guide」を参考にする

公式ドキュメントのほかに、「ue5-style-guide」という命名規則に関連する内容をまとめたリポジトリが公開されています。
「us5-style-guide」はアセットの命名規則以外にも、プロジェクトのフォルダ構成などさまざまな命名規則にも触れているので参考になります。

- [アセットの命名規則に関する推奨事項](https://dev.epicgames.com/documentation/ja-jp/unreal-engine/recommended-asset-naming-conventions-in-unreal-engine-projects)
- [ue5-style-guide](https://github.com/akenatsu/ue4-style-guide/blob/master/README.jp.md)
- [ue4-style-guide(日本語訳)](https://github.com/akenatsu/ue4-style-guide/blob/master/README.jp.md)

### Epic公式から配布されているプロジェクトの中身を確認する

Epic公式からさまざまなプロジェクトが公開されています。
自分たちの作成したい作品に一番近いプロジェクトの中身を見て、方針を決めるのがよさそうです。
個人的には、UE5で使用できるアセットがほとんど入っている「機能別サンプル」を参考にしています。

![](/images/articles/ue5_recommended-asset-naming/2024-07-24-17-43-50.png)

## Epic公式から配布されているプロジェクトを調査してみる

気になったのでEpic公式から配布されているプロジェクトの命名規則を調査してみました。
「公式ドキュメントは推奨とされていて、絶対ではないのではない」という結果になりました。

### 機能別サンプルの場合

長年機能が追加されているせいか、命名規則にはバラツキがありました。

#### SkeletalMesh

公式のサンプルも担当者によって命名規則は揃っていないようです。

- Prefixは付けない
- S_
- SK_
- SKM_
- CLOTH_（Clothの説明用のアセットのため特例？）

![](/images/articles/ue5_recommended-asset-naming/T_2024-07-26-19-13-04.png)

#### Skeleton

SkeltonはSuffixを「_Skeleton」に設定しているケースが多かったです。

- Suffixを「_Skeleton」
- SK_ (SKM_に対して)

![](/images/articles/ue5_recommended-asset-naming/T_2024-07-26-19-18-42.png)

#### PhysicsAsset

PhysicsAssetはPrefixを「PA_」と設定するケースが多かったです。

- PA_(Asset名)
- SK_(Asset名)_Physics
- SK_(Asset名)_PhysicsAsset

![](/images/articles/ue5_recommended-asset-naming/T_2024-07-26-19-22-22.png)

### Stack O Botの場合

1つのゲームプロジェクトのため、命名規則が統一されていました。

| アセット名    | アセットの種類 |
| ------------- | -------------- |
| SKM_(Asset名) | SkeletalMesh   |
| PhA_(Asset名) | 物理アセット   |
| SK_(Asset名)  | Skeleton       |

![](/images/articles/ue5_recommended-asset-naming/T_2024-07-26-19-36-37.png)

## 参考にしたURL

- [アセットの命名規則に関する推奨事項](https://dev.epicgames.com/documentation/ja-jp/unreal-engine/recommended-asset-naming-conventions-in-unreal-engine-projects)
- [ue5-style-guide](https://github.com/akenatsu/ue4-style-guide/blob/master/README.jp.md)
- [ue4-style-guide(日本語訳)](https://github.com/akenatsu/ue4-style-guide/blob/master/README.jp.md)
- [Best practices for naming assets in Unreal Engine](https://unrealdirective.com/resources/asset-naming-conventions)
