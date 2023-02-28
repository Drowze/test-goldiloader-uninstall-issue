# Reproducing Goldiloader error on uninstall

This is a minimal rails 6.1 app generated with "rails new" and no args.

To reproduce Goldiloader error described [here](https://github.com/salsify/goldiloader/issues/33):
- install sqlite if you don't have already, then run:
  ```
  $ bundle install
  ```
- install and start redis
- setup the rails database:
  ```
  $ bundle exec rails db:setup
  ```
- On the app with Goldiloader, populate rails cache with an AR object with:
  ```
  $ GOLDILOADER_ENABLED=true bundle exec rails r 'user = User.create && Rails.cache.write("foo", user)'
  ```
- On the the app without Goldiloader, read from the rails cache:
  ```
  $ GOLDILOADER_ENABLED=false bundle exec rails c
  irb(main):001:0> Rails.cache.read("foo")
  /Users/Drowze/.gem/ruby/3.2.0/gems/activesupport-6.1.7.2/lib/active_support/inflector/methods.rb:273:in `const_get': uninitialized constant Goldiloader (NameError)

          Object.const_get(camel_cased_word)
                ^^^^^^^^^^
  Did you mean?  TestGoldiloader
  <internal:marshal>:34:in `load': undefined class/module Goldiloader:: (ArgumentError)
  ```
