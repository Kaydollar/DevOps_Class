# Creating a Freestyle Project

From the Dashboard menu on the left side, Click on New Item:

![](1.%20New%20Item.png)

ii. Create a freestyle Project and name it. "my-first-job"

![](2.%20Naming.png)

## Connecting Jenkins to our Source Code MAnagement.

* Now that we have created a freestyle project, let connect jenkins with github.

    - i. Create a new github repository called jenkins-scm with a `README.md` file

![](3.%20New%20repo.png)

![](4.%20Naming%20repo.png)



    - ii. Connect `jenkins` to `jenkins-scm` repository by pasting the repository url in the area selected below.
    Make sure your current branch is `main`

![](5.%20Repo%20URL%20on%20Jenkins.png)

    - iii. Save configuration and run "build now" to connect jenkins to our repository

![](6.%20Save.png)

![](7.%20Build%20Now.png)

![](8.%20Status.png)

![](9.%20Success.png)

We have successfully connected jenkins with our github repository (jenkins-scm)

### Configuring Build Trigger

As a engineer, we need to be able to automate things and make our work easier in possible ways. We have connected "jenkins" to "jenkins-scm", but we cannot run a new build with clicking on "Build Now".
To eliminate this, we need to confire a build trigger to our jenkins job. With this, jenkins will run a new build anytime a change is made to our github repository

- i. Click "Configure" on your job and add this configurations

![](10.%20Configure.png)

1. Configure GitHub Webhook (Trigger Jenkins on Push)

On GitHub:

Go to your repository → Settings → Webhooks.

![](11.%20Webhooks.png)

Click “Add webhook”.

![](12.%20Add%20Webhook.png)

Payload URL:

If you're using ngrok, copy your public URL and add /github-webhook/.

Example:

arduino

Copy

Edit

http://<your-ngrok-id>.ngrok.io/github-webhook/

Content type:
application/json

Events to trigger:
Choose “Just the push event”.

Secret: Leave blank for now (unless Jenkins is configured to use one).

Click Add webhook.

![](13.%20Webhook%20details.png)

Enable GitHub Integration in Jenkins
On Jenkins:

Install these plugins if you haven’t already:

GitHub Integration Plugin

GitHub Plugin

Pipeline Plugin

Go to:

Manage Jenkins → Configure System

Under GitHub, click Add GitHub Server → set the API URL:
```
https://api.github.com
```
Job Configuration

🧱 For Freestyle Job:

Go to your Jenkins job → Configure

![](14.%20Configuration.png)

Check GitHub Project and enter your repo URL

![](15.%20Repo%20URL.png)

Copy &Edit

https://github.com/Kaydollar/jenkins-scm

Under Source Code Management, select Git, add your GitHub repo URL and credentials.

Scroll to Build Triggers:
✅ Check:

 GitHub hook trigger for GITScm polling

 ![](16.%20Github.png)

 ![](17.%20Build.png)

 ** Test It!**

 1. Make a Small Change in Your Repo

 ```
 echo "# Update for Jenkins trigger test" >> README.md
```

![](18.%20ADD%20to%20README.png)

Then stage and commit:

```
git add README.md
git commit -m "Test Jenkins webhook trigger"
```

![](19.%20Git%20commit.png)

2. Push to GitHub

```
git push origin main
```

![](20.%20git%20push.png)

Push a commit to GitHub.

Jenkins should automatically start the job if the webhook is set correctly.

