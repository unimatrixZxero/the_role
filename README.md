## TheRole

Gem for providing simple, but powerful and flexible role system for ROR3 applications.
Based on Hashes.

* Based on MVC semantik (easy to understand what's happening)
* Realtime dynamically management with simple interface
* Customizable

##  Installing

Gemfile

``` ruby
  gem 'the_role'
```

``` ruby
  bundle
```

``` ruby
rake the_role_engine:install:migrations
  >> Copied migration 20111028145956_create_roles.rb from the_role_engine
```

``` ruby
rake db:roles:create
  >> Administrator, Moderator of pages, User, Demo
```

When gem initialize, **User**, **Role**, **ApplicationController** classes are extending with next methods:

## User

``` ruby
# will be include into User.rb automaticaly
belongs_to :role
attr_accessible :role
after_save { |user| user.instance_variable_set(:@the_role, nil) }

# methods
the_role
admin?
moderator?(section)
has_role?(section, policy)
owner?(obj)

# instance variables
@the_role
```

## Role

``` ruby
# will be include into Role.rb automaticaly
has_many :users
validates :name,  :presence => {:message => I18n.translate('the_role.name_presence')}
validates :title, :presence => {:message => I18n.translate('the_role.title_presence')}
```

## ApplicationController

``` ruby
# private methods (should be define as before filters)
the_role_access_denied
the_role_object
the_role_require
the_owner_require
```
## Routes

method **the_role_access_denied** needs **root_path** for redirect

### Now try to use into console

``` ruby
rails g scaffold article title:string content:text user_id:integer
rails g scaffold page title:string content:text user_id:integer
rails c
  >> User.new(:login => 'cosmo', :username => 'John Black').save
  >> u = User.first
  >> u.role = Role.first # admin
  >> u.save
  >> u.has_role? :x, :y
    => true
  >> u.role = Role.last # demo
  >> u.save
  >> u.has_role? :pages, :show
    => true
  >> u.has_role? :pages, :delete
    => flase
```

## Example

``` ruby
- if curent_user.has_role? :pages, :show
  Page content
- else
  Access denied
```

Copyright (c) 2011 [Ilya N. Zykin Github.com/the-teacher], released under the MIT license