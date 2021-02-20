# Tools

## Heroku for deployment

`heroku create`

`git push heroku main`

`heroku log` to see logs, good for debug

`heroku run <command>` to run command on heroku. 
- `heroku run rails db:migrate` to do db migration on prod
- `heroku run rails console` to look up prod db
- `heroku maintenance:on/off` to turn on/off maintenace mode of prod, run this before deploying to prod with db:migrate
- `heroku pg:reset DATABASE` to reset pg database from heroku


# Rails

`rails new <app_name>` to create new rail app

Change Gemfile, `bundle install --without production` for reinstall the gems

`rails server` to start server

`rails generate scaffold <model_name> <schema>` to generate scaffold code
`rails generate scaffold User name:string email:string`

This will generate a db/migrate file. Run `rails db:migrate` to udpate database. Run `rails db:rollback` to undo db migration

`rails generate controller StaticPages home help` generate controller for /static_pages/home and /static_pages/help

`rails destroy <command>` will undo the generated code by `rails generate`.


`rails console` neat tool with DSL to query database.

`rails generate integration_test <test_name>` to generate integration test

`rails test:integration` to run integration test

you can put `debugger` in rails code to print debug information in console.



## ActiveRecord (ApplicationRecord)
`rails generate model User name:string email:string` generate data model (migrate file). Rails will generate id, create_ts and update_ts automatically

`rails console --sandbox` bring up sandbox console

```
# create/delete
user = User.new()

user.save

user.create()
user.destroy

# search
User.find(id)  # find by key
User.find_by(id) # find by non-key column
User.all  # get all users

# update
user.email = "different_eamil@test.com"  # update user in memory
user.save

user.update_attributes(name: 'the name', email: 'the email') # update user in database

```

Use db/seed.rb to generate sample data, then use `rails db:seed` to populate user into database.

### `has_secure_password`
A rails method that will generate 
1. ability to save `password_digest` column in database (needs explicit db migration)
2. `password` and `password_confirmantion` _virtual_ attributes for the modeled data.
3. `authenticate` method that returns the modeled data (etc. user) when the password is correct, and `false` otherwise.

### ActiveRecord Callbacks
order of callback execution
(-) save

(-) valid

(1) before_validation

(-) validate

(2) after_validation

(3) before_save

(4) before_create

(-) create

(5) after_create

(6) after_save

(7) after_commit

### `validates`
ensures only valid data is saved into database.

`validates` will only happend on `create`, `save` and `update`, i.e. before deadling with database write

validation errors are stored in `user.errors`

### the 'uglies'

#### `to_users` in db migration has special meaning!

> To implement the data model in Figure 6.9, we first generate an appropriate migration for the password_digest column. We can choose any migration name we want, but it’s convenient to end the name with to_users, since in this case Rails automatically constructs a migration to add columns to the users table. The result, with migration name add_password_digest_to_users, appears as follows:

## Guard automated test
With `Guardfile`, you can specify which test to run for which file change. 
`bundle exec guard init` to init Guardfile
`bundle exec guard` to start automated test

## Rails flavoed Ruby

- Blocks allows you do this `%w{a, b, c}.map{&:upcase}`


## User password and authentication

`rails generate migration add_password_digest_to_user password_digest: string` to create db migration to add column password_digest

`rails db:migrate` to do migration

```
class User < ApplicationRecord
  # ...
  
  # this will let rails create password and password_confirmation to user class as attributes.
  has_secure_password
 end
```

When creating user, pass in `password` and `password_confirmation`. Rails will create a hash `password_digest` and store in database. 

When authenticating user, `user.authenticate('password')` will return user if password matches and return false otherwise.


---

`form_with` to create form to take user input and transform to POST or PATCH request

[`accepts_nested_attributes_for`](https://api.rubyonrails.org/classes/ActiveRecord/NestedAttributes/ClassMethods.html) to populate associated model fields into `params`. Commonly used for `form_with`


