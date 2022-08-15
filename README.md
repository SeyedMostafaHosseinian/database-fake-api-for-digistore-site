Deploy json-server to {{ free hosting site }}
Instructions how to deploy the full fake REST API json-server to various free hosting sites. Should only be used in development purpose but can act as a simpler database for smaller applications.

Create your database
Deploy to Heroku
Deploy to Glitch
Deploy to Azure
Create your database
Press the green Use this template-button in the right corner of this repo
Give your new repo a name and press the green Create repository from template-button
Clone your newly created repository to your computer
4 . Change the contents of db.json to your own content according to the json-server example and then commit your changes to git locally.

this example will create /posts route , each resource will have id, title and content. id will auto increment!

{
  "posts": [
    {
      "id": 0,
      "title": "First post!",
      "content": "My first content!"
    }
  ]
}
Deploy to Heroku
Heroku

Heroku is a free hosting service for hosting small projects. Easy setup and deploy from the command line via git.

Pros
Easy setup
Free
Cons
App has to sleep a couple of hours every day.
"Powers down" after 30 mins of inactivity. Starts back up when you visit the site but it takes a few extra seconds. Can maybe be solved with Kaffeine
Install Heroku
1 . Create your database

2 . Create an account on
https://heroku.com

3 . Install the Heroku CLI on your computer:
https://devcenter.heroku.com/articles/heroku-cli

4 . Connect the Heroku CLI to your account by writing the following command in your terminal and follow the instructions on the command line:

heroku login
5 . Then create a remote heroku project, kinda like creating a git repository on GitHub. This will create a project on Heroku with a random name. If you want to name your app you have to supply your own name like heroku create project-name:

heroku create my-cool-project
6 . Push your app to Heroku (you will see a wall of code)

git push heroku master
7 . Visit your newly create app by opening it via heroku:

heroku open
8 . For debugging if something went wrong:

heroku logs --tail
How it works
Heroku will look for a startup-script, this is by default npm start so make sure you have that in your package.json (assuming your script is called server.js):

 "scripts": {
    "start" : "node server.js"
 }
You also have to make changes to the port, you can't hardcode a dev-port. But you can reference herokus port. So the code will have the following:

const port = process.env.PORT || 4000;
Deploy to Glitch
Not tested 100%. Same as with Heroku, will sleep after a while.

Register for Glitch or go to Glitch/edit
Click New Project
Click Import from GitHub
Paste https://github.com/ikramdeveloper/json-server-deploy into the URL-input and click OK.
Wait for it to setup
Press Share-button to get your URL to live site. It should be something for example like: https://seemly-truthful-scribe.glitch.me/. And your DB will be at https://seemly-truthful-scribe.glitch.me/posts
Deploy to Azure
Azure

You can also use Microsoft Azure to deploy a smaller app for free to the Azure platform. The service is not as easy as Heroku and you might go insane because the documentation is really really bad at some times and it's hard to troubleshoot.

The pros are that on Azure the app will not be forced to sleep. It will sleep automatically on inactivity but you can just visit it and it will start up.

Installation
1 . Create a Microsoft Account that you can use on Azure:
https://azure.microsoft.com/

2 . Install the azure-cli:
https://docs.microsoft.com/en-us/cli/azure/install-azure-cli This might cause some trouble, you will see. Remember to restart your terminal or maybe your computer if the commands after this does not work

3 . Login to the service via the command line and follow the instructions:

az login
You will be prompted to visit a website and paste a confirmation code

Create the project
1 . Create your database

2 . Create a resource group for your projects, replace the name to whatever you want just be sure to use the same group name in all commands to come. You only have to create the resource group and service plan once, then you can use the same group and plan for all other apps you create if you like.

az group create -n NameOfResourceGroup -l northeurope
3 . Create a service plan:

az appservice plan create -n NameOfServicePlan -g NameOfResourceGroup
4 . Create the actual app and supply the service plan and resource group

az webapp create -n NameOfApp -g NameOfResourceGroup --plan NameOfServicePlan
5 . Create deployment details. A git-repo is not created automatically so we have to create it with a command:

az webapp deployment source config-local-git -n NameOfApp -g NameOfResourceGroup
6 . From the command in step 5 you should get a url in return. Copy this url and add it as a remote to your local git project, for example:

git remote add azure https://jesperorb@deploy-testing.scm.azurewebsites.net/deploy-testing.git
7 . Now you should be able to push your app:

git push azure master
You should be prompted to supply a password, this should be the pass to your account. If not, you can choose a different password at your Dashboard for Azure: https://portal.azure.com/

Choose App Services in the sidebar to the left and the choose your app in the list that appears then go to Deployment Credentials to change your password for deployment:
https://docs.microsoft.com/en-us/azure/app-service/app-service-deployment-credentials
