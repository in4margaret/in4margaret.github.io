---
layout: post
title: Building CI/CD pipeline using VSTS and VS Code
description: >
  Building CI/CD pipeline using VSTS and VS Code
image: 
---
## VSTS and VS Code 
1. Register in VSTS because VSTS gives you free unlimited private repos.
https://visualstudio.microsoft.com/team-services/
2.	Find and install extension in VS Code for VSTS
![My helpful screenshot]({{ "/assets/img/VSTS/1.png"}}) 
3. Create new project in VSTS and open it. Choose option – Clone in VS Code. 
![My helpful screenshot]({{ "/assets/img/VSTS/2.png"}}) 
4.	Select a local folder where you will write your code. 
5.	After click on  "Generate Git Credentials" in VSTS.
![My helpful screenshot]({{ "/assets/img/VSTS/2.png"}}) 
Choose "Generate personal access token option"
![My helpful screenshot]({{ "/assets/img/VSTS/4.png"}}) 
Copy generated token.
![My helpful screenshot]({{ "/assets/img/VSTS/5.png"}}) 
Switch to VS Code and click on "Teams" at the bottom left conner and select first option - "Provide an access token manually" and paste token that you just copied.
![My helpful screenshot]({{ "/assets/img/VSTS/3.png"}}) 
6. After you added your token, you would see you project name at the bottom left conner instead of "Teams".
![My helpful screenshot]({{ "/assets/img/VSTS/6.png"}}) 
Add some files for your project and commit changes.
![My helpful screenshot]({{ "/assets/img/VSTS/7.png"}}) 
When you click on tree dots and choose ‘Push to…’ you will see your VSTS repo.
If you return back to VSTS click on Code tab you will see your changes there.
![My helpful screenshot]({{ "/assets/img/VSTS/8.png"}}) 
7. Let's set up our build. Click on 'Build and Release' and then click on ‘New pipeline’
![My helpful screenshot]({{ "/assets/img/VSTS/9.png"}})  
8. Choose a source 
![My helpful screenshot]({{ "/assets/img/VSTS/10.png"}}) 
9. Choose template for Docker container image.
![My helpful screenshot]({{ "/assets/img/VSTS/11.png"}}) 
If you are using tempalte VSTS automatically generate two tasks for you 
Also we can choose an agent – server where VSTS will run your code. (those servers could be private too).
![My helpful screenshot]({{ "/assets/img/VSTS/12.png"}}) 
10. You need to choose your Azure Subscription if you want to push your image to Azure. We are going to use Azure Container Registry for our images. Click on 'Authorize' button, it will allow your to choose your existing Azure Container Registry from the list. (I created Azure Container registry before using Azure portal)
Choose your Dockerfile that you committed to your VSTS repo before.
![My helpful screenshot]({{ "/assets/img/VSTS/13.png"}}) 
11. Lets switch to the second automatically generated task - "Push an image". (You don’t need to save changes to navigate between tasks.) 
Repeat the same steps and after click on 'Save and Queue'. Here you can change some settings or leave it as it is. 
You will see the following  
![My helpful screenshot]({{ "/assets/img/VSTS/14.png"}}) 
12. Click on Build number and you will be able to see the status.
![My helpful screenshot]({{ "/assets/img/VSTS/15.png"}}) 
When your job succeeded you can move to Release by simply pushing on Release button. 
![My helpful screenshot]({{ "/assets/img/VSTS/16.png"}}) 
13. We want to deploy our app to Kubernetes. Let's choose this option from the list:
![My helpful screenshot]({{ "/assets/img/VSTS/17.png"}}) 
14. Lets provide a name to out environment and call it 'dev' environment. 
![My helpful screenshot]({{ "/assets/img/VSTS/18.png"}}) We don't have a save button, but don’t worry VSTS will save your changes.
15. VSTS already added an artifact, but we can delete existing one and choose another artifact, for example, image from our Azure Container registry 
![My helpful screenshot]({{ "/assets/img/VSTS/19.png"}})
16. So let's select existing artifact and confirm that you want ot delete it. After we should add a new one. "Add button" gives you 3 options. Lets explore other artifacts types options by clicking on reference below.
There you can find Azure Container Registry.
 ![My helpful screenshot]({{ "/assets/img/VSTS/20.png"}})
 17. We also can enable Continues deployment trigger.
  ![My helpful screenshot]({{ "/assets/img/VSTS/21.png"}})
After click on Tasks in your dev environment tab. 












