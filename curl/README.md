## Description ##

An example of how to make rest calls using curl.
See http://docs.getcloudify.org/api/#cloudify-rest-api-v2

## Goal ##

examples for the following tasks:
+ Upload/delete blueprints
+ Create deployments with inputs
+ Detect if a deployment is already created
+ Detect if a deployment is in progress
+ Execute workflows
+ Get blueprints/deployments/executions list

## Prerequisites ##

+ OpenStack/AWS manager

## Instructions ##

open your terminal and cd /manager/

1) upload a blueprint to the manager (make sure the blueprint you upload match your manager openstack/aws):
```
curl -X PUT "<manager-ip>/api/v2.1/blueprints/sample-blueprint?application_file_name=blueprint-name.yaml&blueprint_archive_url=https://url/to/archive/master.zip"
```

2) delete a blueprint from the manager:
```
curl -X DELETE <manager-ip>/blueprints/sample-blueprint
```

3) get blueprints list:
```
curl -X GET "<manager-ip>/api/v2.1/blueprints?_include=id,created_at"
```

4) create deployments with inputs (for aws flavor=size/type of the instance):
```
curl -X PUT -H "Content-Type: application/json" -d '{"inputs":{"image":"your-image-id","flavor":"your-flavor-id","agent_user":"your-user"}, "blueprint_id":"sample-blueprint"}' "<manager-ip>/api/v2.1/deployments/sample-dep"
```

5) detect if a deployment is already created:
```
curl -X GET <manager-ip>/api/v2.1/deployments?id=sample-dep&_include=id,created_at
```

6) detect if a deployment is in progress:

```
curl -X GET "<manager-ip>/api/v2.1/executions/execution-id?deployment_id=sample-dep&_include=id,status,created_at"
```

7) get deployments list:
```
curl -X GET "<manager-ip>/api/v2.1/deployments?blueprint_id=sample-blueprint&_include=id,created_at"
```

8) get executions list:
```
curl -X GET "<manager-ip>/api/v2.1/executions?deployment_id=sample-dep&_include=id,status,workflow_id,created_at"
```

9) execute workflows (install):
```
curl -X POST -H "Content-Type: application/json" -d '{"deployment_id":"sample-dep","workflow_id":"install"}' "<manager-ip>/api/v2.1/executions"
```

10) cancel execution:
```
curl -X POST -H "Content-Type: application/json" -d '{"deployment_id":"sample-dep","action":"cancel"}' "<manager-ip>/api/v2.1/executions/execution-id"
```