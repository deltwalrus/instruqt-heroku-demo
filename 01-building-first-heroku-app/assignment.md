---
slug: building-first-heroku-app
id: tzlxz3ukgszk
type: challenge
title: Building your first Heroku app
notes:
- type: text
  contents: |-
    # Welcome to Heroku!
    ![Heroku ASCII art](https://www.heroku.com/assets/wallpapers/eco-ascii/screen-cdde8356816d71eb0a0cdd77c8fc16a460cd95733b2b88c9c238cb6d5bcd6034.png)
    This tutorial can help you deploy a Node.js app to Heroku in minutes.

    The tutorial assumes that you have:
    - A verified Heroku account
    - An Eco dynos plan subscription (*recommended*)
tabs:
- id: 6lptvm6m8pae
  title: "\U0001F4BB Terminal"
  type: terminal
  hostname: heroku-ubuntu
  cmd: /bin/bash
- id: 1ehcwkxgnpry
  title: Code editor
  type: code
  hostname: heroku-ubuntu
  path: /root/src/node-js-getting-started
- id: axvuxhs8a0oc
  title: Heroku account
  type: browser
  hostname: heroku-account
- id: od9kdejk2ojy
  title: Local Heroku faces app
  type: service
  hostname: heroku-ubuntu
  path: /cool
  port: 5001
difficulty: ""
timelimit: 0
lab_config:
  default_layout_sidebar_size: 0
---
Log into your Heroku account
===
To log in to the Heroku CLI, use the `heroku login -i` command.
When prompted,enter your username. For the password, you must enter your authorization token since MFA is enabled. To find this, go to the [button label="Heroku account tab"](tab-2) and click the pencil icon next to “Heroku CLI” in the “Authorizations” section

![Screenshot 2024-09-11 at 9.46.59 AM.png](https://play.instruqt.com/assets/tracks/ksogktb7i8el/add1a046e22adfa5b546c739d4f326a7/assets/Screenshot%202024-09-11%20at%209.46.59%E2%80%AFAM.png)

Copy the authorization token to enter as the password.

![Screenshot 2024-09-11 at 9.47.21 AM.png](https://play.instruqt.com/assets/tracks/ksogktb7i8el/8bf1619d5f526ff02c6ba06c5979e920/assets/Screenshot%202024-09-11%20at%209.47.21%E2%80%AFAM.png)

The system should confirm that you are logged in successfully.

Prepare the application
===
To clone a local version of the sample application that you can then deploy to Heroku, execute these commands in your local command shell or terminal.

```run
cd src
git clone https://github.com/heroku/node-js-getting-started.git
cd node-js-getting-started
```

You now have a functioning Git repository that contains a simple application and a package.json file, which Node’s dependency manager uses. You can view the files in the [button label="Code Editor tab"](tab-1).

Deploy your app
===
In this step, you deploy the app to Heroku.

> [!IMPORTANT]
> Using dynos to complete this tutorial counts towards your usage. To control costs, Delete your app as soon as you‘re done.

> [!NOTE]
> By default, apps use Eco dynos if you’re subscribed to Eco. Otherwise, it defaults to Basic dynos. If you plan to deploy many small apps to Heroku, we recommend the Eco dynos plan, which is shared across all Eco dynos in your account. Learn more here. Eligible students can apply for platform credits through our Heroku for GitHub Students program.

Create an app on Heroku, which prepares Heroku to receive your source code.
```run
heroku create
```
When you create an app, you also create a Git remote called `heroku`. It’s associated with your local Git repository.

By default, Heroku generates a random name for your app. Or you can pass a parameter to specify your own app name.

Now, let's deploy your code.
```run
git push heroku main
```
You should see output similar to the following:
![Screenshot 2024-09-11 at 10.04.40 AM.png](https://play.instruqt.com/assets/tracks/ksogktb7i8el/4268fc2aae5542beee31cea700088897/assets/Screenshot%202024-09-11%20at%2010.04.40%E2%80%AFAM.png)
Ensure that at least one instance of the app is running:
```run
heroku ps:scale web=1
```
Now you can visit you app at the URL provided, which should look like this:
![Screenshot 2024-09-11 at 10.09.04 AM.png](https://play.instruqt.com/assets/tracks/ksogktb7i8el/b3300cb51a30f89f6907742da4b150fe/assets/Screenshot%202024-09-11%20at%2010.09.04%E2%80%AFAM.png)

View logs
===
Heroku treats logs as streams of time-ordered events aggregated from the output streams of all your app and Heroku components, providing a single channel for all of the events.

View information about your running app using one of the [logging commands](https://devcenter.heroku.com/articles/logging):
```run
heroku logs --tail
```
Visit your application in the browser again, and you see another log message generated.

To stop streaming the logs, press `Control+C` in the [button label="Terminal tab"](tab-0).

About the `Procfile`
===
Use a [Procfile](https://devcenter.heroku.com/articles/procfile), which is a text file in the root directory of your application, to explicitly declare what command executes to start your app.

The `Procfile` in the example app you deployed looks like:
```
web: npm start
```
The name `web` is important because it declares that this single process type attaches to Heroku’s HTTP routing stack and receives web traffic when deployed. This command uses the `start` script specified in the `package.json` file.

Procfiles can contain additional process types. For example, you can declare one for a background worker process that processes items off of a queue.

Scale your app
===
Right now, your app runs on a single web [dyno](https://devcenter.heroku.com/articles/dynos). Think of a dyno as a lightweight container that runs the command specified in the Procfile.

You can check how many dynos are running using the `heroku ps` command.
```run
heroku ps
```
By default, your app deploys on an Eco dyno. If they don’t receive any traffic, Eco dynos sleep after 30 minutes of inactivity. Upon waking, expect a delay of a few seconds for the first request. Subsequent requests perform normally. Eco dynos also consume from a monthly, account-level quota of [Eco dyno hours](https://devcenter.heroku.com/articles/eco-dyno-hours). As long as the quota isn’t exhausted, all Eco apps can continue to run.

To avoid dyno sleeping, you can upgrade to a Basic or Professional dyno type as described in the [Dyno Types](https://devcenter.heroku.com/articles/dyno-types) article. For example, if you migrate your app to a professional dyno, you can scale it by running a command that tells Heroku to execute a specific number of dynos, each running your web process type.

Scaling an application on Heroku is equivalent to changing the number of dynos running. Scale the number of web dynos to zero:
```run
heroku ps:scale web=0
```
Access the app again by hitting refresh on your web browser tab for the app. You get an error message because you no longer have any web dynos available to serve requests.

Scale it up again:
```run
heroku ps:scale web=1
```

Push local changes
===
In this step, you learn how to propagate a local change to the application through to Heroku. As an example, you modify the application to add an additional dependency and the code to use it.

Begin by adding a dependency for `cool-ascii-faces` in `package.json`. Run this command:
```run
npm install cool-ascii-faces
```

Modify `index.js` so that it `requires` this module at the start. Also add a new route (`/cool`) that uses it. You want the final code to look like:
```js,line-numbers
const cool = require('cool-ascii-faces')
const express = require('express')
const path = require('path')

const PORT = process.env.PORT || 5001

express()
  .use(express.static(path.join(__dirname, 'public')))
  .set('views', path.join(__dirname, 'views'))
  .set('view engine', 'ejs')
  .get('/', (req, res) => res.render('pages/index'))
  .get('/cool', (req, res) => res.send(cool()))
  .listen(PORT, () => console.log(`Listening on ${ PORT }`))
```
> [!NOTE]
> The two lines that changed above are 1 and 12.

Test your code locally:
```run
npm install
heroku local --port 5001
```
The [button label="Local Heroku faces app"](tab-3) tab should now show a fun ASCII face (refresh with the circular arrow button if it does not).

Now push your code:
```run
git add .
git commit -m "Add cool face API"
git push heroku main
```
If you visit your online app and append `/cool` to the end of the URL, you should see the ASCII faces deployed there, as well.
