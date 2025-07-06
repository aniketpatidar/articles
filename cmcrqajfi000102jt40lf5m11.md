---
title: "How I contributed notifications to an open-source product"
seoTitle: "How I contributed notifications to an open-source product"
seoDescription: "This post details my contribution to the notification system for Moneygun, an open-source, white-label SaaS boilerplate."
datePublished: Sun Jul 06 2025 13:49:06 GMT+0000 (Coordinated Universal Time)
cuid: cmcrqajfi000102jt40lf5m11
slug: how-i-contributed-notifications-to-an-open-source-product
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1751805807419/b8608399-9201-4f66-ab43-a433def52426.jpeg
tags: opensource, ruby-on-rails, notifications

---

This post details my contribution to the notification system for Moneygun, an open-source, white-label SaaS boilerplate. It provides a practical and actionable guide on integrating notifications into a Rails application using the [Noticed](https://github.com/excid3/noticed) gem.

[https://github.com/yshmarov/moneygun/pull/286](https://github.com/yshmarov/moneygun/pull/286)  
First, I want to share that I learned about [Yaroslav Shmarov](https://x.com/yarotheslav) (@yarotheslav) looking for contributors for their open-source product, Moneygun, through a Twitter post he made. I also wanted to explore the new version of the Noticed gem, so I decided to give it a try. That's how I got the opportunity to work on this [issue](https://github.com/yshmarov/moneygun/issues/285).

Do check out Moneygun if you want to build your next B2B SaaS app (software as a service): [https://github.com/yshmarov/moneygun](https://github.com/yshmarov/moneygun)

[![](https://cdn.hashnode.com/res/hashnode/image/upload/v1751806188505/251884e9-cc45-4543-a204-0a066a36b808.png align="center")](https://x.com/yarotheslav/status/1934196524633706540)

There are two things that need to be implemented. 

* In-app notifications
    
* Email
    

First, I explored the gem to understand how to implement basic boilerplate code for notifications, then began by adding the gem to the Rails application:

```ruby
# notifications
gem "noticed"
```

Or

Run the following command to add Noticed to your Gemfile:

```bash
bundle add "noticed"
```

Generate and then run the migrations:

```ruby
rails noticed:install:migrations
rails db:migrate
```

To start, create a Notifier:

We have two requirements here:

1. The user should be notified when they are invited to an organization.
    
2. The user should be notified when their membership request is accepted.
    

```bash
rails generate noticed:notifier MembershipInvitationNotifier
rails generate noticed:notifier MembershipRequestAcceptedNotifier
```

In this post, we'll deliver notifications by email, and in the next post, we'll cover turbo\_stream, which provides real-time UI updates to users' browsers.

First, let's take a look at how the generated notifier appears.

Three files are created under the notifiers:

```ruby
# app/notifiers/application_notifier.rb
class ApplicationNotifier < Noticed::Event
end
```

```ruby
# app/notifiers/membership_request_accepted_notifier.rb
# To deliver this notification:
#
# MembershipRequestAcceptedNotifier.with(record: @post, message: "New post").deliver(User.all)
class MembershipRequestAcceptedNotifier < ApplicationNotifier
 # Add your delivery methods
 #
 # deliver_by :email do |config|
 #   config.mailer = "UserMailer"
 #   config.method = "new_post"
 # end
 #
 # bulk_deliver_by :slack do |config|
 #   config.url = -> { Rails.application.credentials.slack_webhook_url }
 # end
 #
 # deliver_by :custom do |config|
 #   config.class = "MyDeliveryMethod"
 # end
 #
 # Add required params
 #
 # required_param :message
end
```

```ruby
# app/notifiers/membership_invitation_notifier.rb
# To deliver this notification:
#
# MembershipInvitationNotifier.with(record: @post, message: "New post").deliver(User.all)
class MembershipInvitationNotifier < ApplicationNotifier
 # Add your delivery methods
 #
 # deliver_by :email do |config|
 #   config.mailer = "UserMailer"
 #   config.method = "new_post"
 # end
 #
 # bulk_deliver_by :slack do |config|
 #   config.url = -> { Rails.application.credentials.slack_webhook_url }
 # end
 #
 # deliver_by :custom do |config|
 #   config.class = "MyDeliveryMethod"
 # end
 #
 # Add required params
 #
 # required_param :message
end
```

In the boilerplate code, you can see there's⁣`deliver_by: email`, so we will implement the mailer delivery method first.

We won't complicate things for now; we'll start with one notifier to give you an idea, and then you can implement another notifier on your own.

So we need to notify the user when he is invited to an organization. First, clean up the commented lines and keep only the ones that are needed.

```ruby
class MembershipInvitationNotifier < ApplicationNotifier
 # Add your delivery methods
 #
 # deliver_by :email do |config|
 #   config.mailer = "UserMailer"
 #   config.method = "new_post"
 # end
 #
 # Add required params
 #
 # required_param :message
end
```

Notifiers can use different helper methods. Inside a notification\_methods block, we also set up the message and URL helpers.

```ruby
notification_methods do
   # I18n helpers
   def message
     t(".message")
   end

   # URL helpers are accessible in notifications
   # Don't forget to set your default_url_options so Rails knows how to generate urls
   def url
     user_invitations_url
   end
 end
```

These helpers can be helpful when rendering a user’s notifications on the web.

```erb
<div>
  <% @user.notifications.each do |notification| %>
    <%= link_to notification.message, notification.url %>
  <% end %>
</div>
```

Calling the message helper in the ERB view will look for the following translation path:

```yaml
# config/locales/en.yml
en:
 notifiers:
   membership_invitation_notifier:
     notification:
       message: You've been invited to join %{organization_name}
```

Notifiers can choose required parameters using the `required_params`method. We need to declare `:organization` as a required parameter to ensure `params[:organization]` is available in our notifier:

```ruby
 required_params :organization
```

```ruby
 def message
   t(".message", organization_name: params[:organization].name)
 end
```

In this case, we need the mailer, so generate it by running

```bash
rails generate mailer MembershipMailer invitation_email
```

```ruby
# app/mailers/membership_mailer.rb
class MembershipMailer < ApplicationMailer
  # Subject can be set in your I18n file at config/locales/en.yml
  # with the following lookup:
  #
  #   en.membership_mailer.invitation_email.subject
  #
  def invitation_email
    @greeting = "Hi"
    mail to: "to@example.org"
  end
end
```

app/views/membership\_mailer/invitation\_email.text.erb

```erb
Membership#invitation_email

<%= @greeting %>, find me in app/views/membership_mailer/invitation_email.text.erb
```

Instead of using this default greeting, we want to send our message and include the action\_url. To do this, we need to have the notification object available, so we should pass it from our notifier like this:

```ruby
deliver_by :email do |config|
  config.mailer = "MembershipMailer"
  config.method = :invitation_email
  config.args   = -> { [ self ] }
end
```

Now we need to make a small change to our mailer method.

```ruby
# app/mailers/membership_mailer.rb
class MembershipMailer < ApplicationMailer
  def invitation_email(notification)
    setup(notification)
    mail(to: @recipient.email, subject: t(".subject", organization_name: @organization.name))
  end

  private

  def setup(notification)
    @organization = notification.params[:organization]
    @message = notification.message
    @recipient = notification.recipient
    @action_url = notification.url
  end
end
```

```ruby
# app/views/membership_mailer/invitation_email.text.erb
<%= @message %>

View your invitations: <%= @action_url %>
```

Now our notifier code looks like this:

```ruby
class MembershipInvitationNotifier < ApplicationNotifier
  # Add your delivery methods
  deliver_by :email do |config|
    config.mailer = "MembershipMailer"
    config.method = :invitation_email
    config.args   = -> { [ self ] }
  end

  # Add required params
  required_params :organization

  notification_methods do
    # I18n helpers
    def message
      t(".message", organization_name: params[:organization].name)
    end

    # URL helpers are accessible in notifications
    # Don't forget to set your default_url_options so Rails knows how to generate urls
    def url
      user_invitations_url
    end
  end
end
```

Our notifier setup is now complete. Next, we need to trigger this notifier:

```ruby
# app/models/membership.rb
class Membership < ApplicationRecord
  after_create :send_invitation_notification

  private

  def send_invitation_notification
    MembershipInvitationNotifier.with(organization: organization).deliver(user)
  end
end
```

With this setup in place, we have everything necessary to notify a user when they are invited to join an organization. Thank you for taking the time to read.