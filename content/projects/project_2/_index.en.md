---
pre: <b>Project 2. </b>
title: "Open Payments"
weight: 2
summary: "Project 2: Investigate federal Open Payments data to understand how financially intertwined medical companies and practitioners are."
format:
    hugo:
      toc: true
      output-file: "_index.en.md"
      reference-links: true
      code-link: true
---



-   [Overview][]
-   [Project Description][]
    -   [A Data Subset][]
    -   [Words of Warning][]
-   [Technical Details][]
-   [Project Strategy][]
-   [Tips for Success][]

## Overview

![][1]

In 2010, the the Physician Payments Sunshine Act (2010) was passed, requiring medical drug and device manufacturers to disclose payments and other transfers of value made to physicians, non-physician practitioners, and teaching hospitals (known as *covered recipients*). This law was put into place to promote transparency in our medical system - enabling the U.S. government and citizens to monitor for potential medical conflicts of interest.

Today, every time a drug or medical device manufacturer makes a payment to a covered recipient, they must disclose the nature of that payment and the amount to the U.S. Centers for Medicare & Medicaid. Data about payments is then aggregated, reviewed by (and sometimes disputed by) recipients, corrected, and then published as an open government dataset.

Definitions for what counts as a reporting entity, a covered recipient, and a reportable activity have been expanding since the passing of the Physician Payments Sunshine Act as legislators have raised concerns over the degree of transparency of diversifying financial arrangements in the healthcare system. In 2020, the first settlement for violations to the Sunshine Act was announced, requiring Medtronic Inc. to pay \$9.2 million to resolve allegations for failure to report. This served as a signal that enforcement is ramping up in the coming years. In 2022, the state of California passed a law requiring that medical practitioners disclose to patients that this data resource exists.

We will be looking at the [Open Payments][] database of payments to medical professionals for our project 2.

[Click here for the Github Classroom Assignment for Project 2.][]

## Project Description

You will use the [Open Payments][2] data to investigate the patterns of payments to doctors in the United States. Your goal is to explore the dataset, and come away with **one significant pattern** noticeable in the data per team member. Each member will present their finding as a 600 word write-up with accompanying data product(s); these must be presented in such a way that anyone, regardless of their familiarity with data science, could understand them.

If you would like some inspiration for your analyses, you can look through the [Dollars for Docs][] series at ProPublica which used the same data from earlier years; an archive of all the outputs from this project can be [found here][]. If you would simply like to re-create one of the analyses you find there with updated or expanded data you can, but I encourage you to follow your own curiosity.

To better understand the data, [first watch this video on the dataset][]. Next, download and start studying the [data documentation][]. You should pay special attention to the data dictionary for the open payments dataset that spans pages 22-36 as you select variables for your analysis.

### A Data Subset

You can use the following code to read in a subset of the 2021 for Massachusetts. You can choose to use this as the sole data source for your project if you would like; this will limit the questions you can ask. The full data set can be accessed on the [Open Payments Data Explorer][]. Once we cover APIs on 11/16, you could also use the Open Payments API to download data and move into the *Exceeds Standard* proficiency level of *Data Importing* if that is something you want to pursue.

``` r
op_2021_ma <- read.csv("https://raw.githubusercontent.com/sds-192-intro-fall22/sds-192-public-website-quarto/main/website/data/open_payments_ma.csv")
```

### Words of Warning

It's important to note that the unit of observation in this dataset is not one medical practitioner, and it is not one manufacturer. Instead it is one payment from a manufacturer to a medical practitioner. That means that a medical practitioner can appear multiple times in the dataset if they've received multiple payments, and a medical drug or device manufacturer can appear multiple times in the dataset if they've disbursed multiple payments. We can identify medical practitioners with the `covered_recipient_npi` column and manufacturers with the `applicable_manufacturer_or_applicable_gpo_making_payment_id` column.

Because covered recipients' names are manually entered into a database every time a payment is made to them, sometimes the formatting for one payment can differ from how the same covered recipient's name is formatted when entered for another payment. The same goes for other variables related to that practitioner. For example, import the `op_2021_ma` dataframe above, and search for "1003040676" using the search bar in the upper right of the dataframe viewer. The recipient name is formatted several different ways. In some cases, there is a middle initial, while in others, there is a full middle name; in some cases, the practitioner's name is all capitalized, and in other cases it is not.

## Technical Details

You have 2 weeks (until midnight on 11/25) to work on this project, including two full days of class time (11/11 & 11/21). The due date occurs over Thanksgiving recess; you are free to turn it in early, and I am providing extra time in class to work on this project. I strongly encourage you to do the necessary prep work to make the most of the second in-class day. Try to have your minimal viable product finished by that class period, then you can work on taking it to the next level while classmates and myself are available to help.

You have full freedom to make the project as simple or intricate as you desire. Each member of the team must make *at least one* data product, with an associated write-up. This can be a visualization, or some other product like a table, dashboard, or anything else you feel helps tell a story regarding the data set. Regardless of what each member creates, your final team report **must** successfully render. You will include the output in the `docs/` directory of your project.

Your final submission should include the following:

**In your team Github repo `docs/` folder:**

-   A **RENDERED** Quarto portfolio document (any format) containing:
    -   An introduction to your team's overarching theme
    -   All data products you wish to present
    -   The code used to generate those items (in your code chunks set *echo* to true)
    -   An accompanying 600 word write-up for each person's work
    -   A summary of what lessons your team learned about the data

**Through Moodle ([Turn in here][]):**

-   A rendered Quarto pdf (there is a template in the docs folder) explaining your contributions to the project containing:
    -   A 1-2 paragraph summary of how you contributed to your team
    -   A list of each standard (per the course syllabus) you *individually* worked with in this project
    -   A justification for each standard describing what proficiency level you demonstrated **per the text in the [standards matrix][]**.

## Project Strategy

This project has significantly more messy-ness in the data than project 1. This represents a more realistic project workflow; where step one is getting the data into a usable state. Rather than trying to clean a specific thing for your use-case, I recommend working as a team to define *all* the elements you will need to clean as a team, then assigning a cleaning task to each person. If each person cleans one thing, and you compile those steps into a script to clean your data and save a new *clean* version of your data file, now everyone has access to a higher quality data source to work with.

I *highly* recommend you make use of the git project skills we went over in our Advanced git lecture. Work on branches to avoid constant merge conflicts. Work on one task per branch, make a [pull request][] to integrate those changes into main, have someone else review and integrate those changes, then create a new branch for the next task. Be aware that you may still run into merge conflicts, but it is much easier to resolve one when you merge a branch than one every single time you want to push or pull. If you need a refresher on resolving conflicts, The Turing Way has a fairly [short guide][]. You can even resolve a conflict [right on Github][].

Keep the directory structure of your project clean. All of your scripts should live in the `src/` directory, all data should be in `data/`, etc. Try to name script files based on what they do, not who made them. Establish a clear order of scripts; for example script `01_cleaning.r` creates a clean dataset, `02_analyses.r` creates new measures, and then a series of `03_<XX>_PLOT.r` scripts make individual plots.

While your project must center on the [Open Payments][] data, you are not restricted to using only it. Say you want to look at payments per capita. You can combine population data from the [Census Bureau][] to do that. Say you wanted to map the hospitals who take the most money? Combine the payment data with spatial data from the [National Historical Geographic Information Systems (NHGIS)][] data.

This project is looking at *how much money our doctors take from drug companies*. Keep a keen eye out for any ethical concerns in the data. *Do you think this data set is portraying an accurate representation of the truth?* If not, voice your concerns and how they may impact how we interpret the data and your report.

## Tips for Success

1.  Focus on creating a *minimum viable product* first. This means make something simple that satisfies all of the requirements, then go back and expand on what you have. Don't try to create something ultra-fancy as your first milestone.
2.  Start by spending time understanding all of the variables in the provided data. Include a document in your project explaining what all the variables mean.
3.  Each person should work in a separate R script for each table/summary/visualization you want to create. Make combining them into your finalized portfolio a separate step.
4.  If there is a specific standard you want to raise, look for opportunities to do so. Each part of the data will have different challenges that you can overcome to show your growth.
5.  Don't be afraid to branch out! There is no ceiling for this project. If you want to try a new tool or visualization style we haven't covered in class, give it a go. You can explain how you think it shows your proficiency in your Moodle submission.

  [Overview]: #overview
  [Project Description]: #project-description
  [A Data Subset]: #a-data-subset
  [Words of Warning]: #words-of-warning
  [Technical Details]: #technical-details
  [Project Strategy]: #project-strategy
  [Tips for Success]: #tips-for-success
  [1]: img/open-payments-logo-tm.png "Open Payments program logo"
  [Open Payments]: https://www.cms.gov/OpenPayments
  [Click here for the Github Classroom Assignment for Project 2.]: https://moodle.smith.edu/mod/url/view.php?id=963007
  [2]: https://openpaymentsdata.cms.gov/
  [Dollars for Docs]: https://projects.propublica.org/docdollars/
  [found here]: https://www.propublica.org/series/dollars-for-docs
  [first watch this video on the dataset]: https://www.youtube.com/watch?v=47IAP4SSVso
  [data documentation]: https://www.cms.gov/OpenPayments/Downloads/OpenPaymentsDataDictionary.pdf
  [Open Payments Data Explorer]: https://openpaymentsdata.cms.gov/datasets
  [Turn in here]: https://moodle.smith.edu/mod/assign/view.php?id=963014
  [standards matrix]: https://intro-to-data-science-template.github.io/intro_to_data_science_reader/syllabus/#standards
  [pull request]: https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/proposing-changes-to-your-work-with-pull-requests/creating-a-pull-request
  [short guide]: https://the-turing-way.netlify.app/reproducible-research/vcs/vcs-git-merge.html#merge-conflicts
  [right on Github]: https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/addressing-merge-conflicts/resolving-a-merge-conflict-on-github
  [Census Bureau]: https://data.census.gov/cedsci/
  [National Historical Geographic Information Systems (NHGIS)]: https://www.nhgis.org/
