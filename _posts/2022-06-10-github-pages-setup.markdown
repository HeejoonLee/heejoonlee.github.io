---
title: GitHub Pages Setup
date: 2022-05-10 00:00:00 +0900
categories: [Web, GitHub Pages]
tags: [github, blog, pages]
---
### Download And Install Ruby

1. Download Ruby source code: [Ruby](https://www.ruby-lang.org/en/downloads/)

2. Extract the downloaded tarball:
```shell
$ tar -xf ruby-3.1.2.tar.gz
```
3. Update package list and install dependencies before building and installing Ruby:
```shell
$ sudo apt-get update
$ sudo apt-get install autoconf bison build-essential libssl-dev libyaml-dev libreadline6-dev zlib1g-dev libncurses5-dev libffi-dev libgdbm6 libgdbm-dev libdb-dev
```

4. Build and install Ruby:
```shell
$ cd ruby-3.1.2
$ ./configure
$ make
$ sudo make install
```
    > Step 4 will take around 10 minutes to complete
    {: .prompt-info }


5. Check your Ruby installation:
```shell
$ ruby -v
ruby 3.1.2p20 (2022-04-12 revision 4491bb740a) [x86_64-linux]
```

### Install Bundler
Bundler is a package manager for Ruby. Bundler uses a file named *Gemfile* to manage and resolve dependencies between *gems*(Ruby packages).

1. Install Bundler:
```shell
$ sudo gem install bundler
```
    > *gem* is the built-in package manager for Ruby. *gem* also refers to a Ruby package
    {: .prompt-tip }


2. Check your Bundler installation:
```shell
$ bundle
Could not locate Gemfile
```


### Install Jekyll
```shell
$ sudo apt install jekyll
```


### Create a GitHub Pages Site
1. Create a **git repository** with the following credentials:
* repository name: heejoonlee.github.io
* visibility: public

2. Clone the GitHub Pages repo just created:
```shell
$ git clone https://github.com/[username]/[username].github.io.git
```

3. At the root of the repo, create a new Jekyll site:
```shell
$ cd heejoonlee.github.io
$ jekyll new --skip-bundle .
New jekyll site installed in /home/heejoon/work/heejoonlee.github.io. 
Bundle install skipped. 
```

4. A *Gemfile* will be created at the root of the repo. Edit the *Gemfile* as follows:
* **Comment** the line that says `gem "jekyll"`
* **Uncomment** the line that says `gem "github-pages"` and add the latest supported version:
ex) `gem "github-pages", "~> 226", group: :jekyll_plugins`

5. Install gems from the *Gemfile* using bundler:
```shell
$ bundle install
```

6. Check your site locally:
```shell
$ bundle exec jekyll serve
 Auto-regeneration: enabled for '/home/heejoon/work/heejoonlee.github.io'
    Server address: http://127.0.0.1:4000/
  Server running... press ctrl-c to stop.
```
Go to `http://127.0.0.1:4000/` in a web browser to check the generated site
* **ERROR**(Missing gem: webrick)
```shell
$ bundle exec jekyll serve
bundler: failed to load command: jekyll (/usr/local/bin/jekyll)
/usr/local/lib/ruby/gems/3.1.0/gems/jekyll-3.9.2/lib/jekyll/commands/serve/servlet.rb:3:in `require': cannot load such file -- webrick (LoadError)
```
Resolve the error by adding `gem "webrick"` to *Gemfile* and running `bundle install`

7. Add and commit the changes and push the commit to the remote repository:
```shell
$ git add .
$ git commit -m "Initial commit"
$ git push
```

8. Check your hosted web site at the address: [https://heejoonlee.github.io/](https://heejoonlee.github.io/)

