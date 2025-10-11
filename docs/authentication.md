# Authentication (Default)

```bash
rails g authentication
```

สร้าง user แล้วก็จะมี session ด้วย

```bash
rails db:migrate
```

ดู `concerns/authentication.rb` ใน `controllers`

```bash
rails db:fixtures:load
```

```bash
rails g model Group name:string
```

Logout