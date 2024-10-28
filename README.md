![ Istio ](https://sysdig.com/wp-content/uploads/image4.jpeg)

# Enable Canary Deployment in K8s with Istio

A canary release is a testing strategy that involves rolling out new versions of applications gradually, so engineers can test new features in production with a limited set of users before making them avaiable to everyone. In the K8s ecosystem, combining Istio with K8s is a good way to implement canary releases.

This article will guide you through the steps necessary to set up a canary release strategy using Istio on K8s

## Introduction to canary deployment
canary deployments allow engineers to release a new version of software to a small subset of users or servers before making it avaiable to an entire user base. In this way, the method functions as an early warning system, allowing teams to detect potential problems before those problems can have a huge impact. One effect way to select subset of users for a canary deployment is by geographic region. This method is useful for servies with a global user base, where certain regions may not get to use the new feature. It makes more sense to deploy to the region that uses the feature.

## Introduction to Istio
Istio is an open-source service mesh that provides a way to control how microservices share data with one another. At its core, Istio is designing to handle issues that stem from deploying and managing a microservices architecture, especially at scale.

As applications grow to multiple services across different environments, they often become harder to manage and secure. Istio addresses these challenges by providing a comprehensive suite of tools focused on traffic management, security, and observability.

One of Istio's core capabilities is traffic management. In this tutorial, we'll demonstrate controlling the flow of traffic and API calls between services with fine-grained routing rules. We'll use Istio to divert a small percentage of traffic to the new version of an applicaiton while directing the majority to the stable production version

## How Istio works
Istio integrates with K8s, but it can also be used with other orchestration platforms. It operates at the platform layer, controlling how different parts of an applicaiton interact with one another. The primary components of Istio include:
- Envoy proxy: Istio uses a proxy called Envoy to manage traffic at the network level. Envoy proxies are deployed as sidecars to services, meaning that they are run in the same
K8s pod as the service. These proxies mediate all inbloud and outbound traffic, adding a layer of abstraction of control.
- Control plane: The control plane manages and configures proxies to route traffic. It also configures mixers to enforce policies and collect telemetry. The control plane offers
a single point of control that can dynamically alter the behaviour of the network.
- Pilot, Citadel, and Galley: These components serve distinct purposes in the Istio architecture. Pilot translates high-level routing rules that control traffic behaviour
into configurations that the Envoy proxies can understand. Citadel provides the security capabilities, such as identity and credential management. Galley is responsible for
validating, ingesting, aggregating, transforming, and distributing config with Istio.

## Lab Steps:

1. set up local minikube env - https://minikube.sigs.k8s.io/docs/

2. Installing Istio on your cluster

```
> curl -L https://istio.io/downloadIstio | sh -
> cd istio-*
> export PATH=$PWD/bin:$PATH
```

3. Deploy Istio on your cluster

```
istioctl install --set profile=default -y
```

4. Enable automatic sidecar injection

```
kubectl label namespace default istio-injection=enabled
kubectl get pods -n istio-system
```

5. Apply v1 app deployment manifest
6. Apply Istio configs manifest
7. Apply v2 app deployment manifest

8. run minikube tunnel command

```
minikube tunnel
```

You will now be able to access your application at http://localhost/home/. If you refresh the page rapidly, you will notice that v1 shows up more than v2.

## Conclusion
Implementing canary releases with Istio and K8s allows to minimize the risk of introducing new software versions in production. By carefully adjusting traffic between different versions based on real-world performance and feedback, you can ensure a smooth experience for your users. This approach not only improves reliability, but also allows for more aggressive innovation cycles.