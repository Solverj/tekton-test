# Resources used:
1. https://kubernetes.io/docs/tasks/access-application-cluster/list-all-running-container-images/
2. https://medium.com/@nikhilthomas1/cloud-native-cicd-on-openshift-with-openshift-pipelines-tektoncd-pipelines-part-3-github-1db6dd8e8ca7
3. https://developers.redhat.com/blog/2020/02/26/speed-up-maven-builds-in-tekton-pipelines/
4. https://developer.ibm.com/devpractices/devops/tutorials/build-and-deploy-a-docker-image-on-kubernetes-using-tekton-pipelines/#create-a-task-to-build-an-image-and-push-it-to-a-container-registry
5. https://zhimin-wen.medium.com/caching-for-building-spring-boot-app-image-with-kaniko-48563750bf3a 


# Steps:

1. kubectl apply -f tasks except pingTask.yaml (mvn, deployToKubectl)
2. kubectl apply -f serviceAccount.yaml (rbac access to make deployment to k8s)
3. kubectl apply -f pvcBuildDeploy.yaml (storage for mvn and git clone etc)
4. kubectl apply -f https://github.com/tektoncd/catalog/blob/v1beta1/kaniko/kaniko.yaml
5. kubectl apply -f https://github.com/tektoncd/catalog/blob/v1beta1/git/git-clone.yaml
6. Modify git repo within build-pipeline.yaml
7. Modify source-to-image.IMAGE within build-pipeline.yaml to your repository to push to and expected tags and such
8. kubectl apply -f build-pipeline.yaml
9. kubectl apply -f pipelineRun.yaml
10. kubectl proxy --port=8100
11. goto http://localhost:8100/api/v1/namespaces/tekton-pipelines/services/tekton-dashboard:http/proxy/#/namespaces/default/pipelineruns/build-pipeline-run?pipelineTask=source-to-image