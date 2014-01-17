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
3. [Placeholder](#placeholder)
4. [Placeholder](#placeholder)
5. [Placeholder](#placeholder)
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

01:12 - **Daniel Shaw**: So, today's sponsors are [Clock](http://clock.co.uk), [&yet](http://andyet.com), and [Modulus](http://modulus.io). And I'm Dan Shaw, I run The Node Firm. Let's go around the horn and let everyone introduce themselves before we get into dockering all the things. Mike, you want to kick off the blurbs?

01:32 - **Mike Brevoort**: I'm Mike Brevoort. I work at Pearson. Working to innovate and create and deploy our next-gen products globally. We've been using node for a little over three years there. 

01:43 - **Daniel Shaw**: Awesome! Niall?

01:44 - **Niall O'Higgins**: Hey, I'm Niall. I'm the founder of [FrozenRidge.co](http://frozenridge.co/). We do node software development. I'm also the primary author of [StriderCD.com](http://stridercd.com/), an opensource continuous deployment platform written in node. And if I don't sound very good on this call it's because I'm currently continuously deploying myself around Mexico. 

02:06 - **Daniel Shaw**: Jacob? Mr. Groundwater.

02:10 - **Jacob Groundwater**: Yes, Jacob Goldwater here. I work at New Relic. I work on the node agent there. Also, in my spare time, I work on [NodeOS](http://node-os.com/blog/get-involved).

02:20 - **Daniel Shaw**: Nice.

02:21 - **Nuno Job**: Hey, Nuno Job here. I used to work for Nodejitsu, where I built the first continuous deployment there, actually. I wrote some of the stuff that you might have read on docker. And I'm currently working on a super-stealth startup called percent, which you can see online so it's not very stealth. I'm the creator of LXJS and financing myself through consulting. 

02:40 - **Daniel Shaw**: Awesome. So let's fill in a little bit of context around docker. But before that, let me give a shout out to our awesome sponsor Clock. Clock is a digital development agency based in the outskirts of London. They make beautiful websites and web-based applications. They're great at integrating legacy systems, devices, and all the things that really don't want to be integrated. They are experts in publishing, customer insight, and customer loyalty. They have developed hardware devices that retailers have put into stores and venues. It 100% runs on node.js, it's called SwipeStation -- you can learn more about that at [swipestation.co.uk](http://swipestation.co.uk/). They've been making websites since 1997, they've been building node since 0.4, and since then they've fallen in love and gone all-in on node. Their clients include BBC, Newscorp, Nielson, Joyent, Eddie Izzard, Hearst Media. Fantastic company. A couple of their projects are (NeverUnderdressed.com](http://www.neverunderdressed.com/) -- a high-end online fashion magazine. SunPlus is a access-controlled loyalty platform for the Sun newspaper -- that's at perks.thesun.co.uk. And Sunday World, Ireland's biggest tabloid at (SundayWorld.com)[SundayWorld.com]. They are the developers of the Node Bowler, a bowler hat with some really cool hardware hacks. Have a look at [clock.co.uk](http://clock.co.uk). Get in touch with them at [hello@clock.co.uk](mailto:hello@clock.co.uk). Be sure to follow [@clock](https://twitter.com/clock) on Twitter. Send them a message and say you heard them on NodeUp, and thank them for supporting node.

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

11:08 - **Daniel Shaw**: Excellent! So, Nuno, Mike, and Jacob? Do you guys want to add any additional information about your digesting of what docker is and why it's meaningful?

11:22 - **Jacob Groundwater**: Sure. This is Jacob, here. I tried LXC containers for a bit, before trying docker. The biggest thing that I liked as something that was going to be open source was that I push could my docker images to the docker index. Which I think is a very important part of the project. So if I build something up in docker, I can push the layers -- kind of like doing a `git push`, where it sends your objects -- I can send those to the docker index and then someone can do a `docker pull` and pull down the exact container as I made it. And run it. 



## Plugs

WIP

