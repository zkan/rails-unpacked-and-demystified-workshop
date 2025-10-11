# Hotwire Basics

HTML over the Wire

Hotwire stands for HTML over the Wire. So literally, Hotwire refers to the idea of sending HTML instead of data.

Hotwire is the default front-end framework for Ruby on Rails applications. It is a combination of Stimulus, Turbo, and Turbo Drive. It was introduced in Ruby on Rails 6 as an optional feature. By Ruby on Rails 7, it was included as a default option.

The goal of hotwire is to speed up SPA (single page application) and other applications.

## Why use Hotwire?

Using hotwire has the following benefits:

- Reduce server requests by using Turbo Streams to update parts of a page without a full server request.
- Use Turbo Drive to load linked pages in the background and swap out only the content that needs to be changed.
- Use stimulus.js to progressively enhance the user interface, making it feel faster and more responsive.
- Use Turbo Streams to send only the data that needs to be updated
on the page, reducing the amount of data transferred between server and
client.
- Reduce your development time by using Hotwire’s cohesive set of
tools to avoid learning and integrating multiple third-party libraries.
- It offers CSRF and XSS attack prevention by default, which will give you more security.

## Turbo Frames

```erb
<%# app/views/todos/show.html.erb %>
<%= turbo_frame_tag @todo do %>
  <p><%= @todo.description %></p>

  <%= link_to "Edit this todo", edit_todo_path(@todo) %>
<% end %>

<%# app/views/todos/edit.html.erb %>
<%= turbo_frame_tag @todo do %>
  <%= render "form" %>

  <%= link_to "Cancel", todo_path(@todo) %>
<% end %>
```

ตอนที่กดปุ่ม "Edit this todo" แล้ว turbo frame จะเอาของที่อยู่ใน `edit.html.erb` มาแทนที่
