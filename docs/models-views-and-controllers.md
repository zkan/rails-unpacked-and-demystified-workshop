# Models, Views, and Controllers (+ Minitest)

เพิ่มโค้ดนี้ใน `config/routes.rb`

```ruby
get "/projects", to: "projects#index"
```

เราจะให้ลิ้งค์ไปที่ Action ที่ชื่อ `index`

```bash
rails g controller Projects
```

```ruby
class ProjectController < ApplicationController
  def index
    @projects = Project.all
  end
end
```

```bash
rails g model Project name:string
```

```bash
rails db:migrate
```

```ruby
class Project < ApplicationRecord
end
```

พวก Method ต่าง ๆ จะอยู่ใน `ApplicationRecord` อยู่แล้ว

สร้างไฟล์ `app/views/project/index.html.erb`

```erb
<h1>Projects</h1>

<ul>
  <% @projects.each do |project| %>
    <li><%= project.name %></li>
  <% end %>
</ul>
```

ลองเพิ่มข้อมูลใน Rails Console

```ruby
Project.create(name: "Plan a Vacation")
Project.create(name: "House Repairs")
```

ถ้าเราอยากจะดูข้อมูลแต่ละข้อมูลแบบ Dynamic เพิ่ม `config/routes.rb`

```rb
get "/projects/:id", to: "projects#show", as: "project"
```

เราจะได้ `params` เข้ามาใน Action ที่ชื่อ `show` ให้เราเพิ่มโค้ดตามนี้

```ruby
def show
  @project = Project.find(params[:id])
end
```

ให้เพิ่มไฟล์ `show.html.erb`

```erb
<h1><%= @project.name %></h1>
<p>Project details coming soon ...</p>
```

ทำลิ้งค์ที่หน้า Index เราสามารถใช้ `link_to` ได้ ให้แก้โค้ดให้เป็นตามนี้

```erb
<%= link_to project.name, project_path(project) %>
```

## MVC Pattern

Controller

`app/controllers`

Model

`app/models`

View

`app/views`

When we create a webpage, we’ll edit `config/routes.rb`

Rails มี generator คอยช่วยให้เราไม่ต้องเขียนโค้ดเยอะ

```bash
rails generate controller Home index
```

หรือ

```bash
rails g controller Home index
```

![Screen Shot 2564-12-26 at 17.53.15.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/388644b5-3e79-4200-950e-e0812e48b05c/Screen_Shot_2564-12-26_at_17.53.15.png)

คำสั่งนี้จะไปแก้ไฟล์ `config/routes.rb` ให้เราด้วย ลองเข้าหน้า http://127.0.0.1:3000/home/index ก็จะเห็นหน้าที่เพิ่มเข้ามา

ถ้าอยากให้หน้าแรกเป็นอะไรจะทำแบบนี้

```ruby
Rails.application.routes.draw do
  root 'home#index'
end
```

ใช้คำสั่งด้านล่างเพื่อดู routes ทั้งหมด

```bash
rails routes
```

แก้ไฟล์ `app/views/home/index.html.erb` แล้วลองเข้า [http://127.0.0.1:3000](http://127.0.0.1:3000/home/index)

## Create a New Page Manually

สร้างไฟล์ `app/views/home/about.html.erb`

สร้าง `about` ใน `app/controllers/home_controller.rb`

```ruby
class HomeController < ApplicationController
  def index
  end

  def about
  end
end
```

แก้ `config/routes.rb`

```ruby
Rails.application.routes.draw do
  get 'home/index'
  get 'home/about'
  root 'home#index'
  # Define your application routes per the DSL in https://guides.rubyonrails.org/routing.html

  # Defines the root path route ("/")
  # root "articles#index"
end
```

ลองเข้าหน้า http://127.0.0.1:3000/home/about

### Partial Template

สร้างไฟล์ `app/views/home/_header.html.erb`

ไปแก้ไฟล์ `app/views/layout/application.html.erb`

```html
<%= render 'home/header' %>
<div class="container">
  <%= yield %>
</div>
```

Rails จะรู้ว่าตอน render จะไปหยิบ `_header.html.erb` เอง

## Navigating with link_to

ของ root ต้องใช้ `root_path`

```html
<%= link_to 'Rails Friends', root_path, class:"navbar-brand" %>
```

```html
<a class="navbar-brand" href="#">Rails Friends</a>
```

ของหน้าอื่นๆ เราสามารถใช้ชื่อ controller ได้เลย เช่น controller `home` มี method ชื่อ `about` ตัว route ก็จะเป็น `home_about_path` สังเกตว่า suffix จะเป็น `_path` เสมอ

```html
<%= link_to 'About Us', home_about_path, class:"nav-link" %>
```

```html
<a class="navbar-link" href="#">About Us</a>
```

เราสามารถดู routes ทั้งหมดได้โดยสั่ง

```bash
rails routes
```

### Associations

เชื่อมระหว่าง 2 tabls (user กับ friend) เพื่อให้เราสามารถแยกเพื่อนของแต่ละ user ได้

`app/models/friend.rb`

```ruby
class Friend < ApplicationRecord
  belongs_to :user
end
```

`app/models/user.rb`

```ruby
class User < ApplicationRecord
  # Include default devise modules. Others available are:
  # :confirmable, :lockable, :timeoutable, :trackable and :omniauthable
  devise :database_authenticatable, :registerable,
         :recoverable, :rememberable, :validatable

  has_many :friends
end
```

ต่อไปเราจะเชื่อมกันระหว่าง friend กับ user ซึ่งเราจะเพิ่มฟิลด์ `user_id` ที่ table ชื่อ friend

สร้างผ่าน generate

```bash
rails g migration add_user_id_to_friends user_id:integer:index
```

ใส่ index ด้วย เพื่อบอกว่าให้ index ที่ฟิลด์นี้ด้วย

สั่งเสร็จจะเกิดไฟล์ migration ขึ้นใหม่

![Screen Shot 2564-12-26 at 20.05.48.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/a3e03ca5-4769-4723-878b-f15ae6ee6e83/Screen_Shot_2564-12-26_at_20.05.48.png)

สั่ง

```bash
rails db:migrate
```

### Refer to Variable in Controller from View

`app/controllers/home_controller.rb`

```ruby
class HomeController < ApplicationController
  def index
  end

  def about
    # instance variable can be called in view
    @about_me = "My name is Kan Ouivirach"
  end
end
```

`app/views/home/about.html.erb`

```html
<h1>About Me</h1>

<p>
  <%= @about_me %>
</p>
```

### Debugging with Rails Console

สั่ง

```bash
rails console
```

หรือ

```bash
rails c
```

เราจะสามารถใช้ Active Record ได้หมดเลย

![Screen Shot 2564-12-26 at 21.05.19.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/9513920c-fff6-42fb-b842-04375d4d7088/Screen_Shot_2564-12-26_at_21.05.19.png)


### Creating About Page (Manually)

`config/routes.rb`

```ruby
Rails.application.routes.draw do
  # Define your application routes per the DSL in https://guides.rubyonrails.org/routing.html

  # Defines the root path route ("/")
  # root "articles#index"

  get "about", to: "about#index"
end
```

`app/controllers/about_controller.rb`

```ruby
class AboutController < ApplicationController

  def index
  end

end
```

`app/views/about/index.html.erb`

```html
<h1>About Us<h1>
```