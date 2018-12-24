---
comments: true
layout: post
title:  "My experience of deploying keras/tensorflow model on Heroku"
excerpt: This blog is for all those enthusiast who wants to deploy there deep learning models freely on Heroku without the hassle of Google cloud platform or Amazon web services.
date:   2018-12-16
---

> Spoiler : Heroku is not good enough for deploying deep learning models, so go for GCP, AWS or DigitalOcean. If you want to know why and don't believe me until you do it yourself go ahead and read full content.  

This was the first time I was deploying a model and creating a REST api endpoint to call from my mobile app or flask app. I already used all my digital ocean credit and didn't want to use GCP or AWS. So i went for **Heroku**. I searched a lot whether we can deploy keras model on heroku or not, but didn't get anything. So I went ahead and deployed my own model.

All the code snippets I use in this blog are on my [Github](https://github.com/ashukid/Deploy-keras-model-on-Heroku/tree/master)

### **1. Creating the REST Api endpoint**

First let's create a simple api endpoint and test it on local server. Go head and install `Flask` first. Then create a file with name *main.py* and insert the following piece of code.

```
1. app=Flask(__name__)
2. @app.route('/api',methods=['POST'])
3. def photoRecognize():

4.    im=request.form['image']
5.    im=unquote(im)
6.    answer=getFlowerClass(im)
7.    return jsonify(status='OK', results=answer)

if __name__=='__main__':
    app.run(host='0.0.0.0', port=3000)
```
In the 1st line I simply defined app name. In the 2nd line I created a route on which I'll make post request (If these things are unclear go through flask basics and then come back and read next). In 3rd line I defined the function *(photoRecognize)* that will be called when we make request to the route. Inside the function I took the image (url encoded string, 4th line) that'll be sent as POST request parameter. Then in 5th line url decoded the string and called the main function (*getFlowerClass*) which will process the image and send me the class the flower as string, which will be returned as JSON. If you want to have a look at the function (*getFlowerClass*) see my [Github](https://github.com/ashukid/Deploy-keras-model-on-Heroku/tree/master) page.


### **2. Installing Heroku CLI**

I'm a big fan of command line, so I'll be doing everything from command line itself. Go ahead and install `Heroku CLI`. Create a folder and put the file *main.py* inside it. Open up terminal and navigate to the folder just created and execute the command `heroku create`. This creates a heroku app and and will print something like :

```
Creating app... done, ⬢ serene-caverns-82714
https://abc-xyz-12345.herokuapp.com/ | https://git.heroku.com/abc-xyz-12345.git
```

First https link is the domain name of the app and second one is the link of github repository. A remote with name heroku is also created. So `git init` the folder and add the remote :
```
git remote add heroku https://git.heroku.com/abc-xyz-12345.git
```

### **3. Configurations for Heroku App**

Now is the time for adding configurations files for Heroku. Create following files in the same folder :

#### A. runtime.txt : 
This file is used to specify the run time for the app and the specific version. In this case we're using *python* runtime environment. So add this line in the file :
```
python-3.7.0
```

#### B. requirements.txt : 
Write down all the dependency of your app in this file like :

```
Flask==1.0.2
Keras==2.1.5
tensorflow==1.7.0
```
Heroku will install all those dependencies at the time of deployment. 

> NOTE : If you need OpenCV for your app, it has some dependencies that needs to be installed with apt. For that follow step C.

#### C. Aptfile :
For all those dependencies that have to be installed with  `apt`, first install herok-buildpack-apt with following command :

```
heroku buildpacks:add --index 1 https://github.com/heroku/heroku-buildpack-apt
```
Then create an `Aptfile` and write down all those dependencies like :

```
libsm6
libxrender1
libfontconfig1
libice6
```
These 4 dependencies are for OpenCV.

#### D. Procfile :
This file tells Heroku what command to execute after app is deployed. In my case I wanted a web app, the file to run is `main.py`, so my Procfile content was like this:
```
web: gunicorn main:app --loga-file=-
```
Gunicorn is Python Web Server Gateway Interface HTTP server. Go ahead and install it first.

### **4. Deploying the App**
Push all the codes to the repository.
```
git push heroku master
```

This command will push all the codes, install all the requirements specified and release the 1st version of the app.
Go ahead and run  `heroku open` to run the app.

Since I was passing image as post request I had to use some app that can send post request to the endpoint. For that I used *Postman*.

Everything worked till I sent the post request. The app was deployed properly, all the requirements was installed and app was running all well locally. So I expected it to work from heroku server as well. 
But as soon as I requested the server - boom !! App crashed.
I saw the logs (`heroku logs --tail`), there I found sometimes *request timeour error* and sometimes *memory limit exceeded error*.

Here is the whole crush, heroku gives only **30s** for one request and a total of **500MB** of RAM. Both of them is insufficient for deep learning models as evaluation and processing takes more time as well as more memory .

Hence after a hard work of 2 days, I finally realised I've to buy credit on DigitalOcean or create account on GCP. 