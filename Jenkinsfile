pipeline
    {
    agent any
    
    environment 
    {
        region = "us-east-1"
        
        VERSION = "${env.BUILD_ID}-${env.GIT_COMMIT}"
        
        docker_repo_uri = "387232581030.dkr.ecr.us-east-1.amazonaws.com/jen-ecr"
       
        task_def_arn = "arn:aws:ecs:us-east-1:387232581030:task-definition/TASKDEFINITION-LARAVEL:6"
        
        cluster = "JEN-ECS"
        
        exec_role_arn = "arn:aws:iam::387232581030:role/ecsTaskExecutionRole"
        
        TASK_DEFINITION_NAME = "TASKDEFINITION-FRONTEND"
  
        SERVICE_NAME = "FONTEND-SERVICE"
    }
    stages
    {
    
    stage('deploy to ECS')
    {
    steps{
        
        // Deploy to ECS
        sh ''' echo "${docker_repo_uri}:${VERSION}"
TASK_DEFINITION=$(aws ecs describe-task-definition --task-definition "$TASK_DEFINITION_NAME" --region "${region}")
NEW_CONTAINER_DEFINTIION=$(echo $TASK_DEFINITION | jq --arg IMAGE "${docker_repo_uri}:${VERSION}" \'.taskDefinition.containerDefinitions[0].image = $IMAGE | .taskDefinition.containerDefinitions[0]\')
echo "Registering new container definition..."
aws ecs register-task-definition --region "${region}" --family "${TASK_DEFINITION_NAME}" --requires-compatibilities FARGATE --cpu 512 --memory 1024 --network-mode "awsvpc" --execution-role-arn "${exec_role_arn}" --container-definitions "${NEW_CONTAINER_DEFINTIION}" 
echo "Updating the service..."
aws ecs update-service --region "${region}" --cluster "${cluster}" --service "${SERVICE_NAME}"  --task-definition "${TASK_DEFINITION_NAME}" '''
    }    
    }
    }
   }
