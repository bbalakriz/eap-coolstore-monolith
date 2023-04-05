# CoolStore Monolith

This repository has the complete coolstore monolith built as a Java EE 7 application. To deploy it on OpenShift follow the instructions below

## Pre requisite

* OpenShift 4.12 with cluster admin access
* OpenShift CLI, logged into cluster with cluster admin access

## Create project

```
oc new-project coolstore
```

## Deploy RH SSO operator

```
oc apply -f ./openshift/sso-operator.yml
```

Wait for RH SSO Operator to be deployed (waiting until the ClusterServiceVersion's `PHASE` is set to `Suceeded`)

```
oc get csv -w
```

Once the RH-SSO operator is installed, it will display:

```
NAME                           DISPLAY                           VERSION         REPLACES                       PHASE
rhsso-operator.7.6.2-opr-001   Red Hat Single Sign-On Operator   7.6.2-opr-001   rhsso-operator.7.6.1-opr-005   Succeeded
```

## Deploy & configure the RH SSO instance

```
oc apply -f ./openshift/sso.yml
```

Run `oc get route keycloak ` (it may take a few attempts for route to be created) and update `KEYCLOAK_URL` value in `helm.yaml` with correct route for SSO.
** Make sure to prepend `https://` and append `/auth`to this URL.**. The value should look like:

```
  - name: KEYCLOAK_URL
    value: https://keycloak-coolstore.apps.92393e11c4ffbef7e179.hypershift.aws-2.ci.openshift.org/auth
````

## Deploy PostgreSQL database

```
oc apply -f ./openshift/psql.yml
```

## Install Active MQ broker operator

```
oc apply -f ./openshift/amq-broker-operator.yml
```


Wait for  AMQ Broker to be deployed (waiting until the ClusterServiceVersion's `PHASE` is set to `Suceeded`)

```
oc get csv -w
```

Once the RH-SSO operator is installed, it will display:

```
NAME                           DISPLAY                           VERSION         REPLACES                       PHASE
amq-broker-operator.v7.10.2-opr-2-0.1676475747.p   Red Hat Integration - AMQ Broker for RHEL 8 (Multiarch)   7.10.2-opr-2+0.1676475747.p   amq-broker-operator.v7.10.2-opr-1   Succeeded
```

## Create and configure Active MQ broker instance

```
oc apply -f ./openshift/amq-broker.yml
```

## Deploy application

Create config map from cm.yaml

```
oc apply -f ./openshift/cm.yaml
```

From the developer UI, click on "+Add", then "Helm Chart", and select the "Eap74" Helm chart.

Paste the contents of `openshift/helm.yml` as the config.

## Testing the application

Once the application is running you should be able to access it via the external route. From the application, click on "Sign in" link at the top right hand corner.  You should be brought to the RH SSO login, login with the credentials: `user1` / `pass`

You should now be able to add products to your cart and complete the checkout process.

