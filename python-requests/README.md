## Description ##

An example of how to make rest calls using python requests.
See http://cloudify-rest-client.readthedocs.io/en/3.3/

## Goal ##

examples for the following tasks:
+ Upload/delete blueprints
+ Create deployments with inputs
+ Detect if a deployment is already created
+ Detect if a deployment is in progress
+ Execute workflows
+ Get blueprints/deployments/executions list

## Prerequisites ##

+ Cloudify manager
+ Make sure your cloudify-rest-client is up-to-date (run -> pip install cloudify-rest-client==3.4)
+ Python version>=2.7.10

## Instructions ##

open your terminal and run -> python
all tasks should start with-
```
import requests
```

1) upload a blueprint to the manager (make sure the blueprint you upload matches your manager):
```
url = "http://<manager-ip>/api/v2.1/blueprints/sample-blueprint"
querystring = {"application_file_name":"sample-blueprint.yaml","blueprint_archive_url":"https://url/to/archive/master.zip"}
headers = {'content-type': "application/json"}
response = requests.request("PUT", url, headers=headers, params=querystring)
print(response.text)
```

2) delete a blueprint from the manager:
```
url = "http://<manager-ip>/blueprints/sample-blueprint"
headers = {'content-type': "application/json"}
response = requests.request("DELETE", url, headers=headers)
print(response.text)
```

3) get blueprints list:
```
url = "http://<manager-ip>/api/v2.1/blueprints"
querystring = {"_include":"id,created_at"}
headers = {'content-type': "application/json"}
response = requests.request("GET", url, headers=headers, params=querystring)
print(response.text)
```

4) create deployments with inputs (openstack example. make sure your inputs matches your blueprint):
```
url = "http://<manager-ip>/api/v2.1/deployments/sample-deployment"
payload = "{\"inputs\":{\"image\":\"your-image-id\",\"flavor\":\"your-flavor-id\",\"agent_user\":\"your-user\"}, \"blueprint_id\":\"sample-blueprint\"}"
headers = {'content-type': "application/json"}
response = requests.request("PUT", url, data=payload, headers=headers)
print(response.text)
```

5) detect if a deployment is already created:

```
url = "http://<manager-ip>/api/v2.1/deployments"
querystring = {"id":"sample-deployment","_include":"id,created_at"}
headers = {'content-type': "application/json"}
response = requests.request("GET", url, headers=headers, params=querystring)
print(response.text)
```

6) detect if a deployment is in progress:

```
url = "http://<manager-ip>/api/v2.1/executions/execution-id"
querystring = {"deployment_id":"sample-deployment","_include":"id,status,create_at"}
headers = {'content-type': "application/json"}
response = requests.request("GET", url, data=payload, headers=headers, params=querystring)
print(response.text)
```

7) get deployments list:
```
url = "http://<manager-ip>/api/v2.1/deployments"
querystring = {"blueprint_id":"sample-blueprint","_include":"id,created_at"}
headers = {'content-type': "application/json"}
response = requests.request("GET", url, headers=headers, params=querystring)
print(response.text)
```

8) get executions list:
```
url = "http://<manager-ip>/api/v2.1/executions"
querystring = {"deployment_id":"sample-deployment","_include":"id,created_at,workflow_id,status"}
headers = {'content-type': "application/json"}
response = requests.request("GET", url, data=payload, headers=headers, params=querystring)
print(response.text)
```

9) execute workflows (install):
```
url = "http://<manager-ip>/api/v2.1/executions"
payload = "{\"deployment_id\":\"sample-deployment\",\"workflow_id\":\"install\"}"
headers = {'content-type': "application/json"}
response = requests.request("POST", url, data=payload, headers=headers)
print(response.text)
```

10) cancel execution:
```
url = "http://<manager-ip>/api/v2.1/executions/execution-id"
payload = "{\"deployment_id\":\"sample-deployment\",\"action\":\"cancel\"}"
headers = {'content-type': "application/json"}
response = requests.request("POST", url, data=payload, headers=headers)
print(response.text)
```