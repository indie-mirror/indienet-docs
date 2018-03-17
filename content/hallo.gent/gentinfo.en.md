---
title: "Gent Info"
weight: 10
---

Notes from meetings on 2018/01/10-11.

## Introduction to Gent Info

Gent Info consists of phone, mail, and chat channels between the city and the citizens. They provide general city information, as well as information on tourism, sports, library, public services, mobility (there is a new plan for sick relations that has been getting a lot of calls), facility management, and cultural events.

In 2017, there were 150,000 contacts as a part of Gent Info. Generally, Gent Info has answers to 95% of the questions they are asked. All of these contacts are logged in the CRM (Citizen Relationship Manager), and are then searchable for the future.

The office we visited is “front office” where there are fifteen people taking calls. In the “mid office” there are eight people working with city contacts to ensure the information is correct. There are seventy connections in the city administration who help ensure the city information is up to date.

Only the hotline for the night shelter is toll free, the rest of the calls made to Gent Info are charged at a local rate.

Most contact comes via the phone, however chat has become increasingly popular. Occasionally Gent Info still receives handwritten letters. There is face-to-face contact between Gent Info and citizens through the reception desks that are found around the city. There are also mobile service centres (buses/trucks) that cover different city areas.

The exact demographics are unclear/unrecorded, but older people seem to use calls as their most frequent contact point with Gent Info.

### How Gent Info works

Gent Info has a huge database of information, usually one page per topic, which makes up ~1400 pages/topics in the form of FAQs (Frequently Asked Questions.) Their opening hours are 8am to 7pm, including Saturdays and some holidays, even when other city departments are closed.

#### Portal

The Gent Info portal contains all the information and tools used by Gent Info. All of the applications the Gent Info team uses can be accessed via this portal. Team members can also create pages with information themselves.

The portal is a Drupal 7 site, which does not come with an API (application programming interface) but can have an API added at a later date if needed.

Separate from the portal, there is a document database that is built in Sharepoint. The database is based on answers to real questions, and has been developed with Gent Info’s fifteen years of experience, so it contains very valuable information. The document database is well-organised with one page per topic, full text search (ranked in “Google Style”), and also shows the number of clicks per topic, enabling the team to see more popular topics emerging.

The information in the database is not shared with the public Gent Info site or the private portal (and there are no links between them), however this is a problem the city is working on. Each page has a section with “internal” non-public information such as the phone numbers of key contacts inside the city departments, but otherwise all of the information would be beneficial if shared on the public website.

Microsoft Dynamics is used as a workflow manager to manage cases between the front and mid offices.

#### Chat

When a citizen starts a chat via the chat interface, the chat appears in the Gent Info software as soon as they start chatting. There are seven people that can then assign themselves to that conversation through responding to the chat via the software.

Gent Info shares a few licenses for the chat software, so the representatives find it tricky to use the software with their own names. Instead they are named as Gent Info \#1, Gent Info \#2 etc.

After-hours, the chat is still active, but citizens trying to initiate a conversation will be notified that there is no person to respond to them, and so their message will be transferred to email. When this happens, people are not redirected to a site or other location where they could look up that information for themselves.

Citizens interacting with Gent info and more frequently choosing electronic and non-direct communication because it is low-effort. However, the citizen will often send a quick question/message that does not supply enough information for the Gent Info representative to be able to provide a helpful answer. Citizens also frequently leave/close chat as soon as they’ve received their desired information without any further feedback or communication. This leaves the representative unsure if their answer helped, and can also find it a little rude!

### Gent Info + Indienet

We were looking at a potential use case for Indienet, where it could be used as another interface for Gent Info, listening for new information in particular/chosen areas. However Gent Info also handles transactions, such as registering people for services including sports camps, and shelters for the homeless, as well as simpler transactions such as registering complaints. There are around fifty applications for these types of transactions.

We decided we needed to get a better overview of Gent Info from their offices, and see what they are doing for ourselves. (And we did so on 2017/01/11.)

#### Other use case ideas

- **Public announcements/alerts**. Gent Info is not able to push out announcement/alert information to citizens. (This is a political restriction.) However, the team at Gent Info are available to respond/post on behalf of the city on social media, so they already produce publicly-broadcast information.

- **Reporting app**. The city has an app where citizens can report issues such as rubbish that needs collecting, broken pavements, and noise. It was built by Digipolis. When the app is opened, it records the location of the person using it (or you can specify a location on the map.) They then go through multiple steps selecting the category of issue (e.g road) and what the item/problem is (e.g. dumped electrical goods, paper etc.) Then the person can add a photo and/or description of the problem. Often people using the app just use it to send a photo of the issue. The app then forwards this information to the relevant cleaning/maintenance department. The app is fairly new, having been soft-launched to city employees in July, and only announced in the press in September. It is being built so it can be extended for further applications and notifications. Tina (who showed the app to us) explained that she finds it very easy and convenient to use, however there is no feedback on when an issue has been resolved, and no reference number to use to find out further information.

- **Fietsbult**. Roel showed us [Fietsbult](https://fietsbult.wordpress.com), a blog where a cycling collective in Gent share their issues with city cycling infrastructure. People often report particular issues shared on Fietsbult to Gent Info. Citizens in Gent also start Facebook and Whatsapp groups to share city issues, but Fietsbult was particularly interesting to us because the collective had created a website especially for these issues, rather than using social media. This shows a requirement for more permanent/less temporal content.

### Further notes from the meeting on 2018/01/10.

After Aral presented our current aims for iGent, there were a few questions. Dieter asked about the costs involved with hosting and having SSL (Secure Sockets Layer) certificates for every domain for every citizen in Gent. Karl-Filip and Aral explained that we are having a meeting with the domain provider this week, and will discuss hosting with them then. We are also aiming to have multiple providers to give us options. Karl-Filip also mentioned that this should be a cost to the city.

Dieter also expressed a concern that the management of the domains could be a tricky area, as some people believe domains should be managed by local government, but others believe they should be managed by the citizens themselves. Aral and Karl-Filip agreed that the domains should be managed by the city, but that the system should be architected in a way that allows citizens to manage their own domain if they wish. Karl-Filip mentioned that the mayor is in agreement with this.

It was also noted that if the city itself is hosting the Indienet, the city is still keeping/accessing personal data in a different manner. However this is not our intention, and any personal information will be end-to-end encrypted.

### Other items of note from the meeting on 2018/02/10.

We need to consider forwarding of information: what happens when a person cannot access information from one place and needs to check for that information in another location?

How do we do logging for interactions such as the transactions carried out by Gent Info.

We will keep any connection between the Indienet and Mijn Gent until the next phase of the project.

### Other items of note from the meeting on 2018/02/11.

Gent has its own mapping data accessible from an API.
