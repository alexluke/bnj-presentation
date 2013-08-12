# The BNJ Garden

Today I'm going to talk about the tools that we've developed in-house for
building websites and emails. These make up the "BNJ garden" because they're
all named after flowers. We've had a history of bad naming decisions in the
paste, such as Spaminator, Email-o-Tron and Asset Face, so we decided to pick
generic names with a theme.

First I'm going to go through a little bit of history.

# Marketing Automation Platform

Marketing automation is a relatively new term, but we've actually been doing
exactly that here at BNJ for a long time.

## History

In 1997 (long before my time here) we built the first version of what we called
the WebProfiler. This was a framework that allowed us to build landing pages
and track user activity. It was very much a developer tool, and didn't really
have the automation part of a MAP.

Many revisions and updates were made to the platform over the years, but in
2008 we started work on something completely different. MyRegistrationForm was
to be a SaaS offering that allowed anyone to create landing pages and forms and
track user activity. Users could select a template and drop in copy and images.

The product never actually launched, but we ended up combining the front-end
landing page builder with our WebProfiler system to create the Response
Management System. The RMS was great for building sites that had a simple
landing page to registration form to thanks page flow, which was most of the
sites we were doing at the time. For super customized sites it was hard to get
around the strict templating that was in place. This is the same problem we're
facing now with Marketo. We're 3 or 4 years ahead of the curve.

Then in 2010 we decided to rebuild our entire infrastructure and started on the
Email-o-tron. This was later renamed to Magnolia for the reasons I mentioned
earlier.

## Email-o-tron

This is what the Email-o-tron looked like originally.

# So what are they?

So that's some of the history of our platforms here at BNJ. Now I'm going to go
into the individual components and explain the parts that make up our current
platform.

## Diagram

Thistle is the tracking database, and is at the center of the whole thing.
It feeds and receives data from Magnolia, which is the email and site building
tool. Everlasting is the reporting dashboard, which pulls the tracking data
out of Thistle and runs reports on it. Dandelion is our nurture stream engine.

# Magnolia

Magnolia is really the face of the whole system. It's what we use to create all
the sites and emails that are built on the platform. It recently got a
facelift.

## Screenshot

    [Demo: Drill down to Winback Nurture Email]

Here's a drill down view into a specific campaign. Magnolia allows you to set up
multiple copy or design versions of a page or email.

    [Demo: Click into Nurture 1 Body block]

Here you can see that there's a default version and a Financial version of this
block.

    [Demo: Click back to Nurture 1 and into the Financial Copy Deck]

We can also look at the whole email for a specific version the Copy Deck view.
Here's all the blocks for this version, and you can see that some of them are
using the defaults, and some are overridden.

    [Demo: Preview HTML]

And here's a rendered preview of this email.

    [Demo: Close preview]

You can also set these blocks to render from markdown or textile, which are
alternative markup languages that instead of can make it easier for
non-developers to edit.

## Features

Magnolia features fully customizable PURLs. There absolutely no technical
restrictions as long as they're unique within a campaign, but some PURLs are
better than others. Hopefully Ben's PURL presentation has enlightened you.

There also aren't any restrictions on the sort of HTML you can use. At the end
of the day these pages end up as regular HTML when they're rendered. So we can
use responsive design, web fonts, even the glorious blink tag if you really
want.

# Thistle

Thistle is the tracking database that stores contact and response data for our
campaigns. It also handles distribution of leads.

## Screenshot

As you can see, it's very pretty. Thistle is the database schema and set of
APIs that interact with the data.

## Features

Thistle tracks data at the individual contact, touch, and response levels.
Touches are outbound marketing that we do, either an email, a DM piece or a
media campaign. A response happens when someone responds to one of those
touches.

The Thistle database can track every event someone does on a page or email,
even if we aren't showing it on the dashboard. We can go in and pull reports
as needed. If anyone is interesting in the other types of data we have, please
let us know.

Leads can be distributed out of Thistle in any format and on any schedule. Most
commonly we send out a daily spreadsheet during a campaign, but we can also
send an email to a specific sales rep as soon as someone responds.

We can also intergrate into any system that can accept them. We've integrated
with Marketo, Eloqua, Salesforce, Siebel, Aprimo, Webex and I'm sure many more.

## Recent Features

Here's some features that we've rolled out recently that we should start
leveraging on new campaigns.

Previously we didn't store a specific event for sending an email. Our impression
counts were based off of how many people we were sending to with the bounces
removed. With our new email vendor, we're now storing when an email actually
gets sent, so we have more data to work with. Now we can factor in last chance
suppressions and anything else that causes an email to not be sent.

We're now also storing unsubscribes down to the touch that they happened on.
This allows us to determine unsubscribe rates by the touch and see if there's
something about a specific email that's driving more unsubscribes than usual.

The final new features is better handling of direct mail events, specficially
returns. Historically Thistle has been a very email based system, but this
change will allow us to have been insight into the performance of direct mail
campaigns.

# Dandelion

Dandelion is our nurture stream engine. It handles sending a series of emails
on a set schedule.

## Features

Nurture streams can be triggered based on any action on a landing page or an
email, such as visiting a landing page, downloading a whitepaper or filling out
a registration form.

Emails are built in Magnolia, and Dandelion handles scheduling them to be sent
out based on the defined schedule. It will also handle moving contacts between
streams as the business rules require.

# Everlasting

Everlasting is the reporting dashboard which presents a high level view of a
campaign's performance.

## Screenshot

Everlasting is meant to be a high level dashboard, not a deep introspection of
data. However it does allow drill down to company metrics when used on a
targeted accounts campaign. Reports are regenerated once per hour from the data
available in Thistle.

I want to stress that there is a ton more data in Thistle that doesn't get
shown on the dashboard, so let us know if there is something specific you're
looking for on a campaign.

# Fully distributed architecture

This system is based on a fully distributed architecure. A failure in any
component doesn't affect any other part of the system.

## Diagram

If any one of these components fails for any reason (which never happens
because we're such good developers), or the communication between any of them
is broken (which also never happens because Alex keeps everything running
smoothly), but if something does go wrong, all the other components are
unaffected.

If for example Thistle goes down, Everlasting will continue to show the last
data it was able to get. The sites in Magnolia will keep running and queuing up
their response data.

# So why shouldn't we just use Marketo?

So why shouldn't we just use a MAP like Marketo or Eloqua? I want to first
point out that we're a technology and platform agnostic agency. Sure we have
our preferences, but in the end we're very flexible. We need to pick the right
tool for the job.

Sometimes it may make sense to use a commercial marketing automation platform.
However our stack is generally easier to get set up for a quick campaign, and
then maybe we can switch to Marketo once the client gets everything together.
We can easily integrate our stack into Marketo, and we do that on a lot of our
targeted accounts campaigns.

Marketo and Eloqua do fill their niche, as long as you use them as they're
intended. But sometimes once you get too complicated, they become limiting.
Some projects fit very well into the MAP mold, but others have a larger
creative vision that would be reduced if you're forced to strictly use a MAP.
Again, our tools integrate very well with other systems.

## Comparison

Here's a quick comparision of some of the features.

# Questions

Any questions?


