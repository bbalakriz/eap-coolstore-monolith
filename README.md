# CoolStore Monolith

This repository has the complete coolstore monolith built as a Java EE 7 application. To deploy it on OpenShift follow the instructions below

## Pre requisite

* OpenShift 4.12 with cluster admin access
* OpenShift CLI, logged into cluster with cluster admin access

## Create project

```
oc new-project coolstore
```

## Deploy the RH-SSO anD AMQ Operators

```
oc apply -f ./openshift/operators
```

Wait for both operators to to be deployed (waiting until their ClusterServiceVersion's `PHASE` is set to `Suceeded`)

```
oc get csv -w
```

Once they are installed, it will display:

```
NAME                           DISPLAY                           VERSION         REPLACES                       PHASE
rhsso-operator.7.6.2-opr-001   Red Hat Single Sign-On Operator   7.6.2-opr-001   rhsso-operator.7.6.1-opr-005   Succeeded
amq-broker-operator.v7.10.2-opr-2-0.1676475747.p   Red Hat Integration - AMQ Broker for RHEL 8 (Multiarch)   7.10.2-opr-2+0.1676475747.p   amq-broker-operator.v7.10.2-opr-1   Succeeded
```

## Deploy the RH-SSO instance, the AMQ Broker and the PostgreSQL

```
oc apply -f ./openshift/resources
```

Run `oc get route keycloak ` (it may take a few attempts for route to be created) and update `KEYCLOAK_URL` value in `helm.yaml` with correct route for SSO.
** Make sure to prepend `https://` and append `/auth`to this URL.**. The value should look like:

```
  - name: KEYCLOAK_URL
    value: https://keycloak-coolstore.apps.92393e11c4ffbef7e179.hypershift.aws-2.ci.openshift.org/auth
````

## Deploy the application

Create config map from cm.yaml

```
oc apply -f ./openshift/app/cm.yaml
```

From the developer UI, click on "+Add", then "Helm Chart", and select the "Eap74" Helm chart.

Paste the contents of `openshift/app/helm.yml` as the config.

## Testing the application

Once the application is running you should be able to access it via the external route. From the application, click on "Sign in" link at the top right hand corner.  You should be brought to the RH SSO login, login with the credentials: `user1` / `pass`

You should now be able to add products to your cart and complete the checkout process.

