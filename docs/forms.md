# Forms

```ruby
def index
  @tweet = Tweet.new
end
```

```html
<%= link_to root_path do %>Home<% end %>
<p>Find me in app/views/tweets/index.html.erb</p>

<h1>Tweets</h1>

<%= form_with model: @tweet do |form| %>
  <% if form.object.errors.any? %>
    <div>
      <h2><%= pluralize(form.object.errors.count, "error") %> prohibited this Tweet from being saved:</h2>

      <ul>
        <% form.object.errors.each do |error| %>
          <li><%= error.full_message %></li>
        <% end %>
      </ul>
    </div>
  <% end %>
  <%= form.text_area :body, placeholder: "What's on your mind?" %>

  <div>
    <%= form.submit "Tweet" %>
  </div>
<% end %>
```

```ruby
get 'tweets/index', as: :tweets
```

```ruby
resources :tweets, only: [:index, :create]
```

```ruby
def create
  @tweet = Tweet.new(tweet_params)
  @tweet.user = User.find_by(username: "zkan")
  respond_to do |format|
    if @tweet.save
      puts "success"
      #format.turbo_stream
      #format.html { redirect_to tweet_url(@tweet), notice: "Tweet was successfully created." }
      format.html { redirect_to root_path, notice: "Tweet was successfully created." }
    else
      puts "error"
      format.html do
        #flash[:tweet_errors] = @tweet.errors.full_messages
        redirect_to root_path
      end
    end
  end
end

private

def tweet_params
  params.require(:tweet).permit(:body)
end
```

```ruby
def index
  @tweet = Tweet.new
  @tweets = Tweet.all.order(created_at: :desc)
end
```

```html
<div id="tweets">
  <%= render @tweets %>
</div>
```

```html
<div id="<%= dom_id tweet %>">
  <%= tweet.body %>
</div>
```
