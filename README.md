# Pushpad - Web Push Notifications

Add native push notifications to your web app using [Pushpad](https://pushpad.xyz).

Features:

- notifications are delivered even when the user is not on your website
- users don't need to install any app or plugin
- you can target specific users or send bulk notifications

Currently push notifications work on the following browsers:

- Chrome (Desktop and Android)
- Firefox (44+)
- Safari

## Installation

Add this line to your application's Gemfile:

```ruby
gem 'pushpad'
```

And then execute:

    $ bundle install

Or install it yourself as:

    $ gem install pushpad

## Getting started

First you need to sign up to Pushpad and create a project there.

Then set your authentication credentials:

```ruby
Pushpad.auth_token = '5374d7dfeffa2eb49965624ba7596a09'
Pushpad.project_id = 123 # set it here or pass it as a param to methods later
```

- `auth_token` can be found in the user account settings. 
- `project_id` can be found in the project settings. If your application uses multiple projects, you can pass the `project_id` as a param to methods (e.g. `notification.deliver_to user, project_id: 123`).

## Collecting user subscriptions to push notifications

Pushpad offers two different products. [Learn more](https://pushpad.xyz/docs)

### Pushpad Pro

Choose Pushpad Pro if you want to use Javascript for a seamless integration. [Read the docs](https://pushpad.xyz/docs/pushpad_pro_getting_started)

If you need to generate the HMAC signature for the `uid` you can use this helper:

```ruby
Pushpad.signature_for current_user.id
```

### Pushpad Express

Add a link to let users subscribe to push notifications: 

```erb
<a href="<%= Pushpad.path %>">Subscribe anonymous to push notifications</a>

<a href="<%= Pushpad.path_for current_user # or current_user_id %>">Subscribe current user to push notifications</a>
```

`current_user` is the user currently logged in on your website.

When a user clicks the link is sent to Pushpad, automatically asked to receive push notifications and redirected back to your website.

## Sending push notifications

```ruby
notification = Pushpad::Notification.new({
  body: "Hello world!", # max 90 characters
  title: "Website Name", # optional, defaults to your project name, max 30 characters
  target_url: "http://example.com" # optional, defaults to your project website
})

# deliver to a user
notification.deliver_to user # or user_id

# deliver to a group of users
notification.deliver_to users # or user_ids

# deliver to some users only if they have a given preference
# e.g. only "users" who have a interested in "events" will be reached
notification.deliver_to users, tags: ['events']

# deliver to segments
notification.broadcast tags: ['segment1', 'segment2']

# deliver to everyone
notification.broadcast
```

If no user with that id has subscribed to push notifications, that id is simply ignored.

The methods above return an hash: 

- `"scheduled"` is the number of devices to which the notification will be sent
- `"uids"` (`deliver_to` only) are the user IDs that will be actually reached by the notification (unless they have unsubscribed since the last notification)

## License

The gem is available as open source under the terms of the [MIT License](http://opensource.org/licenses/MIT).

