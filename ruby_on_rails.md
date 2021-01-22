# Tools

## Heroku for deployment

`heroku create`

`git push heroku main`

`heroku log` to see logs, good for debug

`heroku run <command>` to run command on heroku. 
- `heroku run rails db:migrate` to do db migration on prod
- `heroku run rails console` to look up prod db


# Rails

`rails new <app_name>` to create new rail app

Change Gemfile, `bundle install --without production` for reinstall the gems

`rails server` to start server

`rails generate scaffold <model_name> <schema>` to generate scaffold code
`rails generate scaffold User name:string email:string`

This will generate a db/migrate file. Run `rails db:migrate` to udpate database.

`rails destroy <command>` will undo the generated code by `rails generate`.


`rails console` neat tool with DSL to query database.

