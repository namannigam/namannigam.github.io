---
layout: post
title:  "Places - Multitenant Microservice"
date:   2024-06-30 21:00:00 +0530
tags: [SystemDesign, GooglePlaces, Customisation]
published: true
comments: true
---
Platform to capture user location input and serve interaction across businesses centrally.

### Requirement

First of the steps in a user's journey on a platform brings along the need to capture the location preference 
that is supposed to be carried forward. This brings along an interesting problem to solve for: _showcase relevant results_ 
in context of a business and build over a _user selection_ for future relevance.

### Approach

The problem at hand is not very new, yet the building of context particular to a business with possible 
optimisations and enrichment of the data brings in complexities to solve for. These are accompanied by -
- _validations_: every tenant has its own set of rules for processing
- _selections_: relevance and ranking of the results per tenant
- _limitations_: avoid throttling at API level while limiting per tenant

Pivoting towards a platform which can provide these capabilities as a centralised service across businesses while 
ensuring a single responsible layer to interact with public third parties.

### System Components

Primary components to build the complete interaction of end users with such an application involved:

- **<u>Tenants</u>**: Services with which the consumer clients can integrate based upon interaction. e.g. P&M, Rental Search etc.
- **<u>Places</u>**: A web based application, serving per tenant place suggestions and details.
- **<u>DataStore</u>**: The persistence layer for data such as Elasticsearch over meaningful places in context of the business. 
- **<u>Queue</u>**: Messaging queue such as Kafka to perform async operations ensuring non-blocking execution 
at higher throughput.

### Interaction

There are majorly two user flows around the integration for a location selection:
1. typing to fetch a list of suggestions and
   ![AutocompleteSeq]({{ site.baseurl }}/assets/projects/blob/places/AutocompleteSeq.png){: class="center_85" }

2. selecting to get more details of an element
   ![DetailsSeq]({{ site.baseurl }}/assets/projects/blob/places/DetailsSeq.png){: class="center_85" }

### Entities

While the platform offers a simple integration of places and its details. With the consideration that every
building searched for is another place in itself, we could carve out an identifier such as a place type with Building,
Metro being the first few use cases onboarding it right away. Entities involved in designing the system based on our requirements were broadly the following:

- **Place**: Representation of a location with type identifiers, suggesters and other relevant attributes.
- **Building**: Category of places meant to represent a Society or a Project.
- **Metro**: Category of places to represent metro stations and facilitate a view of metro lines.
- **Locality**: Category to represent a boundary driven region for searches.

![PlacesEntity]({{ site.baseurl }}/assets/projects/blob/places/PlacesEntity.png){: class="center_85" }

### Takeaways

 - With the evolution of the platform and the requirements posed further, one of the key learnings has been the 
importance of the quality/authenticity of information associated with the places, which is currently a user generated 
content moderated and curated by Google. With our intermediate store, we would though be in need to seek some data 
duplication and refinement to be holding much more relevance towards the user searches.
