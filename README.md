# Coolstore Application on EAP 8

This repository contains updates to run the **Coolstore Application** on **JBoss EAP 8** in an OpenShift environment.

## Deployment Guide

To deploy and run the Coolstore application on OpenShift, follow the detailed steps outlined in the Red Hat Developer article:

[How to migrate a complex JBoss EAP application to OpenShift](https://developers.redhat.com/articles/2023/08/23/how-migrate-complex-jboss-eap-application-openshift#testing_the_application_by_deploying_on_openshift)

Additionally, connect to the POSTGRES DB from pod terminal and execute the 2 .sql files available under `src/main/resources/db/migration` folder

## Key Highlights

- **Updated for JBoss EAP 8**: The application has been updated to leverage the latest features and capabilities of JBoss EAP 8.
- **Optimized for OpenShift**: Modifications ensure seamless deployment and performance in OpenShift environments.

For more information, refer to the linked article or explore the repository for specific configuration and deployment files.

## Additional references

- https://developers.redhat.com/learning/learn:java:develop-modern-java-applications-jboss-eap-8/resource/resources:deploy-modern-jboss-eap-8-application-openshift
- https://developers.redhat.com/articles/2024/11/08/jboss-eap-images-galleon#
