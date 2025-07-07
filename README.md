# Mini-Project---Integrating-Helm-with-Jenkins

1. Jenkins Setup:

- Install Jenkins on your system with the default recommended plugins. If Jenkins is not already installed, follow the official [Jenkins installation guide](https://www.jenkins.io/doc/book/installing/).


2. Determine Helm Binary Path:

- The full binary path of Helm is required in the Jenkins pipeline script.

- To find it, use:

	Linux/macOS: 
          ```
		which helm
          ```

- Windows: 'Get-Command helm | Select-Object -ExpandProperty Source' in PowerShell.

3. Create a Jenkins Pipeline:

- In Jenkins, create a new pipeline job.

- Set the pipeline source as the Git repository you pushed your code to.

-  Configure the pipeline to trigger a build on commit to your repository.

<img width="1318" alt="image" src="https://github.com/user-attachments/assets/212c7fcb-c52a-430a-b94d-fccee6567913" />

<img width="1428" alt="image" src="https://github.com/user-attachments/assets/750dc234-5a5c-4da0-9fd8-ceab05f171a4" />

<img width="1434" alt="image" src="https://github.com/user-attachments/assets/1f98ccb7-1191-4b6d-bf22-bb4e67f1920d" />


<img width="1410" alt="image" src="https://github.com/user-attachments/assets/937fba07-e5e6-447c-89d9-e20a9c61a822" />


**Congiguring ngrok**

GitHub’s webhook requires a publicly reachable URL. since we installed Jekins locally we would need a publicly accessible ip or dns.
For this we would use the toool ngrok.

Install ngrok (if not installed):
```
brew install ngrok

```

Run ngrok to expose your Jenkins:

```
ngrok http 8080

```

![image](https://github.com/user-attachments/assets/d336caf9-f6ba-423d-9a6c-951e2deee8bf)

Create a github webhook using jenkins ip address and port

<img width="1177" alt="image" src="https://github.com/user-attachments/assets/4b6218b3-91db-4a2c-b32c-f8be050ca2d6" />

4. Pipeline Script Example with Full Helm Path:
   
- Use the determined full path of Helm in the pipeline script for Jenkinsfile. For example:

```

pipeline {
    agent any

    triggers {
        githubPush()
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Deploy with Helm') {
            steps {
                script {
                    sh '/usr/local/bin/helm upgrade --install my-webapp ./webapp --namespace default --create-namespace'
                }
            }
        }
    }

    post {
        success {
            echo '✅ Helm deployment succeeded.'
        }
        failure {
            echo '❌ Helm deployment failed.'
        }
    }
}


```

- Replace '/usr/local/bin/helm' with the path determined in Step 2.



Step 2: Update Helm Chart and Trigger Jenkins Pipeline

1. Update Helm Chart and Push Changes:

- Edit the 'values. yaml' File:

- Open 'values yaml' in your 'webapp' chart directory.

- Change the ' replicaCount' to '4' to increase the number of replicas.

- Save the changes.

- Edit the ' templates/deployment.yaml' File:

- Open 'deployment.yaml' located in the ' templates' directory.

Locate the ' resources' section under 'spec.template.spec.containers'.
Update the resource requests as follows:

```
resources:
  requests:
    memory: "180Mi"
    cpu: "120m"
```

- Save the file after making your changes.

2. Commit and Push the Changes:

- Use Git commands to commit these changes and push them to your remote repository. This will trigger the Jenkins pipeline.
- Execute the following commands in your terminal:

```
git add .
git commit -m "Updated replicas, memory and CPU requests"
git push


```

![image](https://github.com/user-attachments/assets/73d13128-abb6-4670-a088-1fc62dea84c7)


. Jenkins Pipeline Trigger:

- Once you push the changes to the repository, the configured Jenkins pipeline will detect the   commit.

- Jenkins will then automatically start a new build, deploying your updated Helm chart with the new configurations.

![image](https://github.com/user-attachments/assets/667f026b-e34c-4025-8ca8-32cf9d705a65)

![image](https://github.com/user-attachments/assets/de96814e-f10f-4a4f-a20e-631984d22d6e)

![image](https://github.com/user-attachments/assets/6e7a6626-36ad-4928-9eea-2de8d76b0d2b)

![image](https://github.com/user-attachments/assets/5c0362f6-3c43-4415-b170-3ebf62b64d7a)

![image](https://github.com/user-attachments/assets/a1fb968a-8c1a-4fed-ba2e-6474296d2ae5)











  
