# Setting up a Ruby on Rails Project

ติดตั้ง Rails ลงบนเครื่องโดยใช้คำสั่ง

```bash
mise exec ruby -- gem install rails
```

> The name of the scaffold follows the convention of models, which are singular, rather than resources and controllers, which are plural. Thus, we have User instead of Users.
>

เวลาสร้าง controller ให้ใช้ plural เช่น จาก User ก็เป็น Users

> Scaffolding is a temporary structure used to support a work crew to aid in the construction, maintenance and repair of buildings, bridges and all other man-made structures. – Wikipedia

*— Hartl, Michael. Ruby on Rails Tutorial (p. 72). Pearson Education. Kindle Edition.*

สังเกตว่าเราจะมีคำสั่ง `mise exec ruby --` นำหน้าอยู่ตลอดเวลา เพื่อใช้คำสั่งผ่าน mise ทีนี้เราสามารถละคำสั่งได้โดยการรัน

```bash
eval "$(mise activate zsh)"
```

หรือเอาคำสั่งด้านบนนี้ไปใช้ไว้ใน Configuration ของ Shell ของเรา (เช่นไฟล์ `~/.zshrc` เป็นต้น)

ปล. จากนี้ไปจะละคำสั่ง `mise exec ruby --` ไว้ในฐานที่เข้าใจ

## Scaffold

```bash
rails generate scaffold Todo name:string description:text
```

หรือเราจะย่อคำว่า `generate` ให้เหลือ `g` แทน

```bash
rails g scaffold Todo name:string description:text
```

เวลาสร้าง Scaffold มาจะมีไฟล์ Controller, Model และ View มาให้ รวมไปถึงการมี Route ใหม่ และไฟล์ Migration มาให้เลย

จากนั้นให้เราสั่ง

```bash
rails db:migrate
```

ซึ่งถ้าเรามีการแก้หรือมีโค้ดเกี่ยวกับ Database เราจะใช้คำสั่งข้างต้นนี้เสมอ

ให้ลองเข้าหน้า [http://127.0.0.1:3000](http://127.0.0.1:3000){target=_blank} ดู

## References

- [What is Scaffolding in Ruby on Rails?](https://www.rubyguides.com/2020/03/rails-scaffolding/){target=_blank}
- [A Guide to Scaffolding in Ruby on Rails](https://ashvinchoudhary.medium.com/a-guide-to-scaffolding-in-ruby-on-rails-6c080f67e7fd){target=_blank}
