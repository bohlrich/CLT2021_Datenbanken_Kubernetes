# CLT2021_Datenbanken_Kubernetes
Die Inhalte zum Vortrag "Datenbanken auf Kubernetes" bei den Chemnitzer Linux Tagen 2021

Systeme
=======

* OpenShift Container Platform 4.6
* OpenShift Pipelines
* ArgoCD

Deployment
==========

SQL Server Big Data Cluster
---------------------------

"azdata" installieren: https://docs.microsoft.com/de-de/sql/azdata/install/deploy-install-azdata?view=sql-server-ver15

OpenShift SCC für SQL Server Big Data Cluster installieren:

```bash
curl https://raw.githubusercontent.com/microsoft/sql-server-samples/master/samples/features/sql-big-data-cluster/deployment/openshift/bdc-scc.yaml -o bdc-scc.yaml
oc create -f bdc-scc.yaml
```
Neues Project (Namespace) in OpenShift für SQL BDC anlegen

```bash
oc new-project mssqlbdc
```

Liste mit verfügbaren BDC Konfigurationen anzeigen:

```bash
azdata bdc config list
```

Im Beispiel wird ein "openshift-dev-test" erstellt:

```bash
azdata bdc config init --source openshift-dev-test --target custom-openshift
```

Bearbeiten des Clusternamen in den JSON Dateien "bdc.json" und "control.json"

Erstellen des Clusters:

```bash
azdata bdc create --config custom-openshift --accept-eula yes
```

Nach ca. 40 - 60 Minuten sollte der Cluster fertig deployed sein.

MvcMovie3 Demo Applikation
--------------------------

Neues Projekt für die Demo Applikation erstellen:

```bash
oc new-project cltsqldemoapp
```

Pipeline erstellen

```bash
cd ./tekton
oc create -f .
```


