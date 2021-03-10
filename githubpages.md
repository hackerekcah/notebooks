# Github Pages

## [Getting started with gh pages](https://docs.github.com/en/github/working-with-github-pages/getting-started-with-github-pages)
* hackerekcah.github.io/[RepoName]

## [Jekyll and Github pages](https://docs.github.com/en/github/working-with-github-pages/setting-up-a-github-pages-site-with-jekyll)
* Jekyll is a static webpage generator
* Jekyll support writing webpage using markdown
* Each time push to github, github will use Jekyll to build page automatically.

## [Jekyll Homepage](https://jekyllrb.com/)
* Jekyll, ruby, gem, bundle
  * Jekyll is a ruby gem
  * Ruby is a language.
  * `gem` is a pakage manager for ruby programs
  * `bundle` manages dependencies of gems, using GemFile with required gems
## [Install ruby](https://www.ruby-lang.org/en/documentation/installation/)
```
# this methods may not get the newest ruby
sudo apt-get install ruby-full
```

## Install [`rbenv`](https://github.com/rbenv/rbenv) to manage multiple ruby
```
# step 1
 sudo apt install rbenv

# step 2: setup terminal, put this line at the end of .bashrc
eval "$(rbenv init -)"

# step3: open new terminal

# step4: check for success
 curl -fsSL https://github.com/rbenv/rbenv-installer/raw/master/bin/rbenv-doctor | bash
```

### `ruby-build` plugin to allow install ruby using `rbenv`
* Install [`ruby-build`](https://github.com/rbenv/ruby-build#readme)
  * [requires](https://github.com/rbenv/ruby-build/wiki#suggested-build-environment): install some required packages for ruby-build to work.
```
# Depending on your version of Ubuntu/Debian/Mint, libgdbm6 won't be available.
# In that case, try an earlier version such as libgdbm5.
apt-get install autoconf bison build-essential libssl-dev libyaml-dev libreadline6-dev zlib1g-dev libncurses5-dev libffi-dev libgdbm6 libgdbm-dev libdb-dev

# Install ruby-build as an rbenv plugin
$ mkdir -p "$(rbenv root)"/plugins
$ git clone https://github.com/rbenv/ruby-build.git "$(rbenv root)"/plugins/ruby-build
```
* some useful rbenv command
```
# list installed ruby versions
rbenv versions

# list the selected ruby version
rbenv version

# list remote possible versions
rbenv install --list

# install ruby version:
rbenv install x.x.x

# select ruby 
rbenv global x.x.x

# after selecting ruby, needs to rehash to make shims reflect the current ruby version
rbenv rehash

# check gem env correct
gem env home
# => ~/.rbenv/versions/<ruby-version>/lib/ruby/gems/...
```

## Install Jekyll
  * For developing purpose, may install jekyll locally
  * Install ruby, rbenv, ruby-build first.
```
gem install jekyll bundler

# new site at current folder
jekyll new .

# serve if using GemFile
bundle exec jekyll serve

# serve without GemFile
jekyll serve

# serve with live reload
jekyll serve --livereload
```
