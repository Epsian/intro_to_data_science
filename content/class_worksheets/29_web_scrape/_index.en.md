---
pre: <b>11/14. </b>
title: "Web Scraping"
weight: 29
summary: "Harvest the internet"
format:
    hugo:
      toc: true
      output-file: "_index.en.md"
      reference-links: true
      code-link: true
      
---



-   [Overview][]
-   [A Legal Refresher (not legal advice)][]
-   [The Target][]
-   [Checking the Rules][]
-   [Figuring out the Structure][]
-   [Building our Bot][]
    -   [Names][]
    -   [Positions][]
    -   [Links][]
-   [The Next Level][]

## Overview

Web scraping is a handy skill to have if you want to create a dataset of your own. Today we'll practice scraping some information about the SDS faculty from the Smith website. The same principles can be applied to any simple website (those without interactive elements). Just remember that you must be very careful when constructing scraper bots to make sure you obey the terms of service for the site you are scraping, and that the bots you build are polite.

{{% notice warning %}}
**Failure to build polite bots can result in your (or the school's) IP address being banned from a website forever.**
{{% /notice %}}

## A Legal Refresher (not legal advice)

Remember from our lecture that the legalities of web scraping are in a grey area. The legalities depend on several factors including:

-   The kind of data you are trying to get
-   How you are getting and saving it
-   What you plan to do with the data once you have it

In general, you should *never* scrape:

-   Anything under copywrite
-   Anything about private people
-   Anything you need to log-in to see

## The Target

Rather than working with a dataset today, you will be making one. Our goal is to start with the [Statistical & Data Sciences][] webpage, and end up with a dataframe containing the name, title, and URL for all the Smith SDS faculty.

## Checking the Rules

Before we start writing any code, we need to make sure we are *allowed* to scrape the website. A good first check is the `robots.txt` of the website. For Smith's site, that would be <https://www.smith.edu/robots.txt>. On this page we are given a map of where we are allowed and not allowed to scrape. Anything page listed with "Disallow" is off limits, as is anything under that directory. For example, if `www.site.com/page1` is off limits, so is `www.page.com/page1/sub_page`. Seems the parent directory of the SDS page, `www.smith.edu/academics`, is in the clear.

Once that quick check is passed, we have to do the harder work of reading and understanding the website Terms of Service (ToS); [you can find Smith's here][]. Yes, you actually need to read it. You are actually bound by these terms *just by looking at the site*, so it is worth the time.

The first thing you may notice is under section 4, that we are authorized to save pages from the site onto one *single* hard drive. That conversely means we are not able to save multiple copies, or save it anywhere that will be shared online. You may also notice that section 7.2 mentions scraping, in that we cannot use it to access parts of the site not made publicly available. What we want *is* publicly available, so we should be in the clear.

<div class="question">

What other things do you find interesting regarding the Terms of Service for the Smith website?

</div>

## Figuring out the Structure

Now that we know what we are and are not allowed to do, let's go look at the [SDS page][Statistical & Data Sciences]. Our goal here is to figure out what we need to get from this page to progress us closer to our goal. We know we want to look at each individual faculty box, so what could we do to get a link to all of them?

We *could* just go to each faculty member, copy the data into an excel file, and proceed from there. That would work for the 15 items we want here, but what if we wanted 150? What if we wanted to re-use our code for another department? Best to write a programmatic solution.

Most of the information we want is clearly visible, but the link to each faculty page is not. We know that clicking on each faculty portrait will take us to their page, so there is a link in there somewhere, we just need to get it out. We'll use `SelectorGadget` to help with that. First, we'll need to add it to our bookmarks bar. Right click on the bookmarks bar in your browser of choice, and make a new bookmark called "SelectorGadget", and set the URL to the following:

    javascript:(function(){var%20s=document.createElement('div');s.innerHTML='Loading...';s.style.color='black';s.style.padding='20px';s.style.position='fixed';s.style.zIndex='9999';s.style.fontSize='3.0em';s.style.border='2px%20solid%20black';s.style.right='40px';s.style.top='40px';s.setAttribute('class','selector_gadget_loading');s.style.background='white';document.body.appendChild(s);s=document.createElement('script');s.setAttribute('type','text/javascript');s.setAttribute('src','https://dv0akt2986vzh.cloudfront.net/unstable/lib/selectorgadget.js');document.body.appendChild(s);})();

That looks like gibberish to us, but the computer will get it.

Once that is done, save the bookmark, and get back to the SDS page. While you are on the page, click on the "SelectorGadget" bookmark in your bar. A grey and white bar will appear in the lower right of your browser, and orange boxes will start to appear wherever you have your mouse. Click on the thing you want to find, in this case the name of a faculty member; it doesn't matter which one, just make sure the highlight box is *only* around the name. If you did it right, the name you clicked on will highlight green, and the rest will highlight yellow like the following.

![][1]

We're not done yet though. Look around and make sure we haven't included anything extra.

<div class="question">

Look over the page and make sure *only* the names are highlighted. If anything else is highlighted, click on it to turn it red and exclude it.

</div>

<div class="answer">

The Program Committee header needs to be excluded.

</div>

Look at the `SelectorGadget` grey box in the lower right and you will see a short string starting with `.fac-inset`, that's the HTML section for our names and links. Copy the whole string and keep it somewhere handy.

## Building our Bot

### Names

The first step of our scraping is to find the names for all the faculty. The dataframe we create will also be used to store the rest of our data later.

Start by loading `rvest`, and scraping the whole SDS page into R. This can be done with the `read_html()` function, much like reading a CSV. Save the webpage to an object called `sds_home`. It is important to note *this is the step* that can get us in trouble. Once we have the page in R, we are working with it like anything else on our computer. But the process of reading the page from the internet can cause problems if we do so too fast. From the `robots.txt` page, we know Smith only wants us to pull **one** page every **10 seconds**. If we do any more, they can ban us from the website. <span class="red">Be careful!</span>

<div class="question">

Scrape the SDS home page into R and store it in an object called `sds_home`.

</div>

<div class="answer">

library(rvest)

sds_page = read_html('https://www.smith.edu/academics/statistics')

</div>

Once we have the whole page, we can start pulling information from it. The usual workflow here is to tell R what HTML structure we are interested in, and then what we want from it. For example, if we are interested in the names of faculty, do we want the names themselves, or the links to other pages within them?

Let's get the names themselves first. To do that, we need to say "from this page, look at this structure, and take the actual text of it." In R, that corresponds to the `html_elements()` and `html_text2()` functions. Give our `sds_home` object to `html_elements()`, and as an argument, specify that `css =` the string we got from `SelectorGadget` earlier. Either pipe the results from that, or wrap it in the `html_text2()` function to get the actual names of the faculty.

{{% notice note %}}
There is *also* a `html_element()` function (singular, not plural). This will only give you the *first* thing.
{{% /notice %}}

<div class="question">

Create a dataframe called `sds_faculty` with a column called `name` for all the SDS faculty names.

</div>

<div class="answer">

sds_faculty = data.frame('name' = html_text2(html_elements(sds_page, '.fac-inset h3')))

</div>

### Positions

Next we want to get the titles for all the faculty. The process is exactly the same as the above, but we need to set a different target using `SelectorGadget`

<div class="question">

Using the same process as before, add the title of each faculty member to our `sds_faculty` dataframe into a new column called `title`.

</div>

<div class="answer">

sds_faculty\$title = html_text2(html_elements(sds_page, '.fac-inset p'))

</div>

### Links

This last one will be a bit different. Rather than wanting the actual text on the page, we want the link that the text is tied to; i.e. when we click on a faculty name, it follows a link to their individual pages. Rather than using `html_text2()`, we will use the more general `html_attr()` function. This lets us have more control to tell R that we want the link the text is representing, not the text itself.

In HTML speech, the page a link points to is designated by the `href`, or `Hypertext Reference`. We need to tell R that is what we want. We can do that by passing "href" to the `name` argument of `html_attr()`; "href" is the name of the attribute we want to get.

<div class="question">

Using `html_elements()` and `html_attr()`, get the links from the faculty names and add them to our dataframe in a column called `link`.

</div>

<div class="answer">

sds_faculty\$link = html_attr(html_elements(sds_page, '.linkopacity'), name = 'href')

</div>

## The Next Level

You may be thinking "that's kind of neat, but it doesn't tell me anything I can't see with my own eyes." You'd be right. However, in a typical web scraping process, this is only step one. We now have a column of all the links for each of the individual faculty pages. If we were to write code that iterates over those links, we could then get more specific info from each faculty member. We could add things like email, office location, educational history, etc. to our dataframe. Once we figure that out, we could also iterate over all of the departments at smith, and before you know it, we have a full blown database on our hands.

With this sort of power, you must be very careful. Be sure to build polite bots that obey the website rules, especially with how fast they iterate through pages. Always use the `Sys.sleep()` function to give your bot a break between each page. The Smith site specifically asks that you wait 10 second between each page, so your code should include a `Sys.sleep(10)` inside each iteration.

  [Overview]: #overview
  [A Legal Refresher (not legal advice)]: #a-legal-refresher-not-legal-advice
  [The Target]: #the-target
  [Checking the Rules]: #checking-the-rules
  [Figuring out the Structure]: #figuring-out-the-structure
  [Building our Bot]: #building-our-bot
  [Names]: #names
  [Positions]: #positions
  [Links]: #links
  [The Next Level]: #the-next-level
  [Statistical & Data Sciences]: https://www.smith.edu/academics/statistics
  [you can find Smith's here]: https://www.smith.edu/about-smith/terms-of-use
  [1]: img/selector.png
