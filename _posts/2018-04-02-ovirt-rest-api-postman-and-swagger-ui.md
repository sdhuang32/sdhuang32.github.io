---
title:  "oVirt REST API, Postman, and Swagger UI"
excerpt: "I'm going to share my understanding of oVirt REST API, 
and my experiene in testing and documenting those APIs using Postman and Swagger UI."
classes: wide
tags:
  - REST API
  - oVirt
  - Postman
  - Swagger
date: 2018-04-02
last_modified_at: 2019-08-16
---

## oVirt REST API

oVirt engine natively supports management control over REST API. The following is some notes of my study about REST API itself and the details related to oVirt.

<div class="embed-container"
 style="position: relative; padding-bottom: 59.27%; height: 0; overflow: hidden; max-width: 100%;">
 <iframe style="position: absolute; top: 0; left: 0; width: 100%; height: 100%;"
  src="https://docs.google.com/presentation/d/e/2PACX-1vRXf-UwP-7T9aTOh5lpPXcTqH8k0pUSedCENdtvEln8zHfRyZfQSJt_e7JRNiELjZxCSeitRFh3NcWW/embed?start=false&loop=false&delayms=3000" frameborder="0" width="960" height="569" allowfullscreen="true" mozallowfullscreen="true" webkitallowfullscreen="true"></iframe>
</div>

## Examples of using Postman

Then the following are some examples where I used [Postman](https://www.getpostman.com/) as my REST client to test some functions of oVirt.

<div class="embed-container"
 style="position: relative; padding-bottom: 59.27%; height: 0; overflow: hidden; max-width: 100%;">
 <iframe style="position: absolute; top: 0; left: 0; width: 100%; height: 100%;"
  src="https://docs.google.com/presentation/d/e/2PACX-1vRfUXydiXIs2ELDAyGIvFOYHF1jRn2oIXfS7ZYIxIAK20EEOR60mQiuL1JMKybd3smdTnY9fvPVR_Zu/embed?start=false&loop=false&delayms=3000" frameborder="0" width="960" height="569" allowfullscreen="true" mozallowfullscreen="true" webkitallowfullscreen="true"></iframe>
</div>

## Examples of using Swagger UI

OpenAPI Specification and Swagger are useful tools that enable developers to design, generate template code of, document and test your own APIs. Also it has become the standard of REST API development in many companies.

<div class="embed-container"
 style="position: relative; padding-bottom: 59.27%; height: 0; overflow: hidden; max-width: 100%;">
 <iframe style="position: absolute; top: 0; left: 0; width: 100%; height: 100%;"
  src="https://docs.google.com/presentation/d/e/2PACX-1vTXcGCc-uNYQkdcXTYW04rEHCVsLJf7-YGZ34Q2lCDzjC2l7NcxROLnf9EtDEDJVQYoe4Rx66BYdOia/embed?start=false&loop=false&delayms=3000" frameborder="0" width="960" height="569" allowfullscreen="true" mozallowfullscreen="true" webkitallowfullscreen="true"></iframe>
</div>

When we need a document to display our own REST API list we can use Swagger UI. Here is an [example](/assets/sdhuang32/misc/all-in-one-swagger-ui.html) using a static Swagger UI to show partial oVirt API with OpenAPI specification v2.0. ([Reference](https://github.com/sdhuang32/offline-swagger-ui))
