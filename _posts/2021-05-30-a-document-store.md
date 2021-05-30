---
layout: post
title:  "Dynamic Configuration Management"
date:   2021-05-30 14:00:00 +0530
tags: [SystemDesign, ConfigManagement]
published: false
---

user/state
(multiple aggregated info - session, device and configs)
Based on the user state update history response.
The client makes a call to fetch the bundle/platform configurations.

resource/{platform}/appConfigs
resource/clients/{client}/configTypes/{configType}

Startup:

ConfigResponse
    Map<String, Document> configs;
    Map<String, Rule> rules;
    boolean success;

UpdateHistoryResponse
    Map<String, UpdateHistory> clientUpdateHistoryMap;
    private Long timestamp;
    private String author;

Used also to persist static configs for the consumer API client mobile-api itself. 
e.g. landing page layouts, banner sizes, data configs etc

Clients: mobile-api, android
DocumentType: data,
Document
    String name;
    String namespace;
    String id;
    int version;
    Object val;
    List<Document> children;
    String author;
    Map<String, Object> attributes;

UIConsole

High Level Design:
![DocumentStore]({{ site.baseurl }}/assets/projects/DocumentStore.png){: class="center_85" }
