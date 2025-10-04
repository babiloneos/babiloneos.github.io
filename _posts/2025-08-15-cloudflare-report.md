---
layout: post
title:  "Cloudflare report for a selection process"
date:   2025-08-15 10:50:00 -0600
tags: Cloudflare CTF
author: Guillermo Ballesteros
---

# Table of content
- [Table of content](#table-of-content)
- [What is this?](#what-is-this)
- [The requirements](#the-requirements)
- [Prerequisites](#prerequisites)
    - [1. Public web page up and running](#1-public-web-page-up-and-running)
    - [2. Domain for the webpage already set](#2-domain-for-the-webpage-already-set)
    - [3. TLS Certificates to use HTTPS](#3-tls-certificates-to-use-https)
      - [Ready to go!](#ready-to-go)
- [Tasks list](#tasks-list)
  - [1. Report detailing how you implemented the technical requirements and where or how we can validate your deliverables.](#1-report-detailing-how-you-implemented-the-technical-requirements-and-where-or-how-we-can-validate-your-deliverables)
    - [Create an account](#create-an-account)
    - [Onboard a domain](#onboard-a-domain)
      - [Select a Plan](#select-a-plan)
      - [DNS Records](#dns-records)
      - [Update nameservers](#update-nameservers)
      - [Replication of nameserver](#replication-of-nameserver)
      - [Finished process](#finished-process)
    - [HTPPS for all the web page](#htpps-for-all-the-web-page)
      - [Always Use HTPPS](#always-use-htpps)
      - [Minimum TLS Version](#minimum-tls-version)
      - [TLS 1.3](#tls-13)
      - [Automatic HTTPS Rewrites](#automatic-https-rewrites)
    - [Firewall Rules](#firewall-rules)
      - [Block traffic from at least five countries.](#block-traffic-from-at-least-five-countries)
      - [Challenge rule](#challenge-rule)
      - [Avoid too many requests on a particular part of the website](#avoid-too-many-requests-on-a-particular-part-of-the-website)
- [Conclusion](#conclusion)


# What is this?

For the past few months I have been actively seeking employment due to "restructuring" reasons.
In this seeking I arrived into a Cloudflare related job opportunity, and thankfully I've came to the interviews round where they asked me to prove my capacity to learn quickly, explain clearly, and do any needed research in order to solve issues.
And I say "thankfully" because **this little homework took me back into the things I loved about computer engineering back in the university.**
So as part of this, and guided by my enthusiasm, I decided to publish an entry here, in my personal blog, to expose all I've learned and how impressed I'm with Cloudflare.

# The requirements

1. **Deploy a basic Cloudflare setup** including onboarding a domain into Cloudflare and basic security protection for it.
2. All pages in the website **must use https**.
3. Setup **firewall rules** to:
 a. Block traffic from at least five countries.
 b. Avoid too many requests on a particular part of the website. 
4. **Create a response header** containing:
 a. Name: CF-Custom-Header
 b. Value: True
5. **Write the report** as if you where creating a customer facing document.

# Prerequisites 

So, based on the requirements, I need to have all the bellow things already:

### 1. Public web page up and running

I struggled a lot with this task, as **I didn't had a public web page**, and the one I think about to use was not fully functional.
Additionally, I didn't know anything about docker, or how to deploy it with any cloud service.

My solution was:

   1. Focus on making the web site fully functional
   2. Use my recent knowledge on Azure to test the deployment of my web page there
   3. And finally, use the cloud configuration that worked the best for me

I don't want to go deeper on this, as this will be a separate post, but **the result was** a CTF project from the university, coded with Python and Flask, hosted as a Docker Container in an Azure Web Application.

You can consult it in here (for a brief period):

> 🌐 **[atletasctf.site](https://atletasctf.site/)**  

But keep in mind that it won't be online for a long time as it is costing me money, and I'm not interested on paying $30/month just for the fun of having a college-level CTF online.

![Azure Web Page Overview](/assets/img/CloudflareCTF/AzureWebApp.png)

However you can always deploy it with the public docker repository, or with the github repository (with an update pending for the README):

   - *babiloneos/atletas:latest*
   - *[github.com/babiloneos/Atletas](https://github.com/babiloneos/Atletas)*

✅ **DONE**

### 2. Domain for the webpage already set

So once the webpage was up and running, good news, Azure provided me with an unique url. That works as a domain, right? ... well, no.

Azure provided me a really weird URL: _atletascontainer-[random string].mexicocentral-01.azurewebsites.net_ which works, but poorly.

Additionally, and thanks to an "in-advance" investigation I did, **Cloudflare requires me to be able to modify the DNS records for this domain**, and that is something Azure won't allow me to do.

So yes, we will have to get a domain name.
There are some free options, like [www.freenom.com](https://www.freenom.com) but it always showed me that my custom domain was not available, no matter what string I used. Here an example:

![Freenom results](/assets/img/CloudflareCTF/Freenom.png)

So after some hours trying with whatever name came to my mind, I decided to go for a paid but cheap domain to skip all this struggle.

As I didn't want to waste more time on this, I did a quick google search and went for the easier provided I found at first view, which was **Hostinger**.

There isn't much to show here, just to mention that **I adquired the domain "atletasctf.site" for like a dollar for the first year.**

✅ **DONE**

### 3. TLS Certificates to use HTTPS

Oh, holly HTTPS! Fortunatelly **this was solved without any hard work**.
Turns out that Azure provides free certificates for HTTPS, and they are re-signed automatically.

How beauty is that?!

All I had to do was to went into "Certificates" module in the Admin dashboard, and add it.

Now, quick note, I did this AFTER getting and configuring the domain in Azure, so my certificate was provided directly to the domain atletasctf.site.

![Azure Certificate](/assets/img/CloudflareCTF/AzureCertificate.png)

**Do you want a better new?** This was wortless as **Cloudflare also provides TLS certificates for free**. So yes, at the end I decided to use the Cloudflare certificate, but we will see that in a moment.

✅ **DONE**

#### Ready to go!

With the webpage up and running on a personalized domain, and TLS certificates, lets star with Cloudflare hands-on.
> Please notice 🟡 Because of the deliverable indications, the next section will have a more professional writting, or at leat I will try for it to have it. (See [The requirements](#the-requirements), point 5, to understand)

# Tasks list

## 1. Report detailing how you implemented the technical requirements and where or how we can validate your deliverables.

### Create an account

**Cloudflare offers a free plan** for anyone to protect their web pages, covering the fundamental protections as DDos, WAF, Caching, and more.

1. Go to the Sing Up page:
[dash.cloudflare.com/sign-up](https://dash.cloudflare.com/sign-up)

Here you will see this familiar form asking for your email and a password to create an account:

<p align="center"><img src="/assets/img/CloudflareCTF/CF_signin.png" height="600"/></p>

Remember that password must match the security requirements, and that this credencials will be needed to log-in in the future.

After filling the form, click on the "I'm not a robot" box, and click on sign-up button.

### Onboard a domain

At this point you will be at the "**Boost your site's speed and security**" screen. Here we will enroll a new domain in cloudflare:

![Set a new domain](/assets/img/CloudflareCTF/CF_step1_config.png)

In the first input field with the text "_Enter an existing domain_" you will need to put the domain of your already existing web page.

For this example the input was _atletasctf.site_

Then you will find a list of options:
1. Quick scan for DNS records
2. Manually enter DNS records
3. Upload a DNS zone file

We will follow the recommended option, that is the first one **Quick scan for DNS records**. This will  make Cloudflare to scan over the internet Domain Name Servers to get all the information related to the domain name of our web page. This will get the IP addresses of the server, and other useful information.

Additionally Cloudflare has just enable a new configuration to **control the access for AI bots** (like the used by chatGPT or Cloude). With this configurations you can stop them, from getting access to your web page and the information inside them.

The options are:
1. Block on all pages
2. Block only on hostnames with ads
3. Do not block (off)

The use of this setting will depend on the kind of web page that you are hosting, and the data policies inside your company.

For this example, we selected "Do not block (off)" because the information from the app is blocked with a login, and even though, it doesn't contain any private information.

The last option called "Instruct AI bot with robots.txt" is also related to the AI bots, so its usage will depend on the previous selected option. For our web page it is off, as there is nothing to map in robots.txt with a log-in blocked web page.

When everything os properly selected, click on "Continue" for the next step.

#### Select a Plan

Cloudflare offers a lot of options based on their huge catalog of protection and improvement tools. And they offers them based on "packages". There are 3 base packages for small and mid web pages, but if your web page and company is billing more than $2400 dollars per year, I suggest to go with the Enterprise option that will create a package that fits with your needs.

For this example we are using the Free plan, that is the first one on the left side. 

The **free plan covers all the basic protections** for DDoS,  bots, rate-limiting, etc.

Select your plan with the "_Select plan_" button

![Select a plan](/assets/img/CloudflareCTF/CF_step2_plan.png)

#### DNS Records

Cloudflare works as a protection layer that the incoming traffic must pass in order to get into your actual web page.
For this to work, Cloudflare "re-route" the traffic directly from the DNS records that are available all around the internet.

What cloudflare does is to cover the real addresses of your web page with cloudflare ones in the DNS records. This way, when someone whats to go to your web page they will try to resolve the domain name, and this solution will guide them to a Cloudflare's IP address. Then when cloudflare receives the traffic, it will identify the actual web page requested, will apply its protections on the traffic, and if the traffic is safe, it will be redirected to your actual web page.
And this works the same way on the backwards traffic, as the data is received by the requestor as if it came from Cloudflare.

This is what's called a **Reverse Proxy**

![Cloudflare reverse proxy](/assets/img/CloudflareCTF/reverse-proxy.webp)

It is important to understand this to proceed with this step.
In this screen cloudflare is showing us what it found in current DNS records for the domain name to protect.

You will see in the list a column with a "cloud" icon. This indicates which entries from the DNS records can be covered with Cloudflare, process that is called "Proxied" in here.

The entries that must have the orange cloud "icon" are the ones with " and AAAA in the "Type" column.

Our duty in this screen is to review each entry, and validate that they are correct, and that mandatory entries are Proxied by cloudflare.

After doing this, click on "Continue to activation".

#### Update nameservers

As explained in the previous step, covers the real addresses of your web page with its owns. To do this, it modifies a paremeter called "nameserver", which is the parameter that indicates what DNS has the original directions for the domain name.

In other words, the **nameserver** is who indicates the IP addresses of your web page to all the internet. So other DNS will consult with the nameserver the addresses for you web page domain name.

![Name server](/assets/img/CloudflareCTF/CF_step4_nameserver.png)

Cloudflare provides a step by step guide on how to change the nameserver for your domain name in this screen. However, here you have a brief overview:

1. Get into your domain name provider's web page, and go to the settings for your domain name.
2. Disable any DNSSEC configuration
3. Go into the nameserver setting and modify them to be the ones provided by cloudflare, and not the default ones. You will find them in this same screen at the point 3 along with a orange cloud icon.
4. Save the changes

Once this is ready, click on "Continue"

#### Replication of nameserver

This is the last step, and it does not requires any interaction from, our side.

After updating the nameserver, all what is needed is to propagate this change on the DNS records all around the internet, so that any new visitor for your web page is routed to cloudflare instead of your web page.

As the message in the screen says, this could take up to 24 hours. But this depends as not every region will take the same time.

![Propagation message](/assets/img/CloudflareCTF/CF_step5_propagation.png)

For you to know if this is finished you have 2 options:

- Click on the "Check nameserver now" button
- Validate the records on internet

For this last point, i suggest the web page **[DNS CHECK](https://dnschecker.org/)**. Here you will search for your web page name, and it will show you the IP addresses that it resolves in different countries. All you have to do is to validate that all of them are Cloudflare IP addresses, and not anything else.

This is useful also as troubleshoot for any problematic request during this 24 hours.


![DNS cehcker](/assets/img/CloudflareCTF/DNSChecker.png)

#### Finished process

Once the name server replication starts, Cloudflare will redirect you to the **Overview** page, where you will see a Success message, and will be able to star configuring your web page protections.

![Success](/assets/img/CloudflareCTF/CF_step6_protected.png)

### HTPPS for all the web page

The first requirement to cover once the web page is protected by Cloudflare is to force the HTTPS protocol for all the web page.

For this we need to go to the _SSL/TLS_ submenu in the left-side menu, and then select _Edge Certificates_ option:

<p align="center"><img src="/assets/img/CloudflareCTF/HTTPS_01.png"/></p>

Here we will see a screen with our certificates provided by Cloudflare. And at the buttom of this list, we will see some setting if we scroll down.

Down here we will find the settings for:
- Always Use HTPPS
- Minimum TLS Version
- TLS 1.3
- Automatic HTTPS Rewrites
- etc.

#### Always Use HTPPS

This is the option to cover the first requirement.
With this option cloudflare will redirect all the HTTP request to HTTPS ones. No exception.

![Success](/assets/img/CloudflareCTF/HTTPS_02.png)

#### Minimum TLS Version

To ensure a more complete protection, we will enable Minimum TLS Version to _TLS v1.2_. This is based on the knowledge that [TLS v1.0 and v1.1 are deprecated](https://www.ietf.org/rfc/rfc8996.html), and thus, we cannot trust any request that uses any of these 2 options.

This is additional to [TLS v1.0 is no longer supported on modern web browsers](https://caniuse.com/sr_tls1).

#### TLS 1.3

TLS v1.3 is the most recent and secure version of the protocol. Enable this to make the web site compatible with it.

#### Automatic HTTPS Rewrites

This works as a reinforce of the "Always Use HTTPS". But in difference from it, this option will directly rewrite all the redirections or links inside your website if they have an "_http_", turning them into "_https_".

### Firewall Rules

Cloudflare have a Web Application Firewall (WAF) module that is usefull for us to control the requests that our application recevies in order to filter out the risks or threats.

You can find the Firewall rules under the name of "Security rules" in the section _Security_ and _Security rules_:

<p align="center"><img src="/assets/img/CloudflareCTF/Security_01.png"/></p>

Inside we have two tabs: Security rules, and DDoS protection.

Inside _Security Rules_ tab we will be able to set _custom rules_, and _Rate limiting rules_.
In DDoS protection, we will be able to look if the protection is active and nothing else.

To create a new rule we have to click on "+ Create a rule" button, and select "Custom rule":

<p align="center"><img src="/assets/img/CloudflareCTF/Security_02.png"/></p>

This will open a new screen where we will create the rule.

First name your rule in the _Rule name_ field.

After this is the field "_When incoming request match..._". Here you will create the filters to apply the rules.
Select the _Field_, that is the parameter of the request to filter. 
Then the _Operator_ that indicates how to evaluate the parameter.
And finally the _Value_ that indicates what we are looking for in the parameter.

At the end of thus form you can see "And" and "Or" buttons. This is to anidate more filters into the same rule.

And at the bottom of the screen is the section "_Then take action..._". Here we will define what to do with the traffic that matches our filter.

The options are:
- Managed Challenge: Cloudflare performs a test to validate if the traffic is legit and can be trust.
- Block: Deny all the access to the web page
- JS Challenge: Cloudflare performs a test on the traffic via JS
- Skip: Pass all the rules that are bellow the current one. Like an "Allow" to prevent the traffic from falling into a "block" rule in any lower priority rule.
- Interactive Challenge: Cloudflare request the user to validate its traffic with a captcha (_I'm not a robot_ box)

And lastly, you have to select the order that will have the rule in the firewall rules processing in the field "_Place at_".

#### Block traffic from at least five countries.

To block countries the indications are:

- _Field_: Country
- _Operator_: is in
- _Value_: Here select any desired country

For our example this will return us the Expression Preview:

```
(ip.src.country in {"CN" "IL" "KP" "MN" "PS" "RU" "TW" "UA"}) or (http.request.version eq "HTTP/1.0")
```

You can notice it adds a filter for HTTP/1.0. This was added by us to only use one rule to block traffic.

And we selected the Actions _Block_ and the order _First_ as it is one of the two rules that we have.

<p align="center"><img src="/assets/img/CloudflareCTF/Security_03.png"/></p>

#### Challenge rule

As an extra example, we created a rule to also challenge all the requests that come from non convencional regions. As the web page is deployed in Mexico, our expected public came from North America and South America. So anything out from them is being challenged.

The indicators are:
- _Field_: Continent
- _Operator_: is not in
- _Value_: North America or South America

And was ordered just after the rule from the previous point as it is a complementary rule.

<p align="center"><img src="/assets/img/CloudflareCTF/Security_04.png"/></p>

This rule was created after the first day of having our web page in cloudflare as many traffic from Europe and Asia was detected, most of them performing recognition over the web page with forged requests.

#### Avoid too many requests on a particular part of the website

In the Free plan of Cloudflare, it allows us to create a single _Rate Limit Rule_. This kind of rules are really useful to control the amount of request a user can do in a period of time.

One of the request of this project is to implement a rule like this to _avoid too many requests_.

The format is similar to the _Custom Rule_: Select a name, select a filter, and the action. With the difference that this one includes the Rate of requests for a single period (blocked to 10 seconds in the Free plan). And it includes the duration of the action taken (also blocked to 10 seconds in the Free plan).

![Rate Limit Rule](/assets/img/CloudflareCTF/Security_05.png)

With this we protect ourselves from scripted attacks, and any other kind of automated threat that depends on performing multiple requests in a shor period of time.

# Conclusion

I omited some questions about how I researched some topics, or what were the most difficult parts for me, as it would have been a much more longer text than it already is.

However, I would like to express how great this experience was, and how surprised I am by Cloudflare's ease of use for websites cyberprotection. I even remember with great respect and admiration the [Project Galileo](https://www.cloudflare.com/galileo/) that I learned about during this selection process.
This is a humanitarian aid program that provides paid cloudflare services to organizations working in human rights, civil society or journalism, for free.

Though I dropped out the selection process because of personal preferences, I really learned a lot about the company, their technology and their impact on the Internet, rooting in me a great sense of respect, and obviously, a lot of interest on using them for personal projects.