---
date: 2020-02-15T18:43:45+10:30
hero_image: "/content/images/nasa-Q1p7bh3SHj8-unsplash.jpg"
title: On serverless/JAMstack deployments
author: Joshua Steele

---
This is a long one, it is separated into two parts.

Implementation issues and why you should power through them.

# JAMStack + Serverless

## _A great idea, with a bit of complexity._

You might be asking yourself: I thought the whole point of serverless was to get rid of complexity? Yes it is, and on that front its concept is successful when it comes to the abstraction of systems. I went through the motions of trying to get this site set up and I've had a lot of issues.

![Stacked Jam Jars](/content/images/esther-pervis-NGp4xEotut0-unsplash.jpg "A JAMStack")  
Photo by [Esther Pervis](https://unsplash.com/@easter291?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/s/photos/jam?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText). Not that kind of jam stack.

The scene is, I have five sites in development. Two for myself, and three for other people or organisations. Not having to deal with server configuration is really a dream for me. Rather than spending days trying to get the right packages and software configured to create a service, I can just get the requisite components and write some code that frankly I'd have to write anyway. It's a great timesaver when everything goes to plan. My big problem, or so I've noticed is when something stops working that joy stops. For example, headless CMS: I've tried several, and to be honest I love the concept. Every one of them has caused me issues, Netlify CMS wouldn't allow me to log in as the login route failed, Sanity's Gatsby template wouldn't build and finally I'm on Forestry.io currently. Is Forestry perfect? No. Was the deploy functional? Yes. Am I still having issues? Yes, but far less than the other ones I've tried. (I would still highly recommend Forestry though)

If you are using a serverless architecture and/or JAMstack, get used to a bit of pain. Most of the support teams I have no doubt would be helpful, but in the early days of development it is easy to switch out systems that why would you until you exhaust your options? If there is something that works better or costs less I will switch to it because the cost of switching is low.

There are exceptions to this rule: Authentication and authorisation is a big one, it is really hard to switch between auth services as quite often you have an entire SDK to change. I'd assume its impossible to export users later as well. Despite this I'd highly recommend them, but do your research first. Those services are costly to get right on your own and put risk onto your business or person. It's definitely worth delegating identity to some company that makes it their expertise. Mind you auth is still something I dread writing code for, as there are so many ways you can screw everything up. A lot of the code dependency issues can actually be mitigated by using something known as the clean architecture and just abstracting your dependency, probably a bit too much extra work for most.

I still like rolling my own things, because I love the challenge. You would however be hard pressed to see me use certain projects of mine in production without having some of my more security oriented friends look through it first.

**Update:** Forestry.io has already gotten back to me about my issue even though I wasn't expecting a response yet. (It's the weekend) They said they'll have a look on Monday.

**Update II:** Forestry.io was going through some downtime. I guess that was my poor luck. I'm glad it's functional now anyway. At least that's what I'm assuming was going on as it is working now and their app status is showing downtime in the correct period.

# Why I'm in love with JAMstack

I live in the fifth largest city of a country with notoriously poor internet infrastructure. Where do I live?

![Adelaide, Australia from Victoria Square](/content/images/vicsquare.jpg "My hometown")  
_Photo - By Silveryway -_ [_https://www.flickr.com/photos/53038006@N06/15592637199/,_](https://www.flickr.com/photos/53038006@N06/15592637199/, "https://www.flickr.com/photos/53038006@N06/15592637199/,") _CC BY-SA 2.0,_ [_https://commons.wikimedia.org/w/index.php?curid=41760092_](https://commons.wikimedia.org/w/index.php?curid=41760092 "https://commons.wikimedia.org/w/index.php?curid=41760092")

**Adelaide, Australia.**

_Fun note:_ For some odd reason despite being geographically near Singapore globally, I've noticed that when I used to host servers there the speed was appalling. Turns out Optus in all their wisdom were routing to Singapore through the United States. Provided you have a good CDN, this would never happen on JAMstack.

What do we all want from our websites or information systems in general?  
Functionality, Usability, Reliability, Performance and Security.  
(So my Systems class tells me)

How does JAMstack help meet these goals for our system?

I'll be coming from a perspective of using Gatsby.

## Functionality

* Fundamentally, your JAMstack site is a static web application. You have access to the entire JS community's efforts for making services work well on your platform.
* Most websites are at least publishing API's designed to be consumable in JS and some are even embracing SPA's as well.
* As JAMstack utilises Version Control Systems (VCS) such as Git to trigger deployment it's easier to create dev or staging branches to test new features.
* Services likely exist already that can provide the functionality required, you can utilise these to speed up your development.

## Usability

This section is just about general Accessibility in single page apps.

* Accessibility issues are easier to detect with the magic of linting.
* Gatsby in particular has great documentation of accessibility in SPA's.
* It's a lot easier to make accessibility changes to your site using SPA frameworks as changes propagate per component.
* I really love the library emotion for dynamic text sizing and theming.
* UI/UX in modern web apps is just super darn cool.

## Reliability

**Scenario:** Your CMS goes down.  
**Plain Old CMS:** Your site is broken.  
**Headless CMS:** Your _CMS_ is down, your site should still work.

**Scenario:** Your Auth server is down.  
**Old Auth:** Your site is likely broken as it's a big sad monolith.  
**JWT Auth:** You're fine until your token expires, then unauthenticated.

**Scenario:** Your web server/cluster is down.  
**Traditional:** Dead.  
**Cloud (No CDN):** AWS outage I guess, oops.  
**JAMstack:** That is unlikely as heck. Global outage of a CDN? Sure.

Your site architecture straight up encourages usage of microservices.

As you might have guessed, the modularity and geographic distribution of these apps means that a full outage is very unlikely.

## Performance

This is where JAMstack really shines.  
Provided you are using a good CDN and framework JAMstack sites are fast.

Renders are speedy and resources are loaded as needed. Server side rendering isn't a thing because it doesn't have to be, data is pre-rendered with nothing done at runtime before serving the site. That isn't saying you don't have to wait for data, you can have dependency on APIs you just don't let it hold up the page.

Gatsby Image is great to exemplify this as it shows a low-res placeholder image and fetches the high quality image after the render.

Gatsby (on this blog) rebuilds itself with all pages every time I create a post meaning everything is super fast. Seriously, try switching pages on here. No latency!

Your CDN uses some secret routing sauce to make sure your request is geographically close to you. No more being routed crazy distances for a request!

## Security

You have access to all of the advancements made in Single Page App (SPA) security as your app is again just another JS app. Auth0 and Okta both have great documentation on how to secure SPAs using their respective services. API security on this front hasn't really changed. I'd say it's easier to implement secure applications by decoupling the renderer from authentication and authorisation. It encourages common APIs between clients and use of open proven security standards like OAuth and OpenID Connect.

# Conclusion

JAMstack is a new architecture that is inexpensive, fast and uses exciting frameworks that the community has already learned to love. Serverless architectures empowered Single Page Applications to be scalable and made deployment and development much easier and cheaper. It's understandable that there will always be pain points for new programs and I have no doubt that I could probably have used any platform in this post. I'd encourage you to at least try this new workflow and deployment style. I think it's very exciting.

## Some great resources I love

[https://jamstack.org](https://jamstack.org "JAMstack Website")

[https://developer.okta.com/docs/](https://developer.okta.com/docs/ "Okta Documentation")

[https://auth0.com/docs/quickstarts/](https://auth0.com/docs/quickstarts/ "Auth0 Quickstarts")

[https://www.netlify.com](https://www.netlify.com "My current host")

[https://www.gatsbyjs.org](https://www.gatsbyjs.org "Gatsby")

[https://forestry.io](https://forestry.io "Forestry CMS")

Cover Image: Photo by [NASA](https://unsplash.com/@nasa?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/s/photos/internet?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)