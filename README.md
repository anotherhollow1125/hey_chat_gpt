# Proc-Macro to summon ChatGPT (English)

A macro to delegate implementation to the ChatGPT API.

This macro sends the entire file containing it to the [OpenAI API](https://platform.openai.com/),
and replaces it with the result returned by the API.

## Example

```rust
use hey_chat_gpt::take_care_of_the_rest;

fn main() {
    println!("{}", fib(10));
}

take_care_of_the_rest!();
```

## Options

You can pass several options in the form of `key = value`.
Additionally, although it's not strictly necessary to mention here,
you can pass string literals as well, which can be used to provide a prompt.

| key                   | Type   | Default         | Possible Values               | Description |
|:----------------------|:-------|:----------------|:-------------------------------|:------------|
| model                 | String | "gpt-4o"        | "o1-preview", etc.             | Specifies the GPT model to use. |
| seed                  | Integer| File hash       | Integer value ≤ 9223372036854775807 | Provides a seed for reproducibility. Try this if the default results are unsatisfactory. |
| max_completion_tokens | Integer| None            | | Sets the maximum number of tokens for the response. Might help when the output is truncated (unverified). |

Example with options:

```rust
fn main() {
    println!("{}", fib(10));
}

hey_chat_gpt::take_care_of_the_rest!(
    model = "o1-preview";
    seed = 20;
    max_completion_tokens = 4096;
    "Implement a function that generates a Fibonacci sequence (a_{n-1} + a_{n} = a_{n+1}) starting with 1, 1."
);
```

## Preparation

> [!IMPORTANT]
> This crate requires `nightly` toolchain!
>
> `cargo add` command also needs to be `nightly`.
>
> ```bash
> cargo +nightly add hey_chat_gpt
> ```

To compile, run the below command.

```bash
OPENAI_API_KEY=sk-YOUR-API-KEY RUSTFLAGS='--cfg procmacro2_semver_exempt' cargo +nightly run
```

- `OPENAI_API_KEY`: api key.
- `RUSTFLAGS=...`: to enable [`source_file` method](https://doc.rust-lang.org/proc_macro/struct.Span.html#method.source_file) of [Span](https://doc.rust-lang.org/proc_macro/struct.Span.html).
- `cargo +nightly run`: the reason of specify `nightly` is same as above.

These settings can also be enable by setting files.

`.cargo/config.toml`

```toml:.cargo/config.toml
[build]
rustflags = ["--cfg=procmacro2_semver_exempt"]

[env]
OPENAI_API_KEY = "sk-YOUR-API-KEY"
```

`rust-toolchain.toml`

```toml:rust-toolchain.toml
[toolchain]
channel = "nightly"
```

In this case, the options are not necessary.

```bash
cargo run
```

# Cache

Responses from ChatGPT are stored in the gpt_responses directory and used as a cache when the code remains unchanged. If you get undesirable code or an error response from ChatGPT, remove those cache files before trying to compile again.

# ChatGPT召喚手続きマクロ (日本語)

ChatGPT APIに実装を代行してもらうマクロです。

このマクロを記述したファイル全体を[OpenAI API](https://platform.openai.com/)に投げ、返ってきた結果で置換します。

## Example

```rust
use hey_chat_gpt::あとは任せた;

fn main() {
    println!("{}", fib(10));
}

あとは任せた!();
```

## Option

`key = value` 形式でいくつかのオプションを渡すことができます。また、(ここに記述する必要性はないですが)文字列リテラルも受け取れるのでここにプロンプトを記述することもできます。

| key                   | 型     | デフォルト       | 候補                            | 説明 |
|:----------------------|:------ |:---------------|:-------------------------------|:-----|
| model                 | 文字列  | "gpt-4o"       | "o1-preview" 等                | 使用するGPTのモデルを指定します。 |
| seed                  | 整数値 | ファイルハッシュ   | 9223372036854775807 以下の整数値 | 再現性確保のために与えるシード値を与えます。デフォルトだと芳しくない結果になった時に指定してみてください。 |
| max_completion_tokens | 整数値 | 指定なし          | | 返答の最大トークン数を設定します。生成が中途半端になった時に使えるかも...？(未検証) |

オプションを指定した場合の例

```rust
fn main() {
    println!("{}", fib(10));
}

hey_chat_gpt::あとは任せた!(
    model = "o1-preview";
    seed = 20;
    max_completion_tokens = 4096;
    "1, 1で始まるフィボナッチ数列(a_{n-1} + a_{n} = a_{n+1})を生成する関数を実装してください。"
);
```

## 使用のための準備

> [!IMPORTANT]
> 本クレートでは `nightly` ツールチェイン必須です！
> 
> `cargo add` コマンドもまた `nightly` を必要とします。
>
> ```bash
> cargo +nightly add hey_chat_gpt
> ```

コンパイルするには以下を実行します。

```bash
OPENAI_API_KEY=sk-YOUR-API-KEY RUSTFLAGS='--cfg procmacro2_semver_exempt' cargo +nightly run
```

- `OPENAI_API_KEY`: 取得してきたOpenAIのAPIキーを設定してください。
- `RUSTFLAGS=...`: [Span](https://doc.rust-lang.org/proc_macro/struct.Span.html) の [`source_file`](https://doc.rust-lang.org/proc_macro/struct.Span.html#method.source_file) メソッドを使用するために指定しています。
- `cargo +nightly run`: `nightly` の指定理由は上記と同じです。

設定ファイルを通しての設定も可能です。

`.cargo/config.toml`

```toml:.cargo/config.toml
[build]
rustflags = ["--cfg=procmacro2_semver_exempt"]

[env]
OPENAI_API_KEY = "sk-YOUR-API-KEY"
```

`rust-toolchain.toml`

```toml:rust-toolchain.toml
[toolchain]
channel = "nightly"
```

この場合オプションは不要になります。

```bash
cargo run
```

# キャッシュファイルについて

ChatGPTからの返答は `gpt_responses` ディレクトリに保存され、コードが変わらないうちはこちらのキャッシュがコンパイルに利用されます。もし望まない結果になったりエラーレスポンスが帰ってきた場合は、キャッシュファイルを削除の上再コンパイルしてみてください。
