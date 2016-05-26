## Description ##

An example of how to make rest calls using jquery AJAX.

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
+ JQuery AJAX installed

## Instructions ##

1) upload a blueprint to the manager (make sure the blueprint you upload match your manager openstack/aws):
```
var settings = {
  "async": true,
  "crossDomain": true,
  "url": "http://<manager-ip>/api/v2.1/blueprints/sample-blueprint?application_file_name=blueprint-name.yaml&blueprint_archive_url=https://url/to/archive/master.zip",
  "method": "PUT",
  "headers": {"content-type": "application/json"},
  "processData": false
}

$.ajax(settings).done(function (response) {
  console.log(response);
});
```

2) delete a blueprint from the manager:
```
var settings = {
  "async": true,
  "crossDomain": true,
  "url": "http:<manager-ip>/blueprints/sample-blueprint",
  "method": "DELETE",
  "headers": {"content-type": "application/json"},
  "processData": false
}

$.ajax(settings).done(function (response) {
  console.log(response);
});
```

3) get blueprints list:
```
var settings = {
  "async": true,
  "crossDomain": true,
  "url": "http://<manager-ip>/api/v2.1/blueprints?_include=id%2Ccreated_at",
  "method": "GET",
  "headers": {"content-type": "application/json"},
  "processData": false
}

$.ajax(settings).done(function (response) {
  console.log(response);
});
```

4) create deployments with inputs (for aws flavor=size/type of the instance):
```
var settings = {
  "async": true,
  "crossDomain": true,
  "url": "http://<manager-ip>/api/v2.1/deployments/sample-dep",
  "method": "PUT",
  "headers": {"content-type": "application/json"},
  "processData": false,
  "data": "{\"inputs\":{\"image\":\"your-image-id\",\"flavor\":\"your-flavor-id\",\"agent_user\":\"your-user\"}, \"blueprint_id\":\"sample-blueprint\"}"
}

$.ajax(settings).done(function (response) {
  console.log(response);
});
```

5) detect if a deployment is already created:

```
var settings = {
  "async": true,
  "crossDomain": true,
  "url": "http://<manager-ip>/api/v2.1/deployments?id=sample-dep&_include=id%2Ccreated_at",
  "method": "GET",
  "headers": {"content-type": "application/json"},
  "processData": false
}

$.ajax(settings).done(function (response) {
  console.log(response);
});
```

6) detect if a deployment is in progress:

```
var settings = {
  "async": true,
  "crossDomain": true,
  "url": "http://<manager-ip>/api/v2.1/executions/execution-id?deployment_id=sample_dep&_include=id%2Cstatus%2Ccreated_at",
  "method": "GET",
  "headers": {"content-type": "application/json"},
  "processData": false
}

$.ajax(settings).done(function (response) {
  console.log(response);
});
```

7) get deployments list:
```
var settings = {
  "async": true,
  "crossDomain": true,
  "url": "http://<manager-ip>/api/v2.1/deployments?blueprint_id=java-blueprint&_include=id%2Ccreated_at",
  "method": "GET",
  "headers": {"content-type": "application/json"},
  "processData": false
}

$.ajax(settings).done(function (response) {
  console.log(response);
});
```

8) get executions list:
```
var settings = {
  "async": true,
  "crossDomain": true,
  "url": "http://<manager-ip>/api/v2.1/executions?deployment_id=sample-dep&_include=id%2Ccreated_at%2Cworkflow_id",
  "method": "GET",
  "headers": {"content-type": "application/json"},
  "processData": false
}

$.ajax(settings).done(function (response) {
  console.log(response);
});
```

9) execute workflows (install):
```
var settings = {
  "async": true,
  "crossDomain": true,
  "url": "http://<manager-ip>/api/v2.1/executions",
  "method": "POST",
  "headers": {"content-type": "application/json"},
  "processData": false,
  "data": "{\"deployment_id\":\"dep2\",\"workflow_id\":\"install\"}"
}

$.ajax(settings).done(function (response) {
  console.log(response);
});
```

10) cancel execution:
```
var settings = {
  "async": true,
  "crossDomain": true,
  "url": "http://<manager-ip>/api/v2.1/executions/execution-ip",
  "method": "POST",
  "headers": {"content-type": "application/json"},
  "processData": false,
  "data": "{\"deployment_id\":\"dep2\",\"action\":\"cancel\"}"
}

$.ajax(settings).done(function (response) {
  console.log(response);
});
```
