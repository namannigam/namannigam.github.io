---
layout: post title:  "Dynamic Configuration Management"
date:   2021-06-05 21:00:00 +0530 tags: [SystemDesign, ConfigManagement]
published: true
---

### Requirement

Home page and product pages are very crucial landing aspects for e-commerce customers. Continuous evolution of the user
experience, would require frequent business changes on such pages. Application rollouts for these changes mostly get
coupled with other feature development and also places a risk of putting off customers with frequent downloads.

### Solution

First thought towards solving this problem is to be able to _dynamically load configurations_ mapped to each business
requirement without frontend deployment. The underlying capabilities of a service providing these configurations would
require additional:

- _namespacing_: after all, we wouldn't want to fetch web product page layout on android homepage!
- _versioning_: not all versions of your application in the market would support each layout enhancement for a page!
- _update history_: we wouldn't want to eagerly fetch all these layouts from server, unless they are not updated!
- _tags_: wouldn't you require to just query documents based on certain attributes!

### System Components

Primary components to build the complete interaction of end users with such a store involved:

- <u>**Consumer API**</u>: An aggregator layer that the clients could talk to understand when to ask for which layouts.
- <u>**Document Store**</u>: A web service to serve the configurations persisted.
- <u>**DataSore**</u>: The persistence layer for the various types of configurations dealt with.
- <u>**Console**</u>: A layer for receiving updates over the configurations.
- <u>**Cache**</u>: A guardrail towards the service and persistence layer against the throughput.

![DocumentStoreSystem]({{ site.baseurl }}/assets/projects/blob/DocumentStoreComponents.png){: class="center_85" }

### Entities

Entities involved in designing the system based on our requirements were broadly the following:

- **Client**: Representation of each team onboarded to the service, for instance mobile, ads, games, etc.
- **DocumentType**: Type specification for logical grouping of a set of documents, for example appConfigs
- **Document**: The actual documents with updates, versioning, values to reflect as configurations etc.
- **Rule**: Construct per documentType to define the rules for a document belonging to a namespace.
- **UpdateHistory**: History of a client's update transitively derived by a document update.

![DocumentStoreEntities]({{ site.baseurl }}/assets/projects/blob/DocumentStoreEntities.png){: class="center_85" }

### Interaction

Interaction between the components could be illustrated as below:

![DocumentStoreEntities]({{ site.baseurl }}/assets/projects/blob/DocumentStoreComponents.png){: class="center_85" }

### Learnings

During the time I had spent on the project, the feature asks for which I was at the receiving end, and the production
incidents that I had come across definitely made me learn that:

1. Namespacing convention provided us an ease in scaling for other pages such as flyout, landing pages, banners etc. to
   board the service.

2. Cache invalidation strategies aligned as per the business requirements holds equal importance with the functional
   optimisation to sustain using minimal infra resources during high throughput circumstances.