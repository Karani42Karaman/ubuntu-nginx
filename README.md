# ubuntu-nginx
This project  created as an assignment for BIL4214-Cloud Computing Lesson in Sivas Cumhuriyet University Department of Computer Engineering.
### Purpose
-It is a test application with nginx installed on ubuntu in its dockerfile file.
When this application runs, you will see my CV information that I have prepared on my html page on 127.0.0.1 ip.
## Usage 


####Creating Containers With Single-Layer Images

```script
docker run -d --name single -p 8082:80 karanikaraman/2018141025_o:latest
```

###ex:
 <p align="center">
<img src="https://github.com/Karani42Karaman/ubuntu-nginx/blob/main/images/single_commend.png" alt="single" width="100%" height="100%"/> 
</p>

####Creating Containers With Multistage-Layer Images

```script
docker run -d --name single -p 8082:80 karanikaraman/2018141025_m:latest
```

###ex:
 <p align="center">
<img src="https://github.com/Karani42Karaman/ubuntu-nginx/blob/main/images/multistage_commend.png" alt="multistage_commend" width="100%" height="100%"/> 
</p>

-Let's look at the resulting images and the size differences between them.
##images 
<p align="center">
<img src="https://github.com/Karani42Karaman/ubuntu-nginx/blob/main/images/image.png" alt="image" width="100%" height="100%"/> 
</p>

###RESULT:
<p align="center">
<img src="https://github.com/Karani42Karaman/ubuntu-nginx/blob/main/images/cv.png" alt="image" width="100%" height="100%"/> 
</p>

