This is a repo which contains all necessary yaml files and custom built docker file for install fluentd on your gke cluster.

Note: The configMap used here for fluentd basically focuses on shipping logs to loki and gcs.


For more plugins and configuration setup refer to https://docs.fluentd.org/


Step 1: Build the Dockerfile, 
	Note: You can add your custom plugins inside Gemfile


Step 2: You can push the obtained image to either any container registry or DockerHub
	Note: You will be using this image in your daemonSet.


Step 3: Edit your fluentd configMap according to your preference and apply it using:
	
	kubectl apply -f fluentd-configmap.yaml

	Note: I have used loki-grafana as the namespace, please change it according to your preference.


Step 4: Create your Secrets file for gcs service account ( Hoping that your already have the credentials.json file )
	
	kubectl create secret generic -n=loki-grafana fluentd-secret --from-file=./cred.json

	Note: This is for shipping logs to GCS bucket ( You can check the env in daemonSet yaml file )


Step 5: Apply RBAC yaml file for basically giving access to fluentd to watch, list and get pods and namespaces.

	kubectl apply -f fluentd-rbac.yaml


Step 6: Check the fluentd-daemon.yaml and edit it according to your preference and deploy the fluentd daemonSet.

	kubeectl apply -f fluentd-daemon.yaml


Step 7: Check if your pods are running by doing:

	kubectl get po -n=<your-namespace>


Step 8: There you go if your pods are up and running you can check in Loki if your labels are showing up or not.

	If they are showing up, then check url for loki which you have given in configMap.

	This url can be either a service running for loki pod or an external endpoint.
