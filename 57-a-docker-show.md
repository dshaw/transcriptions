[57: A Docker Show](http://nodeup.com/fiftyseven)
===

Panel:

* [Daniel Shaw](https://twitter.com/dshaw)
* [Nuno Job](https://twitter.com/dscape)
* [Mike Brevoort](https://twitter.com/mbrevoort)
* [Niall O'Higgins](https://twitter.com/niallohiggins)
* [Jacob Groundwater](https://twitter.com/0x604)

Table of Contents:

1. [Introduction](#introduction)
2. [Why Docker is Meaningful](#why-docker-is-meaningful)
3. [Docker in Open Source](#docker-in-open-source)
4. [Testing with Docker](#testing-with-docker)
5. [Orchestration and Docker Linking](#orchestration-and-docker-linking)
6. [Placeholder](#placeholder)
7. [Placeholder](#placeholder)
8. [Placeholder](#placeholder)
9. [Placeholder](#placeholder)
10. [Plugs](#plugs)

## Introduction

00:17 - **Daniel Shaw**: Hi, welcome to 2014! Welcome to NodeUp 57: A Docker All The Things Show. I am joined today by Nuno Job, Mike Brevoort, Niall O'Higgins on the craziest setup in the world -- he is connected on a telephone through Nuno's laptop in London and Niall is in Mexico -- it's nuts, but we're gonna do it anyway. And Jacob Goldwater, last but not least.

00:52 - **Jacob Groundwater**: Groundwater, buddy!

00:53 - **Daniel Shaw**: Oh shit, alright! 

00:57 - **Nuno Job**: What did you call Jacob?

01:01 - **Jacob Groundwater**: Goldwater!?

01:03 - **Nuno Job**: "I like goooooold!"

01:06 - **Jacob Groundwater**: That's not too bad.

01:09 - **Daniel Shaw**: Thank you, Jacob!

01:11 - **Jacob Groundwater**: Yeah, no problem!

01:12 - **Daniel Shaw**: So, today's sponsors are [Clock](http://clock.co.uk), [&yet](http://andyet.com), and [Modulus](http://modulus.io). And I'm Dan Shaw, I run [The Node Firm](http://thenodefirm.com/). Let's go around the horn and let everyone introduce themselves before we get into dockering all the things. Mike, you want to kick off the blurbs?

01:32 - **Mike Brevoort**: I'm Mike Brevoort. I work at Pearson. Working to innovate and create and deploy our next-gen products globally. We've been using node for a little over three years there. 

01:43 - **Daniel Shaw**: Awesome! Niall?

01:44 - **Niall O'Higgins**: Hey, I'm Niall. I'm the founder of [FrozenRidge.co](http://frozenridge.co/). We do node software development. I'm also the primary author of [StriderCD.com](http://stridercd.com/), an opensource continuous deployment platform written in node. And if I don't sound very good on this call it's because I'm currently continuously deploying myself around Mexico. 

02:06 - **Daniel Shaw**: Jacob? Mr. Groundwater.

02:10 - **Jacob Groundwater**: Yes, Jacob Goldwater here. I work at New Relic. I work on the node agent there. Also, in my spare time, I work on [NodeOS](http://node-os.com/blog/get-involved).

02:20 - **Daniel Shaw**: Nice.

02:21 - **Nuno Job**: Hey, Nuno Job here. I used to work for Nodejitsu, where I built the first continuous deployment there, actually. I wrote some of the stuff that you might have read on docker. And I'm currently working on a super-stealth startup called percent, which you can see online so it's not very stealth. I'm the creator of LXJS and financing myself through consulting. 

02:40 - **Daniel Shaw**: Awesome. So let's fill in a little bit of context around docker. But before that, let me give a shout out to our awesome sponsor Clock. Clock is a digital development agency based in the outskirts of London. They make beautiful websites and web-based applications. They're great at integrating legacy systems, devices, and all the things that really don't want to be integrated. They are experts in publishing, customer insight, and customer loyalty. They have developed hardware devices that retailers have put into stores and venues. It 100% runs on node.js, it's called SwipeStation -- you can learn more about that at [swipestation.co.uk](http://swipestation.co.uk/). They've been making websites since 1997, they've been building node since 0.4, and since then they've fallen in love and gone all-in on node.

Their clients include BBC, Newscorp, Nielson, Joyent, Eddie Izzard, Hearst Media. Fantastic company. A couple of their projects are [NeverUnderdressed.com](http://www.neverunderdressed.com/) -- a high-end online fashion magazine. SunPlus is a access-controlled loyalty platform for the Sun newspaper -- that's at perks.thesun.co.uk. And Sunday World, Ireland's biggest tabloid at [SundayWorld.com](http://sundayworld.com). They are the developers of the Node Bowler, a bowler hat with some really cool hardware hacks. Have a look at [clock.co.uk](http://clock.co.uk). Get in touch with them at [hello@clock.co.uk](mailto:hello@clock.co.uk). Be sure to follow [@clock](https://twitter.com/clock) on Twitter. Send them a message and say you heard them on NodeUp, and thank them for supporting node.

## Why Docker is Meaningful

04:33 - **Daniel Shaw**: So let's fill in the context of what docker is and why it's meaningful. And why it's transforming deployment, not only in node, but across the developer ecosystem. Niall, you want to start us off there?

04:54 - **Niall O'Higgins**: Sure, I can talk about docker and why it's important. I guess the key thing, first of all, is that it's built on [LXC](http://en.wikipedia.org/wiki/LXC) or Linux lightweight containers. It's kind of like a chroot -- if you're familiar with Linux system calls -- on steroids. It's used most notably at Heroku and by the guys at DotCloud, who became docker. They built their Platform as a Service on top of this stuff. And one of the big advantages of it over, say, traditional VM-based heavy-weight virtualization is that it's all done by the kernel itself. So you're not running multiple kernels, it's not using a ton of memory, it's all a single kernel image.

One of the advantages that gives you, actually, is that you can run it in most cloud providers. So you can run docker inside of EC2 or Rackspace or Joyent or whatever. And you can slice up those virtual instances using Linux containers quite easily. So that's LXC in a nutshell.

And you could describe docker as a very nice interface around LXC. I think you could say LXC is pretty rough around the edges; the userland tools have been known to contain bugs and so on. And docker, at a high level, encapsulates that very nicely. The docker folks, from all their experience at DotCloud, figured out a lot of those gotchas and work around them for you. So at a high level, docker is some really nice tooling around Linux containers. 

06:47 - **Daniel Shaw**: So how does that compare to a SmartOS zone or something like that? Is it conceptually similar?

06:53 - **Niall O'Higgins**: I think you could say it's analogous to a SmartOS zone. LXC is probably not as cleanly architected, maybe, as SmartOS zones, but I think conceptually it's very similar. And indeed there's no technical reason I know of why you couldn't also have docker backed by SmartOS and ZFS, rather than LXC and aufs or whatever other filesystems you're using in Linux. So I think in theory docker could run on both SmartOS and Linux. Although right now it's just Linux. 

07:35 - **Daniel Shaw**: So right now is it primarily just the API? And LXC is the real meat of what's happening, or is it conventions? Why is docker exploding?

07:48 - **Niall O'Higgins**: Well, one of the really nice ideas behind docker is it uses aufs, which is a copy-on-write filesystem. Which essentially means that you can build up images as a series of layers. So you can layer images on top of one another. For example, you can take a base image, which might be Ubuntu LTS or something like that. You can use that as your base, build whatever userspace tooling you need on top of that, say just an nginx server or node. And essentially this ends of being a series of diffs. It's pretty cheap to maintain diffs to the base image because of the copy-on-write filesystem. And there's a bunch of nice conventions around that.

For example: sharing these images in kind of a graph-like way similar to a version control system such as git -- there are a bunch of similarities there. So it's a really nice system for building portable binary images which can run on other docker systems. You can ship them around as if they were a shipping container. That's kind of the metaphor or analogy the docker folks use.

09:05 - **Daniel Shaw**: So what's the difference between a container and a VM?

09:10 - **Niall O'Higgins**: A container is lightweight virtualization with a shared kernel with a shared kernel. Whereas, a VM typically has a hypervisor at a level above the operating system. So in a VM system you're actually going to have multiple instances of the kernel running and they're completely separate systems, which also means there's a lack of transparency. There's a bunch of memory overhead there, also. Whereas with the docker or LXC lightweight container approach you only have one OS kernel running and the kernel is performing the virtualization for you. So really, a docker container is just unix processes with a bunch of what's called namespacing around it, essentially. They're just like any other unix processes, but the OS is adding some additional security and isolation there.

10:11 - **Daniel Shaw**: How robust is that security? If you're running that in your system, do you have strong security or some lightweight security that's easy to subvert? If we gave Adam Baldwin a docker container, is he gonna get out of that container? Or is he going to be well-contained in that?

10:40 - **Niall O'Higgins**: With the one caveat that I believe it is not safe to give people root access within your containers, I believe it's safe. That is the security model used by Heroku and others. So I think it's pretty battle-tested in production. But wit the caveat that you don't want to be running stuff as root or giving people access to root processes within docker containers. But I think otherwise you're pretty safe. 

## Docker in Open Source

11:08 - **Daniel Shaw**: Excellent! So, Nuno, Mike, and Jacob? Do you guys want to add any additional information about your digesting of what docker is and why it's meaningful?

11:22 - **Jacob Groundwater**: Sure. This is Jacob, here. I tried LXC containers for a bit, before trying docker. The biggest thing that I liked as something that was going to be open source was that I could push my docker images to the docker index. Which I think is a very important part of the project. So if I build something up in docker, I can push the layers -- kind of like doing a `git push`, where it sends your objects -- I can send those to the docker index and then someone can do a `docker pull` and pull down the exact container as I made it. And run it. 

12:00 - **Mike Brevoort**: To add to that, when you build up a docker image you may install some packages and some dependencies, right? So for example, if you need to build node and want node running in your container, you would pull that down and build it. Since your container has isolation, that's running in isolation within the container that's spawned from that image. That means you don't have to worry about running conflicting versions of node and how to handle that with something like nvm or something else. You just need to install node and it runs in your container. When you want to upgrade your application you would build a new image with a new version of node and you'd just run that container. And that goes for all other packages as well. You have these docker files where you define all these dependencies that get bundled up when you build the image. It eliminates some of the need for using tools like Puppet and Chef because you've got this image that's all self-contained and doesn't impact anything else. So that isolation, in and of itself, makes things much more portable and able to be run with other containers on larger docker hosts. Or to be able to be run Vagrant locally or anywhere else.

And like Goldwater said, you can publish those to the registry. There's a public docker registry. When you push something to the registry it's very similar to when you push something with git, as well as when you pull. You do it on a diff basis, so if you make a change at the bottom of your docker file and you pull that, you only pull the diff from the last diff place that was cached. So if you're deploying the same application on a docker version that all uses the same base, you're not copying around this giant VM file like you might if it included a snapshot of a VM or like an Amazon AMI. It's just the diff, which makes it really efficient. 

14:12 - **Daniel Shaw**: That is compelling. Having that entire virtual environment is fantastic, but the sheer size of those things just makes it prohibitive, especially if you're bringing up a brand new instance. There's the spin-up time and there's the time to get that in place. So this is definitely compelling. You mentioned, Mike, that there's a public registry, which is awesome. But you also have an internal one, right? So you can have the images that you're not only sharing with the world, but you can have this library of internal "private cloud" instances as well, right?

14:54 - **Mike Brevoort**: Yeah, the docker registry project is open source and the production version is meant to be backed by an S3 bucket. Which makes it really easy to run, really easy to scale. It's just sort of turn-key. Because your instances that run the registry themselves are basically stateless and if they disappear and you spin a new one up you just bind it to the same S3 container. As well as if you want redundancy. And in some cases we have this strange case where we have different environments -- like a dev and staging and prod environment in different VPCs at Amazon. They can't cross-talk to each other; they're on totally separate networks but they can talk to the same S3 bucket. So to get around that at the moment we have registries running in each, that are bound to the same S3 bucket so you can publish to one and pull from another.

It's really easy to run that project, super easy to set up, and even to run locally as well. The only challenge is that the index itself is isn't open source; the actual docker index where you can search for packages isn't. So we don't have a great solution right now in terms of searching for what images have been published to that registry. We're working on something internal for that. 

16:17 - **Nuno Job**: I think something that has not been covered so far is the fact that one problem we have with VMs is that they take a while to start up. So it's fairly hard to do things like testing. If you're running a testing cloud you have to wait for a VM to come up or provision a pool of resources up ahead just to run the test. Then if you actually want to clean it up, it takes a lot of time. Docker is really meant for things where you can run very quickly, be isolated, and just have the result. I think Joyent used it as an example when they were talking about zones, that in Manta could actually run a command on a database on a zone and then get it back. So the virtualization takes milliseconds, not minutes. And that opens up a whole new set of use cases that people didn't think about before. 

17:05 - **Daniel Shaw**: So that is the key to the architecture of Manta: every query is running in a new, isolated zone. That is the magic that is Manta. That's a real key technical accomplishment. Sounds a bit like the dream of isolates before we had domains. That was going to be something in node. It never really materialized, but it was interesting.

17:33 - **Jacob Groundwater**: There's one more feature when you're deploying docker, that is an advantage over using, say, VMs. When you're booting a bunch of VMs you have to boot the whole system right through init and maybe an ssh daemon or something. But docker, when you run it as a process, if you attach it to the foreground at least, your outside process is talking to the std in and std out of the process which is running inside the docker container. So a lot of people, if they're running for example a node app, whatever was centralizing these apps, which were all isolated, each running in their own docker container, there'd be some piece outside that's running the app inside the container. So you can coordinate it from the outside. And you actually skip booting init on all of these systems. You just boot right into the actual process.

18:25 - **Daniel Shaw**: Interesting. Cool. So we'll go into a bit more detail with how docker's being used. Everyone on the call is doing some really cool things with docker. So Nuno, do you want to queue up our beloved friends at &yet?

18:47 - **Nuno Job**: Right, okay! This show is possible due to the generosity of our sponsors. One fantastic sponsor that we have is called &yet. &yet is a consulting firm and a product shop. That builds all its stuff in node. They are renowned for having some really good members of our community that enable node to be what it is. They work a lot on security for node. You might have heard of the lift security project that enables companies like GitHub and 37Signals to have really proper security. They really focus on security with the node security project. 

They also have this really cool baby monitor that I've been using, which is called talky.io. So what you do is this. You get your Android phone because it has the latests Chrome -- I don't know, don't ask -- and you go to talky.io/yourbabysname and you put it on the crib. And then you get your iPhone and you do the exact same URL. Then while your eating with your wife, instead of being with the kid, you can be talking with him and seeing that he's sleeping properly. So this is a great thing. They have been leading the efforts in WebRTC. Talky work with WebRTC underneath, which is fascinating. My wife has no idea why I'm so fascinated that I have a baby monitor on the phone. She's like "just buy on off the shelf." But it's awesome. And I probably shouldn't recommend you do it for security purposes because they didn't think up the use case, but I think it's great.

And finally, they have a product for realtime chat and team collaboration called AndBang. Which we used a little at Nodejitsu and you should try it out. And Bang will allow you to really collaborate well with remote teams. &yet has a lot of remote employees and they really focused on that. You'll have a lot of fun working with your teams with AndBang.

21:05 - **Daniel Shaw**: Awesome. Thank you, Nuno. So let's introduce some of the real world projects that are using docker. How's our connection to Niall?

21:19 - **Nuno Job**: The connection is good, just a little delayed.

21:21 - **Daniel Shaw**: Okay, so Mike is gonna have to take off in ten minutes, so why don't when throw Mike in here first and let him talk about what he's doing at Pearson?

21:34 - **Mike Brevoort**: Yeah, so one of the things that has excited me about docker is how lightweight it is. And how fast you can spin up containers. And the efficiency with which you can run containers with small differences, without requiring a lot of disk space or disk copy, as long as you clean up after yourself. So we're looking at this use case and continuing to iterate on our continuous deployment process. And really starting to look at using a feature branch approach with git. To be able to for every feature create a new branch, typical feature branching. But the requirement for our applications to be able to do end-to-end and run our full test automation against any one of those branches at any time, which means that they have to be deployed somewhere. And in our case they're deployed somewhere behind a firewall, which makes using something like Travis difficult or some of these other public tools. They need to be deployed somewhere so that you can do some discovery testing against those, you need to be able to access and log into them, you might run selenium tests against them. You want to do the full gamut of testing and full automated testing against these feature branches.

So what we did is we decided to use docker. So on every push to any branch to any of these projects: it kicks off building a docker image, then replaces the container -- or pushes a new one if it's a new branch -- to one of multiple docker hosts. That then has that particular commit point of that particular branch deployed and then to a central dashboard place you could go and see every branch that exists and that's deployed and what the URL to that branch is. You can attach the logs to it. We tag the images in docker with the short git SHA commit point and the version if it's actually versioned when it's ready to go. So what you end up having is this near-realtime state of deployment for every single branch that's pushed and a new one that gets pushed for any new branch that's created that then kicks off all of our test automation. So as we report back build status that gets pumped up through there as well so we can see full automation for builds happen. That's been amazing and revolutionary.

We hooked it into our hipchat robot called bender, so you have bender commands to get the URLs to the versions of the branches that are deployed from there. You can see build status and stuff from there. We're building off of that. So at the point that we do a merge into master that means it's done, tested, and ready to go. And when we do, that triggers a production deploy in a continuous deployment way. And right now we're researching whether or not we're going to do those in docker containers themselves. For this new project we're on I think we are. We're trying to be pretty aggressive with it. So we have those, and we're going to use our internal docker registry as our package repository for that. So deployment actually becomes much simpler because we just need to do a `docker pull` and then start a container up. 

25:18 - **Daniel Shaw**: So do you envision a Facebook cells kind of approach? If you don't know what Facebook cells are, they're isolated clusters of functionality that they deploy and they'll have those in production at the same time. Is production a contiguous thing that's only that point in time, or do you see that as multiple points in time that you're evaluating under real production load?

25:55 - **Mike Brevoort**: It could certainly be that way. And we do some of that today, even on individual we deploy on Amazon on the projects that I'm on. We have automation to do that and we roll new instances on every deploy. And we use this project that we open sourced called [Thalasa](https://github.com/PearsonEducation/thalassa) to do orchestration of load balancers based on app name and version. So this fits right into that -- you may have different versions of your app running at the same time in production. Maybe of sharded or zoned, or maybe A/B-style or hashring-type routing, depending on how you orchestrate that for you application. The benefit that it brings is that it's much faster to start up these containers versus starting up whole new instances. And it's faster to roll back when you don't have existing instances. Because your previous versions on that host actually has the full cache of the images. So it becomes really fast to do that as well.

26:51 - **Daniel Shaw**: Awesome. Be sure to throw in a link to Thalasa in the show notes so people can go check that out. It's a really interesting project. So Mike has to take off. Did you want to get a plug in before you take off, Mike? 

27:04 - **Mike Brevoort**: Sure. So I work at Pearson, we've been doing node for along time. We're in the Denver Front Range area in Colorado so if anyone's interested in that, hit me up. And also I organize the local [DenverJS](http://nodeup.com/www.meetup.com/Denver-JS) meetup, which is a node and JavaScript meetup. And we're looking for both speakers and sponsorship in getting together our 2014 plans. So if you're in the Front Range area come check us out on meetup.com. We'd love to see you there.

27:29 - **Daniel Shaw**: Thanks, Mike. Appreciate it. Let's hear about what Niall's been doing. He's probably gone the deepest down this rabbit hole. He's been doing some really fantastic stuff. Let's see if you have enough connectivity through Portugal to Mexico. 

27:47 - **Niall O'Higgins**: Mike really talked about some great stuff in terms of the possibilities of what you can do in production with docker. Something I'd like to talk about that might be cool, too, is just using docker as a kind of a platform to distribute open source applications. So I think if you look at a lot of open source web applications, node or otherwise, you're likely going to have some sort of dependency on a database. Maybe it's redis. In my own case with StriderCD, we have a MongoDB dependency. And what we're able to do with docker is package up the application itself with the source code from GitHub, plus the npm dependencies already installed, plus the node version that you need, and also an already-configured MongoDB. We put that all into a single docker container and push that up as an image to the public docker registry. So if you very quickly want to pull down the latest version of StriderCD without having to configure node, without having to configure and install Mongo, you can just run `docker pull strider/strider` and you should pretty much have it running on your system. So I think there's a lot of potential there.

I know other folks are already doing this. I've seen a very popular Jenkins image on the docker registry. I know now with container linking, which is a new feature in docker 0.7, this makes it even more interesting where you can start to have, say, an officially-maintained Postgres image. So any application which depends on Postgres can simply link against the officially-maintained docker image. And just link these things together with dependencies. So I see it as a great way for authors of open source software to make it easy for people to get their stuff out there and make it easy to configure. Especially for the more complicated applications.

Another one I can give an example of is Peter Braden -- my co-founder -- has the [node-opencv](https://github.com/peterbraden/node-opencv) project. node-opencv has a binding to the computer vision library but the binary dependency on opencv is very heavyweight. It's pretty complicated and difficult to build opencv on your machine. There's an enormous amount of dependencies. So I know he's worked to create a docker image which has opencv and node and his binding installed. So I think docker is really interesting for open source people, too. I'll just kind of throw that out there.

## Testing with Docker

30:53 - **Daniel Shaw**: Cool! Jacob, do you want to share some of what you've been doing with docker?

30:59 - **Jacob Groundwater**: So I have two very unrelated projects, but they're both using docker. Let's start with the practical one. Boo! But we'll get to the impractical one. I work at [New Relic](https://newrelic.com/) and we work on the node agent, which is what we use to instrument node apps. So if you're running something like a node web application you can install our agent as a module and it will report all of the New Relic goodness. You can look and see what are the slowest parts of your app and speed it up. Now, part of that is supporting the wide array of module configurations that people are using to run their node applications.

And what we wanted to do was have a very solid testing framework. So we had the tests, but we wanted to run the tests against multiple versions of node, and when we did that on the same machine, we were getting a little bit of leaking between tests. We spin up MySQL, Redis, and Mongo instances because we instrument those database connections. And what better way than to just test it for real? But sometimes a MySQL instance wouldn't terminate after the first test and it would leak into the second. And we had issues if I did an `npm install` and I changed the version, sometimes there were slight differences. So what I did is I took the [DNT](https://github.com/rvagg/dnt) module by [Rod Vagg](http://r.va.gg/) and heavily modified it and built a CI tool that runs the unit and integration tests for each version of node that we support in a separate docker container. And then it aggregates all the results and posts them to the web. And it does that based off of when we do a pull request on GitHub.

When one of us wants to add a feature, GitHub will send a post-back notification to our CI server and it pulls the changes and it will run those changes in a suite of docker containers. We know they're isolated. And then it posts the results back. The other thing it does is it saves all of the test runs so if one of them fails and I'm curious what the state of the machine was, I can log right into that docker container and re-run the tests, I can look at the log output, I can look at whatever the state of the machine is. Before I jump into the impractical part, did you have any questions about the CI?

34:01 - **Daniel Shaw**: That's really cool: being able to go back and post-mortem the state. Is one of the most essential things that you need in debugging. So that's fantastic.

34:13 - **Jacob Groundwater**: Yeah I'm looking into some more things I can add to it, like the possibility of core dumping, now that TJ has gotten Linux core dumps to work on the SmartOS tools. And MDB. I'm also looking at parallelizing it a little more because we support different versions of node on there but I actually want to run it on the 40-plus versions of node that we support. Including all the unstable 0.11 changes that are coming out. You know, just so I know. You never know!

So the other project that I've been working on -- which is totally un-related but also uses docker -- is NodeOS. Which is taking a Linux distribution, stripping everything out other than the kernel, including `init` and all the userland, `ls`, `bash`, getting rid of all of it. Then putting node on there, getting `npm` working, and then just using `npm` as the entire operating system's package manager. So if you want an executable like `pwd`, `ls`, or `vim` or something, you have to stick it on npm. It's complicated, and it's probably a topic unto itself as to why someone would do this. I wanted to start the project and get people involved very early, like when we're just duct tape and banana peels and things like that.

Originally I had it booting into a VirtualBox instance. I would mount the filesystem on some other Linux box, mess around with it, close it, then boot that image separately and see if it worked. And I found it was slow to do that, and it also wasn't that fun. I mean, the fun part of doing this was quickly building higher-level tools. And while -- if you're going to build an OS that starts from bare metal, you absolutely need to do the lower bits -- I decided I wanted to work from the top down, instead of from the bottom up. So I started using docker. And docker does a few things: the machine's already booted, the kernel's loaded so you don't have to worry about making sure you know how to load on the hardware that you're running. It will actually configure the external network interface for you, which is nice because other than that you're writing a lot of system calls that node doesn't natively support. And I did get that stuff working eventually.

But the main thing is I just wanted to have a pipeline where I could make a change, hit a build script, push that change out in a matter of minutes and someone else could be using it. So docker has really filled the gap there. So I have a build script that -- if you check out the GitHub project -- you have a checked out version of node and some other stuff, and it will build the docker container, and it will build the NodeOS image. You can just push it and pull it and people can play with it. I have a available so I'll put that in the show notes. It's been great -- it's a great pipeline and I think that's It's a really strong piece to have in your deploy pipeline. 

37:50 - **Daniel Shaw**: Fantastic. Nuno, you have probably one of the most important blog posts up on medium right now about docker. I know a lot of people are referring to that. We'll be sure to include that in the show notes. What are you doing with docker and why are you so excited about it?

38:10 - **Nuno Job**: I'm involved in some projects that are using docker. One of them was actually started by Niall, which is called BrowserStorm. Another one that I've been following very closely is [NodeChecker](http://nodechecker.com/) which is basically a center to test all npm modules. Are you familiar with it, Daniel?

38:30 - **Daniel Shaw**: Yes! I am, but tell everybody else about it.

38:34 - **Nuno Job**: [NodeChecker](http://nodechecker.com/) is a place where, every time you puslish a new npm package, it runs the tests for you. so if you go to nodechecker.com right now, which I am attempting to do, you can see: how many npm packages have tests that time out, have no tests, failed or passed tests. You'll find that over 50% have no tests. That's a big no-no.

39:04 - **Daniel Shaw**: That Nuno says "no-no."

## Orchestration and Docker Linking

39:08 - **Nuno Job**: Yeah, that's my type of humor. So one thing I think is very exciting about docker -- that is really not very well understood yet -- is the linking part that Niall was speaking about. And the reason why it's interesting is, as Niall said, the way we deploy Strider right now is it puts Mongo inside the container, it puts all the dependencies inside the container, and is like "voila, here's a product." But there's limitations to this, of course, because as we know from npm, it's really good to have something that manages dependencies really, really well. There are a bunch of legacy systems -- I'm probably going to be the first person to call Puppet and Chef legacy -- Niall is not laughing, so it's okay.

But how do you do that in this docker era, right? How do you orchestrate all these containers? And for instance if you're running multiple servers, you can be running different OS's and different virtualization layers, and it's still not very well understood how you can still ship with the same ease of use that docker gives you. You can just put Mongo, put node, put Strider, and give it to an end user just with one command. How do you do that while maintaining the integrity of the relationships? That's super-interesting. I think there will be a lot of space to go and do work on that. One, there's new stacks coming up you'll find as a service things like [Orchard](https://orchardup.com/) that try to bundle up the docker experience for you. New stacks like [Salt Stack](http://www.saltstack.com/) etcetra I think are attempting to go on the docker route.

But the way I see it that is there is a lot of space for people to actually write their own scripts and to build new tools with node and with docker. That said, the guy that did [NodeChecker](http://nodechecker.com/), [Pedro Dias](https://github.com/apocas), he also did a project called [dockerode](https://github.com/apocas/dockerode) which is a really cool abstraction to use docker from node. You can check it out at [github.com/apocas/dockerode](https://github.com/apocas/dockerode). But I'm pretty sure that if you Google "dockerode" you'll only find this. And I think it's really an exciting time to create this glue, right? How does it work? Another example that Niall gave was, well, when you're running Mongo and the node Strider version, if Mongo goes down then you probably should restart node. One thing that I've been using is the excellent [node-mongroup](https://github.com/visionmedia/node-mongroup) by [TJ Holowychuck](https://github.com/visionmedia). So doing all this, it's not even orchestration but all this layering just to manage the dependencies right, and have docker containers be independent and work across different servers and even, potentially, different architectures. That's really the part that is interesting for me right now, in docker. So that's it. 

39:04 - **Daniel Shaw**: Awesome! Does anyone want to talk about [Rod Vagg](http://r.va.gg/)'s [DNT](https://github.com/rvagg/dnt)?

## Plugs

WIP

