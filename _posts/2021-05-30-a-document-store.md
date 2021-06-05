---
layout: post title:  "Dynamic Configuration Management"
date:   2021-06-05 21:00:00 +0530 tags: [SystemDesign, ConfigManagement]
published: true
---

## Requirement

Home page and product pages are very crucial landing aspects for e-commerce customers. Continuous evolution of the user
experience, would require frequent business changes on such pages. Application rollouts for these changes mostly get
coupled with other feature development and also places a risk of putting off customers with frequent downloads.

## Solution

First thought towards solving this problem is to be able to _dynamically load configurations_ mapped to each business
requirement without frontend deployment. The underlying capabilities of such a service would require storing the
configurations with:

- _namespacing_: after all, we wouldn't want to fetch web product page layout on android homepage!
- _versioning_: not all versions of your application in the market would support each layout enhancement for a page!
- _update history_: we wouldn't want to eagerly fetch all these layouts from server, unless they are not updated!

## Design Components

![DocumentStore]({{ site.baseurl }}/assets/projects/DocumentStore.png){: class="center_85" }

Console Document Store Datastore Mobile API Cache

user/state resource/{platform}/appConfigs resource/clients/{client}/configTypes/{configType}

## Entities

**Client**

**DocumentType**

**Document**

```
id
version
value
attributes
```

**UserState**: State of the user across pages, sessions, in several devices.
(multiple aggregated info - session, device and configs)
Based on the user state update history response. The client makes a call to fetch the bundle/platform configurations.
**PlatformConfiguration**
ConfigResponse Map<String, Document> configs; Map<String, Rule> rules; boolean success;

**UpdateHistoryResponse**
Map<String, UpdateHistory> clientUpdateHistoryMap; private Long timestamp; private String author;

## Learnings

Namespacing convention provided us an ease in scaling for other pages such as flyout, landing pages, banners etc. to
board the service.