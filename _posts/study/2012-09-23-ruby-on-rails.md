---
layout: post
title: Ruby On Rails 环境搭建
tags: [study]
---

![Ruby On Rails](http://7q5cfr.com1.z0.glb.clouddn.com/rails.png)

##前言

[Ruby On Rails][3]是最流行和体验最好的敏捷开发WEB框架（没有之一），介绍性的内容这里就不赘述了。本文介绍下Ruby On Rails环境的搭建流程。

环境如下：

* 操作系统采用[Ubuntu 12.04.1 LTS (Precise Pangolin)][1] Server Editon
* [Ruby][2]版本为1.9.3-p0
* [Rails][3]为3.2.8

默认情况下Ubuntu有很多库和软件没有安装，我们首先安装一下所需要的库：

	$ sudo apt-get upgrade
	$ sudo apt-get install build-essential libssl-dev libreadline-gplv2-dev \
	  lib64readline-gplv2-dev zlib1g zlib1g-dev libyaml-dev libsqlite3-dev

##安装Ruby

由于系统自带的Ruby版本较低，所以这里我们先安装Ruby：

	$ wget http://ftp.ruby-lang.org/pub/ruby/1.9/ruby-1.9.3-p0.tar.gz
	$ tar xzvf ruby-1.9.3-p0.tar.gz
	$ cd ruby-1.9.3-p0
	$ ./configure
	$ make
	$ sudo make install

安装完成之后，查看下Ruby信息：
	
	$ ruby -v
	ruby 1.9.3p0 (2011-10-30 revision 33570) [i686-linux]
	
##安装Gem

Rails需要通过[Gem][4]来安装，下面安装Gem：

	$ wget http://production.cf.rubygems.org/rubygems/rubygems-1.8.24.tgz
	$ tar -zxvf rubygems-1.8.24.tgz
	$ cd rubygems-1.8.24
	$ ruby setup.rb
	RubyGems 1.8.24 installed
	== 1.8.24 / 2012-04-27

	* 1 bug fix:

	  * Install the .pem files properly. Fixes #320
	  * Remove OpenSSL dependency from the http code path

	---------------------------------------------------------------------

	RubyGems installed the following executables:
		/usr/local/bin/gem

更新Gem：

	$ gem update --system 
	Latest version currently installed. Aborting.
	
##安装Rails

接下来安装Rails:

	$ sudo gem install rails
	Successfully installed i18n-0.6.1
	Successfully installed multi_json-1.3.6
	Successfully installed activesupport-3.2.8
	Successfully installed builder-3.0.3
	...
	...
	Successfully installed rack-ssl-1.3.2
	Successfully installed thor-0.16.0
	Successfully installed railties-3.2.8
	Successfully installed bundler-1.2.1
	Successfully installed rails-3.2.8
	28 gems installed

如果出现以下的错误信息：

	ERROR:  Loading command: update (LoadError)
	    no such file to load -- zlib
	ERROR:  While executing gem ... (NameError)
	    uninitialized constant Gem::Commands::UpdateCommand
		
这是因为Ruby的zlib模块没有安装，安装zlib模块：

	sudo apt-get install zlib1g-dev
	cd /ruby-source-files/ext/zlib
	ruby extconf.rb
	make
	sudo make install

如果出现以下错误信息：

	Installing ri documentation for rails-3.2.8...
	file 'lib' not found
	Installing RDoc documentation for rails-3.2.8...
	file 'lib' not found

是因为rdoc没有安装，安装rdoc：

	sudo gem install rdoc

安装[Rails Completion](https://github.com/jweslley/rails_completion)：

	$ wget -O ~/.rails.bash https://raw.github.com/jweslley/rails_completion/master/rails.bash
	$ echo source ~/.rails.bash >> ~/.bashrc
	$ source ~/.bashrc
	
##第一个应用

Rails安装完成之后，你可以创建自己的Rails应用了：
	$ cd /srv
	$ rails new myapp
	create  
	create  README.rdoc
	create  Rakefile
	create  config.ru
	create  .gitignore
	create  Gemfile
	...
	...
	Using rails (3.2.8) 
	Using sass (3.2.1) 
	Using sass-rails (3.2.5) 
	Using sqlite3 (1.3.6) 
	Installing uglifier (1.3.0) 
	Your bundle is complete! Use `bundle show [gemname]` to see where a 
	bundled gem is installed.

Rails应用创建成功之后，你可以测试下这个应用是否可以运行：

	$ cd myapp
	$ rails server
	rails server
	=> Booting WEBrick
	=> Rails 3.2.8 application starting in development on http://0.0.0.0:3000
	=> Call with -d to detach
	=> Ctrl-C to shutdown server
	[2012-09-23 15:02:22] INFO  WEBrick 1.3.1
	[2012-09-23 15:02:22] INFO  ruby 1.9.3 (2012-04-20) [x86_64-darwin12.0.0]
	[2012-09-23 15:02:22] INFO  WEBrick::HTTPServer#start: pid=9907 port=3000

执行以上的操作后，系统会启动WEBrick服务器，你可以通过`http://127.0.0.1:3000`来访问应用的内容了。不过WEBrick只可以作为测试用，
不能应用在大规模的生产环境中，所以我们需要一个高性能的服务器，这就是下面将要介绍的[Unicorn][5]。

##安装Unicorn

[Unicorn（独角兽）][5]是一个高性能的[Rack][7] HTTP server，更多介绍参见[Github的介绍][6]。

Unicorn依赖于[JavaScript Runtime](https://github.com/sstephenson/execjs)，首先我们安装Node.js：

	$ sudo apt-get install nodejs

接下来我们通过Gem来安装Unicorn：

	$ gem install unicorn
	Fetching: kgio-2.7.4.gem (100%)
	Building native extensions.  This could take a while...
	Fetching: raindrops-0.10.0.gem (100%)
	Building native extensions.  This could take a while...
	Fetching: unicorn-4.3.1.gem (100%)
	Building native extensions.  This could take a while...
	Successfully installed kgio-2.7.4
	Successfully installed raindrops-0.10.0
	Successfully installed unicorn-4.3.1
	3 gems installed
	Installing ri documentation for kgio-2.7.4...
	Installing ri documentation for raindrops-0.10.0...
	Installing ri documentation for unicorn-4.3.1...
	Installing RDoc documentation for kgio-2.7.4...
	Installing RDoc documentation for raindrops-0.10.0...
	Installing RDoc documentation for unicorn-4.3.1...

安装完成之后，启动Unicorn：

	$ cd /srv/myapp
	$ unicorn -l 127.0.0.1:8080 -E development
	I, [2012-09-23T15:22:39.793852 #12107]  INFO -- : listening on addr=0.0.0.0:8080 fd=5
	I, [2012-09-23T15:22:39.794149 #12107]  INFO -- : worker=0 spawning...
	I, [2012-09-23T15:22:39.795973 #12107]  INFO -- : master process ready
	I, [2012-09-23T15:22:39.797613 #12108]  INFO -- : worker=0 spawned pid=12108
	I, [2012-09-23T15:22:39.797927 #12108]  INFO -- : Refreshing Gem list
	I, [2012-09-23T15:22:41.245205 #12108]  INFO -- : worker=0 ready
	
其中`-l 127.0.0.1:8080`指定Unicorn监听8080端口，`-E development`指定运行环境为开发环境，更多内容参见`unicorn -h`。Unicorn启动成功之后，可以通过`http://127.0.0.1:8080`来访问Rails应用。

##环境配置

以上只是一个快速使用Unicorn的例子，我们还需要进行设置，使其可以自动化的启动。我们采用[Nginx][8]作为我们的Web服务器，并使用反向代理的方式对外提供服务：

首先，在myapp下创建配置文件`/srv/myapp/config/unicorn.rb`，内容如下：

	worker_processes 4
	working_directory "/srv/myapp" # available in 0.94.0+

	listen "/tmp/unicorn.sock", :backlog => 64
	listen 8080, :tcp_nopush => true

	timeout 30

	pid "/srv/myapp/tmp/unicorn.pid"
	stderr_path "/srv/myapp/log/unicorn.stderr.log"
	stdout_path "/srv/myapp/log/unicorn.stdout.log"

	# combine Ruby 2.0.0dev or REE with "preload_app true" for memory savings
	# http://rubyenterpriseedition.com/faq.html#adapt_apps_for_cow
	preload_app true
	GC.respond_to?(:copy_on_write_friendly=) and
	  GC.copy_on_write_friendly = true

	before_fork do |server, worker|
	  # the following is highly recomended for Rails + "preload_app true"
	  # as there's no need for the master process to hold a connection
	  defined?(ActiveRecord::Base) and
	    ActiveRecord::Base.connection.disconnect!

	  # The following is only recommended for memory/DB-constrained
	  # installations.  It is not needed if your system can house
	  # twice as many worker_processes as you have configured.
	  #
	  # # This allows a new master process to incrementally
	  # # phase out the old master process with SIGTTOU to avoid a
	  # # thundering herd (especially in the "preload_app false" case)
	  # # when doing a transparent upgrade.  The last worker spawned
	  # # will then kill off the old master process with a SIGQUIT.
	  # old_pid = "#{server.config[:pid]}.oldbin"
	  # if old_pid != server.pid
	  #   begin
	  #     sig = (worker.nr + 1) >= server.worker_processes ? :QUIT : :TTOU
	  #     Process.kill(sig, File.read(old_pid).to_i)
	  #   rescue Errno::ENOENT, Errno::ESRCH
	  #   end
	  # end
	  #
	  # Throttle the master from forking too quickly by sleeping.  Due
	  # to the implementation of standard Unix signal handlers, this
	  # helps (but does not completely) prevent identical, repeated signals
	  # from being lost when the receiving process is busy.
	  # sleep 1
	end

	after_fork do |server, worker|
	  # per-process listener ports for debugging/admin/migrations
	  # addr = "127.0.0.1:#{9293 + worker.nr}"
	  # server.listen(addr, :tries => -1, :delay => 5, :tcp_nopush => true)

	  # the following is *required* for Rails + "preload_app true",
	  defined?(ActiveRecord::Base) and
	    ActiveRecord::Base.establish_connection

	  # if preload_app is true, then you may also want to check and
	  # restart any other shared sockets/descriptors such as Memcached,
	  # and Redis.  TokyoCabinet file handles are safe to reuse
	  # between any number of forked children (assuming your kernel
	  # correctly implements pread()/pwrite() system calls)
	end

然后，在Nginx中添加如下配置：
	
	upstream app_server {
		# fail_timeout=0 means we always retry an upstream even if it failed
		# to return a good HTTP response (in case the Unicorn master nukes a
		# single worker for timing out).

		# for UNIX domain socket setups:
		server unix:/tmp/unicorn.sock fail_timeout=0;
	}

	server {
	    listen       8888;
	    server_name  _;

	    #access_log  logs/host.access.log  main;

	    location / {
	        # an HTTP header important enough to have its own Wikipedia entry:
			#   http://en.wikipedia.org/wiki/X-Forwarded-For
			proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;

			# enable this if you forward HTTPS traffic to unicorn,
			# this helps Rack set the proper URL scheme for doing redirects:
			# proxy_set_header X-Forwarded-Proto $scheme;

			# pass the Host: header from the client right along so redirects
			# can be set properly within the Rack application
			proxy_set_header Host $http_host;
			# we don't want nginx trying to do something clever with
			# redirects, we set the Host: header above already.
			proxy_redirect off;

			# set "proxy_buffering off" *only* for Rainbows! when doing
			# Comet/long-poll/streaming.  It's also safe to set if you're using
			# only serving fast clients with Unicorn + nginx, but not slow
			# clients.  You normally want nginx to buffer responses to slow
			# clients, even with Rails 3.1 streaming because otherwise a slow
			# client can become a bottleneck of Unicorn.
			#
			# The Rack application may also set "X-Accel-Buffering (yes|no)"
			# in the response headers do disable/enable buffering on a
			# per-response basis.
			# proxy_buffering off;

	      proxy_pass http://app_server;
	    }
	}

之后，添加配置文件`/etc/unicorn/myapp.conf`，内容如下：

	RAILS_ENV=development  #environment 
	RAILS_ROOT=/srv/myapp #
	UNICORN=/usr/local/bin/unicorn

最后，创建启动脚本：

	$ wget -O unicornd https://raw.github.com/gist/3769241/293df374dd7946ac44ac5575feccbfe80f6aa18d/unicornd
	$ sudo cp unicornd /etc/init.d/
	$ sudo chmod +x /etc/init.d/unicornd
	$ echo /etc/init.d/unicornd start >> /etc/rc.local
	$ sudo /etc/init.d/unicornd start


##参考

[https://github.com/blog/517-unicorn](https://github.com/blog/517-unicorn)

[1]: http://releases.ubuntu.com/12.04/ "Ubuntu 12.04.1 LTS (Precise Pangolin)"
[2]: http://www.ruby-lang.org/en/ "Ruby"
[3]: http://rubyonrails.org/ "Ruby On Rails"
[4]: https://rubygems.org/ "Gem"
[5]: http://unicorn.bogomips.org/ "Unicorn"
[6]: https://github.com/blog/517-unicorn "Github Unicorn"
[7]: http://rack.github.com/ "Rack"
[8]: http://nginx.org/ "Nginx"
