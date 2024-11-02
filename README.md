# Repository for Kubernetes liveness and readiness probe solutions for enhanced health monitoring

![workflow-diagram](https://github.com/user-attachments/assets/cfa10df7-c079-48c3-8516-73d469afe4c6)


# Workflow Diagram:
##############################################################################################

## What are Probes?

Kubernetes Probes are health checks that allow the platform to manage the lifecycle of your Pods. There are two main types of probes:

1. **Readiness Probe:** Checks if a container is ready to start accepting traffic.
2. **Liveness Probe:** Checks if a container is still alive and should be restarted if it fails.

## Why Use Probes?

Probes help in maintaining the reliability and availability of your applications. 

- **Readiness Probe:** Ensures that your service doesn't receive traffic until it's fully ready.
- **Liveness Probe:** Detects when your container is in a bad state and needs a restart to recover.


## How Probes Works?
![probes](https://github.com/user-attachments/assets/27b036cd-ca92-4634-9c02-e0250a10853b)


## How to Implement Probes

### Readiness Probe

A Readiness Probe determines if a container is ready to handle requests. If the probe fails, the container is temporarily removed from service. 

**Example:**

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: readiness-example
spec:
  containers:
  - name: myapp
    image: myapp:latest
    readinessProbe:
      httpGet:
        path: /healthz
        port: 8080
      initialDelaySeconds: 5
      periodSeconds: 10
```

In this example:

- **httpGet:** The probe sends an HTTP GET request to `/healthz` on port `8080`.
- **initialDelaySeconds:** The probe waits 5 seconds before starting checks.
- **periodSeconds:** The probe checks the endpoint every 10 seconds.

### Liveness Probe

A Liveness Probe ensures that your container is running properly. If the probe fails, Kubernetes will restart the container.

**Example:**

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: liveness-example
spec:
  containers:
  - name: myapp
    image: myapp:latest
    livenessProbe:
      httpGet:
        path: /healthz
        port: 8080
      initialDelaySeconds: 3
      periodSeconds: 5
```

In this example:

- **httpGet:** Similar to the Readiness Probe, but used to determine if the container should be restarted.
- **initialDelaySeconds:** The probe starts checking 3 seconds after the container starts.
- **periodSeconds:** The probe checks the endpoint every 5 seconds.

## Practical Example

Imagine you have a web server that takes a few seconds to start. You can use a Readiness Probe to ensure it’s ready before receiving traffic. If your web server crashes or becomes unresponsive, the Liveness Probe will restart it automatically.

### Sample Application Deployment

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  labels:
    app: app1
  name: app1
spec:
  replicas: 3
  selector:
    matchLabels:
      app: app1
  template:
    metadata:
      labels:
        app: app1
    spec:
      containers:
        - image: rajpractise/kubegame:v1
          name: kubegame
          readinessProbe:
            httpGet:
              path: /index.html
              port: 80
            initialDelaySeconds: 5
            periodSeconds: 10
          livenessProbe:
            httpGet:
              path: /index.html
              port: 80
            initialDelaySeconds: 15
            periodSeconds: 20

    
```

In this deployment:

- **Readiness Probe** checks if the application is ready to serve traffic by accessing `/index.html`.
- **Liveness Probe** monitors the health of the container through the `/index.html` endpoint.

## Test the health-checks:

- **Container is running**:

  ![cont-running](https://github.com/user-attachments/assets/ccaa6524-f84f-4f2b-928e-f57cda8d0249)


- **Lets try to do some changes to stop the conatiner**:

  ![stop-cont](https://github.com/user-attachments/assets/22c50960-b5e3-4e8e-9164-64da34f306eb)



- **container got stopped**:

  ![cont-stopped](https://github.com/user-attachments/assets/0e4fe9ad-f35f-44a8-a994-f02e36c41b25)




- **container got restarted**:

  ![cont-restarted](https://github.com/user-attachments/assets/410fd780-4cab-4c7d-aa2d-edbd68aacb4b)




## Conclusion

Using Readiness and Liveness Probes in your Kubernetes deployment ensures that your application is reliable, responsive, and capable of self-healing. Start implementing these probes to make your services more robust and fault-tolerant.

