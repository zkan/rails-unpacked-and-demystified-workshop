# Solid Cable, Solid Cache and Solid Queue


## Real-time Messaging with Solid Cable


## Caching with Solid Cache


## Managing Asynchronous Tasks with Solid Queue

```bash
rails g job task_archiver_job
```

```ruby
class TaskArchiverJob < ApplicationJob
  queue_as :default

  def perform(*args)
    Task.where(status: "completed").update_all(status: "archived")
  end
end
```

ปรับโค้ดเป็นแบบนี้ได้ เพราะ Method นี้ไม่ได้รับค่าอะไรเข้ามาใช้งาน

```ruby
  def perform
    Task.where(status: "completed").update_all(status: "archived")
  end
```

เข้าไปแก้ `config/database.yml` เพิ่ม `queue` ใน `development`

ต่อไปเราจะ Configure ให้ Development Environment ของเราไปใช้ Solid Queue เป็น Queue Adapter ให้เข้าไปที่ไฟล์ `config/environments/development.rb` ให้แก้ค่าของ `config.active_job.queue_adapter` เป็น `:solid_queue` และค่าของ `config.solid_queue.connects_to` เป็น `{ database: { writing: :queue } }`

ทดสอบการรัน Job โดยให้เข้าไปที่ Rails Console

```ruby
TaskArchiverJob.perform_later
```

แล้วเราค่อยรันคำสั่งด้านล่างนี้

```bash
bin/jobs
```

เพื่อให้รัน Solid Queue

Solid Queue มี Scheduler Process ด้วย ทำให้เราสามารถสร้าง Recurring Jobs ได้ ให้เราเข้าไปที่ไฟล์ `config/recurring.yml` แล้วให้เพิ่ม `development` เข้าไป

```yml
development:
  periodic_cleanup:
    class: TaskArchiverJob
    queue: background
    schedule: every 30 seconds
```

### Mission Control — Jobs

แก้ `Gemfile` เพิ่มบรรทัดด้านล่างนี้เข้าไป

```
gem "mission_control-jobs"
```

เสร็จแล้วสั่ง

```bash
mise exec ruby -- bundle install
```

หรือใช้คำสั่งด้านล่างนี้คำสั่งเดียวก็ได้

```bash
mise exec ruby -- bundle add mission_control-jobs
```

หลังจากติดตั้งเสร็จแล้วให้เรา Configure ตามนี้ [Basic configuration](https://github.com/rails/mission_control-jobs?tab=readme-ov-file#basic-configuration){target=_blank}


```bash
rails g controller Admin
```

```ruby
class AdminController < ActionController::Base
  http_basic_authentication_with name: ENV['ADMIN_NAME'], password: ENV['ADMIN_PASSWORD']
end
```

```ruby
class AdminController < ActionController::Base
  http_basic_authentication_with name: "admin", password: "password"
end
```

เพิ่มบรรทัดด้านล่างนี้ใน `config/application.rb`

```ruby
config.mission_control.jobs.base_controller_class = "AdminController"
```
