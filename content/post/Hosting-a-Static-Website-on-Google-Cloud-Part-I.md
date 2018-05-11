---
title: "Hosting a Static Website on Google Cloud: Part I"
date: 2018-03-27T16:05:48-04:00
draft: false
tags: ["google cloud", "cloud storage", "hugo", "CI", "terraform", "cloudflare"]
categories: ["DevOps"]

autoCollapseToc: true
---

After putting it off for some time, I finally decided to revamp my personal website.
I had a few goals in mind when I started, mainly:

- Switch from using Jekyll to Hugo for static site generation
- Move from AWS to GCP for hosting
- Define all the infrastructure as code using Terraform
- Automate the build/deploy process with CI/CD

The result being a highly available, fault tolerant, reproducible website that
is easily deployed and almost free to host. I couldn't find any other examples
of someone doing something similar to this using the same stack,
so I decided to write up the process myself for my own reference as well as
for others.

## Static Site Generation

Static site generators are great for websites that don't have a need for dynamic
content or user input. By doing away with a database and not generating content for
each request you can increase site load times and security while being able to
handle large amounts of traffic with relatively little infrastructure compared to
a dynamic site. For more details, there is a great introduction to static
site generators [here](https://davidwalsh.name/introduction-static-site-generators).

Originally I was using [Jekyll](https://jekyllrb.com/) when I first set up my site. Jekyll is
arguably the most popular static site generator around at the time of writing this and has a large community with a wide
variety of plugins and themes. Despite Jekyll being a popular option, it
seemed to have many features that I really didn't need that made it
significantly more complicated to setup and use. After some searching I came
across [Hugo](https://gohugo.io/), a static site generator written in Go. Hugo seemed like an
attractive alternative to Jekyll as it was simple to setup and deploy, fast,  and was
written in a language I was familiar with.

## Hosting

There are lots of options when it comes to hosting a static website including
free ones like [GitHub Pages](https://pages.github.com/). While this is a great
easy to use option, I wanted something that I had more control over as well as
an excuse to use and learn new platforms.

For a long time I used a method similar to the one described
[here](https://docs.aws.amazon.com/AmazonS3/latest/dev/WebsiteHosting.html) to
host my site on Amazon Web Services (AWS) in an S3 bucket. While this proved to
be a cheap and reliable method, I started to get curious about what options
were out there.

Since I was already familiar with what AWS had to offer, I decided to take
a look to see if Google Cloud Platform (GCP) had any similar offerings. Compared to the flood
of blog posts and guides for setting up a static site hosted with S3, there
was relatively little information on doing the same with GCP. Google itself has
[a nice overview of the process](https://cloud.google.com/storage/docs/hosting-static-website)
that served as a basic guide, but it seemed that for the rest of my goals
(configuration management, infrastructure as code, and HTTPS setup)
I was on my own.

## Infrastructure As Code

The concept of infrastructure as code (IAC) is being adopted by more and more
organizations today, and for good reason. With IAC your hardware is defined in
software, allowing you to take advantage of the best of software development
practices and tooling. Imagine being able to define your entire environment's
hardware stack in a .json file while using version control like
Git for change tracking and releases. This is just one of the great things IAC
has to offer. For a great look at some more benefits, AgileBits has [a great blog
post](https://blog.agilebits.com/2018/01/25/terraforming-1password/) about
using IAC in their recent environment upgrades.

My tool of choice for this will be [Terraform](https://www.terraform.io/),
a great tool by the folks at HashiCorp (as if they make any other kind).
Terrafrom allows you to define your infrastructure as code and deploy to
a variety of cloud environments or *providers* as they are referred to in
Terraform. Getting started with Terraform is beyond the scope of what I am
going to cover here, but there is a great [series of blog
posts](https://blog.gruntwork.io/an-introduction-to-terraform-f17df9c6d180)
written by the folks at Gruntwork that will help get you started.
This series of posts was also the basis for a book by the same author called [Terraform: Up
& Running](https://www.terraformupandrunning.com/?ref=gruntwork-blog-comprehensive-terraform)
which I highly recommend.

## Process Automation

With the plans set for hosting my static site on GCP while using Terraform to define
all of the infrastructure, I needed a reliable and automated way to
makes changes to both the content and the configuration of the site. This is
where CI/CD steps in.

CI/CD stands for continuous integration (CI) and continuous deployment (CD) respectively, and are
common DevOps practices. CI encourages rapid changes merged into the main
codebase as often as possible. These changes are then validated by a suite of
automated builds and tests to validate the changes. With CD, if these changes
are successfully validated by your tests, they are automatically pushed to your production
environment without further manual intervention. As you might see, complete
test coverage of your code is extremely important in this scenario.

Properly implemented, these two practices can lead to fewer bugs, faster
release cycles, and when paired with versioning control such as Git,
change/release tracking for each update made to the codebase.

## Putting It All Together

So far I've outlined what my goals were for this project as well as the tools
I needed, giving a little insight into my decision making process along the way. In
Part II of this series, I will get into more specifics of the implementation as
well as provide code samples for my infrastructure and provide some Terraform modules
to make the process easier. Stay tuned!
