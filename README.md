# Use local docker images for kubernetes example

## Initially it doesn't work
First builds an image that echoes a dummy message

    docker build . -t kubernetes/hello-world

Run it

    docker run --rm kubernetes/hello-world

Create a job from that image

    kubectl create -f helloworld.yml

If you run **kubectl get pods** you will notice that the image is not found:

    kubectl get pods


> NAME              READY STATUS       RESTARTS AGE
>
> hello-world-lfrzh 0/1   ErrImagePull 0        6s


### Remove the job and create it again

1. Delete


        kubectl delete -f helloworld.yml


2. Recreate


        kubectl create -f helloworld.yml


3. Verify


        kubectl get pods


> Now you get a diferent error: **ErrImageNeverPull**

***

## Adjusting the enviroment to work

Check the variables you will need to change to point to the local docker repository

    minikube docker-env

To apply these variables, you should use the proposed command:

- Linux


        eval $(minikube -p minikube docker-env)

- PowerShell


        & minikube -p minikube docker-env --shell powershell | Invoke-Expression

Now you should build the image again to install it in the minikube registry

    docker build . -t kubernetes/hello-world

Recreate it again

    kubectl delete -f helloworld.yml
    kubectl create -f helloworld.yml
    kubectl get pods

Check the logs

    kubectl logs hello-world-x2d9

> It now should print the *hello world* message