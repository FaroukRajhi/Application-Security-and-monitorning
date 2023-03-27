# Learning objectives

Deploy Prometheus to OpenShift
Deploy Grafana to OpenShift
Connect Prometheus as a datasource for Grafana
Create a dashboard with Grafana

git clone https://github.com/ibm-developer-skills-network/ondaw-prometheus-grafana-lab.git

cd ondaw-prometheus-grafana-lab

# Step 1: Deploy node exporters

oc create deployment node-exporter1 --port=9100 --image=bitnami/node-exporter:latest
oc create deployment node-exporter2 --port=9100 --image=bitnami/node-exporter:latest
oc create deployment node-exporter3 --port=9100 --image=bitnami/node-exporter:latest

1. use the oc expose command to create services that expose the 3 node exporters so that Prometheus can communicate with them:

oc expose deploy node-exporter1 --port=9100 --type=ClusterIP
oc expose deploy node-exporter2 --port=9100 --type=ClusterIP
oc expose deploy node-exporter3 --port=9100 --type=ClusterIP

# Step 2: Deploy Prometheus

In this step, you will confiugure and deploy Prometheus. While normally you would modify configuration files to configure Prometheus, this is not the case for Kubernetes. For a Kubernetes environment the proper appoach is to use a ConfigMap. This makes it easy to change the configuraton later. You will find the configuration files from which to make the ConfigMap in a folder named ./config.

You will also need Kubernetes manifests to describe the Prometheus deployment and to link the ConfigMap with the Prometheus. These manifests can be found in the ./deploy folder.

1. Create a configmap

oc create configmap prometheus-config \
   --from-file=prometheus=./config/prometheus.yml \
   --from-file=prometheus-alerts=./config/alerts.yml
2. deploy prometheus

oc apply -f deploy/prometheus-deployment.yaml

# Step 3: Deploy Grafana

Now that you have 3 node exporters to emit metrics, and Prometheus to collect them, it is time to add Grafana for dashboarding. You will deploy Grafana into OpenShift and create a route which will allow you to open up the Grafana web UI and work with it.

oc apply -f deploy/grafana-deployment.yaml

1. se the oc expose command to expose the grafana service with an OpenShift route. Routes are a special feature of OpenShift that makes it easier to use for developers.

oc expose svc grafana

2. Use the following oc patch command to enable TLS and the https:// protocol for the route.

oc patch route grafana -p '{"spec":{"tls":{"termination":"edge","insecureEdgeTerminationPolicy":"Redirect"}}}'

oc get routes

# Step 4: Log in to Grafana

Now that Prometheus and Grafana are deployed and running, it is time to configure Grafana. In order to do this, you will need the URL from the route that you created in the last step.

1. Use the oc describe command along with grep to extract the URL of the Requested Host for the grafana route:

oc describe route grafana | grep "Requested Host:"

2. Copy the URL after the words “Requested Host:” to the clipboard and paste it into a new web browser window outside of the lab environment.

3. From the Grafana login screen, log in with the default userid admin and default password admin, and then click the [Log in] button. You will be prompted to change your password. You can press the skip link to bypass that for now.

# Step 5: Configure Grafana

Follow th images to get the instructions

# Step 6: Create a Dashboard

Now you can create your first dashboard. For this lab, you will use a precreated template provided by Grafana Dashboard. This template is identified by the id 1860.

1. On the Grafana homepage, click the Dashboards icon, and select + Import from the menu to start creating the dashboard.

2. Next,enter the template identifier id 1860 in the space provided and click the [Load] button to import that dashboard from Grafana.

3. You will see the default name for the template, Node Exporter Full, displayed. You are allowed to change it if you want to. The change will be local and valid only for your instance.

# Step 7: view the dashboard

You should now see the General / Node Exporter Full dashboard which shows CPU Busy, Sys Load, RAM used and other information. This dashboard will allow you to get a broad overview of how the system is performing, as well as drilling down into individual nodes to see how they are performing. Now, try a few things out.

