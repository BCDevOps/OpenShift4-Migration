# Planning your migration

## Migration Considerations

Check out OpenShift 4

- [docs.openshift.com](https://docs.openshift.com/container-platform/latest/welcome/index.html)
- [learn.openshift.com](https://learn.openshift.com) for hands on!
- Add links to more resources:

### Staffing considerations

Migrating applications can be an intense activity.  With many moving parts, and lots of details and challenges waiting to surprise you, having a team that understands both your application details as well as your deployment approaches will be a key success factor.  This is an activity that will test and stretch the staff that energize your DevOps roles.

### Network considerations

#### Ingress

Whether you already manage an application vanity URL (eg: my-awesome-app.gov.bc.ca) or you currently leverage the pathfinder.gov.bc.ca wildcard (my-justasawesome-app.pathfinder.gov.bc.ca), you will need to plan how people will find your app once it's moved.  With the move to an enterprise service, the platform wildcard has been deemed unsuitable for production application deployments.  This means if you don't have a vanity URL for your application yet, you will want to get started on provisioning one, and if you do, you'll want to plan how to switch the name to the new platform.  (Timed DNS change, external load-balancer, BigIP DNS [iStore reference available?])

For exposing dev and test services, the wildcard ingress will still be available at *.apps.silver.devops.gov.bc.ca

#### Egress

Many applications also have dependencies on external (out of cluster) services.  If access to these services is restricted, external access rules may be required (ie: firewall rules to allow traffic from the silver cluster)

The new silver (and eventually gold) platforms will use a shared egress IP (or range), and restriction to an individual application by IP address will not be supported.  (this mimics the current pathfinder network model).

Extending secure communication from applications within the cluster to applications in either another cluster, or the existing datacentre networks is a service that is in development.

#### Internal namespace

Zero trust network security (inside the cluster) will require NetworkSecurityPolicy objects to be present in projects and namespaces to allow traffic to flow between Pods/deployments in a namespace.

With the starting stance of Zero Trust, there will no longer be restrictions on inter-namespace communication either.  Both namespaces will need corresponding NetworkSecurityPolicy rules to allow traffic between namespaces.

### Storage considerations

If your application leverages persistent storage, there are a few differences you can expect in the new **Silver** platform environment.

#### StorageClass changes

The key storageClass types will be available (Block/File).  StorageClass names are changing slightly (to better match the storage provisioning.)  You may need to modify your manifests to ensure you're using the new storageClasses names where applicable.

- netapp-block-standard
- netapp-file-standard

#### Enterprise backup integration

A big improvement for integration into the enterprise backup system can be found via a normal persistent volume request for a new storageClass: `netapp-file-backup` (instead of the service catalog provisioning)  Additional documentation on on this new storageClass can be found [at a link to be determined].

#### Migrating data

Existing persistent data will need to be copied from one cluster to another (see [https://github.com/bcdevops/StorageMigration] repository for a sample tool/approach).
