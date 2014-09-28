[![Build Status](https://travis-ci.org/rubymonsters/speakerinnen_liste.png)](https://travis-ci.org/rubymonsters/speakerinnen_liste) [![Code Climate](https://codeclimate.com/github/rubymonsters/speakerinnen_liste.png)](https://codeclimate.com/github/rubymonsters/speakerinnen_liste)

speakerinnen_liste is a searchable web directory designed specifically for female conference speakers. 'Speakerinnen' are encouraged to sign up and provide professional information, including their area of expertise, any previous conferences they've presented at, contact details, etc.

The aim of the app is to provide a way for conference/event organisers to find and contact appropriate female speakers. (But obviously there are many different contexts in which it can be used...)

==================

Do you want to contribute?

If you want to contribute, you can get an overview over the open issues. We are happy to answer your questions if you consider to help. All the issues have a link to their specification. If you want to work on an issue feel free to assign yourself.

https://github.com/rubymonsters/speakerinnen_liste/issues/216

For your database.yml please copy config/database_example.yml 

==================

User Stories

Here the user is the organizer.

As a user, I want to search for speakers by topic, native language, and location without signing up/in.
As a user, I want to email speakers invitations to submit proposals.
As a user, I want to know that the speakers are "curated"--ie, that they're good.

As an admin, I want to see a list of all speakers.
As an admin, I want to edit speakers' toopics. 

As a speaker, I want to be able to sign up.
As a speaker, I want to receive an email to confirm signing up.
As a speaker, I want to receive an email "You've been invited to submit a proposal to X conference"
As a speaker, I want to be able to ask other speakers for help.

==================

Configuration

Here are the steps for configuring the development environment in Ubuntu 14.04.

First step is to install git. Run these commands in your terminal:
    sudo apt-get update
    sudo apt-get install git
    
Replace the name and email address in the following steps with the ones you used for your GitHub account.:    
    git config --global color.ui true
    git config --global user.name "YOUR NAME"
    git config --global user.email "YOUR@EMAIL.com"
    ssh-keygen -t rsa -C "YOUR@EMAIL.com"

For adding the newly generated SSH key to your GitHub account, enter the following command and paste the result here: https://github.com/settings/ssh
    cat ~/.ssh/id_rsa.pub

Check if it worked:
    ssh -T git@github.com

You need to see a message like this:
    "Hi elcin! You've successfully authenticated, but GitHub does not provide shell access."

Next step is to install some dependencies for Ruby:
    sudo apt-get update
    sudo apt-get install git-core curl zlib1g-dev build-essential libssl-dev libreadline-dev libyaml-dev libsqlite3-dev sqlite3 libxml2-dev libxslt1-dev libcurl4-openssl-dev python-software-properties

Next, install ruby using rbenv:
    cd
    git clone git://github.com/sstephenson/rbenv.git .rbenv
    echo 'export PATH="$HOME/.rbenv/bin:$PATH"' >> ~/.bashrc
    echo 'eval "$(rbenv init -)"' >> ~/.bashrc
    exec $SHELL
    git clone git://github.com/sstephenson/ruby-build.git ~/.rbenv/plugins/ruby-build
    echo 'export PATH="$HOME/.rbenv/plugins/ruby-build/bin:$PATH"' >> ~/.bashrc
    exec $SHELL
    rbenv install 2.1.2
    rbenv global 2.1.2
    ruby -v
    echo "gem: --no-ri --no-rdoc" > ~/.gemrc

Since Rails ships with so many dependencies these days, you need to install a Javascript runtime like NodeJS. 
This lets you use Coffeescript and the Asset Pipeline in Rails which combines and minifies your javascript to provide a 
faster production environment.
    sudo add-apt-repository ppa:chris-lea/node.js
    sudo apt-get update
    sudo apt-get install nodejs
    gem install rails
    rbenv rehash

You can run the rails -v command to make sure you have everything installed correctly.

For PostgreSQL, add a new repository to easily install a recent version of Postgres 9.3.
    sudo sh -c "echo 'deb http://apt.postgresql.org/pub/repos/apt/ precise-pgdg main' > /etc/apt/sources.list.d/pgdg.list"
    wget --quiet -O - http://apt.postgresql.org/pub/repos/apt/ACCC4CF8.asc | sudo apt-key add -
    sudo apt-get update
    sudo apt-get install postgresql-common
    sudo apt-get install postgresql-9.3 libpq-dev

You need to create a database user and grant superuser rights:
    sudo su - postgres
    psql template1
    template1=# CREATE USER user WITH PASSWORD 'user_password';
    template1=# GRANT ALL PRIVILEGES ON DATABASE "speakerinnen_test" to user;
    template1=# GRANT ALL PRIVILEGES ON DATABASE "speakerinnen_development" to user;
    template1=# \q

Now, go to https://github.com/rubymonsters/speakerinnen_liste and click on Fork on the top right side of the page.
Then use this command to clone the repository:
    git init
    git clone https://github.com/rubymonsters/speakerinnen_liste.git
    
Go to speakerinnen_liste/config. Create a file named database.yml. Paste the following lines into database.yml (user and 
user_password are the username and the password of your database user):
    development:
      adapter: postgresql
      database: speakerinnen_development
      pool: 5
      timeout: 5000
      host: localhost
      username: user
      password: user_password
    test:
      adapter: postgresql
      database: speakerinnen_test
      pool: 5
      timeout: 5000
      username: user
      password: user_password
      host: localhost

Now you can test if the code is working. Enter the following commands and go to http://localhost:3000/ on your web browser:
    cd ..
    rake db:create
    rake db:migrate
    rails server

You can use RubyMine 6.3 as IDE. Go to http://www.jetbrains.com/ruby/download/