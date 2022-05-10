# client-go-eks

Basically there are two types for the application using client-go to talk to k8s, the first one is in-cluster(e.g. app is a pod in k8s), the second one is out-of-cluster. For those two general ways, you can check with the [official client-go repo ](https://github.com/kubernetes/client-go#how-to-use-it) for guidence.  

Regardings this repo, we focus on building "out-of-cluster" connection between client-go and Amazon EKS. Since EKS introduce the IAM for authentication and leverage RBAC of k8s for authorization, there is a slight difference between EKS and opensource k8s.  

**Example1:** It is as same as the offical example from client-go repo, which is using kubeconfig to build the connection. For building the connection with EKS, follow the below steps:  
 1. Refer to [EKS User Guide](https://docs.aws.amazon.com/eks/latest/userguide/create-kubeconfig.html#create-kubeconfig-automatically) to construct the kubeconfig file for connecting to EKS, make sure you have set up the proper AK/SK of user or role for accessing EKS
 2. Run `example1.go` to test the connection for client-go talking to EKS  

 >  Be careful with the version of client-go, if you use the latest one, you probably will encount the issue with "client.authentication.k8s.io/v1alpha1", that because currently kubeconfig for EKS is till using v1alpha1, and the latest client-go use api v1 version. In this case we use v0.22.1 as a workaround.

**Example2:** You can also leverage [aws-iam-authenticator](https://github.com/kubernetes-sigs/aws-iam-authenticator) to dynamically obtain a "BearerToken" to access EKS cluster. In this way, you don't need to construct the kubeconfig file before running codes. Refer to below steps:
 1. Image you have multiple profiles of AKSK(for instances profile-a, profile-b), and each AKSK profile which is represented of IAM user/role must have the authority to describe EKS cluster as well as has the authority to access EKS
 2. Run `example2.go` to test the connection for client-go talking to EKS  

 >  Note we use "os.Setenv("AWS_PROFILE", aws_profile)" to chose the functional AKSK.
 
**Example3:** In case you don't want to use "os.Setenv("AWS_PROFILE", aws_profile)" to set environment variables, you manually set the profile and pass the "session token" to "aws-iam-authenticator" to get a valid "BearerToken". Steps are similary to above use case.
 




 


