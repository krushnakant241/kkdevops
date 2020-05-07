Purpose - Auto deployment on live and staging with QA team

To complete this task i have already installed git, github, docker and jenkins(with sudo privileged)
In this project I follow 2 step as below.

First step- Git setup
1) Create local git repository(master and dev1 branch) with commits and push it to Github repo
          (refer images - GitHub_dev1, GitHub_master, Local_Git)
          
   master br command -  git init, git add . , git commit . -m "comment" , git remote add origin "GitHub_URL", git push -u origin master
   dev1 br command -  git branch dev1, git checkout dev1, git commit . -m "comment" , git remote add origin "GitHub_URL",                                        git push -u origin dev1,
   (After that setup git hooks to push every commit automatically done by developer)
   
Second step-Jenkins automatic jobs
1) Job1 - upload code from dev1 branch to staging server for tesing environment using poll SCM build trigger
          this is use for QA testing (refer images - Job1_Jenkins_1, Job1_Jenkins_2)
To run docker below is the script - 
    sudo cp -rvf * /home/minidevops/staging
    if sudo docker ps | grep staging_env
    then
    echo "OS is already running"
    else
    sudo docker run -dit -p 1501:80 -v /home/minidevops/staging:/usr/local/apache2/htdocs/ --name staging_env httpd
    fi
    
2) Job2 - upload code from master branch to live server for Production environment using poll SCM build trigger
          this is use for client review (refer images - Job2_Jenkins_1, Job2_Jenkins_2)
to run docker below is the script
    sudo cp -rvf * /home/minidevops/live
    if sudo docker ps | grep live_env
    then
    echo "OS is already running"
    else
    sudo docker run -dit -p 1502:80 -v /home/minidevops/live:/usr/local/apache2/htdocs/ --name live_env httpd
    fi
    
3) Job3 - QA team check the code on testing server and once website is ok, they will trigger this job using Trigger builds remotely.
          URL sample - JENKINS_URL/job/mini_devops_proj_QA_job3/build?token=TOKEN_NAME
          (refer images - Job3_Jenkins_1, Job3_Jenkins_2)
code will automatically merge from dev1 branch to master branch and job2 will execute when new commit done in master branch of github repository.
          
      
