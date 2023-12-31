# mtc-showcase
This repository is used to showcase the capabilities of the Migration Toolkit for Containers (MTC).

In the ```base``` directory, you will find the default objects needed for the MTC Operator to work, such as: <br>
* ```Namespace```; <br>
* ```OperatorGroup```; <br>
* ```Subscription```; <br>
* ```MigrationController``` (installed CRD). <br>

In the ```overlays``` directory, you will find the various host types:

* ```host```, the OCP Cluster where the Migration Controller / UI will be located (usually the target cluster, but can be an external one):
  * ```ObjectBucketClaim``` because an S3 bucket is required by the migration process (note: we are using OpenShift Data Foundation as the S3 provider in this example, which is provided here: https://github.com/lpanza/multicluster-devsecops);
  * a Kustomize patch to add the Migration Controller and Migration UI capabilities to the ```MigrationController``` already defined in ```base```.
* ```source```, the OCP Cluster where the migration process will start:
  * ```test-migration Namespace``` where we are going to set up our workload;
  * ```mariadb``` all the required objects to create a stateful application based on MariaDB;
    * This directory also contains a job to automatically create a table and fill it with test rows;
  * ```nginx``` all the required objects to create a stateless application based on Nginx;
* ```target``` the OCP Cluster where the source workload will be migrated, so the overlay just consists of the ```base``` resources. <br>

In order to apply the changes to the desired cluster scope, run:
```
### Using Kustomize
kustomize build overlays/<role>/ | oc apply -f -

### Using the oc binary
oc apply -k overlays/<role>/
```
