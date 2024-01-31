---
title: "Sindri CLIを使って年齢確認のゼロ知識証明回路をデプロイする"
emoji: "🙆"
type: "tech"
topics: [Sindri]
published: true
---

## Sindri API とは

[Sindri](https://sindri.app/) は、ゼロ知識証明を簡単に生成できるAPIを提供します。これにより、開発者はプライバシーを保護しながら、特定の情報（例えば、年齢の証明）が真実であることを証明できます。

## 実際にやってみる

### 1. Sindri API キーの取得

[Sindri](https://sindri.app/) にログインするためのユーザー情報を発行して貰う必要があります。
直接メールをするなりしてコンタクトをしてログインするためのユーザー情報を発行してもらいます。
無事ログインができるとAccount Settings -> API KeysからAPI Keyを発行できます。
なお、APIやCLI経由でもAPI Keyは発行できます。
具体的なやり方は[チュートリアル](https://sindri.app/docs/getting-started/cli/)に書いてありますが次のようになります。

```bash
npm install -g sindri@latest
sindri login
```

と実行してログインをするとAPI Keyが発行されます。

### 2. プロジェクトのセットアップ

```bash
sindri init <プロジェクトの名前>
```

を実行するとプロジェクトが作られます。
その後回路を作成して、以下のコマンドを実行することでデプロイまでできます。
例えば入力値が20以上であるかを確認する回路を[Noir](https://noir-lang.org/)で作ってみます。

```noir
fn main(input: u8) -> pub bool {
    input >= 20
}

#[test]
fn test_main_with_valid_age() {
    assert(main(20));
}

#[test]
fn test_main_with_invalid_age() {
    let result = main(19);
    assert(!result);
}

#[test]
fn test_main_with_zero() {
    let result = main(0);
    assert(!result);
}

#[test]
fn test_main_with_200() {
    let result = main(200);
    assert(result);
}

```

```bash
sindri lint
```

```bash
sindri deploy
```

デプロイ後はダッシュボードで詳細を確認できます。

### 確認

見つけられていないです。見つけ次第更新します。

### 結論

今回はCLIを使ってみましたがとても簡単にデプロイまでできました。継続してチェックしていきます。
