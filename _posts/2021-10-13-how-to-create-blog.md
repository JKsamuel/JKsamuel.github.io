---
title: Create GitHub Pages
layout: post
post-image: /assets/images/howto.png
description: This post will giude how to create new Github Pages
tags:
- Create
- Blog
- Git
- Pages
- Terminal
---

# How to create GitHub pages
For developer, the Github Pages seemed to have a prerequisite. Especially for people like me who have no experience in this industry, it seems more necessary.

To make a GitHub blogs, this seemingly really simple work was actually not easy for me. To create this homepage, I had to find out what Git is and know the git command. And I had to know about Jekyll and Markdown, althogh not yet depp. Moreover I had to know Ruby too.

I need to organize what I studied. I'm going to start with this page.

___
## Step1: Creating a repository on Github
Before you starting, absolutely you need an account of Github. If you don't have, create an account.
Then you can see '+' on the top right side of the browser and also you can find the green button which are in the red box on the picture below.

![Create a repository](/assets/createPages/step1_0.png)

Once you click the '+' or New button, then you can create a new repository

![New repository](/assets/createPages/step1_1.png)

Like a webpage which has own name, your repository also needs a special name to generate website.

![Namning website](/assets/createPages/step1_2.png)
1. **The syntax of respository name**
	```
	username.github.io
	```
	I already created my repository, therefore I put "test" between {username} and {github}
	(username is your actual GitHub username)
	According to [GitHub Docs](https://docs.github.com/en/pages/getting-started-with-github-pages/creating-a-github-pages-site), you must be named user.github.io

	Ref: 
	1. [GitHub Guides](https://guides.github.com/features/pages/)
	2. [About GitHub Pages](https://docs.github.com/en/pages/getting-started-with-github-pages/about-github-pages#types-of-github-pages-sites)

2. **Select "Public" radio button.**
3. **Check "Add a README file"**
4. **Click Create repository.**


Conglats! Now you have a repository.

___
## Step2: Clonning

I assume that there are many different way to create a pages. My way is below:

1. Firstly, you have to create a folder on your laptop or PC. Go back to your repository page on GitHub

    ![Clone](/assets/createPages/step2_0.png)

    **Click the Code button**

    
    ![Clone2](/assets/createPages/step2_1.png)

    **Click the button then it will copy your repository address**

2. Open your shell(Terminal or iterm2)
And set the address where you create a new folder

    ```
    git clone [copied address] 
    ```

    e.g.1
    ```
    git clone https://github.com/JKsamuel/JKsamuel.github.io.git
    ```


    e.g.2

    ![example](/assets/createPages/step2_2.png)


**Done! Easy!**

Now, We need to check it is done.
Go to the folder you create for git repositor, you can see the README.md and hidden folder named .git
hidden folder .git is When you init git, then created automatically.

___
## Step3: Choosing a them.
There are some websites which provide theme.

You can pick one from below.

* [jamstackthemes.dev](https://jamstackthemes.dev)
* [jekyllthemes.org](https://jekyllthemes.org)
* [jekyllthemes.io](https://jekyllthemes.io)
* [jekyll-themes.com](https://jekyll-themes.com)

Or You can find "Settings" under your repository name,

1. Click **Settings**.

	![step3-0](/assets/createPages/step3_0.png)

2. In the left sidebar, click **Pages**.

	![step3-1](/assets/createPages/step3_1.png)

3. Click **Choose a theme** button

	![step3-2](/assets/createPages/step3_2.png)

4. You can choose a theme what you want and then **Download** .zip file.

	![step3-3](/assets/createPages/step3_3.png)


Lastly, unzip the downloaded compressed file and copy and paste all the files in that folder into the newly created folder earlier.

So far, three procedures have been a preparation process for creating a web page. It is kind of preparing a canvas to draw.

___
## Step4: Excute Terminal

1. Go to the position where you created, and init jekyll
    ```
    jekyll new ./
    ```

2. install bundle
    ```
    bundle install
    ```
3. posting jekyll to local server
    ```
    bundle exec jekyll serve
    ```
4. and then you can see the server address

    ![step4-0](/assets/createPages/step4_0.png)

	  Put the address on your browser as above or you can put address as "https://localhost:4000" on your browser.
	  Then you can see what you changed.

This is only hosting a webpage you created on a local computer. When you created and edited the page in the way you wanted, you should upload it to GitHub Pages.


___
## Step5: Hosting the web to "username.github.io"

To host the webpages we created on GitHub Pages, you need to know the Git command. There seems to be nothing easy in the world.
(I will post about Git command and what Git is later)
I had to devote today to creating this page. I would like to define Git as a tool that allows developers to manage and collaborate on versions of programs produced. It's not official. It's my own opinion. Fortunately, I wwas able to understand Git a little while learning php language this semester.

anyway, open your Ternimal or iTerm2.
you go to your respository folder.
1. **Check what is changed.**
	```
	git status
	```
	The git status command displays the state of the working directory and the staging area.
	You can realize what has been changed.

2. **Adds to the staging area**

	```
	git add --all
	```

	The git add command adds a change in the working directory to the staging area. 
	This is not really affect the repository in any significant way-changes are not actually recorded until you run git commit.


3. **run git commit**

	```
	git commit -m "[Commit message]"
	```

	The git commit command captures a snapshot of the project's currently staged chages. Git will never change them unless you explicity ask it to.


4. **Upload**
  ```
	git push
	```


The git push command is used to upload local repository content to remote repository.

Reference: [Atlassian.com](https://atlassian.com/git/tutorials)