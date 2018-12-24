---
comments: true
layout: post
title:  "Hosting Deep Learning Projects locally"
excerpt: In this blog I'll explain you why hosting projects locally is a better option, how to start server on localhost and generate public url with custom domain name.
date:   2018-12-24
---

I needed to make an app that can classify flowers from images. I already had trained model ready for deployment, only thing left was actual deployment. Front end was ready, only bottleneck was server - where to host the server *freely*. I can't do it on localhost because my IP will change evertime I reconnect to internet and obviously I'm not going to rebuild the app each time IP is changed. So I needed a fixed public URL which I can use to build my app.

I know there are various options to host your server like `GCP`, `AWS` and `Digital Ocean`. But they all are paid and no matter how cheaper they are, still the same for me. So I tried hosting my model on `heroku`. See this [blog](https://ashukid.github.io/2018/12/16/deploy-models-on-heroku.html) to know more about hosting on `heroku`. That didn't work either becuase in short they give you only *500MB RAM* in free version, which is definitely not sufficient for running deep learning models.

After spending complete 2 days on Heroku I came to know that AWS gives free EC2 with *1GB of RAM for 1 year*. I was like - huh ! why don't I spend my time on research !! So next I went for `AWS`. I deployed my model on `AWS`, but it turned out even 1GB is not sufficient for my model (I was using Resnet50 model). Finally I sat down and tried to figure out what is the problem :

1. I can host on my localhost but can't use the chaning IP address for building app.
2. Cloud service do give fixed public URLs, but RAM is not sufficient.

So the problem was - RAM and public URL. My localhost is solving the RAM problem, so I tried to figure out can I solve URL problem as well. 

It turned out yes - [ngrok](https://ngrok.com/) is the solution. `ngrok` basically provides a tunnel from your local server endpoint to public endpoint. But again they don't give you custom domain for free, everytime you run `ngrok` new public URL will be generated, so clearly can't use it as well. 
So then I searched for open source alternatives of `ngrok`.

Then I found [serveo](https://serveo.net/). And hell yeah, this was the solution of my problem. They provide custom domain freely and the better part no installation is required at all. I was really contended by their service and this was one of my findings that is gonna help me a lot in the future for sure.

Enough of background, let's get directly to the steps for hosting your app :

#### **1. Create an API endpoint for your app :**
First you need to create and API endpoint that you can use to call the server from your app. One of the easiest way to do that is to use `flask`. Here is simple code to do that :
```
app=Flask(__name__)
@app.route('/api',methods=['POST'])
def function(parameters):
    
    // do something
    // return result as json
    return jsonify(status='OK', results=answer)


if __name__=='__main__':
    app.run(host='0.0.0.0', port=3000)
```
Fire up terminal and executre - `python your_flask_app.py`. This will start hosting your API on localhost. You can check it on - `http://localhost:3000`.

#### **2. Tunnel Localhost to Public URL :**
Fire up another terminal and write this command : `ssh -R your_subdomain:80:localhost:3000 serveo.net`.

Where:
1. `your_domain` is the custom name you choose.
2. `localhost:3000` is your localhost endpoint (remember to change port 3000 in case you're running the API on different port). 
3. `80` is the http port
4. `serveo.net` is the public domain.

After executing this command, a url will be generated  -`your_subdomain.serveo.net`, which will be the public endpoint of your localhost, you can use it to build the app. Do remember serveo will generate this url only when `your_subdomain` is available, so choose some creative name.

And that's it. With this simple two step you can host your project locally. This is big aid for me as my local PC has enough RAM for deploying model and I don't need APP for long term purposes.
