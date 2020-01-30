# YamlDb

YamlDb is a database-independent format for dumping and restoring data.  It complements the database-independent schema format found in db/schema.rb.  The data is saved into db/data.yml.

This can be used as a replacement for mysqldump or pg_dump, but only for the databases typically used by Rails apps.  Users, permissions, schemas, triggers, and other advanced database features are not supported - by design.

Any database that has an ActiveRecord adapter should work.

This gem supports Rails versions 3.0 through 5.2.

[![Build Status](https://travis-ci.org/yamldb/yaml_db.svg?branch=master)](https://travis-ci.org/yamldb/yaml_db)

## Installation

Simply add to your Gemfile:

    gem 'yaml_db', :git => 'git://github.com/semo94/yaml_db.git'

All rake tasks will then be available to you.

## Usage

    rake db:data:dump   ->   Dump contents of Rails database to db/data.yml
    rake db:data:load   ->   Load contents of db/data.yml into the database

Further, there are tasks db:dump and db:load which do the entire database (the equivalent of running db:schema:dump followed by db:data:load).  Also, there are other tasks recently added that allow the export of the database contents to/from multiple files (each one named after the table being dumped or loaded).

    rake db:data:dump_dir   ->   Dump contents of database to curr_dir_name/tablename.extension (defaults to yaml)
    rake db:data:load_dir   ->   Load contents of db/#{dir} into database (where dir is ENV['dir'] || 'base')

If you would like to limit a database dump to a single table add a table parameter to the rake task.

    rake db:data:dump["table_name"]   ->   Dump contents of Rails database table "table_name" to db/data.yml

If you use dump from Ruby code you can add a list of tables to dump.

    Rake::Task['db:data:dump'].invoke(%w(table1 table2 table3))  ->   Dump contents of Rails database tables "table1" "table2" "table3" to db/data.yml

In addition, we have plugins whereby you can export your database to/from various formats.  We only deal with yaml and csv right now, but you can easily write tools for your own formats (such as Excel or XML).  To use another format, just load setting the "class"  parameter to the class you are using.  This defaults to "YamlDb::Helper" which is a refactoring of the old yaml_db code.  We'll shorten this to use class nicknames in a little bit.

## Examples

One common use would be to switch your data from one database backend to another.  For example, let's say you wanted to switch from SQLite to MySQL.  You might execute the following steps:

1. rake db:dump

2. Edit config/database.yml and change your adapter to mysql, set up database params

3. mysqladmin create [database name]

4. rake db:load

## Credits

Created by Orion Henry and Adam Wiggins. Major updates by Ricardo Chimal Jr. and Nate Kidwell.
