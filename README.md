# WoopraRails

A gem for Ruby on Rails (version 3 and higher, wont work with Rails 4) to connect through the HTTP API interface to log pageviews, identities, and custom events to Woopra Analytics

## Installation

Add this line to your application's Gemfile:

    gem 'woopra_rails'

And then execute:

    $ bundle

Or install it yourself as:

    $ gem install woopra_rails

## Initial setup
  Within your app/config directory, add the file 'woopra.yml' that looks like the following (but with your account):

    development:
      account: woopra-rails
      dryrun: true
    test:
      account: woopra-rails
      dryrun: true
    production:
      account: woopra-rails
      dryrun: false

  At this point, Woopra should initialize on app load and be ready for usage.
  The last step is to extend your application controller with the ActiveSupport::Concerns functionality to allow for user identity tracking for anonymous users.  Add the following to your application_controller.rb

    include WoopraRails::Session

## Usage
  All API actions return a WoopraRails::Response object.  To check for success, assign the return to a variable, e.g. response, and call success? for a boolean return
    
    response = WoopraRails.identify("me")
    response.success? #returns true

  Identify the current user:
  We use the Rails session_id for user cookie management.  This session id is not sent over the wire, and is done with MD5 digest.
    
    response = WoopraRails.identify("Kevin Hopkins", "me@kevinlhopkins.com", session[:session_id])
  
  Log a pageview:
    
    WoopraRails.log_pageview("Page Title", "/path/to/this/page")

  Log a custom event:
  This functionality is pretty extensible.  It expects an event name, and a hash of params that are turned into Woopra event descriptors.
    
    WoopraRails.log_event(
      "my_test_event",
      {
        user_name: "Kevin Hopkins",
        user_state: "Virginia",
        user_skills: "Development, Rails, Woopra, Analytics"
      }
    )

  When running in dev or test where you don't want to record to your production dataset of analytics, set dryrun to true in your woopra.yml.  This will return success whenever a call is made, but the request wont be sent.

## Contributing

1. Fork it
2. Create your feature branch (`git checkout -b my-new-feature`)
3. Commit your changes (`git commit -am 'Add some feature'`)
4. Push to the branch (`git push origin my-new-feature`)
5. Create new Pull Request

## Contributions
  Rob Gleeson -- Thanks for the help and code reviews! [Github](http://github.com/robgleeson)

  GeneralAssemb.ly -- Thanks for the use-case to give me the opportunity to give back to the  Rails community (http://generalassemb.ly)
