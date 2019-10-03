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
