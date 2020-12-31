---
title: Introduction
---

## Introduction

GrantNZ is high performance authentication and authorization multi tenant platform.  
It manages multiple service auth at the centralized.

GrantNZ is cloud native and distributed system. It's assuming to work on kubernetes.  
There are three component. `API server` and `Cache server` and `Role management console`.  

### GrantNZ

Features of Grant NZ.

* High Performance
* Distributed system
* High Availability
* Cloud Native

### RBAC

GrantNZ is to enable a role based access control.   
It defines the GrantNZ client role and the group role on the Service.  
If you want to check the terms: [Terminology](/grant-n-z-docs/articles/terminology/)

* GrantNZ client role
  * The GrantNZ client have to register GrantNZ server. Then it can issue ApiKey.
* Group role on the Service
  * There are multiple services
  * There are multiple groups in the service.
  * Service administrators can manage user privileges within a group by role

### Use Cases

GrantNZ is effective in the following cases.

1. Offering multiple services
2. It wants to integrate authentication and authorization for multiple services
3. It wants to reduce the latency of certification and authorization

