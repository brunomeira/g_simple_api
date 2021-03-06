[![Gem Version](https://badge.fury.io/rb/google_simple_api.svg)](http://badge.fury.io/rb/google_simple_api)
[![Build Status](https://travis-ci.org/brunomeira/google_simple_api.svg?branch=master)](https://travis-ci.org/brunomeira/google_simple_api)

# GoogleSimpleApi

This gem wraps up calls executed by [google-api-ruby-client](https://github.com/google/google-api-ruby-client)
and makes it simple for you to be authorized and access the data provided by Google APIs.

## Getting Started

Add this line to your application's Gemfile:

    gem 'google_simple_api'

And then execute:

    $ bundle

Or install it yourself as:

    $ gem install google_simple_api

## How to Use

To use the gem you need to have a project set up on Google. In order to do it so, you have to visit [https://console.developers.google.com](https://console.developers.google.com), allow access to the required APIs and ,lastly, create a new Client ID with a callback URL associated with it.

After you complete the pre-setting process, you can go back to your app and work on the following steps:

#### First time use
1. Create a *google_simple_api.rb* in /initializers

         GoogleSimpleApi.setup do |config|
          config.name = "test" #The name of your application
          config.version = "1" #The current version of your application
          config.api = "drive" #The api that will be used.
          config.api_version = "v2" #If not specified, it will defaults to v1
          config.client_id = "XXXXXXXXXXXXXXXX" #Value provided in the Credentials Section.
          config.client_secret = "XXXXXXXXXXXXXX" #Value provided in the Credentials Section.
          config.scope = "drive" # The scope(s) required to use the api, it can be an Array.
         end

2. You now have to send the user to Google's authorization page. Use:

         GoogleSimpleApi.process.authorize_url("specified callback")

to receive the proper URL and then redirect the user to it.

3. If the user accepts the access to the app, Google will redirect you back to the "specified callback" along with a "code" parameter. Use this code to get a valid token.

        manager = GoogleSimpleApi.process
        manager.get_token(params[:code])
For further use, You should store the values returned from this call.

4. To make any call to the api, you have to invoke:

        manager.execute("method.name", {key1: "value", key2:"value2"}) # Example: manager.execute("files.list")
where the first parameter is the action that will be executed and the second is any required or optional parameter.

5. I hope you enjoy it.

#### Using refresh token to request token

Tokens may expire due a variety of different reasons. If you have stored the user's refresh token then you won't need to ask
the users to go through all the acceptance process again (This is true for cases where the users didn't remove access to the app, you didn't change the scope of the app, or etc...). In order to do it so, you only need to call:

        manager = GoogleSimpleApi.process
        manager.get_token("refresh_token", true) # The second parameter tells the function you are passing a refresh_token

## Further Reading
If you are having trouble to understand the gem or want something more customizable, check the links below:

1. [google-api-ruby-client](https://github.com/google/google-api-ruby-client) - GoogleSimpleApi is based on this gem. If you need something more complex than get a token and access the api, you should check it out.

2. [Google OAUTH process](https://developers.google.com/accounts/docs/OAuth2WebServer)  If you need more background on how the authorization process works.

3. [APIs services](https://developers.google.com/apis-explorer) - Interesting resource, If you need to know which apis,services and scopes are available.

## Contributing

1. Fork it ( https://github.com/brunomeira/google_simple_api/fork )
2. Create your feature branch (`git checkout b my-new-feature`)
3. Commit your changes (`git commit am 'Add some feature'`)
4. Push to the branch (`git push origin my-new-feature`)
5. Create a new Pull Request
