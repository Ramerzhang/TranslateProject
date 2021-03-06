Moving to Docker
================================================================================
![](http://cocoahunter.com/content/images/2015/01/docker1.jpeg)

[TL;DR] 这是系列的第一篇文章，这系列讲述了我的公司如何把基础服务从PaaS迁移到Docker上。如果你愿意，你可以直接跳过介绍（这篇文章）直接看技术相关的话题（链接在页面的底部）。

----------

上个月，我一直在折腾开发环境。这是我个人故事和经验，关于尝试用Docker简化Rails应用的部署过程。

当我在2012年创建我的公司 – [Touchware][1]时，我还是一个独立开发者。很多事情很小，不复杂，不他们需要很多维护，他们也不需要不部署到很多机器上。经过过去一年的发展，我们成长了很多（我们现在是是拥有10个人团队）而且我们的服务端的程序和API无论在范围和规模方面都有增长。

### 第1步 - Heroku ###

我们还是个小公司，我们需要让事情运行地尽可能平稳。当我们寻找可行的解决方案时，我们打算坚持用那些可以帮助我们减轻对硬件依赖负担的工具。由于我们主要开发Rails应用，而Heroku对RoR，常用的数据库和缓存（Postgres/Mongo/Redis等）有很好的支持，最明智的选择就是用[Heroku][2] 。我们就是这样做的。

Heroku有很好的技术支持和文档，使得部署非常轻松。唯一的问题是，当你处于起步阶段，你需要很多开销。这不是最好的选择，真的。

### 第2步 - Dokku ###

为了尝试并降低成本，我们决定试试Dokku。[Dokku][3],引用GitHub上的一句话 

> Docker powered mini-Heroku in around 100 lines of Bash

我们启用的[DigitalOcean][4]上的很多台机器，都预装了Dokku。Dokku非常像Heroku，但是当你有复杂的项目需要调整配置参数或者是需要特殊的依赖时，它就不能胜任了。我们有一个应用，它需要对图片进行多次转换，我们无法安装一个适合版本的imagemagick到托管我们Rails应用的基于Dokku的Docker容器内。尽管我们还有很多应用运行在Dokku上，但我们还是不得不把一些迁移回Heroku。

### 第3步 - Docker ###

几个月前，由于开发环境和生产环境的问题重新出现，我决定试试Docker。简单来说，Docker让开发者容器化应用，简化部署。由于一个Docker容器本质上已经包含项目运行所需要的所有依赖，只要它能在你的笔记本上运行地很好，你就能确保它将也能在任何一个别的远程服务器的生产环境上运行，包括Amazon的EC2和DigitalOcean上的VPS。

Docker IMHO特别有意思的原因是:

- 它促进了模块化和分离关注点：你只需要去考虑应用的逻辑部分（负载均衡：1个容器；数据库：1个容器；web服务器：1个容器）；
- 在部署的配置上非常灵活：容器可以被部署在大量的HW上，也可以容易地重新部署在不同的服务器或者提供商那；
- 它允许非常细粒度地优化应用的运行环境：你可以利用你的容器来创建镜像，所以你有很多选择来配置环境。

它也有一些缺点：

- 它的学习曲线非常的陡峭（这是从一个软件开发者的角度来看，而不是经验丰富的运维人员）；
- 搭建环境不简单，尤其是还需要自己搭建一个私有的registry/repository (后面有关于它的详细内容)。

下面是一些提示。这个系列的最后一周，我将把他们和一些新的放在一起。

----------

在下面的文章中，我们将看到如何建立一个半自动化的基于Docker的部署系统。

- [建立私有的Docker registry][6]
- [配置Rails应用的半自动化话部署][7]

--------------------------------------------------------------------------------

via: http://cocoahunter.com/2015/01/23/docker-1/

作者：[Michelangelo Chasseur][a]
译者：[mtunique](https://github.com/mtunique)
校对：[校对者ID](https://github.com/校对者ID)

本文由 [LCTT](https://github.com/LCTT/TranslateProject) 原创翻译，[Linux中国](http://linux.cn/) 荣誉推出

[a]:http://cocoahunter.com/author/michelangelo/
[1]:http://www.touchwa.re/
[2]:http://cocoahunter.com/2015/01/23/docker-1/www.heroku.com
[3]:https://github.com/progrium/dokku
[4]:http://cocoahunter.com/2015/01/23/docker-1/www.digitalocean.com
[5]:http://www.docker.com/
[6]:http://cocoahunter.com/2015/01/23/docker-2/
[7]:http://cocoahunter.com/2015/01/23/docker-3/
[8]:
[9]:
[10]:
[11]:
[12]:
[13]:
[14]:
[15]:
[16]:
[17]:
[18]:
[19]:
[20]:
