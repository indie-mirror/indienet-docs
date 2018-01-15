+++
title = "Gent Info"
weight = "10"
+++

Notes from meetings on 2018/01/10-11.

## Introduction to Gent Info

Gent Info consists of phone, mail, and chat channels between the city and the citizens. They provide general city information, as well as information on tourism, sports, library, public services, mobility (there is a new plan for sick relations that has been getting a lot of calls), and cultural events.

In 2017, there were 150,000 contacts as a part of Gent Info. Generally, Gent Info has answers to 95% of the questions they are asked. All of these contacts are logged in the CRM (Citizen Relationship Manager). There are fifteen people taking calls, and eight people working with city contacts to ensure the information is correct. There are seventy connections in the city administration who help ensure the city information is up to date.

We were looking at a potential use case for Indienet, where it could be used as another interface for Gent Info, listening for new information in particular/chosen areas. However Gent Info also handles transactions, such as registering people for services including sports camps, and shelters for the homeless, as well as simpler transactions such as registering complaints. There are around fifty applications for these types of transactions.

We decided we needed to get a better overview of Gent Info from their offices, and see what they are doing for ourselves.

### Further notes from the meeting on 2018/01/10.

After Aral presented our current aims for iGent, there were a few questions. Dieter asked about the costs involved with hosting and having SSL (Secure Sockets Layer) certificates for every domain for every citizen in Gent. Karl-Filip and Aral explained that we are having a meeting with the domain provider this week, and will discuss hosting with them then. We are also aiming to have multiple providers to give us options. Karl-Filip also mentioned that this should be a cost to the city.

Dieter also expressed a concern that the management of the domains could be a tricky area, as some people believe domains should be managed by local government, but others believe they should be managed by the citizens themselves. Aral and Karl-Filip agreed that the domains should be managed by the city, but that the system should be architected in a way that allows citizens to manage their own domain if they wish. Karl-Filip mentioned that the mayor is in agreement with this.

It was also noted that if the city itself is hosting the Indienet, the city is still keeping/accessing personal data in a different manner. However this is not our intention, and any personal information will be end-to-end encrypted.

### Other items of note from the meeting on 2018/02/10.

We need to consider forwarding of information: what happens when a person cannot access information from one place and needs to check for that information in another location?

How do we do logging for interactions such as the transactions carried out by Gent Info.

We will keep any connection between the Indienet and Mijn Gent until the next phase of the project.
