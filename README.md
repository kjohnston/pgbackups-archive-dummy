# pgbackups-archive-dummy

A test harness for [pgbackups-archive](http://github.com/kjohnston/pgbackups-archive) - a means of automating Heroku's pgbackups and archiving them to Amazon S3 via the `fog` gem.

This repository contains Rake tasks for building a dummy database on Heroku and profiling resource utilization during backup processes via New Relic to aid in the development of the `pgbackups-archive` gem.

## Use

* Fork this repo and create a Heroku app to push it to.
* Follow the [pgbackups-archive](http://github.com/kjohnston/pgbackups-archive) setup instructions.
* Use the `heroku run rake db:generate` rake task to generate a dummy database.
* Use the `heroku run rake pgbackups:archive` rake task to perform a backup of the dummy database.

## Contributing

1. [Fork it](https://github.com/kjohnston/pgbackups-archive/fork_select)
2. Create your feature branch (`git checkout -b my-new-feature`)
3. Commit your changes (`git commit -am 'Added some feature'`)
4. Push to the branch (`git push origin my-new-feature`)
5. [Create a Pull Request](https://github.com/kjohnston/pgbackups-archive-dummy/pull/new)

## License

* Freely distributable and licensed under the [MIT license](http://kjohnston.mit-license.org/license.html).
* Copyright (c) 2012-2013 Kenny Johnston
