
Airflow Helm Chart
=======================

Author: Mike Pieters (me@mikepieters.com)

This Airflow helm chart is meant as a basis for your own helm chart. I made this as other Helm charts were often to complex to know what is going on exactly.
It is simple enough to understand and easy to extend, so that you can create a realiable deployment of Airflow yourself.
This helm chart will bootstrap a complete airflow cluster. It is as simple as possible by design. 

Prerequisites
-------------
- Running Minikube Cluster (start with `minikube start`)
- Kubectl (To interact with the Kubernetes API)
- Helm (To install helm-packages like this)
- A custom value file

Install
-------

Remove any previous versions from Airflow by (note: [] is OPTIONAL):
```
helm uninstall airflow [-n airflow-build]
```

Note: This will install airflow in the `airflow-build` namespace. So add `-n airflow-build` to your command to retreive Kubernetes object from the correct namespace.

```
> helm dependency update
> helm install airflow . --values ./environment_value_files/build.yaml --set environment.development=true -n airflow-build --create-namespace
```

This will run the `database_migration` and `create_user` job during install. You can check the status of the jobs by executing `kubectl get jobs -n airflow-build` or `kubectl get pods -n airflow-build` in a different terminal.

After the jobs are finished, you can check whether the `airflow-webserver` service is up by running: `kubectl get services`.

Expose service with Minikube
----------------------------

You can expose the `airflow-webserver` service by running:

```
> minikube service airflow-webserver -n airflow-build
```

This will start the Airflow GUI. The default login is admin/admin.
