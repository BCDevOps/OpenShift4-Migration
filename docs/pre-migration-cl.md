# Checklist Prior to Migration

## High Level starter

- [ ] Inventory application assets for migration
  - [ ] application services
  - [ ] state/data
  - [ ] configuration items
  - [ ] secrets
  - [ ] images
  - [ ] DNS/names
- [ ] Identify external dependencies
  - [ ] vanity DNS
  - [ ] external supporting services (SSO, external DB, etc)
  - [ ] network access rules (internal to cluster, external to cluster)
  - [ ] users (direct to user, external applications, internal services)

## Timing constraints

- [ ] DNS migration
- [ ] Availability/Outage requirements
- [ ] phase dependencies/prerequisites

## Migration Methods

### Redeploy

For the most reliable migration, re-deploying your build/deploy pipeline in the new platform allows for full feature validation of not just your application, but your pipeline as well.

Pros

- validates build and deployment pipeline automation in the new environment
  - ensures teams can return to normal development faster
  - identifies potential feature additions for the deployment pipeline
- leverages existing integration tests for environment validation
- provides working pipeline to leverage for any application modifications required for the migration
- with a working build/deploy pipeline, only state data is required to be moved
  - reducing potential issue surface
  - reducing potential cut-over downtime

Cons

- requires a CI and CD pipeline
  - pipeline may not be complete (does not fully deploy application)
- Does not migrate state (separate migration method for data is still required)

### Lift and Shift

The "hire the movers" method that creates copies of all OpenShift objects and stages state data between clusters.

Pros

- does not require a working CI and CD pipeline
- creates an exact duplicate of the deployed application in the new cluster
- can include all data as part of the migration

Cons

- manual re-configuration of manifests to match new cluster services (such as default image registries, etc)
  - Creates potentially longer cut-over downtime
- coordination of multiple teams required for other external cut-over dependencies (DNS)
