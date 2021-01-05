# OpenShift4-Migration
Scripts and info for Ministry teams migration from OpenShift 3.11 to 4.x

To start migrating an app from OCP 3 to OCP 4, the app's Product Owner (BCGov employee) must submit a project set provisioning request in the [OCP 4 Project Registry](https://registry.developer.gov.bc.ca/public-landing). We currently only provision namespaces in the OCP 4 Silver cluster, the Gold/Gold DR cluster will be available in April 2021. 

**When you finish migrating your app to OCP 4, DO NOT FORGET to submit a request to remove the old project set from OCP 3 using [this template](https://github.com/BCDevOps/devops-requests/issues/new?assignees=caggles%2C+mitovskaol%2C+ShellyXueHan&labels=openshift-project-set%2C+pending&template=openshift_project_request.md&title=). Only when your OCP 3 project set is removed, the migration is considered complete.**
