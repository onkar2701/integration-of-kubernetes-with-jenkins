# integration-of-kubernetes-with-jenkins

As we know that the proper management of every company application is essential to a company's fundamental financial health and operational success as a business.It maintains a balance between growth, profitability and liquidity of business.

Docker enables us to run, create and manage containers on single operating system. But there may be case of container failure, traffic issue at server end. load balancing,etc. which are not fruitful for our application.

so here is my solution for this major problem. we will be using kubernetes with jenkins which will allow us to automate container provisioning, load balancing and scaling across all the nodes.

Tools Used:

1. Redhat VM

2. Docker

3. Kubernetes

4. Minikube

5. Jenkins

Pre-requisites:

We need to install Redhat VM and kubectl to work on the kubernetes cluster.

Start Minikube

minikube start

so, Let's start our journey!

Go to Redhat VM. Create a folder.

mkdir /workspace
cd /workspace

Copy all the required files to the same folder.

cp ca.crt /workspace
cp client.key /workspace
cp client.crt /workspace
cp config /workspace

All the above copied files are required for kubernetes installation.

Create container image that’s has Jenkins installed using dockerfile.

Dockerfile :

![](/1.png)

Use following command to build the image using Dockerfile.

docker build -t "jenkin:v1" .

![](/2.png)

![](/3.png)

![](/4.png)

Here our image jenkin:v1 is built. The image contains pre-installed jenkins ans kubermetes and some softwares like git, net-tools,etc.

Run container with the image created.

docker run -dit -p 9195:8080 --name jenkin-os1 jenkin:v1

Launch Jenkins with https://<redhat_vm_ip>:(published port for host)

e.g. https://192.168.99.101:9195

This image will automatically start pre-installed jenkins server.

Configure Jenkins

![](/5.png)

Let's create some jenkins job to automate our project.

Job - 1 :-

![](/6.png)

![](/7.png)

![](/8.png)

This job will automatically copy code from github to the locally created folder when developer commits code into github. Here we are using poll SCM to trigger build .

Success Output:

![](/9.png)


Job - 2 :

This job will trigger whenever previous job build successfully. By looking at the code or program file, Jenkins will automatically start the respective language interpreter installed image container to deploy code on top of Kubernetes. As well as the pod is exposed so that testing team can test the website and work accordingly.

![](/10.png)

![](/11.png)

kubernetes will solve issues of load balancing, traffic issues at server end , scaling and also provides single IP generated url to all the clients.

Note: We are here using sleep command because for running pod some sort of time requires. If we don't give sleep command, further command copy of kubectl will try to instantly.It may shows error of container not found. Because the container is in Creation Mode.

Success Output:

![](/12.png)

The pods will be created according to language interpreter and single IP generated url is given to the testing team for testing environment.

![](/13.png)

Job - 3:

Here the role of testing team comes.The testing team will test the website using url. If the website is not running properly then the testing team will trigger the following job with authentication token url. The job will run exit 1 which allows failure value in a sub shell causes the sub shell to fail. We are here using email notification after build. It will notify the developers that the website is not working properly.

![](/14.png)

![](/15.png)


For email notification after build, we need to configure email notification settings. Go to configure system.

![](/16.png)

Use smtp port of Gmail

![](/17.png)
![](/18.png)
The developers are notified by mail. The developer team will again check the code and deploy the website.

e.g.:
![](/19.png)

If the website works properly then our work completes.

![](/20.png)

Thus the integration of jenkins with kubernetes done successfully!!
