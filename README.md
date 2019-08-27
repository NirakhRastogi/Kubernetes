# Kubernetes Basics
#### Nirakh Rastogi

- `kind`: Describe kubernetes object kind
- `metadata`
  - `name`: Descibe kubernetes object metadata.
- `spec`: Describe kuberenets object specificcation

# Pod
Pods are the smallest unit of kubernetes. Containers run inside pod. You can run multiple as well as single container inside a pod.

- `spec`
  - `containers`
    - `name` : Name of conatiner
    - `image` : Name of image
    - `env` : Containers Env variables
      - `name` : Name of Env Variable
      - `value` : Value of Env Variable
    - `ports` :
      - `containerPort` : exposed container port
    - `command`: Command to run at containe start
    - `args`: Arguments

## NginxPod.yaml
## SpringPetclinicDeployment.yaml
Run `kubectl -f apply NginxPod.yaml`

    This will run a pod with name test-pod which will run nginx image.

To get all running pods,
`kubectl get pods`

    This command will give you list of pods wiht there state.

To get information about single pod,
`kubectl describe pod/<pod_name>`

To delete pod,
`kubectl delete pod/<pod_name>`

To get pod shell,
`kubectl exec -it -- <pod_name>`


# Service
By default when you run a pod, a application running inside a pod is not open to a network. The way to expose an application running inside a pod is service.
## SpringPetclinicService.yml

- `spec`:
    - `ports`:
      - `name` : http
      - `port` : 80
      - `targetPort` : 8080
      - `protocol` : TCP
  - `selector`:
  - `type`: NodePort

Run `kubectl -f apply Service.yaml`

    This will start a service.

To get all running pods,
`kubectl get servicce`

    This command will give you list of services running.

To get information about single service,
`kubectl describe service/<service_name>`

To delete service,
`kubectl delete service/<service_name>`

# Secrets
Kubernetes secrets object lets you manage and store sensitive information like, tokens, username, passwords, etc.

To create a secrets,
`kubectl create secret <secret_name> --from-file=<var_name>='filename' --from-literal=<var_name>=<value>`

--from-file, will create secret from file and --from-literal will create a secret from env var.

You can use secret as EnvVar in container or as a File inside container.

1. Using secret as a env var,
### ApplicationPod.yaml
In a pod definition, you have specify,
- `spec`:
  - `containers`:
    - `env`:
      - `key`
      - `valueFrom`:
        - `secretKeyRef`
          - `name`: secretName
          - `key`: secretKey

2. Using secrets as file
### SecretAsFile.yaml
You have create a volume definition and specify secrets in the volume and then mount the volume in a container at specific path.
