---
layout: post
title: 使用 Buildstep 把一个 Heroku 应用转化成 Docker 镜像
tags: [docker]
---

# Creating a Docker image from your code

# [](https://github.com/tobegit3hub/docs/blob/master/creating-a-docker-image-from-your-code.md#%E9%80%9A%E8%BF%87%E4%BB%A3%E7%A0%81%E6%9D%A5%E5%88%9B%E5%BB%BA%E4%B8%80%E4%B8%AAdocker%E9%95%9C%E5%83%8F)通过代码来创建一个Docker镜像

One of the great developments that Heroku has open sourced are Buildpacks. Buildpacks are a collection of scripts that detect your application language/framework and install the required interpreters, libraries, etc. to run.

[Heroku](https://www.heroku.com/)最重要的开源项目之一是[Buildpacks](https://devcenter.heroku.com/articles/buildpacks).Buildpacks实际上是一系列脚本，它们可以自动侦测到应用的语言/框架,然后安装所运行所需要的解释器,库等等。

Buildstep is a great tool developed by progrium which uses these buildpacks to transform any application code (supported by any of the included buildpacks) into a self-sufficient Docker image. It is also used in the extremely popular Dokku project made by the same author.

[Buildstep](https://github.com/progrium/buildstep)是[progrium](http://progrium.com/)开发的一个非常优秀的工具，它使用这些buildpacks把任意一些应用代码(被这些buildpacks所支持)转换成一个自给自足的Dokcer镜像.它也同样被用于这个作者开发的一个非常流行的项目[Dokku](https://github.com/progrium/dokku).

In order to take full advantage of this amazing tool, we have created a base image called tutum/buildstep which can be used directly in a Dockerfile to produce application images with minimum effort.

为了充分利用这个神奇的工具，我们创建了一个叫[tutum/buildstep](https://index.docker.io/u/tutum/buildstep/)的基础镜像，它可以直接使用在Dockerfile中，以最快的速度生成一个应用程序的镜像。

Let’s see a simple example.

让我们看一个简单的例子

## [](https://github.com/tobegit3hub/docs/blob/master/creating-a-docker-image-from-your-code.md#converting-a-django-app-into-a-docker-image-using-buildstep)Converting a Django app into a Docker image using buildstep

使用buildstep把一个Django的应用转换成一个Docker的镜像

Imagine we have a Django app and we want to convert it into a Docker image to be able to move it between hosts. In the application code folder, we’ll create the following Dockerfile:

想象一下我们有一个Django的应用，我们希望把它转化成一个Docker的镜像可以方便我们在服务器间移动。在应用代码的目录中，我们要创建一个如下的Dockerfie:
    
    FROM tutum/buildstep
    EXPOSE 80
    CMD ["python", "manage.py", "runserver", "80"]
    

That’s it. The minimum required information for tutum/buildstep to work is the port to expose and the command used to launch your application. Adding the application code and setting the working directory is automatically done by tutum/buildstep using the ONBUILD directive.

就是那样. 运行`tutum/buildstep`所需要的最少信息是暴露的端口以及运行应用所需要的命令。把应用代码添加进去以及配置工作目录都是`tutum/buildstep`通过使用`ONBUILD`指令自动完成的.

Let’s build it: 我们来构建一下:
    
    $ docker build -t fermayo/myapp .
    Uploading context 99.84 kB
    Uploading context
    Step 0 : FROM tutum/buildstep
    # Executing 3 build triggers
    Step onbuild-0 : RUN mkdir -p /app
     ---> Running in 0d65c9537e8f
     ---> 9d7c609c38ce
    Step onbuild-1 : ADD . /app
     ---> 3b7ff5e4f126
    Step onbuild-2 : RUN /build/builder
     ---> Running in 6e7a796d93c3
           Python app detected
    -----> No runtime.txt provided; assuming python-2.7.6.
    -----> Preparing Python runtime (python-2.7.6)
    -----> Installing Setuptools (2.1)
    -----> Installing Pip (1.5.4)
    -----> Installing dependencies using Pip (1.5.4)
           Downloading/unpacking Django==1.6.2 (from -r requirements.txt (line 1))
           Installing collected packages: Django
           Successfully installed Django
           Cleaning up...
    
    -----> Discovering process types
           Procfile declares types -> web
     ---> 8758f592da19
     ---> 8758f592da19
    Step 1 : EXPOSE 80
     ---> Running in 3f10763973a8
     ---> db32c55e948b
    Step 2 : CMD ["python", "manage.py", "runserver", "80"]
     ---> Running in 7d122de7f8d3
     ---> ed1a72bfa5d0
    Successfully built ed1a72bfa5d0
    Removing intermediate container 0d65c9537e8f
    Removing intermediate container d79aa6530641
    Removing intermediate container 6e7a796d93c3
    Removing intermediate container 3f10763973a8
    Removing intermediate container 7d122de7f8d3
    

Our application is now Dockerized into an image called fermayo/myapp. Let’s try it out:

现在我们的应用已经变成了一个叫`fermayo/myapp`的Docker镜像，我们来尝试运行一下:
    
    $ docker run -d -p 80 fermayo/myapp
    

We have now our Django app up and running!

我们已经把我们的Django应用运行起来了!

## [](https://github.com/tobegit3hub/docs/blob/master/creating-a-docker-image-from-your-code.md#converting-a-heroku-app-into-a-docker-image-using-buildstep)Converting a Heroku app into a Docker image using buildstep

## [](https://github.com/tobegit3hub/docs/blob/master/creating-a-docker-image-from-your-code.md#%E4%BD%BF%E7%94%A8buildstep%E6%8A%8A%E4%B8%80%E4%B8%AAheroku%E5%BA%94%E7%94%A8%E8%BD%AC%E5%8C%96%E6%88%90docker%E9%95%9C%E5%83%8F)使用buildstep把一个Heroku应用转化成Docker镜像

Buildstep also supports using a Procfile to define the application process types. The following would be a simple Procfile for our Django app we used earlier:

Buildstep也支持使用[Procfile](https://devcenter.heroku.com/articles/procfile)来定义应用的处理类型。下面是一个我们为我们的Django应用最早准备的的一个简单的`Procfile`
    
    web: python manage.py runserver 80
    

To use this instead of manually defining the command, we can create the following Dockerfile:

使用这个而不是手工定义启动的命令，我们可以创建下面这个Dockerfile:
    
    FROM tutum/buildstep
    EXPOSE 80
    CMD ["/start", "web"]
    

By entering the desired process type name in the CMD directive we are defining which command should be used to start the application.

通过在CMD指令中输入我们需要的处理类型的名字，我们定义了哪个命令我们会用来启动应用

## [](https://github.com/tobegit3hub/docs/blob/master/creating-a-docker-image-from-your-code.md#running-your-app-without-building-an-image-using-buildstep)Running your app without building an image using buildstep

## [](https://github.com/tobegit3hub/docs/blob/master/creating-a-docker-image-from-your-code.md#%E8%BF%90%E8%A1%8C%E4%BD%A0%E7%9A%84%E5%BA%94%E7%94%A8%E8%80%8C%E4%B8%8D%E9%9C%80%E8%A6%81%E9%80%9A%E8%BF%87buildstep%E6%9D%A5%E6%9E%84%E5%BB%BA%E9%95%9C%E5%83%8F)运行你的应用而不需要通过buildstep来构建镜像

If your application is stored in a git repository, you can also run tutum/buildstep passing in an environment variable GIT_REPO with its address and the container will clone the repo and install the dependencies, etc. on the fly:

如果你的应用存储在一个git的代码库中，那么你可以在运行`tutum/buildstep`的时候传入一个指向项目地址的环境变量`GIT_REPO`,这样容器会在运行时克隆项目并且下载依赖:
    
    # Without a Procfile
    $ docker run -d -p 80 -e GIT_REPO=https://github.com/fermayo/hello-world-django.git tutum/buildstep python manage.py runserver 80
    
    
    # With a Procfile (or relying on the default Procfile provided by the buildpack)
    $ docker run -d -p 80 -e GIT_REPO=https://github.com/fermayo/hello-world-php.git tutum/buildstep /start web
    

So there is no need to build anything.

所以这个不需要构建任何东西.

## [](https://github.com/tobegit3hub/docs/blob/master/creating-a-docker-image-from-your-code.md#give-it-a-try)Give it a try!

## [](https://github.com/tobegit3hub/docs/blob/master/creating-a-docker-image-from-your-code.md#%E8%AF%95%E4%B8%80%E4%B8%8B%E5%90%A7)试一下吧!

This is a great way to getting started with Docker. Let us know if it works for your language/framework and how would you make it better. Thanks for reading!

这是一个很好的入门使用Docker的方法。请让我们知道它是否适用于你们的语言/框架，以及你们如何准备把它变得更好。

谢谢阅读!

About [Fernando Mayo](http://blog.tutum.co/author/fermayotutum/) CTO & Co-founder of Tutum
