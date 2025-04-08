+++
date = '2025-02-20T23:18:42+13:00'
title = 'The AWS Bankruptcy Triggering Bill Rite of Passage'
tags = ['aws']
+++

This post deals with a common theme in the journey of learning cloud data infrastructure and how it manifested for me while trying to learn AWS. 

When I saw prior examples of people relaying stories of spiralling costs in AWS it came from spinning instances of EC2 but my story of how it happened involved using the more harmless seeming Glue.

# How it happened

It started when trying to do an ETL job that was just complicated enough to not be possible with the visual interface. Here I have to confess some of the error was on my part because I named my tables incorrectly in a hurry so I for some reason thought I needed to unnest, convert data types and resolve choice all in the same job while in reality I only needed to do two of those things.

There are two aspects to this experience that I find disconcerting. One is that this is not an archetypal way to be charged a lot of money unexpectedly while using AWS. It makes me weary that the entirety of AWS is a mine field of potentially large costs and while I can get a waiver this time how many more strikes do I get? Another aspect is that immediately after finding out I had a massive bill owing to AWS I could not get to the bottom of it because AWS makes you wait a full day (24 hours I counted) before you are allowed to set up Cost Explorer. Before I set up that service I only had a vague inkling of what may be causing the problem.

# My Investigation

After setting up cost explorer I got a graph like this

{{< figure12 src="/images/baker.png" alt="Alt text" title="nig" >}}

Eventually after fiddling around with some of the filters I got to a usage cost origin of APS2-GlueInteractiveSession-DPU-Hour which is a term that made no sense to me. After some searching online I found I am not the only one with these problems. One other person has experienced [something similar](https://repost.aws/questions/QU_Ff-QeM7Ta2Bg9TwLd0agw/optimizing-aws-glue-costs-how-to-reduce-expenses-for-interactive-sessions).

It seems by default that AWS gives you an enormous enterprise level of online compute so you are not constrained figuring out how your ETL job effects your stored data interactively. I tested it again in the process of writing about my experience and got the exact same boilerplate when starting a notebook as shown below.

{{< figure13 src="/images/boilerplate.png" alt="Alt text" title="nig" >}}

Going by the guidelines of the linked forum post the idle timeout variable should be set at 20 which is a 99% reduction and the number of workers should instead be 1 which is the minimum.

Which brings me to my point that if you are writing a Glue job using a notebook in AWS you need to know exactly what you are doing beforehand. You can't use this as a way to both execute and learn what to write as code for execution. This is a very foreign concept to me as someone who has never really used cloud computing to learn coding because it just seems like something that should be as free as typing into Visual Studio Code as I write this script. It is also hard for me to guage best practices. Sometimes in AWS I get distracted doing something else within the console. In this case do I terminate the notebook first while switching tabs? Should I also spend as little time as possible writing the script in which case I basically prototype the code in a free environment instead?

I am still not sure what exactly has caused the costs to accumulate so high. Is it merely the amount of time I am spending in the notebook? Or maybe it has something to do with executing code blocks which accumulates cost? Or previewing data which you can do even in the visual interface. I also had no choice but to use the entirety of my data rather than a sample because I needed to make sure the job could deal with any values that might cause errors because they related to different quantities.

# Conclusion 

While it is sometimes portrayed as getting a waiver of several hundred dollars of bills in AWS as a piece of cake the reality for me was far from settled. It involved several weeks back and forth of dealing with customer support. To their credit they were responsive but to say this was a stress free experience is not accurate. It also involves a literal phone call with someone.

{{< figure11 src="/images/baker.png" alt="Alt text" title="nig" >}}
