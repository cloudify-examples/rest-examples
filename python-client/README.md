## Description ##

An example of how to make rest calls using python client.
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
from cloudify_rest_client import CloudifyClient
client = CloudifyClient('<manager-ip>')
```

1) upload a blueprint to the manager (make sure the blueprint you upload matches your manager):
```
client.blueprints._upload(archive_location='https://url/to/archive/master.zip',blueprint_id='sample-blueprint',application_file_name='blueprint-name.yaml')
```

2) delete a blueprint from the manager:
```
client.blueprints.delete(blueprint_id='sample-blueprint')
```

3) get blueprints list:
```
blueprints = client.blueprints.list(_include=['id','created_at'])
for blueprint in blueprints:
  print blueprint
```

4) create deployments with inputs (openstack example. make sure your inputs matches your blueprint):
```
client.deployments.create(blueprint_id='sample-blueprint', deployment_id='sample-dep', inputs={'image':'your-image-id','flavor':'your-flavor-id','agent_user':'your-user'})
```

5) detect if a deployment is already created:

```
print client.deployments.get(deployment_id='sample-dep',_include=['id','created_at'])
```

6) detect if a deployment is in progress:

```
print client.executions.get(execution_id='execution-id',_include=['id','created_at','status'])
```

7) get deployments list:
```
deployments = client.deployments.list(blueprint_id='sample-blueprint',_include=['id','created_at'])
for deployment in deployments:
  print deployment
```

8) get executions list:
```
executions = client.executions.list(deployment_id='sample-dep',_include=['id','status','workflow_id','created_at'])
for execution in executions:
  print execution
```

9) execute workflows (install):
```
client.executions.start(deployment_id='sample-dep', workflow_id='install')
```

10) cancel execution:
```
client.executions.cancel(execution_id='execution-id')
```