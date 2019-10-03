This repo exists to demonstrate that the issue described in https://github.com/rails/rails/issues/28827 is still happening in the Rails master branch (as of Oct 3, 2019).

The steps to reproduce are as follows:
```
git clone git@github.com:bbuchalter/rails-issue-28827.git
cd rails-issue-28827
bundle install
bin/rails db:create
```

Observe that we create two databases when invoking `db:create`: development and test.
Now observe what happens when we invoke our drop command while using DATABASE_URL.
```
DATABASE_URL=sqlite3://$(pwd)/db/database_url.sqlite3 bin/rails db:create
```

As expected, the development environment now uses the DATABASE_URL.
What is unexpected is that the test environment does not.

It's unclear what the expected behavior should be in this case, but the cause of it is this:
https://github.com/rails/rails/blob/9f2c74eda07ea5a9e4e624d5575a717714088dbf/activerecord/lib/active_record/tasks/database_tasks.rb#L494

Because of `each_local_configuration`, there seems to be no way invoke these database rake on only the development environment to ensure DATABASE_URL is respected.

The smallest scope of change I can think to make would be to conditionalize this behavior so it does not get applied when DATABASE_URL is present.