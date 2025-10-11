# Authentication (Default)

```bash
rails g authentication
```

สร้าง user แล้วก็จะมี session ด้วย

```bash
rails db:migrate
```

ดู `concerns/authentication.rb` ใน `controllers`

เพิ่มโค้ดด้านล่างนี้ใน Controller ได้ ถ้าเราอยากให้หน้าไหนไม่ติด Authentication

```ruby
allow_unauthenticated_access only: %i[ index show ]
```

ใน Controller หรือใน View เราสามารถใช้โค้ดด้านล่างนี้เพื่อดูว่าเรากำลัง Login อยู่ได้

```ruby
Current.user
```

```bash
rails db:fixtures:load
```

Login

```erb
<%= button_to "Login", new_session_path %>
```

Logout

```erb
<%= button_to "Logout", session_path, method: :delete %>
```

## References

* [Adding Authentication](https://guides.rubyonrails.org/getting_started.html#adding-authentication){target=_blank}