# Setup

## Github
- Click on the user this template & create a new repository
- Give the repository a name and click on create repository

## Cloudflare
Now the template is ready we will build the first version of the site to make sure everything is ready to go!
- Go to https://dash.cloudflare.com
- Go to `Workers & Pages`
- Click `Create`
- Go to pages and connect to git
- Link your github accout if needed
- Choose your repository you just made and click begin setup
- In the build setting choose for `Mkdocs` as framework

![Scherm­afbeelding 2024-07-08 om 17 40 57](https://github.com/svenvg93/mkdocs-material-starter/assets/4511676/c64915c5-cf09-43cf-97ec-c2c686806753)

Now cloudflare will clone your github repro, build the website and publish it.
If succesfull click continue project
You will get a generate domain name like `mkdocs-em9.pages.dev`


##Local Install MacOS
- mkdir DOX
- cd DOX
- python3 -m venv DOX
- source DOX/bin/activate 
- brew install python 
- pip install mkdocs
- pip install mkdocs-material 
- mkdocs --version
- git clone https://github.com/youroldmangaming/DOX.git
- mkdocs serve&                                         

This will result in 

INFO    -  [05:34:21] Browser connected: http://127.0.0.1:8000/

##Editing with vsCode 

code .

You may get the following error if you have not setup a git token

> git push origin master:master
remote: Support for password authentication was removed on August 13, 2021.
remote: Please see https://docs.github.com/get-started/getting-started-with-git/about-remote-repositories#cloning-with-https-urls for information on currently recommended modes of authentication.
fatal: Authentication failed for 'https://github.com/youroldmangaming/DOX.git/'



The error you're encountering is due to GitHub's removal of support for password authentication when pushing to repositories over HTTPS. To resolve this, you need to authenticate using a personal access token (PAT) instead of your GitHub password. Here’s how to fix this:

Steps to Fix Authentication Issue with GitHub
1. Generate a Personal Access Token (PAT)
Go to GitHub and sign in.
Navigate to GitHub's token generation page.
Click Generate new token (or Generate new token (classic) if that option is visible).
Give your token a descriptive name (e.g., Git Push Token).
Set the expiration (recommended: no expiration or select an appropriate duration).
Under Select scopes, choose the following scopes:
repo (this is necessary for full control over repositories).
Click Generate token.
Copy the token immediately as it will not be shown again.

2. Update Local Git Authentication
Now that you have a personal access token, you need to use it in place of your password:

In your terminal, run:

bash
Copy code
git remote -v
This shows the current URLs for your Git remotes. You will likely see something like:

origin  https://github.com/youroldmangaming/DOX.git (fetch)
origin  https://github.com/youroldmangaming/DOX.git (push)
Update the remote URL to include your GitHub username:


git remote set-url origin https://<your-username>@github.com/youroldmangaming/DOX.git

3. Push Using the Personal Access Token
Now try pushing to GitHub again. When prompted for a username and password:

For the username, use your GitHub username.
For the password, paste the personal access token that you generated earlier.
Example command:

bash
Copy code
git push origin master:master

4. (Optional) Store Your Credentials
To avoid entering the token every time you push, you can store your TOKEN locally.

When asked for a passwd when submitting changes to GIT use the token.


