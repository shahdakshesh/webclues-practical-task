# README

# Node Js Application Dockerfile Task

## Task Description
This task contains a simple Node.js application that listens on port 3000. The application is containerized using Docker.

## Steps to Run

### Prerequisites
- Install [Docker](https://www.docker.com/get-started) on your system.

### Build the Docker Image
```sh
docker build -t my-node-app .
```

### Run the Docker Container
```sh
docker run -p 3000:3000 my-node-app
```

### Access the Application
- Open a browser and navigate to `http://localhost:3000`.

## Assumptions
- The application entry point is `server.js`.
- `package.json` and `package-lock.json` exist in the project root.
- The application dependencies are properly defined in `package.json`.
- The application is configured to listen on port 3000.


# Kubernetes NetworkPolicy Task

## Task Description
This task involves creating a Kubernetes `NetworkPolicy` to restrict incoming and outgoing traffic for the `my-app-deployment` pods based on the following specifications:

- Allow incoming traffic **only from pods within the same namespace**.
- Allow incoming traffic **from a specific pod with the label `app=trusted`**.
- Allow outgoing traffic **to pods within the same namespace**.
- Deny all other incoming and outgoing traffic.

## Assumptions
- The `my-app-deployment` pods are labeled with `app=my-app-deployment`. If a different label is used, update the `podSelector` accordingly.
- The Kubernetes namespace for the deployments is `default`. Modify this in the YAML if using a different namespace.
- The `cache-deployment` exists but is not directly impacted by this NetworkPolicy.

## Steps to Apply the NetworkPolicy

1. **Ensure your Kubernetes cluster is running**:
   ```sh
   kubectl get nodes
   ```

2. **Verify existing deployments and services**:
   ```sh
   kubectl get deployments,services,pods -n default
   ```

3. **Apply the NetworkPolicy YAML file**:
   ```sh
   kubectl apply -f my-app-network-policy.yaml
   ```

4. **Verify the NetworkPolicy is applied**:
   ```sh
   kubectl get networkpolicy -n default
   ```

5. **Check the rules in the NetworkPolicy**:
   ```sh
   kubectl describe networkpolicy my-app-network-policy -n default
   ```

## Expected Behavior
- Pods within the same namespace **can communicate** with `my-app-deployment`.
- The pod labeled `app=trusted` can send traffic to `my-app-deployment`.
- `my-app-deployment` can send outgoing traffic **only within the same namespace**.
- All other incoming and outgoing traffic **is denied**.

## Troubleshooting
- If connectivity is blocked unexpectedly, check the pod labels:
  ```sh
  kubectl get pods --show-labels -n default
  ```
- Verify the namespace in which the NetworkPolicy is applied.
- Ensure the Kubernetes cluster supports NetworkPolicies (some CNI plugins do not enforce them by default).




