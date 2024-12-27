---
layout: post
title: "CircleCI 试用记录"
subtitle: ""
author: "yangchw"
catalog: true
header-img: "assets/img/home-bg.png"
header-mask: 0.5
tags:
  - DevOps
  - CircleCI
---

## CircleCI 试用记录

目前市面上的 CI/CD 工具比较多，但要满足企业自身使用场景还需要自己去搭建和探索，本篇文章是我在使用 [CircleCI](https://circleci.com/) 的一个记录，为公司集成和完善CI工具时多一种选择。
公司在使用开源软件如Jenkins和Tekton时，在一般场景下并没有什么问题，但如果多个部门多个开发人员同时提交并构建时，经常出现构建节点卡死的现象，也许这跟我们的构建节点服务器有关系。我想对于大部分公司来说，最终DevOps等工具可能不会再由自身去搭建，而是选择第三方服务，按使用量进行付费来解决。专业的人做专业的时，精细化的分工是不变的趋势。

### CircleCI 简介

CircleCI是基于云的CI/CD工具，可自动执行软件构建和交付过程。它提供快速的配置和维护，没有任何复杂性。由于它是基于云的CI/CD工具，因此消除了专用服务器的困扰，并降低了维护本地服务器的成本。此外，基于云的服务器是可扩展的，健壮的，并有助于更快地部署应用程序。

![架构图](/assets/2021-05-16/arch.png)

目前CircleCI的大部分功能是免费的，它在GitHub上很流行，同时它也提供了托管和自托管的解决方案（需付费）。

![付费计划](/assets/2021-05-16/20210516163703.jpg)

CircleCI 每天管理约一百万个任务，为30,000个组织提供援助。客户倾向于将CircleCI用作他们的CI/CD工具，因为项目执行得更快并且构建得到了很好的优化。CircleCI还可以使用docker层缓存，高级缓存和资源类，快速有效地操作复杂的流水线。作为开发人员，你可以使用SSH设置CircleCI来调试构建中的问题。此外，你还可以设置并行构建，以更快地执行多个进程。

一旦 GitHub 上的代码仓库被授权并以项目形式提交给circleci.com，任何代码更新都将在新的容器或VM中执行自动检查。CircleCI可以在专用容器或文件中执行每个任务。这意味着每次你在CircleCI上运行项目时，都会创建一个新容器来运行任务。

任务执行完成后，CI/CD工具会向测试人员发送电子邮件通知，告知已执行代码是否成功。CircleCI支持IRC通知并集成了Stack。可以从CircleCI上可用的报告库中轻松检索代码的覆盖结果。CircleCI也支持把代码部署到各种平台，例如：

 - Kubernetes
 - Microsoft Azure
 - AWS CodeDeploy，AWS EC2，AWS S3
 - Heroku
 - 支持SSH或配置有API客户端的其他平台。
 - CircleCI 支持多种测试，例如集成测试，单元测试和功能测试等。

CircleCI  与 Linux，OSX，容器兼容，并且可以独立运行，而无需在数据中心或私有云服务器中使用其他插件。

 - CircleCI 的主要功能
 - 它提供了自动并行性，以加快多个应用程序的部署。
 - 快速设置
 - 各种定制
 - 简单易上手
 - YAML设置的解释
 - 不需要专用服务器即可操作CircleCI。
 - 它缓存应用程序规范和第三方配置，而不是系统部署。

### CircleCI 使用步骤

直接使用你的 GitHub 账号登录即可，登录后 CircleCI 会自动把GitHub上的项目同步过来。如下图在左侧的 Project 中可以看到同步过来的项目，这里我创建了一个 `CircleCI-Demo` 项目，是基于 SpringBoot 的。

![Project](/assets/2021-05-16/20210516164454.jpg)

选择 main 分支后，点击 Edit Config，编辑CD的配置文件。

![Edit Config](/assets/2021-05-16/20210516164708.jpg)

CircleCI 配置文件一般由以下部分组成：
 
 - Pipeline: 代表整个配置。仅在 CircleCI Cloud 中可用。
 - Workflows: 负责编排多个作业。
 - Jobs: 负责执行一系列执行命令的步骤。集合中的键为 job 的名称，值是具体执行 job 的内容，如果你使用工作流(workflows)，则 job 的名称在配置文件中必须唯一，如果你不使用 工作流(workflows)，则必须包含名称为build的 job 来作为用户提交代码时的默认 job。
 - Steps: 运行命令（例如安装依赖项或运行测试）和Shell脚本来完成项目所需的工作。

配置文件的位置：

```
├── .circleci
│   ├── config.yml
├── README
└── all-other-project-files-and-folders
```

![配置文件](/assets/2021-05-16/config-elements.png)

Demo中的配置文件：

```yml
version: 2.1

orbs:
  maven: circleci/maven@1.1

workflows:
  maven_test:
    jobs:
      - maven/test

```

Orbs：是 CircleCI 预制的流水线，网站提供了这些预置流水线的仓库，用户可以上传自己的流水线，也可以使用官方或其他用户提供的 Orbs。Orbs 不仅是配置文件，还提供了不同参数和环境的设置功能，可以灵活修改称自己的流水线配置。详细信息可查看官网：https://circleci.com/docs/2.0/concepts/?section=getting-started#orbs。

这里使用了官方提供的 Maven 流水线，包含了编译、构建、测试已经将构建出来的包上传到指定位置的功能。

点击页面的 Run Pipeline ，默认会使用项目下的配置文件进行 CI/CD 。

![Run Pipeline](/assets/2021-05-16/20210516175510.jpg)

查看具体的执行：复杂的流水线将显示在这里，目前项目只有一个 maven/test 。

![Maven_test](/assets/2021-05-16/20210516175510.jpg)

可以查看这个 Obs 的每一步的执行日志和结果。

![Maven/test](/assets/2021-05-16/20210516175557.jpg)

每次执行可以指定某个步骤进行缓存，这里是缓存 maven 的依赖，每个项目的构建也可以共享这些缓存。

![catch](/assets/2021-05-16/20210516175658.jpg)

最后是执行结果显示。

![latest](/assets/2021-05-16/20210516181021.jpg)

