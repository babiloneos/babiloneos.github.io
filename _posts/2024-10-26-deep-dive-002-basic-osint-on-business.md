---
layout: post
title:  "Deep Dive 002 - Basic OSINT report on business"
date:   2024-10-26 13:39:00 -0600
tags: OSINT DeepDiveBook
author: Guillermo Ballesteros
---

We are in chapter **2.3 Collection Phase**.
Here we try to understand in a superfluous way the step of collecting data, and pivoting (yes, as in basketball).

## Planning and Requirements

The book is really clear on what we are looking for:

Pick any company for this exercise and try to answer the following questions that could help you write an OSINT report:

**1. Can you determine the company's email naming structure such as _firstname.lastname@email.com_ or _firstinitial.lastname@email.com_?**

**2. Using the naming structure from the last question, can you use a search engine to locate additional company emails?**

**3. What other information can you find on social media sites like LinkedIn about this company and the employees?**

Unfortuantelly this instructions leave us unarmed against an interesting challenge, as we still haven't learn any technique, or have used any tool that could help us with this task.

1. **What data sources will be used?** Unknown.
2. **What kind of data are we expecting to find?** First, email addresses, and secondly any additional information about the company and the employees.
3. **Is there any legal issue or sensitivities to be aware of?** Yes, as email addresses are considered PII, so we will have to use meassures to protect the company we are looking for. 

Well, with this said, let's go!

## First Business: Mexican candy company

### Domain

For my first attempt on this task, I will take a shoot with a Mexican candies company that we will identify by its first and last letter **D...O**.

As first step we need to find the **domain** they use for their mail services, and any example. For the domain it was easy as it is exposed in their official website, contact page:

![Candies contact page](/assets/img/DeepDive002/candies001.png)

### Email addresses

With this information my first ideas was to do a quick google search putting the mail domain in semmicolons. The first page didn't have any new information:

![Candies google search](/assets/img/DeepDive002/candies002.png)

But going through the result pages I found a couple of pages that displayed some email addresses:

![Candies finding 01](/assets/img/DeepDive002/candies004.png)

![Candies finding 02](/assets/img/DeepDive002/candies005.png)

![Candies finding 03](/assets/img/DeepDive002/candies006.png)

Unfortunatelly, only one of these addresses is from an employee, and it doesn't follows any structure.

```
FINANZAS@D...O.COM
recepcion@d...o.com
luzma@d...o.com
d...o@gmail.com
hola@d...o.com
```

At this point, what else I could do? Facing this as an amateur, my brains is telling me "maybe there is some kind of tool that could help search email addresses based on a given domain. And it makes sense for me, so I start looking for one in google.

Good news, I found a loot of pages that does that. Bad news, most of them have a pay-wall, or at least a login-wall, and I'm not interested in paying for this, so I will have to make this work with free ones even if they have a login-wall.

One of them is [Snov.io](https://snov.io/). This is a webpage that is focused for Sales department to find and contact people in an easies way. This tool requires a login in order to see the full results, so I had to creat a proxy account.
But it worked, and I was able to obtain new email addresses:

![Snov findings](/assets/img/DeepDive002/candies007.png)

As you can see, some of them were already part of our list. So in total we got 5 new email addresses.

```
calidad@d...o.com
martinh@d...o.com
alexg@d...o.com
sergior@d...o.com
lalo@d...o.com
```

Another tool that I found is [Rocket Reach](). A webpage focused on find and verify contact information about people. It has a login-wall, and free-account limits, but its OK for now.

We started looking for the domain _d...o.com_:

![RocketReach findings](/assets/img/DeepDive002/candies008.png)

Here I found only one result with an email address. All other doesn't really have their email address available, or at least that is what the tool says.

I also tried looking for the full name of the company, and even though we had more results, none of the visible ones (pay-wall only shows first page in results) have contact info.

Ok, so for now we have a great total of 11 email addresses from this candies company. BUT one of them is a gmail address, and for me that speak the language that this company uses, I can identify that 4 of them are service email addresses. So actually we only have 6 email addresses from people inside the company.

From those 6, I see that two addresses does not have a format, but a nickname: _Lalo_ is the diminutive that is used in México for people called Eduardo, and _Luzma_ is for people called Luz María.

Other than that I can identify a structure of "_preffered name_" + "_first letter of last name_"@d...o.com in 5 of them, but the one we found in RocketReach have a different streuctur _first name_._preffered last name_@d...o.com:

```
martinh@d...o.com           - Martin H
alexg@d...o.com             - Alex (Alejandro) G
sergior@d...o.com           - Sergio R
lalo@d...o.com              - Lalo (Eduardo)
luzma@d...o.com             - Luzma (Luz María)
osmar.carrillo@d...o.com    - Osmar Carrillo
```

But neither of them fully fits if we look at the 2 adresses with a nickname as user. Looks like this company doesn't really have a strict username policy for their mail addresses.

So Let's stop and do a quick review based on exercise questions:

> 1 **Can you determine the company's email naming structure such as _firstname.lastname@d...o.com_ or _firstinitial.lastname@d...o.com_?**
>
> I suspect that they don't really have a policy for username structure on their email addresses. Only sure thing is the domain that they are using for their mail service, being @d...o.com
> 
> 2 **Using the naming structure from the last question, can you use a search engine to locate additional company emails?**
> 
> This may help us on what to do next, as we could try using Google Dorking, but I don't think it would be that helpful as the search text would be _*@d...o.com_ or _*.*d...o.com" which doesn't return any particular new result. So our job with Snov and RocketReach would be our "search engine" result.
> 
> 3 **What other information can you find on social media sites like LinkedIn about this company and the employees?**
>
> This do is a possible next step, but question is kinda ambiguous as it could go from its story, until its catalog or physical addresses. Lets go deeper on this.

### Social Media

So, our first destination will be **LinkedIn**, where we see that the company does not have an official profile, and instead we have a unclaimed one.

![LinkedIn Company](/assets/img/DeepDive002/candies009.png)

This is unfortunate, as we won't get much more information from here, other than the profile from people that has, or is working in the company.

We found technical, and HR profiles, such as Quality Inspectors, Accountants, e-commers analysts, etc. Between all these profiles I found two very interesting ones.

The HR deppartment leader:

![LinkedIn HR Lead](/assets/img/DeepDive002/candies010.png)


And the most interesting one, a person profile thas is used as the company profile:

![LinkedIn Company Profile](/assets/img/DeepDive002/candies011.png)

This says me that the company does not have a deep usage of technologies, or at least they don't give that much importance to media profiles. Because the profile is really simple, does not have any post, and their description mentions a certification obtained in 2020, so maybe they haven't touch it in 4 years.

Next one will be **Facebook**. Here we found an official profile, that is actually used, and with updated information. Here we also find one of the mail addresses that we previously had. We also obtained their official physical address, located in Mexico City, and a contact phone.

![Facebook Company Profile](/assets/img/DeepDive002/candies012.png)

Lastly I tried with **Instagram**. I also found an official profile, that is frequently used and updated. Only interesting information is that it has in their Bio two links, both already foun during our research (first one is where we got the email domain).

![Instagram Company Profile](/assets/img/DeepDive002/candies013.png)

## Conclusion

So, that's it! 

This was a very interesting, and demanding task where I learned a lot about tools that I could use to find information.

One very interesting lesson was related to pay-walls and login-walls. I noticed that I will need to registry on a loot of different web sites, so I decided to create a new mail address to link it into all these new websites. I think it is a needed resource when getting into the OSINT world.

Also a password generator/manager will be really useful to not reuse the same password, and avoid forgetting them.

This combo of specific email address, and password manager saved me a loot of time during this process.

See you next time :)