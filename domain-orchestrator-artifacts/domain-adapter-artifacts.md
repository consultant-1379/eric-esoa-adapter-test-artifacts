# Introduction

This page will show and provide an example what to onboard and setup in connected systems for domain-orchestrator-adapter artifacts.

## Steps
### Adding domain orchestrator artifacts

We are just adding the service templates and configuration templates to SO catalog manager.

1. Onboard csar and configuration templates to so-catalog manager
   1. Jinja_TMF641_EricssonSO-v1.j2 as configuration Template
   2. TMF641_Ericsson_SO-FT.csar as Service Template
   
### Update Connected Systems

We want to add the subsystem we will need for domain-orchestrator-adapter

1. Create a file for the NaaS subsystem. This will contain all the data needed for the request.

      ````
       {
         "name": "NaaS",
         "subsystemType": {
           "id": 8,
           "type": "DomainOrchestrator",
           "alias": "Domain orchestrator"
         },
         "url": "http://eric-oss-do-stub/naas-managed",
         "connectionProperties": [
           {
             "username": "null",
             "password": "null",
             "auth_url": "/token",
             "auth_body": "{\"client_id\":\"Ericsson\",\"client_secret\":\"P4sS.w0rd\",\"grant_type\":\"complete\"}",
             "auth_headers": "{\"Content-Type\":\"application/json\",\"Accept\":\"*/*\"}",
             "auth_key": "access_token",
             "auth_type": "Bearer",
             "token_ref": ".access_token",
             "encryptedKeys": [
               "password"
             ],
             "subsystemUsers": []
           }
         ],
         "vendor": "Telstra",
         "adapterLink": "eric-oss-domain-orch-adapter"
       }
      ````

2. Use curl command to create it into the system. Please see example:
   ````
   [Login]
   ACCESS_TOKEN=$(curl -k --socks5-hostname localhost:5005 --location https://eric-sec-access-mgmt.supernova-haber003.ews.gic.ericsson.se/auth/realms/master/protocol/openid-connect/token -H 'Content-Type: application/x-www-form-urlencoded' --data-urlencode 'client_id=authn-proxy' --data-urlencode 'username=so-user' --data-urlencode 'password=Ericsson123!' --data-urlencode 'client_secret=ir8hzyEM5tanYOVTnWhSHIfd6MizYMwSx' --data-urlencode 'grant_type=password' | jq -r '.access_token')
         
   [Create NaaS subsystem]
   curl -k --insecure --socks5-hostname localhost:5005 POST --location 'https://gas.supernova-haber003.ews.gic.ericsson.se/subsystem-manager/v2/subsystems' \
   -H "Authorization: Bearer ${ACCESS_TOKEN}" \
   -H 'content-type: application/json' \
   --data-binary @<filepath>/naas.json

### Creating a Service Order

1. When the artifacts are onboarded and the subsystem is created, please follow the steps in this [README](../README.md) file to create a service order, **until** the **Create the Service Order** step. 
   (**Side note**: If you are using **only** the Domain Orchestrator artifacts, you can skip the section "Update Connected Systems", however if you are using both the ecmsol005 and DO artifacts please do create subsystems mentioned in said section.)

2. Once you get there, see the section below called [Other Service Order Examples](../README.md#Other-Service-Order-Examples), and use the [Domain Orchestrator Service Order](../README.md#Domain-Orchestrator-Service-Order) request body.

### Modifying a Service Order

1. Once the service order is created follow the steps in the [Modifying a Service Order](../README.md#Modifying-a-Service-Order) section and use the [Domain Orchestrator Modify Service Order](../README.md#Domain Orchestrator Modify Service Order) request body.