---
title:  "oVirt REST API, Postman, and Swagger UI"
excerpt: "I'm going to share my understanding of oVirt REST API, 
and my experiene in testing and documenting those APIs using Postman and Swagger UI."
classes: wide
tags: 
  - REST-API
  - oVirt
  - Postman
  - Swagger
date: 2018-04-02
last_modified_at: 2019-08-16
---

## oVirt REST API

oVirt engine natively supports management control over REST API. The following is some notes of my study about REST API itself and the details related to oVirt.

<iframe src="https://docs.google.com/presentation/d/e/2PACX-1vRXf-UwP-7T9aTOh5lpPXcTqH8k0pUSedCENdtvEln8zHfRyZfQSJt_e7JRNiELjZxCSeitRFh3NcWW/embed?start=false&loop=false&delayms=3000" frameborder="0" width="960" height="569" allowfullscreen="true" mozallowfullscreen="true" webkitallowfullscreen="true"></iframe>

## Examples of using Postman

Then the following are some examples where I used [Postman](https://www.getpostman.com/) as my REST client to test some functions of oVirt.

<iframe src="https://docs.google.com/presentation/d/e/2PACX-1vRfUXydiXIs2ELDAyGIvFOYHF1jRn2oIXfS7ZYIxIAK20EEOR60mQiuL1JMKybd3smdTnY9fvPVR_Zu/embed?start=false&loop=false&delayms=3000" frameborder="0" width="960" height="569" allowfullscreen="true" mozallowfullscreen="true" webkitallowfullscreen="true"></iframe>

## Examples of using Swagger UI

OpenAPI Specification and Swagger are useful tools that enable developers to design, generate template code of, document and test your own APIs. Also it has become the standard of REST API development in many companies.

<iframe src="https://docs.google.com/presentation/d/e/2PACX-1vTXcGCc-uNYQkdcXTYW04rEHCVsLJf7-YGZ34Q2lCDzjC2l7NcxROLnf9EtDEDJVQYoe4Rx66BYdOia/embed?start=false&loop=false&delayms=3000" frameborder="0" width="960" height="569" allowfullscreen="true" mozallowfullscreen="true" webkitallowfullscreen="true"></iframe>

When we need a document to display our own REST API list we can use Swagger UI. Here is an [example](/assets/sdhuang32/misc/all-in-one-swagger-ui.html) using a static Swagger UI to show partial oVirt API with OpenAPI specification v2.0. ([Reference](https://github.com/sdhuang32/offline-swagger-ui))
