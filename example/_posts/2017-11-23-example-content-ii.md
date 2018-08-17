---
layout: post
title: How to create your own distributed Search on Azure (Elastic Search + Logstash)?
description: >
  Elastic Search in Azure
image: /assets/img/mostap.png
canonical_url: https://pages-themes.github.io/architect/
---
## Create Elastic Search cluster and Kibana using Azure UI

1. Search for Elastic Search in Azure Marketplace. You will find this template:
![My helpful screenshot]({{ "/assets/img/ElasticSearchonAzure/0.png"}}) 

2. Add user name and credentials. 
![My helpful screenshot]({{ "/assets/img/ElasticSearchonAzure/1.png"}})

3. Creare your VNET and subnet. If you want to add some web api after you will need to add your web app to the same directory. 
![My helpful screenshot]({{ "/assets/img/ElasticSearchonAzure/2.png"}}) 

4. Create 1 client node 
![My helpful screenshot]({{ "/assets/img/ElasticSearchonAzure/3.png"}}) 

5. By the way here you don’t need to add user name. For example, for Kibana you will use “elastic” as a user name to login as a superuser. You need to enter only your passwords.

![My helpful screenshot]({{ "/assets/img/ElasticSearchonAzure/4.png"}}) 

6. You want to install Kibana for visualization. You will receive an error if you enable jump box. .

![My helpful screenshot]({{ "/assets/img/ElasticSearchonAzure/5.png"}}) 

7. Click ‘ok’ for everything else after that step and wait when your resources will be deployed. 
When Elastic Search successfully deployed on Azure you will see something like this. 

![My helpful screenshot]({{ "/assets/img/ElasticSearchonAzure/6.png"}}) 
To see deployment status click on the bell – upper right corner. 
![My helpful screenshot]({{ "/assets/img/ElasticSearchonAzure/7.png"}}) 

8. Find Kibana VM. Go to an VM overview. Copy DNS. On the same virtual machine. Check networking tab. You can see here what ports are open. Add Kibana port - 5601 to your ip in the browser and  sign in to Kibana.
During provisioning elastic search cluster you added 2 passwords. To login and to see Kibana dashboard you can use you can use username ‘Elastic’ to your password  to login as a superuser or username 'kibana' and your Kibana password.



### Thanks

> When something is important enough, you do it even if the odds are not in your favor.

```js

}
```


```

```
