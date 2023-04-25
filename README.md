# traefik for fastapi

traefikを使ってfastapiのBASIC認証がしたかったので作成  
例として2つのAPI(fastapi1, fastapi2)でBASIC認証をしたものを提示する

BASIC認証の設定は以下
- user:test
- pass:test

## 実装の説明

fastapi1の設定について詰まった部分を説明

```yml
command: uvicorn main:app --reload --host 0.0.0.0 --port 80 --root-path="/api-template1"
```

commandを使用しなくとも実行できるが、後の`PathPrefix`の設定で`/docs`にアクセスできないため`--root-path=`でpathを指定する必要がある

```yml
- "traefik.http.routers.fastapi_temp1.rule=Host(`fastapi.localhost`) && PathPrefix(`/api-template1`)"
```

複数のAPIがあることを想定して`PathPrefix(`/api-template1`)`でAPIのアクセス先を`http://fastapi.localhost/api-template1`に指定

```yml
- "traefik.http.middlewares.api_name1.stripprefix.prefixes=/api-template1"
```

fastapiのアクセス先の先端は`@app.get("/")`なので`/api-template1`が不要のため`stripprefix`で削除

```yml
- "traefik.http.middlewares.auth1.basicauth.users=test:$$apr1$$iSA3vEsh$51cGxRXQnt05AvjpuEsaS0"
```
BASIC認証は`htpasswd`コマンドで作成

```bash
sudo htpasswd -nb test test
```

※**docker-compose.ymlでBASIC認証を登録する場合、`htpasswd`で作成した暗号の`$`を`$$`に変更すること**
