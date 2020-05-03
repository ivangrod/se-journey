+++
title = "User Story"
date = 2020-04-13T18:59:30+02:00
weight = 1
+++

A **user story** describes functionality that will be valuable to either a user or purchaser of a system or software.

* **CARD**: A written description of the story used for planning and as a reminder
* **CONVERSATION** (++): About the story that serve to flesh out the details of the story
* **CONFIRMATION**: Tests that convey and document details and that can be used to determine when a story is complete. [Acceptance test]({{< ref "/testing/test-types#acceptance" >}})

> Size: Stories that can be coded and tested between half a day and perhaps two weeks by one or a pair of programmers

#### Epic

When a story is too large it is sometimes referred to as an **epic**.

Epic "A user can search for a job". Split:

1. A user can search for jobs by attributes like location, salary range, job title, company name, and the date the job was posted.
2. A user can view information about each job that is matched by a search. (We don't need to further divide it into more specific user stories)
3. A user can view detailed information about a company that has posted a job.

**Benefits**

* User stories emphasize verbal rather than written communication.
* User stories are comprehensible by both you and the developers.
* User stories are the right size for planning.
* User stories work for iterative development.
* User stories encourage deferring detail until you have the best understanding you are going to have about what you really need.

## Writing Stories (INVEST)

### Independent

Avoid introducing dependencies between stories. Dependencies between stories lead to prioritization and planning problems.

**Problem**
 
1. _A company can pay for a job posting with a Visa card._
2. _A company can pay for a job posting with a MasterCard._
3. _A company can pay for a job posting with an American Express card._

Estimating that it will take three days to support the first credit card type (regardless of which it is) and then one day each for the second and third.

**Solution**

* Combine the dependent stories into one larger but independent story
* Find a different way of splitting the stories
* Putting two estimates on the card: one estimate if the story is done before the other story, a lower estimate if it is done after

1. _A customer can pay with one type of credit card._
2. _A customer can pay with two additional types of credit cards._

### Negotiable

Details are negotiated -> Notes on the card help a developer and the customer to resume a conversation where it left off previously. 

Regardless of whether it is the same developer and customer who resume the conversation.

**Problem**

When as much detail is added can lead to the mistaken belief that the story cards reflect all the details and that there’s no further need to discuss the story with the customer.

**Solution**

* Story card as containing:
  * phrase or two that act as reminders to hold the conversation
  * notes about issues to be resolved during the conversation

* Details that have already been determined through conversations become [tests]({{< ref "/testing/test-types#acceptance" >}})

* Story card with few details. Discussion between customer and developer is somewhat abstract, Missing details or them to mistakenlyview their discussion as definitve or their estimate as accurate.

### Valuable to users or customers

Ex. stories valued by purchasers contemplating buying the product but would not be valued by actual users:

* _Throughout the development process, the development team will produce documentation suitable for an ISO 9001 audit._
* _The development team will produce the software in accordance with CMM Level 3.
What you want to ._

**Problem**

Avoid stories that are only valued by developers

* _All connections to the database are through a connection pool._
* _All error handling and logging is done through a set of common classes._

**Solution**

* _Up to fifty users should be able to use the application with a five-user database license._
* _All errors are presented to the user and logged in a consistent manner._

### Estimatable

**Problem**

There are three common reasons why a story may not be estimatable:

1. Developers lack domain knowledge.
2. Developers lack technical knowledge.
3. The story is too big.

**Solution**

1. Developers should discuss it with the customer who wrote the story
2. **Spike**: A brief experiment to learn about an area of the application. Developers learn just enough that they can estimate the task. Timebox: maximum amount of time.
> Consider putting the spike in a different iteration
3. Developers will need to disaggregate it into smaller, constituent stories.

### Small

**Problem**

Epics typically fall into one of two categories:

* The compound story. An epic that comprises multiple shorter stories
* The complex story. An user story that is inherently large and cannot easily be disaggregated into a set of constituent stories.

**Solution**

{{%attachments title="How to split user stories" pattern="Story-Splitting-Flowchart.pdf" style="blue" /%}}

### Testable

Stories must be written so as to be testable. Whenever possible, tests should be automated.

**Problem**

Untestable stories commonly show up for nonfunctional requirements, which are requirements about the software but not directly about its functionality.

* _A user must find the software easy to use._
* _A user must never have to wait long for any screen to appear._

**Solution**

* _New screens appear within two seconds in 95% of all cases._

## User Role Modeling

A user role is a collection of defining attributes that characterize a population of users and their intended interactions with the system. There will be some overlap between different user roles.

Ex. Job Seeker, First Timer, Layoff Victim, Geographic Searcher, Monitor, Job Poster, Resume Reader

#### Brainstorm an initial set of user roles

Steps:

1. The customer and as many of the developers as possible meet in a room.
2. Start with everyone writing role names on cards
3. The name of the new role and nothing more. No dicsussion.
4. Continue until progress stalls and participants are having a hard time thinking up new roles.

> A User Role is One User

#### Organize the initial set

Relationships between the roles.

Overlapping roles are placed so that their cards overlap. If the roles overlap a little, overlap the cards a little. If the roles overlap entirely, overlap the cards entirely.

> System Roles -> If you think it will help, then identify an occasional non-human user role. 

> We don’t need user roles for every conceivable user of the system, but we need roles for the ones who can make or break the success of the project.

#### Consolidate roles

The authors of overlapping cards describe what they meant by those role names. After a brief discusson the group decides if the roles are equivalent.

* _Job Poster | Resume Reader -> Recruiter_
* _Recruiter -> Internal Recruiter | External Recruiter_. A generic role, is positioned above specialized versions of that role.

#### Refine the roles

Model those roles by defining attributes of each role. A **role attribute** is a fact or bit of useful information about the users who fulfill the role. Any information about the user roles that distinguishes one role from another may be used as a role attribute.

* The frequency with which the user will use the software.
* The user’s level of expertise with the domain.
* The user’s general level of proficiency with computers and software.
* The user’s level of proficiency with the software being developed.
* The user’s general goal for using the software. Some users are after convenience, others favor a rich experience, and so on.

#### Extra Technique - Personas

Creating a persona for the role. A persona is an imaginary representation of a user role. A persona should be described sufficiently that everyone on the team feels like they know the persona. 

For example, Mario - Posting new job - may be described as follows:

_Mario works as a recruiter in the Personnel department of SpeedyNetworks, a manufacturer of high-end networking components. He’s worked for SpeedyNetworks six years. Mario has a flex-time arrangement and works from home every Friday. Mario is very strong with computers and considers himself a power user of just about all the products he uses. Mario’s wife, Kim, is finishing her Ph.D. in chemistry at Stanford University. Because SpeedyNetworks has been growing almost consistently, Mario is always looking for good engineers._

> Think about writing a persona definition for one or two of the primary user roles.

#### Extra Technique - Extreme Characters

Use extreme characters when considering the design of a new system.

For example: _PDA for a drug dealer, the Pope, and a twenty-year-old woman who is juggling multiple boyfriends._

## Gathering Stories

* X -> Get all of the user stories in one pass.
  
* The relevance of a story changes based on the passage of time and on what stories were added to the product in prior iterations.

> Write user stories that decrease in detail as the time horizon increases

Having a rough idea of what a project will cost and what benefits it will deliver before getting funding and approval to start the project:

#### User interviews

* Selection of interviewees. Real users whenever possible. Users who fill different user roles.
* Open-Ended and Context-Free questions, which are ones that do not include an implied answer or preference

#### Questionnaires

* Information about stories you already have. 
* Large user population
  * Information about how to prioritize the stories. 
  * Answers to specific questions.
* It's not recommend using questionnaires when trawling for stories

#### Observation

* Observing users interact -> pick up insights. Rapid and direct feedback.
  * Release software as early and often as possible.

#### Story-Writing (++)

[Story-Writing Workshops](https://github.com/ivangrod/agile-pills/blob/master/user-story/story-writing-workshop.md)

