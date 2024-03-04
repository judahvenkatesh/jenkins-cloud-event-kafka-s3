To collect cloud events from jenkins and store in S3 bucket. Follow the below steps.
  1. Create S3 bucket in desired location (AWS, on prem, MinIO, etc)
  2. Install Cloud plugin in Jenkins. [CloudEvents plugin for Jenkins](https://plugins.jenkins.io/cloudevents/)
  3. Write Producer python script with flask and kafka modules.
  4. Write a Dockerfile to create image of the producer.
  5. Use Jenkins pipeline to build the image and push to jFrog.
  6. Create namespace and secret with below commands.
     `kubectl create ns kafka`
     `kubectl create secret docker-registry regcred --docker-server=https://docker.io --docker-username=<user> --docker-password=<pass> -n kafka`
  8. Deploy Apache Kafka deployment on K8s using [Helm chart](https://github.com/meniem/kafka-helm)
  9. Create [deployment yaml](https://github.com/judahvenkatesh/jenkins-cloud-event-kafka-s3/blob/main/producer-deploy.yaml) for Producer POD. 
  10. Deploy the producer container.
      `kubectl apply -f producer-deploy.yaml -n kafka`
  11. Expose the service to connect to HTTP sink from Jenkins.
      `kubectl expose deployment kafka-producer --type=LoadBalancer --name=producer-svc -n kafka`
  12. Generate access tokens and create bucket in the S3 instance.
  13. Write consumer python script with kafka and boto3 module to connect to S3 bucket.
  14. Write Dockerfile to create consumer image and Step 4 to publish the image.
  15. Create [deployment yaml](https://github.com/judahvenkatesh/jenkins-cloud-event-kafka-s3/blob/main/consumer-deploy.yaml) for Consumer POD.
  16. Deploy the consumer container.
      `kubectl apply -f consumer-deploy.yaml -n kafka`
