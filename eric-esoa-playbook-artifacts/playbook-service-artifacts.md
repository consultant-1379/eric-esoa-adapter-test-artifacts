# Introduction

This page will show and provide an example what to onboard and setup in connected systems for playbook service artifacts.

## Steps
### Adding playbook service artifacts

We are just adding the service templates and configuration templates to SO catalog manager.

1. Onboard SO csar and configuration templates to so-catalog manager
    1. sample_playbook.yml as configuration Template
    2. PNF_Playbook.csar as Service Template
    3. pnf_config.json as configuration Template

### Update Connected Systems

We want to add the subsystems we will need for playbook service.

The username and password and client secret used in auth.body may be different depending on your environment.

1. Create Files for the subsystems needed. This will contain all the data needed for the request.
   1. Add enm subsystem
   ````
   {
     "name": "enm",
     "sybsystemType": {
       "id": 1,
       "type": "DomainManager",
       "alias": "Domain manager"
     },
     "url": "http://eric-eo-enm-stub:8080",
     "connectionProperties": [
       {
         "username": "administrator",
         "password": "TestPassw0rd",
         "scriptingVMs": "eric-oss-pm-solution-enm-1",
         "sftpPort": "2020",
         "encryptedKeys": [
           "password"
         ],
         "subsystemUsers" : []
       }
     ],
     "vendor": "Ericsson",
     "adapterLink": "eric-eo-enm-adapter"
   }
   ````

2. Use curl command to create them into the system. Please see example
   ````
   [Login]
   ACCESS_TOKEN=$(curl -k --socks5-hostname localhost:5005 --location https://eric-sec-access-mgmt.supernova-haber003.ews.gic.ericsson.se/auth/realms/master/protocol/openid-connect/token -H 'Content-Type: application/x-www-form-urlencoded' --data-urlencode 'client_id=authn-proxy' --data-urlencode 'username=so-user' --data-urlencode 'password=Ericsson123!' --data-urlencode 'client_secret=ir8hzyEM5tanYOVTnWhSHIfd6MizYMwSx' --data-urlencode 'grant_type=password' | jq -r '.access_token')

   [Create ENM subsystem]
   curl -k --insecure --socks5-hostname localhost:5005 POST --location 'https://gas.supernova-haber003.ews.gic.ericsson.se/subsystem-manager/v2/subsystems' \
   -H "Authorization: Bearer ${ACCESS_TOKEN}" \
   -H 'content-type: application/json' \
   --data-binary @/c/Users/egergle/rest/esoa/templates/domain-adapter/enm.json
   ````

