# Planning your migration

Get an early look at the new platform technology:

- [docs.openshift.com](https://docs.openshift.com/container-platform/latest/welcome/index.html)
- [learn.openshift.com](https://learn.openshift.com) for hands on!
- Add links to more resources:

## Migration Considerations

As the availability date of the new platform gets closer, preparation and planning can start for the work that will need to be done to migrate applications from the current Pathfinder service to the new Silver platform service.  This new Silver platform service is the next step in the Container Platform service journey for BC Gov, taking what we learned during the Pathfinder journey and using that learning to begin the creation of a broader Enterprise service.  With the creation of the new service, we also begin the process to decommission the current pathfinder platform.  All application services hosted on the Pathfinder platform will need to migration to the new enterprise service to continue their service delivery.

### Staff and timeline considerations

Migrating applications can be an intense activity.  With many moving parts, and lots of details and challenges waiting to surprise you, having a team that understands both your application details as well as your deployment approaches will be a key success factor.  This is an activity that will test and stretch the staff that energize your DevOps roles.

The timeline for the migration will be aggressive, beginning with the help of our most experienced Developer teams in the early Fall, with the goal for completion within X months.

### Network considerations

#### Ingress

Whether you already manage an application vanity URL (eg: my-awesome-app.gov.bc.ca) or you currently leverage the pathfinder.gov.bc.ca wildcard (my-justasawesome-app.pathfinder.gov.bc.ca), you will need to plan how people will find your app once it's moved.  With the move to an enterprise service, the platform wildcard has been deemed unsuitable for production application deployments.  This means if you don't have a vanity URL for your application yet, you will want to get started on provisioning one.  If you already use a vanity URL, you'll want to plan how to switch the name to the new platform.  (Timed DNS change, external load-balancer, BigIP DNS [iStore reference available?])

For exposing tools, dev and test services, the wildcard ingress will still be available at *.apps.silver.devops.gov.bc.ca

#### Egress

Many applications also have dependencies on external (out of cluster) services.  If access to these services is restricted, external access rules may be required (ie: firewall rules to allow traffic from the silver cluster)

The new Silver (and eventually gold) platforms will use a shared egress IP (or range), and restriction to an individual application by IP address will not be supported.  (this mimics the current pathfinder network model).

Extending secure communication from applications within the cluster to applications in either another cluster, or the existing datacentre networks is in development.

#### Internal namespace

Zero trust network security (inside the cluster) will require NetworkSecurityPolicy objects to be present in projects and namespaces to allow traffic to flow between Pods/deployments in a namespace.

With the starting stance of Zero Trust, there will no longer be restrictions on inter-namespace communication either.  Both namespaces will need corresponding NetworkSecurityPolicy rules to allow traffic between namespaces.  Please see [Developer Guide to Zero Trust](https://developer.gov.bc.ca/Platform-Services-Security/Developer-Guide-to-Zero-Trust-Security-Model-on-the-Platform) for a more detailed look.

### Storage considerations

If your application leverages persistent storage, there are a few differences you can expect in the new **Silver** platform environment.

#### StorageClass changes

The key storageClass types will be available (Block/File).  StorageClass names are changing slightly (to better match the storage provisioning.)  You may need to modify your manifests to ensure you're using the new storageClasses names where applicable.

- netapp-block-standard
- netapp-file-standard

#### Enterprise backup integration

A big improvement for integration into the enterprise backup system can be found via a normal persistent volume request for a new storageClass: `netapp-file-backup` (instead of the service catalog provisioning)  Additional documentation on on this new storageClass can be found here: [OpenShift 4 - Backup and Restore](https://developer.gov.bc.ca/OCP4-Backup-and-Restore).

#### Migrating data

Existing persistent data will need to be copied from one cluster to another (Sample soon to come to [https://github.com/bcdevops/StorageMigration] repository).

### Build (Docker) considerations

OpenShift 4 now uses podman/buildah instead of docker to build images.  You may encounter differences and irregularities with more complex docker builds.  The following are a couple of great resources for getting started with podman/buildah.

- <https://www.openshift.com/blog/openshift-4-image-builds>
- <https://developers.redhat.com/blog/2019/02/21/podman-and-buildah-for-docker-users>
- <https://buildah.io>
- <https://developers.redhat.com/blog/2019/08/14/best-practices-for-running-buildah-in-a-container/>
