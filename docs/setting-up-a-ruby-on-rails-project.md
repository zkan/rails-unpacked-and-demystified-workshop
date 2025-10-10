# Setting up a Ruby on Rails Project

```bash
mise exec ruby -- gem install rails
```

> The name of the scaffold follows the convention of models, which are singular, rather than resources and controllers, which are plural. Thus, we have User instead of Users.
>

— Hartl, Michael. Ruby on Rails Tutorial (p. 72). Pearson Education. Kindle Edition.

```bash
rails generate scaffold User name:string email:string
```

rails g scaffold Friend first_name:string last_name:string email:string phone:string twitter:
string

ถ้ามีการแก้หรือมีโค้ดเกี่ยวกับ database ให้สั่ง

```bash
rails db:migrate
```

ให้ลองเข้าหน้า http://127.0.0.1:3000/friends ดู

เวลาสร้าง controller ให้ใช้ plural เช่น จาก User ก็เป็น Users

> Scaffolding is a temporary structure used to support a work crew to aid in the construction, maintenance and repair of buildings, bridges and all other man-made structures. – Wikipedia
>

เวลาสร้างมาจะมี controller, model และ view (index, show, new, edit) + route ใหม่ + migration มาให้

One of the big benefits of using a scaffolding command is that all the files are created using the correct naming conventions, which avoids strange error messages. It also saves you the work of having to manually create these files.

It’s considered good practice to delete auto-generated files that you don’t plan on using. So after using a generator like “g controller”, review the list of files created & remove those that you don’t need.


## References

- https://www.rubyguides.com/2020/03/rails-scaffolding/
- https://ashvinchoudhary.medium.com/a-guide-to-scaffolding-in-ruby-on-rails-6c080f67e7fd