---
title: 'デザインシステムのイントロダクション'
tocTitle: 'イントロダクション'
description: 'デザインシステムのために最新のリリース可能なツールのガイド'
---

<div class="aside">このガイドはデザインシステムの構築方法を学ぶ<b>プロの開発者</b>向けです。JavaScript、Git、継続的インテグレーションの中級レベルの経験が推奨されます。また Storybook の基本、ストーリーを書くこととコンフィングファイルを編集すること (<a href="/intro-to-storybook">Intro to Storybook</a> で基本を教えています) を知っている必要があります。
</div>

<br/>

デザインシステムは幅広く拡大しています。Airbnb のようなテック界の重鎮からフットワークの軽いスタートアップまで、どんな形態の組織においても時間とお金の節約のために UI パターンを再利用しています。しかし BBC、Airbnb、IBM、Microsoft により作られるデザインシステムとその他大勢の開発者により作られるものの間には隔たりがあります。

なぜ優れたデザインシステムチームは彼らが採用するツールとテクニックを用いるのでしょうか？共著者の Tom と私はベストプラクティスとみなした Storybook のコミュニティから成功しているデザインシステムの特徴を調べました。

このステップ・バイ・ステップガイドは大規模な本番のデザインシステムで使われている自動化ツールと細やかなワークフローを明らかにします。既存のコンポーネントライブラリーからデザインシステムの組み立てをひと通り行い、それからコアサービス、ライブラリー、ワークフローを構築します。

![Design system overview](/design-systems-for-developers/design-system-overview.jpg)

## どうしてデザインシステムでそんなに騒いでいるのですか？

ちょっと先に片付けておきましょう: 再利用可能なユーザーインターフェースというコンセプトは新しいものではありません。スタイルガイド、UI キット、共有できるウィジェットは数十年に渡り存在しています。今日では、デザイナーと開発者は UI コンポーネント構築のため肩を並べています。UI コンポーネントは別々のユーザーインターフェース部品のビジュアルと機能のプロパティをカプセル化します。LEGO ブロックを想像してみてください。

現代のユーザーインターフェースはそれぞれに異なるユーザー体験を提供するために整理された数百もの規格化された UI コンポーネントで成り立っています。

デザインシステムは複数のプロジェクトを横断してチームが複雑で、丈夫で、アクセシビリティの高いユーザーインターフェースを構築するための再利用可能な UI コンポーネントを包括します。デザイナーと開発者双方が UI コンポーネントに貢献するため、デザインシステムは分野間の架け橋としての役目を果たします。それはまた組織の共通コンポーネントにとって「信頼できる情報源」となります。

![Design systems bridge design and development](/design-systems-for-developers/design-system-context.jpg)

デザイナーは自分たちのツールの中にデザインシステムを構築することについてよく話題にします。デザインシステムの全体的なスコープにはアセット (Sketch、Figma、その他) 、横断的なデザイン原則、貢献の仕組み作り、管理、その他色々な要素を含みます。これらのトピックを深掘りするデザイナー向けのガイドは他で豊富にあるためここでは繰り返しません。

開発者に明確なことは少ないです。本番のデザインシステムは UI コンポーネントだけでなく、その背後にはフロントエンドの基盤を組み込む必要があります。当ガイドで触れるデザインシステムは 3 つのパートに分かれます:

- 🏗 共通の再利用可能な UI コンポーネント
- 🎨 デザイントークン: ブランドカラーやスペーシングのようなスタイリングに特化した変数
- 📕 ドキュメントサイト: 使用方法の説明、語り口、するべし・べからず集

これらの部品はパッケージマネージャーを介してパッケージ化され、バージョン管理され、ユーザーアプリへ配布されます。

## デザインシステムは必要なのですか？

大々的な宣伝に反しますが、デザインシステムは銀の弾丸ではありません。単一のアプリを小さなチームと取り組む場合は、デザインシステムを利用可能にする基盤を立ち上げるよりも UI コンポーネントをフォルダで分ける方が良いです。小規模なプロジェクトにとって、メンテナンス、継続的改善、ツール準備のコストは期待するあらゆる生産性の恩恵のわりに大きすぎる負担になります。

デザインシステムのスケーリングの効果は多くのプロジェクトに渡って UI コンポーネントを共有する際に有効に働きます。異なるアプリかチームをまたがって同じ UI コンポーネントをペーストしていることに気づいたら、当ガイドはあなたのためにあります。

## 私たちが構築するもの

Storybook は[BBC](https://www.bbc.co.uk/iplayer/storybook/index.html?path=/story/style-guide--colours)、[Airbnb](https://github.com/airbnb/lunar)、[IBM](https://www.carbondesignsystem.com/)、[GitHub](https://primer.style/css/)、そして数百もの企業のデザインシステムを推進しています。ここでおすすめする内容は最も賢明なチームのベストプラクティスと手法から着想を得ています。私たちは次のフロントエンドスタックを構築していきます:

#### コンポーネントビルド

- 📚 [Storybook](http://storybook.js.org) UI コンポーネントの開発とドキュメントの自動生成
- ⚛️ [React](https://reactjs.org/) 宣言的コンポーネント中心の UI (create-react-app を介して)
- 💅 [Styled-components](https://www.styled-components.com/) コンポーネントスコープのスタイリング
- ✨ [Prettier](https://prettier.io/) 自動コードフォーマット

#### システムメンテナンス

- 🚥 [GitHub Actions](https://github.com/features/actions) 継続的インテグレーション
- 📐 [ESLint](https://eslint.org/) JavaScript 静的解析ツール
- ✅ [Chromatic](https://chromatic.com) コンポーネント内の目に見えるバグを捕捉 (Storybook の保守管理者よる開発)
- 🃏 [Jest](https://jestjs.io/) ユニットテストコンポーネント
- 📦 [npm](https://npmjs.com) ライブラリーの配布
- 🛠 [Auto](https://github.com/intuit/auto) リリース管理ワークフロー

#### Storybook アドオン

- ♿ [Accessibility](https://github.com/storybookjs/storybook/tree/master/addons/a11y) 開発時のアクセシビリティの課題をチェックする
- 💥 [Actions](https://storybook.js.org/docs/react/essentials/actions) クリックとタップの品質を対話的に検査する
- 🎛 [Controls](https://storybook.js.org/docs/react/essentials/controls) 対話的にプロパティを変更しコンポーネントを検証する
- 📕 [Docs](https://storybook.js.org/docs/react/writing-docs/introduction) ストーリーから自動的に文書生成
- 🔍 [Interactions](https://storybook.js.org/addons/@storybook/addon-interactions/) コンポーネントへのインタラクションのデバッグ

![Design system workflow](/design-systems-for-developers/design-system-workflow.jpg)

## ワークフローを理解する

デザインシステムはフロントエンド基盤への投資です。先に挙げた技術の使い方の紹介に加え、当ガイドでは採用の促進とメンテナンスを簡素化するコアワークフローにも焦点を当てます。できる限り、手作業のタスクは自動化します。以下は私たちが接するアクティビティです。

#### 分離して UI コンポーネントを構築する

全てのデザインシステムは UI コンポーネントで構成されています。私たちは Storybook をユーザーアプリの外側に分離して構築する「ワークベンチ」として使います。それから長く使えるようにするコンポーネントを増やすのを手助けする時間節約アドオン (Actions、A11y、Controls、Interactions) を組み合わせます。

#### 合意に向けてフィードバックを集めるためのレビューをする

UI の開発は開発者、デザイナー、その他の分野との協働が必要なチームスポーツです。私たちはステークホルダーを開発プロセスの輪に入れるために作成中の UI コンポーネントを配布することでより迅速なリリースを可能にします。

#### UI バグを防ぐためにテストする

デザインシステムは信頼できる唯一の情報源であり単一の障害点です。基本コンポーネントの些細な UI バグは会社規模の問題へと膨らみます。私たちは胸を張って長持ちし、使いやすい UI コンポーネントを世に送り出すために避けがたいバグを減らす助けとなるようテストを自動化します。

#### 採用を加速するためにドキュメントを作成する

ドキュメンテーションは必要不可欠なものです、しかしそれを作成するのは開発者の最後の優先順位になりがちです。私たちはよりカスタマイズされた必要最低限のドキュメントを自動生成することで UI コンポーネントのドキュメント作成をずっと簡単にします。

#### デザインシステムをユーザープロジェクトへ配布する

ドキュメントが整備された UI コンポーネントができたら、それを他のチームへ配布する必要があります。私たちはパッケージング、配布、それからユーザーアプリ側の Storybook に向けたデザインシステムの配布方法を取り扱います。

## Storybook のデザインシステム

当ガイドの例であるデザインシステムは Storybook 自体の[本番デザインシステム](https://github.com/storybookjs/design-system)に基づいています。そのデザインシステムは Storybook のエコシステムにおいて 3 つのサイトで利用され何万もの開発者により取り扱われています。

次の章で、私たちは複数の異なるコンポーネントライブラリーからデザインシステムの抽出方法を紹介します。