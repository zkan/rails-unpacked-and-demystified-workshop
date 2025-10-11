# Models, Views, and Controllers (+ Minitest)

## MVC Pattern

* Model จะอยู่ในโฟลเดอร์ `app/models`
* Controller จะอยู่ในโฟลเดอร์ `app/controllers`
* View จะอยู่ในโฟลเดอร์ `app/views`

ไฟล์ `config/routes.rb` จะเป็นไฟล์สำหรับ Routes เราสามารถดู Routes ทั้งหมดได้โดยใช้คำสั่ง

```bash
rails routes
```

Rails มี generator คอยช่วยให้เราไม่ต้องเขียนโค้ดเยอะ

```bash
rails generate controller Home index
```

หรือ

```bash
rails g controller Home index
```

คำสั่งนี้จะไปแก้ไฟล์ `config/routes.rb` ให้เราด้วย ลองเข้าหน้า [http://127.0.0.1:3000/home/index](http://127.0.0.1:3000/home/index){target=_blank} ก็จะเห็นหน้าที่เพิ่มเข้ามา

ถ้าอยากให้หน้า Home ของเราเป็นหน้าแรกของเว็บ เราจะเพิ่มบรรทัดด้านล่างนี้เข้าไปที่ไฟล์ `config/routes.rb`

```ruby
root "home#index"
```

แล้วลองเข้า [http://127.0.0.1:3000](http://127.0.0.1:3000){target=_blank}

## Create a New Page Manually

สร้างไฟล์ `app/views/home/about.html.erb`

```html
<h1>About Us<h1>
```

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

ลองเข้าหน้า [http://127.0.0.1:3000/home/about](http://127.0.0.1:3000/home/about){target=_blank}

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

### Navigating with link_to

ของ root ต้องใช้ `root_path`

```erb
<%= link_to "Home", root_path, class: "navbar-brand" %>
```

ของหน้าอื่นๆ เราสามารถใช้ชื่อ controller ได้เลย เช่น controller `home` มี method ชื่อ `about` ตัว route ก็จะเป็น `home_about_path` สังเกตว่า suffix จะเป็น `_path` เสมอ

```erb
<%= link_to "About Us", home_about_path, class: "nav-link" %>
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

## Adding Project to the App

### Creating a Controller

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

### Creating a Model

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

### Creating a View

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

### Adding CRUD Actions

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

New

```ruby
get "/projects/new", to: "projects#new", as: "new_project"
```

ต้องมาก่อน Route ของ `show` ด้วย เพราะว่า Routes จะ Match ตาม Order จากบนลงล่าง

เพิ่ม Action `new` ให้มีแค่ Initiation

```ruby
def new
  @project = Project.new
end
```

สร้างไฟล์ `new.html.erb`

```erb
<h1>New Project</h1>

<%= form_with model: @project do |f| %>
  <div class="field">
    <%= f.label :name %>
    <%= f.text_field :name %>
  </div>

  <div class="actions">
    <%= f.submit "Create Project" %>
  </div>
<% end %>
```

```ruby
def create
  @project = Project.new(project_params)

  if @project.save
    redirect_to project_path(@project)
  else
    render :new
  end
end
```

เพื่อบอก Rails ให้มีแค่ `name` เท่านั้นที่ยอมให้ผ่าน

```ruby
def project_params
  # params.require(:project).permit(:name)
  params.expect(project: [ :name ])
end
```

```ruby
post "/projects", to: "projects#create"
```

เพิ่ม Validation

```ruby
class Project < ApplicationRecord
  validates :name, presence: true
  # validates :name, presence: { message: "Did you forget to add a name?" }
end
```

เพิ่มโค้ดนี้ใน Form

```erb
  <% if @project.errors.any? %>
    <div class="errors">
      <h2><%= pluralize(@project.errors.count, "error") %> prohibited this project from being saved:</h2>
      <ul>
        <% @project.errors.full_messages.each do |message| %>
          <li><%= message %></li>
        <% end %>
      </ul>
    </div>
  <% end %>
```

```ruby
render :new, status: :unprocessable_entity
```

```ruby
get "/projects/:id/edit", to: "projects#edit", as: "edit_project"
```

```ruby
def edit
  @project = Project.find(params[:id])
end
```

ไฟล์​ `edit.html.erb` ใช้เหมือน `new.html.erb` เลย

```ruby
patch "/projects/:id/edit", to: "projects#update"
```

```ruby
def update
  @project = Project.find(params[:id])

  if @project.update(project_params)
    redirect_to project_path(@project)
  else
    render :edit, status: unprocessable_entity
  end
end
```

ใช้ Partials

เอา Form ทั้งหมดไปใช้ `_form.html.erb`

ปรับไฟล์หลักให้เหลือ

```erb
<% render "form", project: @project %>
```

ส่ง `project` เข้าไป ดังนั้นใน `_form.html.erb` ให้ลบ `@` ออกด้วย

Flash message

```ruby
if @project.save
  flash[:notice] = "Project created successfully!"
  ...
```

เพิ่มโค้ดด้านล่างนี้

```erb
<%= if flash[:notice] %>
  <p class="flash"><%= flash[:notice] %></p>
<% end %>
```

ไว้ที่ `application.html.erb`

```ruby
delete "/projects/:id", to: "projects#destroy"
```

```ruby
def destroy
  @project = Project.find(params[:id])

  @project.destroy
  flash[:notice] = "Project deleted."
  redirect_to projects_path
end
```

```erb
<% button_to "Delete Project", project_path(@project), method: :delete %>
```

เปลี่ยน Routes ทั้ง 7 ให้เหลือแค่นี้ได้เลย

```ruby
resources :projects
```

### Migrations

```bash
rails g migration AddCompletedAndPriorityToTodoes completed:boolean priority:integer
```

```bash
rails g migration AddActiveToProjects active:boolean
```

```bash
rails db:migrate
```

### Associations

Todos <> Projects

```bash
rails g migration AddProjectIdToTodos project:references
```

```bash
rails db:drop db:create db:migrate
```

```ruby
class Todo < ApplicationRecord
  belongs_to :project
end
```

```ruby
class Project < ApplicationRecord
  has_many :todos, dependent: :destroy
  validates :name, presence: true
end
```

เพิ่มฟิลด์ในไฟล์ `app/views/todos/_form.html.erb`

```erb
<div>
  <%= form.label :completed, style: "display: block" %>
  <%= form.check_box :completed %>
</div>

<div>
  <%= form.label :priority, style: "display: block" %>
  <%= form.number_field :priority %>
</div>

<div>
  <%= form.label :project_id, style: "display: block" %>
  <%= form.collection_select :project_id, Project.all, :id, :name, prompt: "Select a project" %>
</div>
```

ต้องอัพเดท Controller ด้วย เพิ่มไปใน Strong Parameters

```ruby
params.expect(todo: [ :name, ..., :compelete, :priority, :project_id ])
```

`app/views/todos/_todo.html.erb`

```erb
<p>
  ...
</p>

<p>
  <strong>Project:</strong>
  <%= link_to todo.project.name, project_path(todo.project) %>
</p>
```

เพิ่มโค้ดด้านล่างนี้ `app/view/projects/show.html.erb`

```erb
<ul>
  <% @proejct.todos.each do |todo| %>
    <li>
      <%= link_to todo.name, todo_path(todo) %>
      <% if todo.completed %>✅<% end %>
    </li>
  <% end %>
</ul>
```
