# Semantic.works Documentation

Documentation is hard, but it is instrumental to helping your code be useful to people that aren't you, and that's a worthwhile pursuit. Even if you're making things just for yourself, the aforementioned "people that aren't you" descriptor also includes you in a few years, months or weeks. To have our documentation be as useful and in-tune as possible with the semantic technologies we craete, the following principles were outlined.

## Design principles
1. [*Don't Repeat Yourself.*](#1-dont-repeat-yourself)
2. [*All-encompassing.*](#2-all-encompassing)
3. [*Ease of use: writing.*](#3-ease-of-use-writing)
4. [*Ease of use: reading.*](#4-ease-of-use-reading)
5. [*Micro-first.*](#5-micro-first)
6. [*Non-proprietary.*](#6-non-proprietary)


### 1. Don't Repeat Yourself
If we're copying documentation/text across places, we're doing something wrong:
- This may cause inconsistencies and out of date versions, breaching [(4) ease of use: readers](#4-ease-of-use-readers), as well as [(2) all-encompassing](#2-all-encompassing)
- This requires extra upkeep/work, breaching [(3) ease of use: writers](#3-ease-of-use-writers)

### 2. All-encompassing
Many documentations are incredibly lacking, as you've probably experienced over your time as a developer. The divio documentation standard, which you can view on [documentation.divio.com](https://documentation.divio.com/), attempts to clearly outline *what* should be documented, and *where* you can organise it. While I recommend viewing the talk and/or text on their website, a [quick start is included in this repository's how-tos](../how-tos/quickstart-writing-documentation.md).

### 3. Ease of use: writing
Minimising maintenance for the people who make the code is essential. Lots of people do not have the time, resources, training, or quite simply the desire to write down docs. This is one of the reasons we stay away from external documentation sites like gitbook: having them sign in to a whole different platform to write the documentation for their code would be way too much friction. This would also make us reliant on the external documentation provider, breaching [(6) Non-proprietary](#6-non-proprietary), and would require navigating to an external location for readers and writers alike, thus additionally breaching [(4) Ease of use: reading](#4-ease-of-use-reading).

### 4. Ease of use: reading
- We have a bunch of projects going on, written in Ruby, Lisp, Elixer, JavaScript, Python... The documentations should nevertheless resemble eachother and be consistent. This is additionally made possible thanks to the the divio documentation standard, as seen in [(2) all-encompassing](#2-all-encompassing).
- End-users should not have to jump through hoops to read the documentation. If you have a local clone of a repository, you should not be required to have an active internet connection to read its documentation.

### 5. Micro-first
Semantic.works is about microservices, so that means a bunch of documentation will be **small**. This means it is important to adhere to the structures of big projects involuntarily. An entire documentation folder for something that can be explained in one README is a no-go. Start from a single (README) file, and expand only if necessary.

### 6. Non-proprietary
Our repositores and projects are made to work anywhere, so this should be the case for our documentation as well. Websites, services and hosts shutting down\* or changing their licenses\*\*  is not unheard of. No editor lock-in, no host lock-in.

- \*[Freshcode/freshmeat](https://jeffcovey.net/2014/06/19/freshmeat-net-1997-2014/), [Microsoft CodePlex](https://devblogs.microsoft.com/bharry/shutting-down-codeplex/), [Google Code](https://code.google.com/archive/)
- \*\*[Terraform/Hashicorp products](https://blog.gruntwork.io/the-future-of-terraform-must-be-open-ab0b9ba65bca), [Red Hat Enterprise Linux](https://thenewstack.io/how-red-hats-license-change-is-reinvigorating-enterprise-linux-distros/)


## Implementation
To achieve all of the above goals, we are putting the following structure(s) in place:

### Write the documentation
All documentation ends up in one of the following places
- If it's about the semantic works stack in its entirety, it should go in the `project` repository (see: this document)
- If it's something useful for people getting started with the semantic works stack as a whole, it should go in the `mu-project` repository. 
- If it's about a specific microservice or project, it should go inside of that projects repository

The documentation should ideally be written as presented in [project/how-tos/quickstart-writing-documentation](../how-tos/quickstart-writing-documentation.md). This also covers how to tag new revisions.

For microservice developers and most people working on semantic.works, this is all you have to do! This will ensure that most design principles are met. However, to have a more readily available & traditionally structured documentation, the following steps get executed.

### Semantic.works
[View the fork on GitHub here](https://github.com/Denperidge-Redpencil/semantic.works)

Semantic.works was (re-)developed to contain all necessary links & information about our repositories. This includes links & documentations to current & previous revisions. However, we ensured that *none* of this would have to be hardcoded into the site, thanks to...

### App-mu-info
[View the fork on GitHub here](https://github.com/Denperidge-Redpencil/app-mu-info-rework)

Which is a service that holds a linked-data database and an endpoint about repositories and their revisions. This includes links to the repos and images, as well as the contents of the documentation, both parsed and unparsed. And this service fetches its data dynamically thanks to...

### Repo-harvester
[View the repo on GitHub here](https://github.com/mu-semtech/repo-harvester)

Which is a microservice that - after some basic configuration - will collect & clone all repositories and images from specified accounts & hosts. It collects all the information app-mu-info's data model needs, including all documentation aggregated & parsed into the divio sections. The documentation stuff is thanks to...

### Divio-docs-parser
[View the repo on GitHub here](https://github.com/Denperidge-Redpencil/divio-docs-parser)

Which is a Python package that, when given a directory, will handle everything in terms of finding & parsing documentationo. It returns a class filled with fully parsed & easily useable Python dictionaries.


*This document has been loosely adapted from Denperidge's learning writeup. You can view it [here](https://github.com/Denperidge-Redpencil/Learning.md/blob/main/Notes/docs.md)*.
