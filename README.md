# DevOps_Task_1 - Auto deployment on live and staging with QA team.

## To complete this task I have already installed git, github, docker and jenkins(with sudo privileged) and follow 2 step as per below.

### First step- Git setup
1. Create local git repository(master and dev1 branch) with commits and push it to Github repo, refer images - [GitHub_dev1](https://github.com/krushnakant241/kkdevops/blob/master/images/GitHub_dev1.JPG), [GitHub_master](https://github.com/krushnakant241/kkdevops/blob/master/images/GitHub_master.JPG), [Local_Git](https://github.com/krushnakant241/kkdevops/blob/master/images/Local_Git.JPG).

master branch commands -
```
git init, git add . , git commit . -m "comment" , git remote add origin "GitHub_URL", git push -u origin master
```
dev1 branch commands -
```
git branch dev1, git checkout dev1, git commit . -m "comment" , git remote add origin "GitHub_URL",                                     git push -u origin dev1
```
(After that setup git hooks to push every commit automatically done by developer)
   
### Second step- Jenkins automatic jobs
#### 1. Job1 - upload code from dev1 branch to staging server for tesing environment using poll SCM build trigger, this is use for QA testing, refer images - [Job1_Jenkins_1](https://github.com/krushnakant241/kkdevops/blob/master/images/Job1_Jenkins_1.JPG), [Job1_Jenkins_2](https://github.com/krushnakant241/kkdevops/blob/master/images/Job1_Jenkins_2.JPG).

To run docker below is the script
```
sudo cp -rvf * /home/minidevops/staging
if sudo docker ps | grep staging_env
then
echo "OS is already running"
else
sudo docker run -dit -p 1501:80 -v /home/minidevops/staging:/usr/local/apache2/htdocs/ --name staging_env httpd
fi
```

#### 2. Job2 - upload code from master branch to live server for Production environment using poll SCM build trigger, this is use for client review, refer images - [Job2_Jenkins_1](https://github.com/krushnakant241/kkdevops/blob/master/images/Job2_Jenkins_1.JPG), [Job2_Jenkins_2](https://github.com/krushnakant241/kkdevops/blob/master/images/Job2_Jenkins_2.JPG).

To run docker below is the script
```
sudo cp -rvf * /home/minidevops/live
if sudo docker ps | grep live_env
then
echo "OS is already running"
else
sudo docker run -dit -p 1502:80 -v /home/minidevops/live:/usr/local/apache2/htdocs/ --name live_env httpd
fi
```
    
####  3. Job3 - QA team check the code on testing server and once website is ok, they will trigger this job using Trigger builds remotely(using this URL sample).
URL sample - JENKINS_URL/job/mini_devops_proj_QA_job3/build?token=TOKEN_NAME.  
     
code will automatically merge from dev1 branch to master branch and job2 will execute when new commit done in master branch of github repository. You need to provide your Git credential to push the code after build. refer images - [Job3_Jenkins_1](https://github.com/krushnakant241/kkdevops/blob/master/images/Job3_Jenkins_1.JPG), [Job3_Jenkins_2](https://github.com/krushnakant241/kkdevops/blob/master/images/Job3_Jenkins_2.JPG), [Job3_Jenkins_3](https://github.com/krushnakant241/kkdevops/blob/master/images/Job3_Jenkins_3.JPG).

web url before merging- refer images - [staging](https://github.com/krushnakant241/kkdevops/blob/master/images/staging.JPG), [Live](https://github.com/krushnakant241/kkdevops/blob/master/images/Live.JPG),  and after merging- refer image - [After merging dev1 to master](https://github.com/krushnakant241/kkdevops/blob/master/images/After%20merging%20dev1%20to%20master.JPG).
         
      
