# Learning objectives

After completing this exercise, you should be able to:

Configure the targets for Prometheus to monitor
Create queries to get the metrics about the target
Determine the status of the targets
Identify information about the targets and visualize it with graphs
Instrument a Python Flask application to be monitored by Prometheus

# Prerequisites

This lab uses Docker to run both Prometheus, and special Node Exporters, which will behave like servers that you can monitor. As a prerequisite, you will pull down the bitnami/prometheus:latest image and the bitnami/node-exporter image from Docker Hub. You will use these images to run Prometheus and create three instances of node exporters to be monitored

1.  use the following docker pull command to pull down the bitnami/node-exporter image from Docker Hub that you will use to simulate three servers being monitored.

docker pull bitnami/node-exporter:latest

2. Then, pull the Prometheus docker image into your lab environment, by running the following docker pull command in the terminal

docker pull bitnami/prometheus:latest

# Step 1: Start the first node exporter

The first thing you will need is some server nodes to monitor. You will start up three node exporters listening on port 9100 and forwarding to ports 9101, 9102, and 9103, respectively. Each node will need to be started up individually.

In this step, you will create a Docker network for all of the node exporters and Prometheus to communicate on, and start just the first node, 9101, and ensure it is working correctly.

# Your task

1. Start by running the following docker network command to create a network called monitor within which we will run all of the docker containers.

docker network create monitor

2. Next, run the following docker run command to start a node exporter instance on the monitor network, listening at port 9101 externally and forwarding to port 9100 internally.

docker run -d --name node-exporter1 -p 9101:9100 --network monitor bitnami/node-exporter:latest

3. Next, check if the instance is running by pressing the [Launch Application] button, which will launch the application on port 9101:

# Step 2: Start two more node exporters

Now that you have one node exporter working, you can start two more so that Prometheus has three nodes to monitor in total. You will do this the same way as you did the first node exporter, except that you will change the external port numbers to 9102 and 9103, respectively.

# Your task

1. In the terminal, run the following commands to start two more instances of node exporter.

docker run -d --name node-exporter2 -p 9102:9100 --network monitor bitnami/node-exporter:latest

and 

docker run -d --name node-exporter3 -p 9103:9100 --network monitor bitnami/node-exporter:latest

# Step 3: Configure and run Prometheus

Before you can start Prometheus, you need to create a configuration file called prometheus.yml to instruct Prometheus on which nodes to monitor.

In this step, you will create a custom configuration file to monitor the three node exporters running internally on the monitor network at node-exporter1:9100, node-exporter2:9100, and node-exporter3:9100, respectively. Then you will start Prometheus by passing it the configuration file to use.

# Your Task

1. First, use the touch command to create a file named prometheus.yml in the current directory. This is the file where you will configure the Prometheus to monitor the node exporter instances.

touch prometheus.yml

2. Then, copy and paste the following configuration contents into the yaml file and save it:

===========================================================

# my global config
global:
  scrape_interval: 15s # Set the scrape interval to every 15 seconds. The default is every 1 minute.

scrape_configs:
  - job_name: 'node'
    static_configs:
      - targets: ['node-exporter1:9100']
        labels:
          group: 'monitoring_node_ex1'
      - targets: ['node-exporter2:9100']
        labels:
          group: 'monitoring_node_ex2'
      - targets: ['node-exporter3:9100']
        labels:
          group: 'monitoring_node_ex3'

=============================================================

Take a look at what this file is doing:

Globally, you set the scrape_interval to 15 seconds instead of the default of 1 minute. This is so that we can see results quicker during the lab, but the 1 minute interval is better for production use.
The scrape_config section contains all the jobs that Prometheus is going to monitor. These job names have to be unique. You currently have one job called node. Later we will add another to monitor a Python application.
Within each job, there is a static_configs section where you define the targets and define labels for easy identification and analysis. These will show up in the Prometheus UI under the Targets tab.
The targets you enter here point to the base URL of the service running on each of the nodes. Prometheus will add the suffix /metrics and call that endpoint to collect the data to monitor from. (For example, node-exporter1:9100/metrics)

3. Finally, you can launch the Prometheus monitor in the terminal by executing the following docker run command passing the yaml configuration file as a volume mount with the -v parameter.

docker run -d --name prometheus -p 9090:9090 --network monitor \
-v $(pwd)/prometheus.yml:/opt/bitnami/prometheus/conf/prometheus.yml \
bitnami/prometheus:latest

# Step 4: Open the Prometheus UI

Check the images

# Your task

1. Ensure you are on the Graph tab, and then copy-n-paste the following query and press the blue Execute button on the right or press return on your keyboard to run it. It will show the graph as given in the image. You can observe the details for each instance by hovering the mouse over that instance.

node_cpu_seconds_total

2. Now, filter the query to get the details for only one instance node-exporter2 using the following query

node_cpu_seconds_total{instance="node-exporter2:9100"}

3. Finally, query for the connections each node has using this query.

node_ipvs_connections_total

# Step 6 : Enable your application

Monitoring node exporters is fine for a demonstration, but you are a software engineer. You need to know how to enable your applications to be monitored by Prometheus. There is no magic here. Metrics do not simply appear out of nowhere. You must instrument your application to emit metrics on an endpoint called /metrics in order for Prometheus to be able to monitor your application.

1. create a file named pythonserver.py

2. Then, paste the following code content into it:

================================================================
from prometheus_flask_exporter import PrometheusMetrics
from flask import Flask

app = Flask(__name__)
metrics = PrometheusMetrics.for_app_factory()
metrics.init_app(app)

@app.route('/')
def root():
    return 'Hello from root!'

@app.route('/home')
def home():
    return 'Hello from home!'

@app.route('/contact')
def contact():
    return 'Contact us!'

if __name__ == '__main__':
    app.run(host="0.0.0.0", port=8080)
================================================================

Notice that you only had to import the PrometheusMetrics class from the prometheus_flask_exporter package and add two lines of code to instantiate a PrometheusMetrics.for_app_factory() as metrics, and call metrics.init_app(app) to initialize it. That is it! Three total lines of code, and you have Prometheus support!

3. Next, you need to deploy this code on the same docker network as Prometheus. To do this, create a file named Dockerfile

4. Paste the following contents into Dockerfile and save it:

================================================================

FROM python:3.9-slim
RUN pip install Flask prometheus-flask-exporter
WORKDIR /app
COPY pythonserver.py .
EXPOSE 8080
CMD ["python", "pythonserver.py"]

================================================================

5. Finally, run the pythonserver Docker container on the monitor network exposing port 8080 so that Prometheus will have access to it:

docker run -d --name pythonserver -p 8081:8080 --network monitor pythonserver

# Step 7: Reconfigure Prometheus

Now that you have your application running, it is time to reconfigure Prometheus so that it knows about the new pythonserver node to monitor. Y
ou can do this by adding the Python server as a target in your prometheus.yml file.

1. Next, create a new job to monitor the pythonserver service that is listening on port 8080. Use the previous job as an example.

================================================================
  - job_name: 'monitorPythonserver'
    static_configs:
      - targets: ['pythonserver:8080']
        labels:
          group: 'monitoring_python'
================================================================

docker restart prometheus

# Step 8: Monitor your application

1. Make multiple requests to the three endpoints of the Python server you created in the previous task and observe these calls on Prometheus.

curl localhost:8081
curl localhost:8081/home
curl localhost:8081/contact

2. Use the Prometheus UI to query for the following metrics.

flask_http_request_duration_seconds_bucket
flask_http_request_total
process_virtual_memory_bytes

