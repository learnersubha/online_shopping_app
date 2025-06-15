 **ctreate a pipeline with aws code build, code deploy, and code pipeline**

Step-1 : **Server creation for deployment**
  -----> First create an EC2 instance, where the app will be running. Go to aws console --> search EC2 and create an EC2 instance
Step-2 : **storage creation**
   -----> Create a S3 bucket where the artifacts will be store. Go to aws console and search S3 --> create a S3 bucket
Step-3 : **credential storing**
   -----> Go to systems manager --> go to parameter store --> create parameters and store dockerhub password, username and url (as per your requirements)
         name:  /myapp/docker-credentials/username --> type: securestring --> value: "put the value"
         name:  /myapp/docker-credentials/password --> type: securestring --> value: "put Personal Access Token"
         name:  /myapp/docker-registry/url         --> type: securestring --> value: "https://index.docker.io/v1/"
Step-4 : **Build the code for the app**
   -----> Open aws console and search code build --> build project --> create project --> give a projectname with type of default project type --> source: github --> repository: public (in my case), and give the repo url and source version: repo branch name --> build type: single build and environment as given --> OS: ubuntu, Runtime: Standerd, image: statnerd 5.0, image version as given --> service role you can create new or existing no matter but must have s3,Ec2, SSM and awscodebuild full access --> buildspec: use a buildspec file (available in repo) --> artifact: s3 and give the bucket name --> logs save in s3 --> now click on start build and check from phase details.
Step-5 :  **Deploy the code**
   ------> in aws console find and go code deploy --> application -->create application --> give the app name and compute platfrom (EC2 on my case) --> create
   --> click on create deployment group --> give deployment group name and service role --> deployment type: inplace --> Environment configuration: Amazon EC2 instances --> key: name, Value: ec2 instance name --> now its time to do Agent configuration with AWS Systems Manager --> connect ec2 with ssh and run the command to install agent -->                   sudo apt update -y
            sudo apt install ruby-full wget -y
            cd /home/ubuntu
            REGION= As your choice (e.g: eu-west-1)
            wget https://aws-codedeploy-${REGION}.s3.${REGION}.amazonaws.com/latest/install
            chmod +x ./install
            sudo ./install auto
            sudo systemctl start codedeploy-agent
            sudo systemctl enable codedeploy-agent
            sudo systemctl status codedeploy-agen
     --> deployment setting : allatonce  --> load balancer: disable --> click on create deployment group --> finally click on create deployment.

Step-6 : **pipeline creation**
   -----> go to aws console and search code pipeline and click on that --> pipeline --> create pipeline --> build custom pipeline --> give pipeline name and service role (aws codepipeline full access) --> advance setting --> artifact store: custom location --> s3 bucket name --> next --> source: git via github app --> establish aws connection with github click on connect to github --> repo name: github username/reponame (learnersubha/Online_shopping_app) and select the branch where container start and stop script available (here available in scripts folder) --> output artifact format: full clone --> webhook: enable --> next --> build provider: other build provider and select aws codebuild --> give the project name --> build type: single and keep everything same --> next -->  tester will writ code so click on skip the stage --> deploy provider: aws codedeploy --> select region --> give application and deployment groups name --> click on next --> create pipeline.

   Now its completed when your developer update your app and push the code in github you just need to click on relese deployment.
        
