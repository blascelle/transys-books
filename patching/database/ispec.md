# ISPEC Database Patching

## Overview
ISPEC (internal specification?) databases are used by certification applications to provide a persistence layer for 
validating network requests and certifying network compliance to WEX specifications before permitting data to be sent to 
the North American Fleet Authorizer. 

## Impacted Applications
 - cert_web
 - cert_auth

## Impacted User Groups
 - External Customers - User
 - Network Operations - User
 - Transaction Systems - Application maintainer
 - Database Architects - Database maintainer

## Coordination
 - Not required for non-prod environments.
 - Patching must be coordinated with Network Operations, Transaction systems, and Database Administrators.
 - Network Operations will coordinate and inform, if necessary, external customers.
 - Data Center will run the shutdown and startup jobs.
 - An email chain with Data Center, Oracle DBAs, Transys support, and NetOps Ticket can be used to facilitate patching. 

## Patching Tasks
 - [ ] Coordinate a time to perform patching with required Network Operations, Database Architects, and Transaction Systems teams.
 - [ ] At patching time, Start an [email thread](mailto:Data.Center@wexinc.com?cc=netopsticket@wexinc.com,oracledbas@wexinc.com,TranSysSupport@wexinc.com&?subject=Patching%20PRDISPEC&body=Data%20Center%2C%0D%0A%0D%0AFor%20the%20current%20ODATE%2C%20Please%20load%20and%20run%20stopTCISPEC-C1-0%2C%20stopTCISPEC-C1-1%2C%20and%20StopSimAuthorizer.%0D%0A%0D%0AGroup1%3A%20ServerApps%0D%0AGroup2%3A%20tcispec%0D%0AJob%3A%20stopTCISPEC-C1-0%0D%0A%0D%0AGroup1%3A%20ServerApps%0D%0AGroup2%3A%20tcispec%0D%0AJob%3A%20stopTCISPEC-C1-1%0D%0A%0D%0AGroup1%3A%20ServerAppsAuth%0D%0AGroup2%3A%20ServerApps-SemiAuth%0D%0AJob%3A%20StopSimAuthorizer%0D%0A%0D%0AThank%20you%2C) with datacenter to shutdown the applications. *The email link will compose the [example](#example-email-to-shutdown-apps) email to shutdown apps
 - [ ] With apps shutdown, inform database architects that we're ready for them to apply the patch.
 - [ ] After the patch is applied and validated by database architects, send the [example](#example-email-to-startup-apps) startup email to the datacenter
 - [ ] After startup jobs are complete, send an email to notify teams that patching is complete.

## Notes / Observations / Improvements
- cert_web may stay online during patching and operation will return to normal after patching is complete. This is because connections are created on-demand. However, the application does not provide a way to notify users of ongoing patching or prohibit users from encountering errors.
- cert_auth maintains a connection pool created at startup and has not been tested if it can remain online during patching and automatically recover.

### Example Email to Shutdown Apps
```
Data Center,

For the current ODATE, please load and run stopTCISPEC-C1-0, stopTCISPEC-C1-1, and StopSimAuthorizer.

Group1: ServerApps
Group2: tcispec
Job:  stopTCISPEC-C1-0

Group1: ServerApps
Group2: tcispec
Job:  stopTCISPEC-C1-1

Group1:  ServerAppsAuth
Group2:  ServerApps-SemiAuth
Job: StopSimAuthorizer

Thank you,
```

### Example Email to Startup Apps
```
Data Center,

For the current ODATE, please load and run startTCISPEC-C1-0, startTCISPEC-C1-1, and StartSimAuthorizer.

Group1: ServerApps
Group2: tcispec
Job:  startTCISPEC-C1-0

Group1: ServerApps
Group2: tcispec
Job:  startTCISPEC-C1-1

Group1:  ServerAppsAuth
Group2:  ServerApps-SemiAuth
Job: StartSimAuthorizer

Thank you,
```

[<< README](../../README.md)