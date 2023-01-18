# Gitlab_from_Scratch
GitLab is an open source code repository and collaborative software development platform for large DevOps and DevSecOps projects.
The repository enables hosting different development chains and versions, and allows users to inspect previous code and roll back to it in the event of unforeseen problems.

This project is an effort to reduce the .gitlab-ci.yml code duplication across the IaC pipelines.
It consolidates the DI code from existing Kubernetes and Terraform job configurations. Those job configurations depend on the Gitlab runner configurations which


ARE NOT captured by this project
Are undocumented, change frequently and are unstable

It is likely that the pipelines and jobs provided here will as a result be unstable.

Common Pipeline
This project provides a library of GitLab CI Pipeline definitions and template jobs that can be included into other client pipeline files.

CI Pipelines

Helm Pipeline
Terraform Pipeline
Application Jobs
Reporting is integrated in the CI pipelines / jobs, see Reporting



CD Template Jobs

Deployment Jobs


Jobs Library

Template Jobs library


RDS utils

RDS utils library




Layout

The Continious Delivery job definitions are under cdjobs.
The Continious Integration job definitions are under cijobs.
Common configuration goes under config.
As GitlabCI only allows YAML anchors to be used within the file they were defined in. All the reusabe YAML elements are defined in jobs/lib.yml and the hidden jobs also defined there using those elements are then available for include in other files.
README for each of the Pipeline definitions can be found in the respective folder.


Pipeline Approach
The pipelines in this library will define a number of Jobs which operate on documented variables.

Convention over Configuration
A number of the variables will have a documented default value. This provides a convention over configuration approach.
If you follow the conventional structure then you don't need to reconfigure.
In the example below the helm_validate job will expect to find the helm chart in a directory called helm-charts.
If you follow that convention then you don't need to set that variable.



Stage
Job
Variable
Default
Description




validate
helm-validate


lints on $CP_HELM_CHART_DIR/$CP_HELM_CHART





CP_HELM_CHART_DIR
helm-charts





CP_HELM_CHART

must be set in client pipeline




Reconfiguration
If however you must reconfigure you can override the default by setting the variable at the job level in the client pipeline.
So taking the example above

helm-validate:
  variables:
    CP_HELM_CHART_DIR: charts



Setting variables in the Client Pipeline
Some jobs will require you to set a variable in the client pipeline. The aim is for these to be defined globally (not inside a job) in the client pipeline. Setting the values this way means you do not have to have a key for each job from the include.
(This assumes you don't need to set the tags key for your jobs to execute )

Naming Conventions
The variables used in this library follow the naming convention CP_{NAMESPACE}_{VARIABLE}.


CP_ for Common Pipeline

{NAMESPACE} to indicate where these variables are used. The aim is to match the name of the file where they are defined or referenced

{VARIABLE} to indicate the purpose of the variable.

Where variables were in widespread use in client pipelines for tasks now incorporated into this library the original names have been retained.
