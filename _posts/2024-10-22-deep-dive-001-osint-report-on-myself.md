---
layout: post
title:  "Deep Dive 001 - OSINT report on myself"
date:   2024-10-22 19:25:21 -0600
tags: OSINT DeepDiveBook Myself
author: Guillermo Ballesteros
---

We are in chapter **2.3 Collection Phase**.
Here we try to understand in a superfluous way the step of collecting data, and pivoting (yes, as in basketball).

For Pivoting, we are following Amy E. Herman, who said "Learning to see what matters can change your world". She suggest to always look at things twice to gain a full understanding of it. An for that she have a method:

1. Look first
2. Consult other preexisting information or options
3. Look again

Also she developed the COBRA method in order to have a better guide on pivoting.

| The COBRA method |
| :---- | :---- |
| C | **C**oncentrate on the camouflaged |
| O | **O**ne thing at a time |
| B | Take a **B**reak. |
| R | **R**ealign your expectations |
| A | **A**sk someone else to look with you |

In order to better understand this, and practice it, the book suggest a couple of exercises.

This post will follow my process for the first one: **Develop a report on a subject (you)**.

Now, we still don't have a well defined structure for investigations, as that is the objective for this chapter, so we will procees with the process that we find easier, and we will structure it as well we can.

## Planning and Requirements

The book is really clear on what we are looking for:

> 1. Using just your personal email address and a search engine, how much information are you able to find on yourself?
> 
> 2. Did you find any of your personal usernames or social media accounts?
> 
> 3. Could you find any official records such as voting information or houses purchases? What did these records reveal about you, and how could that information be used?

This is defining our main questions to star the Intelligence Cycle:

1. **What data sources will be used?** Only search engines.
2. **What kind of data are we expecting to find?** All kind, but specially social media accounts, and maybe Personally Identifiable Information (PII).
3. **Is there any legal issue or sensitivities to be aware of?** Based on the excercise indications, there may be some voting information, house purchases information, and as mentioned, Personally Identifiable Information (PII).

Well, with this said, let's go!

## First search engine: Google

We will start with the most popular search engine, Google.

Fortunatelly for me it was a really short list of findings, however, I see that my personal mail address is completelly exposed.

![Google Results](/assets/img/DeepDive001/googleResult.png)

Here we can see google finded:

1. **My github page**. This one didn't surprised me, as this blog is usually really related to my personal mail address. And of course, it includes my nickname/username.
2. **A Github Repository where I put my email address**. This one did surprised me as I didn't remember I put my email address on there. Of corse I have already take it down.
3. **An old post of my previos github page**. It surprised me that this old version is still recorded on google, but it is no longer online, so nothing to worry about.

And that's all. All other result are not related at all with myself.
But Google still have the Images search engine, and those results do have some related results.

![Google Images Results](/assets/img/DeepDive001/googleImagesResult.png)

Here we found some images. Two related to my github repositories. But **one with my face**.
This image, based on details, was obtained from my previous github page, wich is not online anymore.
Again, it is surprising that is still recorded on google, but nothing to do there considering that you can find my face as well on LinkedIn.

## Second search engine: Bing

For the Microsoft search engine, we get really bad results except for one:

![Bing Results](/assets/img/DeepDive001/bingResult.png)

As you can see the search engine focused on the "gmail.com" from my search input. But, it did found my Twitter (now called _X_) account. I didn't expect to see this kind of results.

One more thing. Bing also have an Image search engine. Let's look at it.

![Bing Image Results](/assets/img/DeepDive001/bingImageResult.png)

Such a surprise! The same image we found in google that was stored in my previous github page.
Looks like bing have that offline web paghe recorded as well.

## Any other interesting finding?

Unfortunatelly, no.

I tried the same with many other search engines: Brave, DuckDuckGo, Yahoo.com. Even Archive and GPT.
But none of them actually show any interesting result other than my eMail provider links.

## Conclusions

Let's answer the questions from the book.

1. **Using just your personal email address and a search engine, how much information are you able to find on yourself?** Not much, actually. But it did show me information that could guide me to much more information. Just with my nickname I could route this investigation into something much more interesting at Social Media level.
2. **Did you find any of your personal usernames or social media accounts?** Yes, I find one of my personal usernames, and one social media account (twitter/X).
3. **Could you find any official records such as voting information or houses purchases? What did these records reveal about you, and how could that information be used?** Not actually, and this is something that I was expecting.

I'd like to elaborate further on the last question, as it's a scenario I expect to see many times throughout this book. Fortunately (or unfortunately) I live in a country that doesn't expose so much information about citizens based on their ID documents. I understand that the United States has a large number of entities that could expose user information based on their email addresses or social security number, and so I understand that some exercises will depend on that situation. But all of those won't work for me.

I've found government information based on my full name, and that's something that's expected in the country I live in, as all citizens have two last names and as many first names as their parents want. So for some of us it's a very specific search, as we're unlikely to share a full name with another person.

So yes, that was the quick excercise about a very basic OSINT report about myself.

## Findings:

- Picture of myself
- Github repository with my email in readme
- Github page link
- Previous Github page post
- Twitter/X account
- Username "babiloneos"