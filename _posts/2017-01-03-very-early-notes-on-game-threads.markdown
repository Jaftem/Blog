---
layout: post
title:  "Very Early Notes on Game Threads"
date:   2017-01-03 13:13:58 -0600
categories: GameThreads
---

Game Threads is an unreleased social platform and CRUD app for sports fans that I have been working on with a few other people lately. The gist of the
web app is that it will create and destroy chatrooms with respect to when sporting events go live. Users in the the chatrooms can upvote
and downvote posts similar to Reddit's system, except everything is live and in realtime.

This is most definitely the first large project that I have undertaken where the goal is to actually get the platform into production and
into the hands of real users. Even though Game Threads isn't an actual startup, the below graphic is especially relevant to the project and
is something that should be kept in mind moving forward:

{: .center}
![Top 20 Reasons Startups Fail]({{ site.baseurl }}/assets/img/20reasons.png){:width="450px"}



Below I've outlined a segment of my strategy to fight the above graphic and increase Game Thread's *chances* of success.

### Creating an MVP 

This is an important step to combat the top bar at 43% on the graph, "No Market Need," if the web app just has no market. The hunch
is that it does, because websites like Reddit and SBNation, while not made specifically for things like game threads, have subreddits
and sections of their sites in which this concept and idea has been immensely popular. During playoffs and championship games, Reddit's
servers are notorious for crashing because of heavy traffic.

Creating an MVP also serves another purpose of tackling another reason on the above list: "Poor Product" at 17%. With an MVP, we can find out
what does and doesn't work with Game Threads so that at launch, we can maximize the potential of the web app.

So our first goal for Game Threads is to create a barebones web app in which we manually create (as opposed to automate) functional
chatrooms with nested realtime threads and live upvoting/downvoting. We need to measure and take into account a few different things
during this trial period:

* Average time users have the web app open.
* Percentage of users who participate (talking, upvoting/downvoting).
* Average posts per minute/unit of time.
* If possible, how traffic stresses the server.
* Different extra chatroom features as variables from game day to game day.

The hope is that users participate at least as close as possible or over the amount of time a sporting event lasts. For all-day
events like NFL Sunday, we ideally want some users would be participating for a relatively long portion of the day.


### Where we are at

We just started organizing this winter break, so we're still at the drawing board. What we know now: we'll be using the MEAN stack
with Angular2, which means each of us has to learn web development and these frameworks and tools. At the time of writing, I have
created a functional *offline* chatroom prototype with a very quick layout design all made in Angular2:

{: .center}
![Game Threads prototype]({{ site.baseurl }}/assets/img/gt-prototype.png)

By the way, while learning Angular2 and reactive data flow, I was able to create [the terminal on my front page](http://www.jaftem.com)
with near-identical code used for the above chatroom. This type of data flow also makes creating a chatroom incredibly easy and flexible:

{% highlight JavaScript linenos %}
// Message data stream
messages: Observable<Message[]>;
// Stream of operations to be applied to all messages
updates: Subject<any> = new Subject<any>(); 


this.messages = this.updates
  // watch for updates & apply operations
  .scan((messages: Message[],
          operation: MessagesOperation) => {
            return operation(messages);
          },
        initialMessages)
  // Share messages with anyone who wants to subscribe and cache
  .publishReplay(1)
  .refCount();
{% endhighlight %}

Now, if a user sends a new message to the chat, we can have `updates` subscribe to a `create` observable which watches for new messages and
applies a concatenate `operation` on the the message list. Just the same, we can have the list cleared by having `updates` subscribe to
a `clear` observable which watches if the user types `/clear`.

Pretty cool. 

And that's pretty much it for right now. This post will be grouped together as a series of posts concerning Game Threads to catalog
its progress in the coming months.
