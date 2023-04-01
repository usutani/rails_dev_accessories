# アクセサリー
PostgreSQL と Redis

## 自己署名証明書の作成
```
openssl req -x509 -sha256 -nodes -days 3650 -newkey rsa:2048 -subj /CN=localhost -keyout server.key -out server.crt
```

## アクセサリーサービス起動
```
dockder compose up
```

# アプリ起動
## web と worker を development 環境で起動
前提：下記のHeroku用Procfileを使用している
```
web: bin/rails server -p ${PORT:-5000} -e $RAILS_ENV
worker: bundle exec sidekiq -q default -q mailers -q active_storage_analysis -q active_storage_purge
```

```
PORT=3000 RAILS_ENV=development REDIS_URL=rediss://localhost:6379 foreman start
```

## 例：コンソールから動作確認
```
REDIS_URL=rediss://localhost:6379 bin/rails c
Sidekiq::Stats.new
MessageMailer.with(to: 'foo@example.com', time: Time.zone.local(2021, 4, 9)).hello_reiwa_date.deliver_later
Sidekiq::Stats.new
```

## 例：ウェブのSidekiqダッシュボードで確認
```
http://localhost:3000/admins/home
admin@example.com / pass1234
http://localhost:3000/admins/sidekiq
```
