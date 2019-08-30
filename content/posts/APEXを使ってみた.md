---
title: APEXを使ってみた
tags: ["APEX", "Terraform"]
draft: false
---

APEXに使ってみたので、ざつにまとめを記録する。

とりあえずNetlifyという自動ビルドツールをいれてみたので早急に試してみたいw
git pushするだけでよしなにしてくれるはずだ・・・


## インストール
https://github.com/apex/apex

```
$ apex version
Apex version 1.0.0-rc2
```

## 初期化

AWS認証情報を設定後、

```
$ apex init


             _    ____  _______  __
            / \  |  _ \| ____\ \/ /
           / _ \ | |_) |  _|  \  /
          / ___ \|  __/| |___ /  \
         /_/   \_\_|   |_____/_/\_\



  Enter the name of your project. It should be machine-friendly, as this
  is used to prefix your functions in Lambda.

    Project name: apex-lambda

  Enter an optional description of your project.

    Project description:

  [+] creating IAM apex-lambda_lambda_function role
  [+] creating IAM apex-lambda_lambda_logs policy
  [+] attaching policy to lambda_function role.
  [+] creating ./project.json
  [+] creating ./functions

  Setup complete, deploy those functions!

    $ apex deploy
```

initしなくてもinitで作成されるファイルを自作してもOK
IAMがパタパタと作成される。

```
$ apex list
   ⨯ Error: loading hello: validating: Runtime: zero value
```
こうなってしまう場合はruntimeの設定ができていないのでfunction/hello以下にfunction.jsonを作成すれば、project.jsonをオーバーライドするのでそこで設定する。ついでにハンドラーの設定もする

```
$ cat functions/hello/function.json
{
  "runtime": "python3.7",
  "handler": "lambda_function.lambda_handler"
}
```

## 関数のリスト表示

```
$ apex list

  hello (not deployed)
    runtime: python3.7
    memory: 128mb
    timeout: 5s
    role: arn:aws:iam::572183290275:role/apex-lambda_lambda_function
    handler: lambda_function.lambda_handler

```

## 関数デプロイ
ロールバックもできるようです。

```
$ apex deploy
   • creating function         env= function=hello
   • created alias current     env= function=hello version=1
   • function created          env= function=hello name=apex-lambda_hello version=1
```

```
$ cat functions/hello/index.py
import json

def lambda_handler(event, context):
    # TODO implement
    return {
        'statusCode': 200,
        'body': json.dumps('Hello from Lambda!')
    }

```

ハンドラは、「ハンドラが形式 file-name.method」の書式なので注意

テストできた。

## 関数の呼び出し
```
$ apex invoke hello
{"statusCode": 200, "body": "\"Hello from Lambda!\""}
```

## 関数の削除
```
$ apex delete hello
Are you sure? (yes/no) yes
   • deleting                  env= function=hello
   • function deleted          env= function=hello
```

## AWSコンポーネントの管理

`apex infra`がterraformコマンドのwrapperになっている

https://github.com/apex/apex/blob/master/docs/infra.md

```
$ apex infra plan|apply
```
