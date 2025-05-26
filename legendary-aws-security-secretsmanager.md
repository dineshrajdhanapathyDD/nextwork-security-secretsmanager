<img src="https://cdn.prod.website-files.com/677c400686e724409a5a7409/6790ad949cf622dc8dcd9fe4_nextwork-logo-leather.svg" alt="NextWork" width="300" />

# Secure Secrets with Secrets Manager

**Project Link:** [View Project](http://learn.nextwork.org/projects/aws-security-secretsmanager)

**Author:** Dineshraj Dhanapathy  
**Email:** dineshrajdhanapathy@gmail.com

---




## Secure Secrets with Secrets Manager

Can AWS keep a secret? 🤫 Let's use AWS Secrets Manager to secure the credentials in your code!

## ⚡️ 30 second Summary

Welcome to this project on **Securing Secrets with AWS Secrets Manager**! 🔐

Think about all the apps you use daily - from your favorite social media to your banking apps. They all need to store sensitive information in their code to connect to their databases and services behind the scenes...

So a leak in their code could become a major security breach! Not only does it expose their code, but it also exposes their credentials. Unauthorized users could get direct access to the company's databases and services, which puts their _customers'_ data at risk.

So how do companies keep their credentials and API keys safe? In this project, we'll show you how to use AWS Secrets Manager to practice secure credential management.

These skills are highly sought after by employers and are essential for roles like **Backend Developer**, **Security Engineer**, or **Cloud Developer**, where it's important to use scalable and compliant methods to protect sensitive information.

### In this project, get ready to...

-   🕵️‍♀️ Identify how a web app is insecurely storing credentials.
    
-   😮‍💨 See how GitHub's secret scanning feature can block insecure code from being pushed to a repository.
    
-   🔐 Update the web app to use AWS Secrets Manager to store and retrieve credentials securely.
    
-   👏 Verify that secured web app code can be made public without exposing sensitive credentials.
    
  
  ![](https://learn.nextwork.org/projects/static/aws-security-secretsmanager/architecture-complete.png)

  
## 👀 Step #0

### 

Before we start Step #1...

In this project, we're going to learn how to use AWS Secrets Manager to securely store and retrieve secrets.

Imagine that you're about to build a web app that displays the names of the S3 buckets under your account. Now, for the web app to showcase your S3 buckets, it's gonna need AWS credentials. How should your web app get those credentials?

The first thing that might come to mind is to store the credentials in the web app's code itself. This is what your web app is going to do initially... but as you'll find out, this is not the greatest idea 🙈

When credentials are **hardcoded** into your code, they can be easily accessed by anyone who has access to your code. And sharing code happens all the time - especially when you decide to showcase your project publicly in a GitHub portfolio!

This is why it's important to store credentials in a secure way. We're going to learn about a better way to store and retrieve these credentials using AWS Secrets Manager.

  
  

**How in the world do we update an insecure web app to use AWS Secrets Manager?**

![](https://learn.nextwork.org/projects/static/aws-security-secretsmanager/architecture-annotated.png)

  

-   In 🔎 Step #1, we'll take a look at a simple web app that lists S3 buckets, clone it to our computer, and set it up to hardcode AWS credentials.
    
-   In 🍴 Step #2, we'll see what happens when we share this code (with the credentials hardcoded!) publicly on GitHub.
    
-   In 🔒 Step #3, we'll secure the credentials using AWS Secrets Manager.
    
-   In 📝 Step #4, we'll update the web app code to use the credentials from Secrets Manager.
    
-   In ⬆️ Step #5, we'll try share the code on GitHub again, and see how you can wipe your repository clean of all the hardcoded credentials!
    
-----

## 🔎 Step #1

### 

Examine and Set Up the Code

In this step, we'll take a look at a simple web app that lists S3 buckets, copy the code to our computer, and set it up to hardcode some AWS credentials 😬

  
  

**In this step, you're going to:**

-   Access a **GitHub** repository containing a web app's code
    
-   Identify where **credentials** are stored (insecurely!)
    
-   **Clone** the repository to your computer
    
-   **Hardcode** AWS credentials (which means writing them directly into the code, instead of retrieving them in a secure way)
    



![](https://learn.nextwork.org/projects/static/aws-security-secretsmanager/architecture-1.0.png)

  

**Access and Explore the Repository**

-   **Open** your web browser and head to [this GitHub repository.](https://github.com/NatNextWork1/nextwork-security-secretsmanager)
    
-   This GitHub repository contains the code for our insecure web app. This web app will display the names of S3 buckets in your AWS account.
    

> 💡 **What is GitHub?**  
> **GitHub** is a platform where developers share and collaborate on code. It's like a social media for code!
> 
> Public repositories on GitHub are great for open-source projects, but it also means that anyone can see the code. This is why it's super important to **never** put sensitive information, like passwords or AWS credentials, directly into your code, especially if it's in a public repository!

![GitHub repository page.](https://learn.nextwork.org/projects/static/aws-security-secretsmanager/screenshot-0.png)

GitHub repository page.

  

  
  

**Understand the Application Code**

-   In the repository's file list, select `app.py`.
    

> 💡 **What is app.py?**  
> `app.py` is the main file containing the logic of our web app i.e. how the app will respond to user requests and interact with AWS services. In our case, `app.py` contains all the code that connects to your AWS account and lists the names of your S3 buckets.
> 
>   
> 💡 **Extra for Experts: How is app.py built?**  
> `app.py` is built using **FastAPI**, a web framework for building **APIs** with Python. Think of it as a set of tools and rules that make it easier to build web apps that can talk to each other over the internet.
> 
> FastAPI helps developers like us create APIs quickly and efficiently, with features like automatic data validation and interactive documentation.

![File directory listing with `app.py` highlighted.](https://learn.nextwork.org/projects/static/aws-security-secretsmanager/screenshot-1.png)

File directory listing with `app.py` highlighted.

  

Let's explore `app.py` and understand how it works!

-   Look at the very top of the `app.py` file.
    
-   Find the lines that start with `import`. These lines are **import statements**.
    

> 💡 **What are import statements?**  
> Import statements bring in external code libraries that our application needs to work. These libraries provide pre-built functionality so we don't have to write everything from scratch.

![Code snippet from `app.py` showing import statements.](https://learn.nextwork.org/projects/static/aws-security-secretsmanager/screenshot-2.png)

Code snippet from `app.py` showing import statements.

  

-   Next, in line 4, notice the line `# Import your temporary, hard-coded credentials` followed by `import config`.
    

> 💡 **What does `import config` do?**  
> This line tells `app.py` to load settings from another file named `config.py`. We'll check out `config.py` soon to see what kind of settings it holds.

![Code snippet from `app.py` highlighting `import config` line.](https://learn.nextwork.org/projects/static/aws-security-secretsmanager/screenshot-3.png)

Code snippet from `app.py` highlighting `import config` line.

  

-   Scroll down to see the `read_index` function. This function defines what happens when you visit the main page of the web app.
    

![Code snippet from `app.py` showing the `read_index` function for the landing page.](https://learn.nextwork.org/projects/static/aws-security-secretsmanager/screenshot-5.png)

Code snippet from `app.py` showing the `read_index` function for the landing page.

  

-   Scroll further down to find the `list_s3_buckets` function. This function is responsible for retrieving and displaying your S3 buckets.
    

![Code snippet from `app.py` showing the `list_s3_buckets` function for retrieving S3 buckets.](https://learn.nextwork.org/projects/static/aws-security-secretsmanager/screenshot-6.png)

Code snippet from `app.py` showing the `list_s3_buckets` function for retrieving S3 buckets.

  

  
  

**Examine `config.py`**

-   In the file list on the left, let's select `config.py` next.
    

![File directory listing with `config.py` highlighted.](https://learn.nextwork.org/projects/static/aws-security-secretsmanager/screenshot-7.png)

File directory listing with `config.py` highlighted.

  

> 💡 **What is `config.py`?**  
> `config.py` is a file that contains the configuration settings for our web app. Think of it as your app's settings file.
> 
> In our case, `config.py` contains the AWS credentials for our web app - `AWS_ACCESS_KEY_ID`, `AWS_SECRET_ACCESS_KEY`, and `AWS_REGION`. Without these credentials, `app.py` wouldn't be able to connect to your AWS account.

![Code snippet from `config.py`](https://learn.nextwork.org/projects/static/aws-security-secretsmanager/new-23.png)

Code snippet from `config.py`

  

-   Let's see how `app.py` uses these credentials.
    
-   Go back to `app.py` by selecting it in the file list.
    
-   Find the `list_s3_buckets` function again. You can see that it uses `config.AWS_ACCESS_KEY_ID`, `config.AWS_SECRET_ACCESS_KEY`, and `config.AWS_REGION` to connect to AWS.
    

![Code snippet from `app.py` highlighting how credentials from `config.py` are used in `list_s3_buckets`.](https://learn.nextwork.org/projects/static/aws-security-secretsmanager/screenshot-9.png)

Code snippet from `app.py` highlighting how credentials from `config.py` are used in `list_s3_buckets`.

  

  
  

**The Security Risks...**

Notice how the `AWS_ACCESS_KEY_ID` and `AWS_SECRET_ACCESS_KEY` are directly written in `config.py`. If this were a real, public application, and you shared the code on a GitHub repository, anyone could see these credentials! 😱

Exposing your AWS credentials publicly is a **major security risk** and is **extremely unsafe** for production applications. Once someone else gets access to your credentials, they can use them to access your AWS account, delete resources, steal data, and cause damage.

It's not just AWS credentials; database passwords, API keys for other services, and any kind of secret should **never** be directly embedded in your code.



To make this web app more secure, we'll make a copy of it in our local computer, and then we'll play around with how `config.py` stores credentials in our copy.

  
  

**Set Up Your Local Environment**

We'll be using the terminal to make a copy of the GitHub repository's code:  
  

Mac/LinuxWindows

-   Open **Command Prompt**. You can find it by searching for "Command Prompt" in the Start menu.
    

> 💡 **What is a terminal?**  
> A terminal is where you can interact with your computer (e.g. download files, run programs, install tools, etc.) using text commands. It's like you're telling your computer what to do in a chat, except you're using special commands instead of words!

![Mac OS desktop showing the Terminal application in Spotlight search.](https://learn.nextwork.org/projects/static/aws-security-secretsmanager/screenshot-10.png)

Mac OS desktop showing the Terminal application in Spotlight search.

  

From your Command Prompt, let's head to your **Documents** folder, a common place to store project files. We'll clone the repository there.

-   In your Command Prompt, type `cd Documents` and press **Enter**.
    

![Terminal showing the command `cd Documents`.](https://learn.nextwork.org/projects/static/aws-security-secretsmanager/screenshot-11.png)

Terminal showing the command `cd Documents`.

  

  
  

**Clone the GitHub Repository**

Now we'll **clone** the GitHub repository to your local machine. **Cloning** copies all the files from the GitHub repository to a new folder in your computer.

-   In your terminal, type the following command to clone the repository:
    


```bash
git clone [HTTPS-URL]

```

> 🙋‍♀️ **Where do I find the HTTPS-URL?**  
> 
> ![GitHub repository page with the green ](https://learn.nextwork.org/projects/static/aws-security-secretsmanager/code.png)
> 
> GitHub repository page with the green
> 
> You can find the HTTPS-URL by heading back to the GitHub repository and clicking the green **Code** button.

![Terminal showing the command `git clone https://github.com/NatNextWork1/nextwork-security-secretsmanager.git`.](https://learn.nextwork.org/projects/static/aws-security-secretsmanager/screenshot-12.png)

Terminal showing the command `git clone https://github.com/NatNextWork1/nextwork-security-secretsmanager.git`.

  

-   Press **Enter** to run the command. You should see output in your terminal showing you that the repository is being cloned.
    

![Terminal showing the output of the `git clone` command, showing you successful cloning.](https://learn.nextwork.org/projects/static/aws-security-secretsmanager/screenshot-13.png)

Terminal showing the output of the `git clone` command, showing you successful cloning.

  

> 🙋‍♀️ **I got an error: `git: command not found`**  
> If you see an error message like `git: command not found`, it means **Git** is not installed on your computer!
> 
> -   **Install Git**. You can download it from [https://git-scm.com/downloads](https://git-scm.com/downloads).
>     
> -   Follow the installation instructions for your operating system.
>     
> -   After installing Git, try running the `git clone` command again.
>     

-   After cloning, we need to go inside the newly created folder that contains the web app's code.
    
-   In your terminal, type `cd nextwork-security-secretsmanager` and press **Enter**.
    

  
  

Let's double-check that we're in the correct folder:  
  

Mac/LinuxWindows

-   Type `cd` and press **Enter** to see your current directory.
    
-   You should see the path to the `nextwork-security-secretsmanager` directory printed in your terminal.
    
-   Type `dir` and press **Enter** to list the files in your directory.
    
-   You should see files like `app.py`, `config.py`, `Dockerfile`, and `requirements.txt`. Yup, these are the same files we saw in the GitHub repository!
    

> 💡 **Extra for Experts: What do these files do?**  
>   
> 
> -   **Dockerfile**: Used for building the Docker image for the application - if you were to deploy this app, you would use this file to deploy it to container services like AWS ECR, Elastic Beanstalk or ECS.
>     
> -   **app.py**: The main application file, containing the FastAPI code that defines the web app.
>     
> -   **config.py**: Contains AWS credentials (currently hardcoded - this is what we'll fix!).
>     
> -   **index.html**: The landing page for the web app, a simple HTML file.
>     
> -   **requirements.txt**: Lists the Python libraries required by the application.
>     
> -   **static**: Folder containing static files (like CSS) for the web app's styling.
>     

-   Open `config.py` in a text editor. You can use `notepad config.`
    

-   Replace the placeholder values in `config.py` with these example AWS credentials:
    
**Dummy Data**


```python
AWS_ACCESS_KEY_ID = "AKIAW3MEFRAFTQM5FHKE"
AWS_SECRET_ACCESS_KEY = "F0b8s5m+pOZsttvBCirr1BOutuvCpqXMW2Y1qAxY"
AWS_REGION = "us-east-2"

```

![Screenshot 2025-04-01 132330](https://github.com/user-attachments/assets/1eafc481-9d19-4c1c-ad0a-3dd068fd0f9c)

> 💡 **What are these credentials?**  
> These are **example access keys** for AWS - we're going to imagine that these are real AWS credentials that would give someone access to your AWS account!
> 
> We're using these test credentials as it's easier and safer to use than real credentials that would expose **your** account to security risks. Because these credentials are fake, putting them in `config.py` won't actually let your web app work (you won't see this web app display your S3 buckets' names).
> 
>   
> 💡 **What are AWS access keys?**  
> **AWS access keys** are like a username (Access Key ID) and password (Secret Access Key) that let applications to access your AWS account programmatically. When an application uses these keys, AWS treats the requests (e.g. creating or deleting resources, accessing data) as if they're coming directly from you.
> 
> This is why securing your access keys is critical - if someone gets hold of them, they could use all your AWS services with your credentials, potentially racking up large bills or accessing sensitive data in your account.
> 
>   
> 🙋‍♀️ **Noooooo, I wanted to see the web app work!**  
> If you want to see the web app work, you'll need to create your own AWS credentials and put them in `config.py`. Wanna do this? You can!
> 
> We'll show you how to in the 💎 Secret Mission below.



-   Save the changes you've made in `config.py`:  
      
    

macOS/LinuxWindows

-   Press `Ctrl+S` to save the file
    
-   Close Notepad
    

## 💎 Secret Mission

### 

Run the Web App with your own AWS Credentials

**Want to see this app in action?** Here's your chance!

Let's put some skin in the game... your mission, should you choose to accept it, is to run this web app using your very own AWS credentials.

By doing this secret mission, you'll also get first-hand experience on why we need to secure our credentials. Later in this project, you'll need to take extra care that the real credentials you put in here don't get exposed publicly!

  
  

**In this step, you're going to:**

-   Create your own AWS credentials.
    
-   Update the app's configuration with real credentials.
    
-   Run the app to see it display your S3 buckets' names!
    

> 💎 **Congratulations** - Secret Mission unlocked!



  
  

**Create Your Virtual Environment**

-   Still in your terminal and project directory, let's create a new virtual environment:
    



```bash
python3 -m venv venv
```

> 💡 **What is a virtual environment?**  
> Just like how you might have different folders for different school subjects, a virtual environment is like a folder that keeps all the tools and packages for a specific project in one place. In our case, the virtual environment will keep all the packages we need to run this web app.
> 
> The best part? When you delete this virtual environment, all the packages you installed get removed too, keeping your computer clean of unused packages.
> 
> Without a virtual environment, tracking the packages or libraries you've installed for a project can be tricky. It also be harder to run two different versions of the same tool, if two projects need different versions.
> 
>   
> 💡 **What are packages or libraries?**  
> Packages or libraries are pre-written code that you can use in your project. For example, if you're building a weather app, you could use a package that lets you make API requests to get the weather data.

-   What do you see in your terminal?  
      
    

Nothing...I ran into an error

If you ran into an error that says `python3` or `pip3` is not found, that tells us that you need to install Python and pip for your operating system.

> 💡 **Why are we downloading both?**
> 
> -   **Python** is the programming language used to build our API. Notice how the `.py` in `main.py` means it's a Python file! You're going to need Python to run the main.py file.
>     
> -   **Pip** is the package installer for Python. You're going to need a few packages to run the API, and Pip will help you install them.
>     

**📝 Install Python and pip**  
  

MacOSWindowsLinux

-   First, let's test using `python` instead of `python3`:
    



```bash
python -m venv venv

```

-   If you still get an error, let's download Python from [python.org](https://www.python.org/downloads/) and follow the installation instructions.
    
-   Make sure to add Python to your PATH during installation. Once you've installed Python, you should also have Pip automatically installed.
    

-   Check that you have both Python and pip installed by running:
    


```bash
python3 --version
pip3 --version

```

-   You should see a version number for both Python and pip in your terminal!
    

![](https://learn.nextwork.org/projects/static/ai-rag-webapp/new-24.png)

  

-   Try creating a new virtual environment:
    



```bash
python3 -m venv venv

```

> 🙋‍♀️ **The command didn't work!**  
> If the command didn't work, try running it again with `python` instead of `python3`.

![](https://learn.nextwork.org/projects/static/ai-rag-api/capture.png)

  

-   Activate your virtual environment:  
      
    



```powershell
.\venv\Scripts\activate

```

> 💡 **What does it mean to activate a virtual environment?**  
> When you **activate** a virtual environment, you're telling your computer to start using the virtual environment you created:
> 
> -   All the Python packages you install will only exist in this environment.
>     
> -   Your project won't interfere with other Python projects on your computer.
>     

-   You should see `(venv)` at the beginning of your command prompt, telling us that the virtual environment is activated.
    

![Terminal showing the command `source venv/bin/activate` and the activated virtual environment prompt `(venv)`.](https://learn.nextwork.org/projects/static/aws-security-secretsmanager/screenshot-18.png)

Terminal showing the command `source venv/bin/activate` and the activated virtual environment prompt `(venv)`.

  

  
  

**Run the Web App**

-   In your terminal, type `python3 app.py` and press **Enter**.
    

![Terminal showing the command `python3 app.py`.](https://learn.nextwork.org/projects/static/aws-security-secretsmanager/screenshot-20.png)

Terminal showing the command `python3 app.py`.

  

Oops! We got an error: `ModuleNotFoundError: No module named 'boto3'`. This means our application needs a library called `boto3` that isn't installed in our virtual environment yet.

![Terminal showing the `ModuleNotFoundError: No module named 'boto3'` error.](https://learn.nextwork.org/projects/static/aws-security-secretsmanager/screenshot-21.png)

Terminal showing the `ModuleNotFoundError: No module named 'boto3'` error.

  

> 💡 **What is boto3?**  
> **boto3** is the **AWS SDK for Python**. SDK stands for Software Development Kit. Think of it as a toolbox that lets Python applications easily talk to and interact with AWS services, like S3, Secrets Manager, and many others.
> 
> We need `boto3` in our application because we want to list S3 buckets, which are AWS resources.

  
  

**Install Requirements**

Aside from `boto3`, there are also other packages that you'll need to run your API. These packages are all listed in the `requirements.txt` file in your project directory.

Let's look at all the required packakges by looking inside the `requirements.txt` file.  
  

Mac/LinuxWindows

-   Run the command `type requirements.txt` and press **Enter**.
    

-   You should see `boto3` listed in the `requirements.txt` file, along with other packages like `fastapi` and `uvicorn`.
    

![Terminal showing the command `cat requirements.txt` and the content of the `requirements.txt` file.](https://learn.nextwork.org/projects/static/aws-security-secretsmanager/screenshot-22.png)

Terminal showing the command `cat requirements.txt` and the content of the `requirements.txt` file.

  

-   In your terminal, type `pip3 install -r requirements.txt` and press **Enter**.
    

> 💡 **What does this command do?**  
> This command uses `pip`, the Python package installer, to install all the libraries listed in `requirements.txt`. In our case, this will install `boto3`, FastAPI, and other necessary packages.

![Terminal showing the command `pip3 install -r requirements.txt` and the installation process.](https://learn.nextwork.org/projects/static/aws-security-secretsmanager/screenshot-23.png)

Terminal showing the command `pip3 install -r requirements.txt` and the installation process.

  

-   You should see output in your terminal showing you that the packages are being installed.
    

![Terminal showing the successful installation of packages from `requirements.txt`.](https://learn.nextwork.org/projects/static/aws-security-secretsmanager/screenshot-24.png)

Terminal showing the successful installation of packages from `requirements.txt`.

  

  
  

**Verify Installed Packages**

Have we installed everything in our `requirements.txt` file? To confirm, let's list all the packages now installed in our virtual environment.

-   In your terminal, type `pip3 list` and press **Enter**.
    

![Terminal showing the command `pip3 list` and the list of installed packages, including `boto3`.](https://learn.nextwork.org/projects/static/aws-security-secretsmanager/screenshot-25.png)

Terminal showing the command `pip3 list` and the list of installed packages, including `boto3`.

  

-   You should see `boto3` and other packages like `fastapi` and `uvicorn` in the list of installed packages.
    
-   Now that we've installed the dependencies, let's run the application again.
    
-   In your terminal, type `python3 app.py` and press **Enter**.
    

![Terminal showing the command `python3 app.py` and the application starting up without errors.](https://learn.nextwork.org/projects/static/aws-security-secretsmanager/screenshot-26.png)

![Screenshot 2025-04-03 042057](https://github.com/user-attachments/assets/8c4a1052-9c24-4e28-8845-b456733c1204)

Terminal showing the command `python3 app.py` and the application starting up without errors.

  

-   This time, you should see a different output, showing you that the application has started successfully. You should see a message like `Uvicorn running on http://0.0.0.0:8000`. This means your web app is now running locally!
    
-   Nice - no more errors 👏
    

![Terminal showing the message ](https://learn.nextwork.org/projects/static/aws-security-secretsmanager/screenshot-27.png)

Terminal showing the message

  

  
  



**Access the Web App in Your Browser**

Looks like the app is running at `http://0.0.0.0:8000` - let's open this address in your browser to see the web app.

-   Open your web browser (like Chrome, Firefox, Safari, or Edge).
    
-   In the address bar, type `http://0.0.0.0:8000` and press **Enter**.
    
-   You should see a simple webpage with the title **Welcome to My AWS Demo App**!
    

![Browser showing the ](https://learn.nextwork.org/projects/static/aws-security-secretsmanager/screenshot-28.png)

Browser showing the

![Screenshot 2025-04-03 042956](https://github.com/user-attachments/assets/e9def569-5e63-4cd6-9435-ea4602584c49)
  

-   Let's test what the app is supposed to do - list the S3 buckets in your AWS account.
    
-   Select the blue button labeled **View My S3 Buckets**. This will trigger the API call in `app.py` to list your S3 buckets.
    

![Browser showing the JSON error message ](https://learn.nextwork.org/projects/static/aws-security-secretsmanager/screenshot-29.png)

Browser showing the JSON error message

![Screenshot 2025-04-03 043026](https://github.com/user-attachments/assets/33ccf3ac-8b0e-4be1-9b23-cb821b9a3af3)
  

-   Damn - we got an error again! Guess that didn't work :(
    
-   Check the JSON response in your browser.
    
-   This time, it's a JSON response saying `{"error":"An error occurred (InvalidAccessKeyId)..."}`.
    

> 💡 **What does this error mean?**  
> This error means the **AWS Access Key ID** provided for the web app is not valid. Our app is running, but it can't access your AWS account because it doesn't have valid credentials!
> 
> This is because we are using the **placeholder credentials** in `config.py`, which are not real AWS credentials.



Let's get you some real AWS credentials and put them in our app! We're going to create an Access Key in IAM (if you don't already have one) and update our `config.py` file with these real AWS credentials. This will let our local web app connect to your AWS account and list your S3 buckets.

  
  

**Set up Your AWS Credentials**

-   Log in to the AWS Management Console [as your IAM Admin user.](https://signin.aws.amazon.com/oauth?redirect_uri=https%3A%2F%2Fconsole.aws.amazon.com%2Fconsole%2Fhome%3Fstate%3DhashArgs%2523%26isauthcode%3Dtrue&client_id=arn%3Aaws%3Aiam%3A%3A015428540659%3Auser%2Fhomepage&response_type=code&iam_user=true&account=)
    

![AWS Management Console Home page.](https://learn.nextwork.org/projects/static/aws-security-secretsmanager/screenshot-30.png)

AWS Management Console Home page.

  

-   In the top right corner of the AWS Management Console, find the region dropdown menu and select your preferred AWS Region. Make sure you're using a region where there is an S3 bucket in your account.
    

> 💡 **Why the same region?**  
> It's important to keep your resources in the **same AWS region** for this project. This makes sure that your web app, the S3 buckets, any other AWS resources you might use in this project (ahem, ahem, the secret you'll later create in Secrets Manager) can access each other.
> 
> Using the same region can also improve performance and reduce latency, so it's generally a good practice to organize your AWS resources in the same region.

![Region selector dropdown menu with Ohio (`us-east-2`) selected.](https://learn.nextwork.org/projects/static/aws-security-secretsmanager/screenshot-31.png)

Region selector dropdown menu with Ohio (`us-east-2`) selected.

  

  
  

I already have an S3 bucket in my regionHmmm, I don't have any S3 buckets in my account!

That's okay! Lucky for us, it's easy to create a new S3 bucket.

-   Head to the `S3` console.
    
-   Select **Create bucket**.
    

> 💡 **What is Amazon S3?**  
> Amazon S3 (Simple Storage Service) is AWS's storage system. Think of it as a digital folder where you can store your documents, photos, videos, and other files. We won't need to store anything for this project, but we'll create a bucket anyway, so you can see your web app working when it displays your bucket's name.

![](https://learn.nextwork.org/projects/static/ai-rag-cli/step-1.7.png)

* For **Bucket name**, enter a unique name like `nextwork-security-secretsmanager-<your-initials>`.

> 💡 **Extra for Experts: Why add your initials?**  
> S3 bucket names must be globally unique across all AWS accounts. Adding your initials helps ensure your bucket name won't conflict with other users' buckets.
> 
> Make sure your bucket name:
> 
> -   Only uses lowercase letters, numbers, dots (.), and hyphens (-)
>     
> -   Starts with a letter or number
>     
> -   Is between 3 and 63 characters long
>     

-   Under **Object Ownership**, leave **ACLs disabled (recommended)** selected.
    

> 💡 **Extra for Experts: Why leave ACLs disabled?**  
> This makes managing permissions simpler. The bucket owner gets full control, which helps prevent accidental permission issues.

-   Under **Block Public Access settings**, make sure **Block all public access** is checked (default setting).
    

> 💡 **Extra for Experts: Why block public access?**  
> This keeps your data secure by ensuring only authorized users and services can access your bucket. Since your chatbot's training data doesn't need to be publicly accessible, this is the safest option.

-   Leave the default versioning and encryption settings.
    

> 💡 **Extra for Experts: Why use encryption?**  
> Encryption automatically encrypts your data when it's stored, adding extra security. If anyone tries to access your documents without the right permissions, they won't be able to read them.

-   Select **Create bucket**.
    

![](https://learn.nextwork.org/projects/static/test_project_full/screenshot-3.png)

  

Now, we'll set up your web app's credentials to access your S3 bucket. Let's go to the **IAM (Identity and Access Management)** service - IAM is where you manage access to your AWS resources.

  
  

**Set up Your Credentials**

-   In the search bar at the top of the AWS Management Console, head to the `IAM` service.
    

> 💡 **What is IAM?**  
> **IAM (Identity and Access Management)** is a service that lets you control who has access to your AWS resources and what they can do with those resources. Think of it as the security guard for your AWS account.
> 
> With IAM, you can create users, groups, and roles, and give them specific permissions to access different AWS services and resources. This helps you manage security and ensure that only authorized people and applications can access your AWS environment.

![AWS Services menu with ](https://learn.nextwork.org/projects/static/aws-security-secretsmanager/screenshot-32.png)

AWS Services menu with

  

-   In the left navigation pane of the IAM Dashboard, select **Users**.
    

![IAM Dashboard with ](https://learn.nextwork.org/projects/static/aws-security-secretsmanager/screenshot-33.png)

IAM Dashboard with

  

-   In the list of IAM users, select your IAM user that has **Administrator** permissions to open the user details page.
    

![IAM Users list, highlighting an example Admin user.](https://learn.nextwork.org/projects/static/aws-security-secretsmanager/screenshot-34.png)

IAM Users list, highlighting an example Admin user.

  

-   In the user's details page, select the **Security credentials** tab.
    

![IAM User details page with the ](https://learn.nextwork.org/projects/static/aws-security-secretsmanager/screenshot-35.png)

IAM User details page with the

  

-   Scroll down to the **Access keys** section.
    
-   Click the **Create access key** button.
    
-   In the **Create access key** page, under **Use case**, select **Local code**.
    

> **💡 Extra for Experts: what are all these options about?**  
> Let's break 'em down!
> 
> -   Pick **Command Line Interface (CLI)** when you're using AWS CLI commands to manage your AWS resources.
>     
> -   Pick **Local code** (what we picked) when you're building an application in your local computer, and need the access key to connect your app with AWS services.
>     
> -   Pick **Application running on an AWS compute service** when you're building an application that's running on an AWS compute service like EC2 or Lambda, and need the access key to connect your app with AWS services.
>     
> -   Pick **Third-party service** when you're running an application from an external company (i.e. it wasn't built by yourself or AWS), but requires access to your AWS environment. For example, Crowdstrike is a third party security service that requires access to your AWS environment to protect them in real time!
>     
> -   Pick **Application running outside AWS** when you're running an application built by yourself but hosted on a physical server or another cloud platform, and you need to integrate it with AWS services.
>     
> 
>   
>   
> 
> Note that the same access key can actually be used for _all_ of the use cases on this page. AWS is just making you pick one so that it can recommend some best practices for your use case.

![](https://learn.nextwork.org/projects/static/aws-security-secretsmanager/screenshot-36.png)

  

-   At the bottom of this page, AWS recommends some best practice alternatives you could consider instead of creating an access key. We'll go ahead and create an access key for our web app. Check the confirmation box.
    
-   Click **Next**.
    

![Confirmation step in ](https://learn.nextwork.org/projects/static/aws-security-secretsmanager/screenshot-37.png)

Confirmation step in

  

-   For the **Description tag value**, enter `nextwork-security-secretsmanager`
    

![](https://learn.nextwork.org/projects/static/aws-security-secretsmanager/screenshot-38.png)

  

-   Click the **Create access key** button to create your access key!  
      
    

![](https://learn.nextwork.org/projects/static/aws-security-secretsmanager/screenshot-39.png)

  

-   Well done! Your access key is created, and AWS has set you up with an **Access key** and **Secret access key.**
    

![](https://learn.nextwork.org/projects/static/aws-security-secretsmanager/screenshot-40.png)

  

-   **✋ STAY ON THIS PAGE**
    
-   🚨 **Select Download .csv file.** This is the ONLY time you can see your Secret Access Key. Downloading the .csv file is how you can keep these credentials! We'll make sure to delete everything at the end of this project.
    

> **🙋‍♀️ Uh oh, I've already left this page**  
> Ah, classic! Don't worry, you'll just need to delete the key you've set up and create another one. Don't forget to stay on the page the second time round.
> 
> If you're stuck while deleting your access key, you can jump to our deletion step for help!

Now that we have our AWS credentials, let's update our `config.py` file. First, stop the web app running in your terminal by pressing `Ctrl+C`.

-   Go back to your terminal where the web app is running.
    
-   Press `Ctrl+C` (Control + C) to stop the running web app.
    

![Terminal showing the server shutdown after pressing `Ctrl+C`.](https://learn.nextwork.org/projects/static/aws-security-secretsmanager/screenshot-42.png)


![Screenshot 2025-04-03 044545](https://github.com/user-attachments/assets/0da34418-2f73-405c-bb89-d7b9db81dc56)

Terminal showing the server shutdown after pressing `Ctrl+C`.

  

-   You should see the Uvicorn server shutdown in your terminal.
    
-   In your terminal, type `nano config.py` for MacOS/Linux or `notepad config.py` for Windows and press **Enter**.
    

![`config.py` file open in the `nano` text editor.](https://learn.nextwork.org/projects/static/aws-security-secretsmanager/screenshot-44.png)

`config.py` file open in the `nano` text editor.

  

-   In `config.py`, find the line starting with `AWS_ACCESS_KEY_ID`.
    
-   Delete the test access key ID.
    

![`AWS_ACCESS_KEY_ID` highlighted for deletion.](https://learn.nextwork.org/projects/static/aws-security-secretsmanager/screenshot-45.png)

`AWS_ACCESS_KEY_ID` highlighted for deletion.

  

-   Head back to the IAM console in your browser.
    
-   Click the **Copy** icon next to the **Access key ID**
    
-   You should see **Access key Copied** confirmation messages after copying each key.
    

![](https://learn.nextwork.org/projects/static/aws-security-secretsmanager/screenshot-41.png)

  

-   Paste the **Access key ID** you just copied from the IAM console in between the quotes:
    

![`config.py` in `nano` with the updated `AWS_ACCESS_KEY_ID`.](https://learn.nextwork.org/projects/static/aws-security-secretsmanager/screenshot-46.png)

`config.py` in `nano` with the updated `AWS_ACCESS_KEY_ID`.

  

-   Let's do the same for the secret access key. Jump your cursor to the line below, which should start with `AWS_SECRET_ACCESS_KEY`
    
-   Delete the test secret access key and paste the Secret access key from the IAM console.
    

> 💡**What is the secret access key?**  
> The secret access key is like the password that pairs with your access key (your username). Your web app needs both to 'log in' to AWS and access your S3 buckets.
> 
> **Secret** is a key word here - anyone who has it can access your AWS account, so make sure to keep this away from anyone else!

![`config.py` in `nano` with the updated `AWS_SECRET_ACCESS_KEY`.](https://learn.nextwork.org/projects/static/aws-security-secretsmanager/screenshot-48.png)

`config.py` in `nano` with the updated `AWS_SECRET_ACCESS_KEY`.

  

> ⚠️ **Important Security Note!** ⚠️  
> **Treat your Secret Access Key like a password!** Do not share it with anyone, do not store it in public places, and definitely **do not commit it to your code repository!**
> 
> For this project, we are temporarily storing it in a local configuration file (`config.py`) for demonstration purposes. In the next steps, we will learn how to store it more securely using **AWS Secrets Manager**.

-   Before we go - in `config.py`, find the line `AWS_REGION = "YOUR_AWS_REGION"`.
    
-   Make sure it's set to the AWS Region you are using.
    
-   You can double-check your region code in the AWS console, next to your account name in the top right corner.
    

![Region selector in AWS console showing the region code for Ohio.](https://learn.nextwork.org/projects/static/aws-security-secretsmanager/screenshot-50.png)

Region selector in AWS console showing the region code for Ohio.

  

-   Make sure the `AWS_REGION` in your `config.py` matches the AWS region where you have your S3 buckets.
    

![`config.py` in `nano` showing the updated `AWS_REGION` code.](https://learn.nextwork.org/projects/static/aws-security-secretsmanager/screenshot-51.png)

`config.py` in `nano` showing the updated `AWS_REGION` code.

  

Once you've updated the credentials and region, save the changes. The instructions for this depend on your operating system:  
  

MacOS/LinuxWindows

-   In nano:
    
    -   Press `Ctrl + X`
        
    -   Press `Y` to confirm.
        
    -   Press **Enter** to save.
        

-   You should be returned to your terminal command prompt after saving and exiting `nano`.
    

![Terminal showing the command prompt after exiting `nano`.](https://learn.nextwork.org/projects/static/aws-security-secretsmanager/screenshot-53.png)

Terminal showing the command prompt after exiting `nano`.

  

**Awesome!** You've updated your `config.py` file with your AWS credentials. Now, let's see if our web app can finally list your S3 buckets!



  
  

**Run the Web App Again** Let's run the web app again with your AWS credentials now in `config.py`. Type `python3 app.py` and press Enter.

-   In your terminal, run `python3 app.py` and press **Enter**.
    

![Terminal showing the command `python3 app.py` being executed again.](https://learn.nextwork.org/projects/static/aws-security-secretsmanager/screenshot-54.png)

Terminal showing the command `python3 app.py` being executed again.

  

-   You should see the Uvicorn server start up again, similar to before.
    

![Terminal showing the application running again after updating `config.py`.](https://learn.nextwork.org/projects/static/aws-security-secretsmanager/screenshot-55.png)

Terminal showing the application running again after updating `config.py`.

  

-   Go back to your browser tab where you saw the `InvalidAccessKeyId` error.
    

![Browser tab showing the ](https://learn.nextwork.org/projects/static/aws-security-secretsmanager/screenshot-56.png)

Browser tab showing the

  

-   Refresh the page.
    

![Browser tab showing the ](https://learn.nextwork.org/projects/static/aws-security-secretsmanager/screenshot-57.png)

Browser tab showing the

![Screenshot 2025-04-03 043026](https://github.com/user-attachments/assets/33ccf3ac-8b0e-4be1-9b23-cb821b9a3af3)  

If everything is set up correctly, you should now see a JSON response in your browser listing the names of your S3 buckets! 🎉

![Browser showing a JSON response with a list of S3 bucket names.](https://learn.nextwork.org/projects/static/aws-security-secretsmanager/screenshot-58.png)

Browser showing a JSON response with a list of S3 bucket names.

![Screenshot 2025-04-03 045058](https://github.com/user-attachments/assets/86b5694f-edfa-487b-88f1-98d11e11aed5)  

I can see bucket names!I don't see any bucket names

Awesome! You've successfully updated the `config.py` file with your AWS credentials, and now the application is able to list your S3 buckets.

  
  

**Try "Pretty print" (Optional)** Some browsers offer a "Pretty print" option to format JSON responses in a more readable format.

-   If you see a checkbox labeled **Pretty print** in your browser's JSON viewer, try checking it.
    

![Browser showing the ](https://learn.nextwork.org/projects/static/aws-security-secretsmanager/screenshot-60.png)

Browser showing the

![Screenshot 2025-04-03 045237](https://github.com/user-attachments/assets/29949b63-6f0b-4d44-83ca-e1c2652a2516)

  

-   This will show you the JSON output to be more human-readable, with indentation and line breaks.
    

![Browser showing the pretty-printed JSON output, making it easier to read.](https://learn.nextwork.org/projects/static/aws-security-secretsmanager/screenshot-61.png)

Browser showing the pretty-printed JSON output, making it easier to read.

  

Nice work! Your app is working as expected, but it also highlights the security risk of storing credentials directly in the code.

Let's move on to seeing what happens when you try to make your code public on GitHub!

------

  

## 🍴 Step #2

### 

Make Your Code Public

Sweet! Our web app now has AWS credentials to get it to work. But here's where things get interesting - we've got our credentials sitting right there in our code, and that's... well, not great 😅

Let's see what happens when we try to share this code on GitHub. To do this, we'll **fork** the original repository (i.e. make our own copy that's stored in GitHub), and then push our hardcoded credentials to the forked repository.

  
  

**In this step, you're going to:**

-   Get set up with a GitHub account (if you don't have one yet)
    
-   Make your own copy (fork) of the original repository
    
-   Try pushing your code with credentials to GitHub
    
-   See GitHub's secret scanning in action (spoiler: it's pretty smart!)
    



![](https://learn.nextwork.org/projects/static/aws-security-secretsmanager/architecture-2.0.png)

  

-   Do you have a GitHub account?  
      
    

I already have a GitHub accountI don't have a GitHub account

-   If you haven't already, [sign in to GitHub](https://github.com/login?return_to=https%3A%2F%2Fgithub.com%2Fsignup).
    

**Fork the Web App Code on GitHub**

> 💡 **What is a fork (in GitHub)?**  
> **Forking** a repository on GitHub is like making a personal copy of an existing repository, and editing and storing that copy in your own GitHub account. This is a great option if you'd like to showcase your version of the web app, and make changes to it without affecting the original repository.
> 
>   
> 💡 **What is forking vs. cloning?**  
> When you **fork** a repository, you're making a copy of it in **your GitHub account** online. If your forked repository is public, anyone can see it!
> 
> -   When you **clone** a repository, you're creating a local, **offline** copy. By default, no one can see it since it's **stored on your computer.**
>     

-   Go back to the original GitHub repository page in your browser: [https://github.com/dineshrajdhanapathyDD/nextwork-security-secretsmanager.git](https://github.com/dineshrajdhanapathyDD/nextwork-security-secretsmanager.git).
    
-   In the top right corner of the repository page, click the **Fork** button.
    

![GitHub repository page with the ](https://learn.nextwork.org/projects/static/aws-security-secretsmanager/screenshot-62.png)

GitHub repository page with the

  

-   In the **Fork this repository** page, you can add a description to your fork like `A fork of an insecure web app that contains AWS credentials`.
    

![](https://learn.nextwork.org/projects/static/aws-security-secretsmanager/screenshot-63.png)

  

-   Click the **Create fork** button to start the forking process.
    

![](https://learn.nextwork.org/projects/static/aws-security-secretsmanager/screenshot-64.png)

  

-   GitHub will create your fork. This might take a few seconds.
    

![](https://learn.nextwork.org/projects/static/aws-security-secretsmanager/screenshot-65.png)

  

-   After forking is complete, you should be redirected to your forked repository.
    
-   The URL in your browser should now start with `github.com/YOUR_GITHUB_USERNAME/nextwork-security-secretsmanager`, where `YOUR_GITHUB_USERNAME` is your GitHub username. This is now your own copy of the web app code!
    

![Your forked repository page, showing your username and the forked repository name.](https://learn.nextwork.org/projects/static/aws-security-secretsmanager/screenshot-66.png)

Your forked repository page, showing your username and the forked repository name.

  



**Push Your Code to GitHub**

Nice! With your forked repository available, you can now push your version of the web app. Let's make it public! We'll get back to our terminal to connect our edited code to the forked repository.

-   Head back to your terminal where the web app is running.
    
-   Press `Ctrl+C` to stop the local web app.
    

![Terminal showing the server shutdown after pressing `Ctrl+C`.](https://learn.nextwork.org/projects/static/aws-security-secretsmanager/screenshot-67.png)

![Screenshot 2025-04-03 050824](https://github.com/user-attachments/assets/375e499c-2666-4ede-84a4-fb4d23f20381)


Terminal showing the server shutdown after pressing `Ctrl+C`.

  

In case you haven't already, let's initialize Git in your local project directory. This tells Git to track the changes you make to the code.

-   In your terminal, type `git init` and press **Enter**. This command might show **Reinitialized existing Git repository** if it's already initialized, which is fine.
    

![Terminal showing the command `git init`.](https://learn.nextwork.org/projects/static/aws-security-secretsmanager/screenshot-68.png)

Terminal showing the command `git init`.

  

Now, we need to connect your local repository to your forked repository on GitHub. To do this, we'll add a remote named "origin" that points to your fork's URL.

> 💡 **What is a remote in Git?**  
> A **remote** is like an address book entry in Git that tells it where your code lives online. Instead of remembering the full URL every time, Git lets you give each location a nickname. In our case, we'll call it "origin" and it points to the URL of our forked repository on GitHub.
> 
> P.S. You can actually name your saved URL anything, but "origin" is the standard that most developers use.

-   Run the following command in your terminal to connect your local repository to your forked repository on GitHub:
    

bash

Copy code

```bash
git remote add origin <your-forked-repo-url>

```

-   Oops! We need to the placeholder `<your-forked-repo-url>` with the HTTPS URL of your forked repository.
    

![Terminal showing the command `git add remote origin`.](https://learn.nextwork.org/projects/static/aws-security-secretsmanager/screenshot-72.png)

Terminal showing the command `git add remote origin`.

  

-   Go to your forked repository on GitHub in your browser.
    
-   Click the green **Code** button.
    
-   Make sure **HTTPS** is selected.
    
-   Copy the **URL**.
    

![GitHub repository page showing the HTTPS URL to copy.](https://learn.nextwork.org/projects/static/aws-security-secretsmanager/screenshot-73.png)

GitHub repository page showing the HTTPS URL to copy.

  

-   Paste this URL into the `git remote add origin` command in your terminal, replacing `<your-forked-repo-url>`.
    

![Terminal showing the forked repository URL pasted.](https://learn.nextwork.org/projects/static/aws-security-secretsmanager/screenshot-74.png)

Terminal showing the forked repository URL pasted.

  

-   Hmmm you might run into another error! Looks like you're trying to push to a repository that already has a remote named "origin". When we cloned the original repository, Git automatically set up a remote named "origin" for us that points to the original repository.
    

![](https://learn.nextwork.org/projects/static/aws-security-secretsmanager/new-21.png)

  

-   Let's change the existing remote "origin" to the new forked repository.
    
-   In your terminal, run the following command:
    



```bash
git remote set-url origin <your-forked-repo-url>

```

![](https://learn.nextwork.org/projects/static/aws-security-secretsmanager/new-21.png)

  

-   Let's verify that the remote "origin" was set up correctly.
    
-   In your terminal, type `git remote -v` and press **Enter**. This command will show the remote repositories configured for your local repository.
    

![Terminal showing the command `git remote -v`.](https://learn.nextwork.org/projects/static/aws-security-secretsmanager/screenshot-78.png)

Terminal showing the command `git remote -v`.

  

-   You should see output like this, showing that `origin` is set to your forked repository URL for both `fetch` (fetching code from GitHub) and `push` (pushing code to GitHub).
    

![Terminal showing the output of `git remote -v`, displaying the ](https://learn.nextwork.org/projects/static/aws-security-secretsmanager/screenshot-79.png)

Terminal showing the output of `git remote -v`, displaying the

  

-   Stage all the changes you've made in your local project by using `git add .`. This adds all files to the staging area, meaning Git is now tracking these files for changes.
    

![Terminal showing the command `git add .`.](https://learn.nextwork.org/projects/static/aws-security-secretsmanager/screenshot-69.png)

Terminal showing the command `git add .`.

  

-   In your terminal, type `git commit -m "Updated config.py with hardcoded credentials"` and press **Enter**. Committing in Git saves a snapshot of your changes.
    

![Terminal showing the command `git commit -m ](https://learn.nextwork.org/projects/static/aws-security-secretsmanager/screenshot-70.png)

Terminal showing the command `git commit -m

  

-   Nice! You should see output showing you that files have been changed and committed.
    

![Terminal showing the output of the `git commit` command, showing you files changed.](https://learn.nextwork.org/projects/static/aws-security-secretsmanager/screenshot-71.png)

Terminal showing the output of the `git commit` command, showing you files changed.

  

-   Finally, let's push your local commits to your forked repository on GitHub. This command uploads your code to the "main" branch of your "origin" remote.
    
-   In your terminal, type `git push -u origin main` and press **Enter**.
    

> 💡 **What does this command do?**  
> This command uploads your local commits to your "origin" remote. The `-u` and `main` tells Git to set make the main branch the upstream branch, so next time you push code, you can just run `git push` and it'll automatically know where to push the code.

![Terminal showing the command `git push -u origin main`.](https://learn.nextwork.org/projects/static/aws-security-secretsmanager/screenshot-80.png)

Terminal showing the command `git push -u origin main`.

  ![Screenshot 2025-04-03 052041](https://github.com/user-attachments/assets/b7e52716-8219-4097-9775-772b8f0009c3)



**GitHub's Secret Scanning Block**

-   Scroll up in the terminal output to see the detailed error message.
    

  
  

Uh oh! You probably see an error message saying **"Push cannot contain secrets"**. GitHub automatically scans your code for secrets and blocks the push if it detects any, like AWS credentials.

![Screenshot 2025-04-03 052052](https://github.com/user-attachments/assets/134badfb-0f92-4949-b4f6-5d4c517f5dcb)

In our case, GitHub detected that you are trying to push code that contains an **AWS Access Key ID** and **Secret Access Key** in `config.py`. This is a great security feature from GitHub to prevent accidental exposure of secrets!

Man, GitHub is so smart 😮‍💨

![Terminal showing the GitHub secret scanning block error, showing you ](https://learn.nextwork.org/projects/static/aws-security-secretsmanager/screenshot-81.png)

Terminal showing the GitHub secret scanning block error, showing you

  
![Screenshot 2025-04-03 052710](https://github.com/user-attachments/assets/5df3ee10-c3eb-4565-8238-99a81945bc9a)






Running into this error highlights the exact problem we're trying to solve in this project: **how do you manage secrets securely** so they're not exposed in your code? In this case, it's even stopped us from sharing our code publicly!

This is where **AWS Secrets Manager** comes in! In the next step, we'll learn how to use Secrets Manager to store your AWS credentials securely, plus retrieve them in your application without hardcoding them in `config.py`.

----

## 🔐 Step #3

### 

Create Secret in Secrets Manager

So, GitHub rightly blocked us from pushing our code with **hardcoded credentials.** This highlights exactly why we need a secure way to manage secrets! The solution is to use a dedicated service for managing secrets securely - **AWS Secrets Manager**.

In this step, we're going to use **AWS Secrets Manager** to store your AWS credentials securely. We'll create a new secret in Secrets Manager, and then update your `config.py` file to retrieve these credentials from Secrets Manager instead of hardcoding them.

  
  

**In this step, you're going to:**

-   Create a new secret in Secrets Manager to store your AWS credentials.


![](https://learn.nextwork.org/projects/static/aws-security-secretsmanager/architecture-3.0.png)

  

-   Head back to your [AWS Management Console](https://signin.aws.amazon.com/oauth?redirect_uri=https%3A%2F%2Fconsole.aws.amazon.com%2Fconsole%2Fhome%3Fstate%3DhashArgs%2523%26isauthcode%3Dtrue&client_id=arn%3Aaws%3Aiam%3A%3A015428540659%3Auser%2Fhomepage&response_type=code&iam_user=true&account=) in your browser.
    
-   This time, let's head to the `Secrets Manager` console.
    

> 💡 **What is AWS Secrets Manager?**  
> **AWS Secrets Manager** is a service that helps you securely store and manage secrets, such as database credentials, API keys, and other sensitive information. Think of it as a **digital vault** for your secrets.
> 
> Secrets Manager encrypts your secrets and allows you to retrieve them programmatically in your applications, without hardcoding them in your code. It also offers features like secret rotation and auditing to further enhance security.
> 
>   
> **Why use Secrets Manager?**
> 
> -   **Security:** Secrets are encrypted at rest and in transit, and access is controlled through IAM policies.
>     
> -   **Centralized management:** Manage all your secrets in one place.
>     
> -   **Rotation:** Automate secret rotation to improve security posture.
>     
> -   **Auditing:** Track access to secrets for compliance and security monitoring.
>     

Now let's create a new secret to securely store our AWS credentials.

-   In the left navigation pane of the Secrets Manager console, select **Secrets**.
    

![AWS Secrets Manager console with ](https://learn.nextwork.org/projects/static/aws-security-secretsmanager/screenshot-83.png)

AWS Secrets Manager console with

  

-   On the Secrets Manager dashboard, select **Store a new secret**.
    

![Secrets Manager console - ](https://learn.nextwork.org/projects/static/aws-security-secretsmanager/screenshot-85.png)

Secrets Manager console -

  

-   In the **Choose secret type** section, select the **Other type of secret** option.
    

> **What types of secrets can Secrets Manager store?**  
> Secrets Manager can store various types of secrets, including:
> 
> -   **Database credentials:** Usernames and passwords for databases like MySQL, PostgreSQL, SQL Server, etc.
>     
> -   **API keys:** API keys for accessing third-party services.
>     
> -   **OAuth tokens:** Tokens for authentication and authorization.
>     
> -   **Any other sensitive information:** You can store any text-based secret in Secrets Manager.
>     

![](https://learn.nextwork.org/projects/static/aws-security-secretsmanager/screenshot-86.png)

  
![Screenshot 2025-04-03 053627](https://github.com/user-attachments/assets/f5365402-12e3-4312-b801-2e562e9632d5)

-   In the **Key** field, enter `AWS_ACCESS_KEY_ID`.
    
-   Click **Add row**.
    
-   In the new **Key** field that appears, enter `AWS_SECRET_ACCESS_KEY`.
    

![](https://learn.nextwork.org/projects/static/aws-security-secretsmanager/screenshot-87.png)

  

-   Go back to your `config.py` file.
    
-   Copy the **Access key ID**.
    
-   Go back to the Secrets Manager console.
    
-   In the **Value** field next to `AWS_ACCESS_KEY_ID`, paste the **Access key ID** you just copied.
    

![](https://learn.nextwork.org/projects/static/aws-security-secretsmanager/screenshot-89.png)

  ![Screenshot 2025-04-03 054003](https://github.com/user-attachments/assets/1342f4d9-92cd-4091-a909-24e5274b1d0a)

-   Then for `AWS_SECRET_ACCESS_KEY`, copy the **Secret access key** from `config.py` and paste it into the **Value** field.
    

![](https://learn.nextwork.org/projects/static/aws-security-secretsmanager/screenshot-91.png)

  



-   Scroll down to the **Encryption key** section below **Key/value pairs**. Secrets Manager encrypts your secrets to keep them secure. By default, it uses an AWS managed key.
    

> 💡 **What is Encryption?**  
> **Encryption** is like scrambling a message so that only someone with the right key can unscramble it and read it. In our case, Secrets Manager uses an encryption key to encrypt your secrets, so the access key ID and secret access key are scrambled and cannot be read by anyone without the right access.
> 
>   
> **What is AWS KMS?**  
> **AWS Key Management Service (KMS)** is a service that helps you create and manage encryption keys securely in AWS. Secrets Manager uses KMS to encrypt your secrets, ensuring that they are protected at rest. You can also learn more in our project on [using AWS KMS!](https://link.nextwork.org/projects/aws-security-kms?utm_source=project-app)

-   Click the **Next** button at the bottom right of the page.
    

![](https://learn.nextwork.org/projects/static/aws-security-secretsmanager/screenshot-94.png)

 


Now, we need to give our secret a name and description.

-   In the **Configure secret name and description** page, under **Secret name**, enter `aws-access-key`.
    
-   Under **Description - optional**, you can enter a description like `Created to replace hard-coded access key credentials in config.py`.
    

![](https://learn.nextwork.org/projects/static/aws-security-secretsmanager/screenshot-95.png)

  

-   There are also optional settings sections that we'll skip for now: **Tags - optional**, **Resource permissions - optional**, and **Replicate secret - optional**.
    

> 💡 **What do these optional settings do?**
> 
> -   **Tags** can help you organize and manage your secrets.
>     
> -   **Resource permissions** allow you to control who and what can access this secret.
>     
> -   **Replicate secret** allows you to replicate this secret to other AWS regions for disaster recovery and lower latency access from those regions.
>     

![](https://learn.nextwork.org/projects/static/aws-security-secretsmanager/screenshot-96.png)

  

-   Click the **Next** button at the bottom right of the page again.
    

![Select ](https://learn.nextwork.org/projects/static/aws-security-secretsmanager/screenshot-97.png)

Select

  

-   We're now at the last setup page **Configure rotation - optional**.
    

![](https://learn.nextwork.org/projects/static/aws-security-secretsmanager/screenshot-98.png)

 ![Screenshot 2025-04-03 054857](https://github.com/user-attachments/assets/b6145d51-5544-4fcf-9b4b-76e303fe6ece)  

> 💡 **What is Secret Rotation?**  
> **Secret rotation** is the process of automatically changing your secrets on a regular schedule. This is a security best practice because it reduces the risk of compromised credentials. If a secret is compromised, it will only be valid for a limited time before it's automatically rotated.
> 
> Secrets Manager can automatically rotate secrets for databases and other services. You can also configure custom rotation for other types of secrets.
> 
>   
> 💡 **When should you use secret rotation?**  
> Secret rotation is best for high-risk credentials like database passwords, privileged API keys, and service account credentials. These types of secrets, if compromised, could give attackers access to sensitive data or critical systems, so regular rotation helps limit the damage window.
> 
> However, you might want to skip rotation for keys (where automated rotation isn't supported), hardware security tokens, or legacy systems that can't handle credential updates without downtime.



-   Select the **Next** button at the bottom right of the page.
    

![](https://learn.nextwork.org/projects/static/aws-security-secretsmanager/screenshot-99.png)

  

-   Let's review the secret we're about to create:
    

Secret type: **Other type of secret**

Encryption key: `aws/secretsmanager`

Secret name: `aws-access-key`

Description: `Created to replace hard-coded access key credentials in config.py`

Secret replication: `Disabled`

Automatic rotation: `Disabled`

-   Scroll down to the bottom of the **Review secret** page.
    
-   Notice the **Sample code** section. This sample code is very helpful! It shows you exactly how to retrieve your secret from Secrets Manager in your application code. We'll use this code to update our `config.py` file.
    

![](https://learn.nextwork.org/projects/static/aws-security-secretsmanager/screenshot-100.png)


![Screenshot 2025-04-03 055048](https://github.com/user-attachments/assets/def73641-2cc5-4cc0-85f1-e4aa3dd969bd)
  





-   Click the **Store** button at the bottom right of the page.
    

![](https://learn.nextwork.org/projects/static/aws-security-secretsmanager/screenshot-101.png)

  ------

## 📝 Step #4

### 

Update config.py

Alright, now comes the fun part! 🎨 We're going to transform our `config.py` file from a security risk into a secure, professional piece of code. Instead of having our AWS credentials just sitting there in plain text (yikes!), we'll use the sample code from Secrets Manager to fetch them securely.

  
  

**In this step, you're going to:**

-   Grab the sample code AWS generated for us.
    
-   Update our `config.py` file to use Secrets Manager instead of hardcoded credentials.
    



![](https://learn.nextwork.org/projects/static/aws-security-secretsmanager/architecture-4.0.png)

  

**Grab the Sample Code**

-   You should see a green banner at the top of the page saying **Successfully stored the secret**.
    
-   Select **See sample code** - let's use the sample code from Secrets Manager to update our `config.py` file.
    

![](https://learn.nextwork.org/projects/static/aws-security-secretsmanager/screenshot-102.png)

  

-   We need the Python code snippet to integrate with our application. Let's copy the relevant portions.
    
-   In the **Sample code** section that appears, click on the **Python 3** tab.
    

![](https://learn.nextwork.org/projects/static/aws-security-secretsmanager/screenshot-103.png)

  

-   Select and copy the code starting from **line 6** (the first `import` statement) down to the end of the code block.
    

![](https://learn.nextwork.org/projects/static/aws-security-secretsmanager/screenshot-104.png)

  

Now we'll update our `config.py` file to use Secrets Manager instead of hardcoded credentials.

It's easier to do this in a code editor (like VS Code) instead of directly in the terminal, but the choice is yours!  
  

Using a code editorUsing the terminal

-   Open VS Code or your preferred code editor.
    
-   Navigate to your project directory and open `config.py`
    

![`config.py` file open in VS Code.](https://learn.nextwork.org/projects/static/aws-security-secretsmanager/screenshot-105.png)

`config.py` file open in VS Code.

  

-   In `config.py`, replace the entire file with the code snippet you copied from Secrets Manager.
    
-   Delete the comment line `# Your code goes here` from the `get_secret()` function.
    

![The comment line in `config.py`.](https://learn.nextwork.org/projects/static/aws-security-secretsmanager/new-24.png)

The comment line in `config.py`.

  

Let's get to know our brand new `config.py` file 🕵

The first two lines are import statements:

![`config.py` in code editor, highlighting the import statements.](https://learn.nextwork.org/projects/static/aws-security-secretsmanager/screenshot-109.png)

`config.py` in code editor, highlighting the import statements.

  ![Screenshot 2025-04-03 060136](https://github.com/user-attachments/assets/a9c0ac4f-98a1-4211-9f03-f7cfc4edea01)

-   `import boto3`: This line imports the `boto3` library, which is the AWS SDK for Python. We need `boto3` to interact with AWS Secrets Manager.
    
-   `from botocore.exceptions import ClientError`: This line imports the `ClientError` exception class from `botocore`, which is a dependency of `boto3`. We'll use this to handle potential errors when retrieving the secret.
    

  
  

The core of the pasted code is the `get_secret()` function:

![`config.py` in code editor, highlighting the `get_secret()` function definition.](https://learn.nextwork.org/projects/static/aws-security-secretsmanager/screenshot-111.png)

`config.py` in code editor, highlighting the `get_secret()` function definition.

![Screenshot 2025-04-03 060230](https://github.com/user-attachments/assets/8b9d77d6-5191-4c91-8add-500e55752989)


  

-   This function is responsible for retrieving the secret from Secrets Manager. Let's break down what it does:
    
    -   `secret_name = "aws-access-key"`: This line defines the name of the secret to retrieve, which is `"aws-access-key"`, the name we gave to our secret in Secrets Manager.
        
    -   `region_name = "us-east-2"`: This line defines the AWS region where Secrets Manager is located. **Make sure this matches the AWS region you are using (e.g., `"us-east-2"` for Ohio).**
        
    -   `session = boto3.session.Session()` and `client = session.client(...)`: These lines create a `boto3` client for interacting with the Secrets Manager service.
        
    -   The `try...except` block handles potential errors when retrieving the secret.
        

![`config.py` in code editor, highlighting the `try-except` block within the `get_secret()` function.](https://learn.nextwork.org/projects/static/aws-security-secretsmanager/screenshot-112.png)

`config.py` in code editor, highlighting the `try-except` block within the `get_secret()` function.

  ![Screenshot 2025-04-03 060242](https://github.com/user-attachments/assets/826400bc-0a9f-4ce4-bf90-0df80353ca7f)




-   Next, we also need to add new lines to the code to retrieve the credentials from `get_secret()`!
    

> 💡 **Why are we adding new lines to the code?**  
> Great question! So far, the code provided by Secrets Manager only help us with creating a function for retrieving the secret. But, config.py still has one more job left to do - it needs to retrieve the secret from the function we create (get_secret()) and assign the values to the `AWS_ACCESS_KEY_ID`, `AWS_SECRET_ACCESS_KEY`, and `AWS_REGION` variables that our `app.py` code expects.

Now let's add the code to use the `get_secret()` function and retrieve our credentials:

Using a code editorUsing the terminal

-   Open `config.py` in your code editor if it's not already open.
    
-   Add the following code block at the end of the file:
    



```python
    return json.loads(secret)

# Retrieve credentials from Secrets Manager
credentials = get_secret()

# Extract the values; if AWS_REGION isn't in the secret, use the region from the session
AWS_ACCESS_KEY_ID = credentials.get("AWS_ACCESS_KEY_ID")
AWS_SECRET_ACCESS_KEY = credentials.get("AWS_SECRET_ACCESS_KEY")
AWS_REGION = credentials.get("AWS_REGION", boto3.session.Session().region_name or "us-east-2")

```

![`config.py` in code editor, showing the updated code with comments explaining each section.](https://learn.nextwork.org/projects/static/aws-security-secretsmanager/screenshot-118.png)

`config.py` in code editor, showing the updated code with comments explaining each section.

  

-   Since this piece of code uses a new package `json`, we need to import it at the top of the file. Add the following line to your import statements:
    


```python
import json

```

![](https://learn.nextwork.org/projects/static/aws-security-secretsmanager/new-25.png)

  

-   Save the updated `config.py` file in your code editor.
    

> 💡 **What are these lines doing?**  
> These lines are responsible for actually retrieving the credentials from the secret and assigning them to the `AWS_ACCESS_KEY_ID`, `AWS_SECRET_ACCESS_KEY`, and `AWS_REGION` variables that our `app.py` code expects:
> 
> -   `secret_json = json.loads(get_secret())`: This line calls the `get_secret()` function to retrieve the secret from Secrets Manager. The secret is returned as a JSON string, so we use `json.loads()` to parse it into a Python dictionary.
>     
> -   `AWS_ACCESS_KEY_ID = secret_json['AWS_ACCESS_KEY_ID']`: This line extracts the value of the `AWS_ACCESS_KEY_ID` key from the `secret_json` dictionary and assigns it to the `AWS_ACCESS_KEY_ID` variable.
>     
> -   `AWS_SECRET_ACCESS_KEY = secret_json['AWS_SECRET_ACCESS_KEY']`: This line does the same for the `AWS_SECRET_ACCESS_KEY`.
>     
> -   `AWS_REGION = region_name`: This line sets the `AWS_REGION` variable to the `region_name` we defined earlier.
>     


Woohoooo! Now our application is configured to retrieve AWS credentials securely from Secrets Manager - instead of hardcoding them 😮‍💨

----

## ⬆️ Step #5

### 

Push Changes

Let's share our newly secured code with the world! 🌎 Remember how GitHub's secret scanning blocked our push earlier? Now that we've properly moved our credentials to AWS Secrets Manager, we can confidently push our code without exposing any sensitive information.

  
  

**In this step, you're going to:**

-   Stage your updated files for commit.
    
-   Commit your changes with a descriptive message.
    
-   Push your secure code to GitHub.
    
-   See GitHub's secret scanning approve your code (no more blocked pushes!)
    

![](https://learn.nextwork.org/projects/static/aws-security-secretsmanager/architecture-5.0.png)

  

  
  

**Save, Commit and Push Your Changes**

-   Make sure you're back in your terminal.
    
-   Stage all the changes you've made in your local project by using `git add .`. As a recap, this adds all files to the staging area, meaning Git is now tracking these files for changes.
    
-   In your terminal, type `git commit -m "Updated config.py with Secrets Manager credentials"` and press **Enter**. Committing in Git saves a snapshot of your changes.
    
-   Nice! You should see output showing you that files have been changed and committed.
    
-   Finally, let's push your local commits to your forked repository on GitHub. Run `git push -u origin main`
    
-   Oh no! We still get the same error in the terminal!
    

![Terminal showing the command `git push -u origin main`.](https://learn.nextwork.org/projects/static/aws-security-secretsmanager/screenshot-80.png)

Terminal showing the command `git push -u origin main`.

![Screenshot 2025-04-03 061747](https://github.com/user-attachments/assets/30935ccb-7ba6-45de-a3f1-ff5c8eda5657)

  

> 💡 **Why is this happening?**  
> Turns out, simply editing the `config.py` file is **not enough** for security best practices 🤦‍♀️
> 
> The hardcoded credentials still live in your **commit history,** which is a record of all commits you've made to your repository. In our case, because we made an older commit in 🍴 Step #2 with the hardcoded credentials, someone else could still go through the commit history to find the credentials.
> 
> To solve this, we need to rewrite the history to completely remove the commit that had the hardcoded credentials.

**Remove Hardcoded Credentials from Commit History**

We'll have to use use a special command called `git rebase` to 🪄 rewrite history 🪄 and remove the commit that introduced the credentials.

-   To rewrite our commit history, we first need to identify the ID that we want to write out of history.
    
-   Let's identify the ID of the commit where you added the AWS credentials - scroll up your terminal's history, and find the commit associated with exposed credentials.
    

![Identifying the commit with exposed credentials.](https://learn.nextwork.org/projects/static/aws-security-secretsmanager/step5-screenshot-69.png)

Identifying the commit with exposed credentials.

  

-   Once you've found the commit ID, take note of the **first 7 digits** of that ID. This is going to be important! You can even copy it and paste it in a random text file to reference it later.
    
-   Once you've identified the commit, run the following command in your terminal:
    



```bash
git rebase -i --root

```

> 💡 **What is `git rebase -i --root`?**  
> `git rebase` starts a rebase session, which means rewriting your commit history.
> 
> The `-i` flag makes it interactive, meaning you can edit the list of commits to be rebased.
> 
> `--root` tells us that the rebase should start from the very first commit in your repository's history. This is useful when you need to modify commits from the beginning of your project.

![](https://learn.nextwork.org/projects/static/aws-security-secretsmanager/new-10.png)

  

-   This will open an interactive rebase editor in your default text editor. You'll see a list of your commits.
    
-   Use your the up and down arrows on your keyboard to get to the commit that contains the AWS credentials. This is the 7-digit ID you identified earlier!
    



-   Then, where it says `pick` at the beginning of that line, replace it with `d`. This tells Git to **drop** i.e. remove this commit in the rebase.
    

> 💡 **What is `drop`?**  
> Drop in the interactive rebase editor tells Git to completely remove a commit from your history as if it never existed.
> 
> This is how we'll erase the commit containing the sensitive credentials. **Use this command with caution**, as rewriting history can cause issues like merge conflicts (which you'll see in just a second).

![Removing the commit with 'd'.](https://learn.nextwork.org/projects/static/aws-security-secretsmanager/step5-screenshot-72.png)

Removing the commit with 'd'.

  



-   **Save** the file and **close** the editor. You can do this by typing `:wq` and pressing **Enter** on your keyboard.
    

![Using `:wq` to save and quit.](https://learn.nextwork.org/projects/static/aws-security-secretsmanager/step5-screenshot-75.png)

Using `:wq` to save and quit.

  ![Screenshot 2025-04-03 063356](https://github.com/user-attachments/assets/70746464-d161-429b-b6e2-22791ac0f332)


-   Git will now start the rebase, so it will remove the commit you marked for deletion from your commit history.
    
-   Oh wait! Looks like the rebase is not so simple after all. You might run into **merge conflicts** during the rebase.
    

> 💡 **What are merge conflicts?**  
> Merge conflicts happen when you've changed the same lines of code in different commits.
> 
> In our case, the commit we removed had the hardcoded credentials, and the next commit we did _also_ changed the `config.py` file. So Git's now confused - if you remove the commit with the hardcoded credentials, should it delete the change you made after that commit too?
> 
> If a merge conflict occurs, Git will pause the rebase and ask you to resolve the conflicts first. To resolve these conflicts, we'll need to manually edit the conflicting files to resolve these conflicts.

![Merge conflict message.](https://learn.nextwork.org/projects/static/aws-security-secretsmanager/new-14.png)

Merge conflict message.

![Screenshot 2025-04-03 064102](https://github.com/user-attachments/assets/0bee3b3b-6f4b-4c19-b718-35e867da8099)

  

**Resolve Merge Conflicts**

-   Open the `config.py` file in your code editor or terminal.  
      
    

Using a code editorUsing the terminal

-   You'll see conflict markers that look like this:
    



```python
<<<<<<< HEAD
# Old version with hardcoded credentials
AWS_ACCESS_KEY_ID = "AKIAXXXXXXXXXXXXXXXX"
AWS_SECRET_ACCESS_KEY   = "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX"
AWS_REGION = "us-east-2"
=======
# New version with Secrets Manager
import boto3
from botocore.exceptions import ClientError
import json

def get_secret():
    # ... rest of the Secrets Manager code ...
>>>>>>> ea89d6b (Updated config.py with Secrets Manager credentials)

```

> 💡 **What are these merge conflict markers?**  
> When Git detects conflicting changes in a file, it adds special markers to show you exactly where the conflicts are:
> 
> -   `<<<<<<< HEAD` marks the beginning of your current version
>     
> -   `=======` separates the two conflicting versions
>     
> -   `>>>>>>> feature-branch` marks the end of the incoming changes
>     
> 
> To resolve the conflict, you need to choose which version to keep and remove all these markers.

-   Delete the entire section between `<<<<<<< HEAD` and `=======` (this removes the hardcoded credentials)
    
-   Then, at the end of the file, delete the `=======` line
    
-   Delete the `>>>>>>> feature-branch` line
    
-   Save the file
    

-   After resolving the conflicts, **save** and exit the file.
    

![Returning to the terminal.](https://learn.nextwork.org/projects/static/aws-security-secretsmanager/step5-screenshot-79.png)

Returning to the terminal.

  



-   In your terminal, run the following commands to stage and commit our changes:
    



```bash
git add config.py
git commit -m "Resolved merge conflicts"

```

![Committing the resolved merge conflicts.](https://learn.nextwork.org/projects/static/aws-security-secretsmanager/new-6.png)

Committing the resolved merge conflicts.

  

-   Then, let's tell Git that you're ready to continue the rebase:
    



```bash
git rebase --continue

```

![Successful rebase message.](https://learn.nextwork.org/projects/static/aws-security-secretsmanager/new-13.png)

Successful rebase message.

  

-   Last, we'll push our changes to GitHub:
    

`bash git push`

![Pushing the changes to GitHub.](https://learn.nextwork.org/projects/static/aws-security-secretsmanager/new-5.png)

Pushing the changes to GitHub.

  ![Screenshot 2025-04-03 065345](https://github.com/user-attachments/assets/9ed155f8-a952-4b81-8c97-26384ff54418)


**Great job!** You've just removed some prettyyyy sensitive data from your Git commit history. Now, let's verify that the AWS credentials can't be found in your public repository!

  
  

**Verify Your GitHub Repository**

Let's check on GitHub that the commit history is updated and your `config.py` file doesn't contain any sensitive information.

-   Head to your forked repository on GitHub.
    

![Your forked repository on GitHub.](https://learn.nextwork.org/projects/static/aws-security-secretsmanager/step5-screenshot-89.png)

Your forked repository on GitHub.

  

-   Check the code in `config.py`.
    

![Verifying the `config.py` file on GitHub.](https://learn.nextwork.org/projects/static/aws-security-secretsmanager/step5-screenshot-90.png)

Verifying the `config.py` file on GitHub.

  

-   You should see that config.py is **clean** of any hardcoded credentials. It only has the code to retrieve credentials from AWS Secrets Manager!
    

![](https://learn.nextwork.org/projects/static/aws-security-secretsmanager/new-4.png)

  
![Screenshot 2025-04-03 065702](https://github.com/user-attachments/assets/adc23015-415f-4433-a109-ee3169a75c36)






-   Then, let's check the commit history of `config.py`.
    

![](https://learn.nextwork.org/projects/static/aws-security-secretsmanager/new-3.png)

  

-   You should see that there is no commit that has your hardcoded credentials!
    

![](https://learn.nextwork.org/projects/static/aws-security-secretsmanager/new-2.png)

  

> 🙋‍♀️ **What are the other commits in the history?**  
> The other commits are the ones made by the NextWork team when preparing this `config.py` for the project!
> 
> Notice how easy it is to click through the commit history and see old versions of the file. As a little Easter egg, you might even notice that one of the older versions contains AWS credentials - these are not your credentials, but credentials that were entered in the original repository before you forked it!
> 
> ![](https://learn.nextwork.org/projects/static/aws-security-secretsmanager/new-1.png)
> 
>   
> 
> This just goes to show how important it is to remove sensitive data from your commit history - even if it's no longer the current version of the file, someone could still get to your secrets over the history of the repository!

Nice work! By taking the time to secure your code and remove sensitive data from your commit history, you've made development easier, not harder. Instead of worrying about accidentally exposing credentials, you can freely share and collaborate on your code!

  -------
## 🗑️ Before you go



#### Delete your resources

Now that we've secured and deployed our web app, it's important to clean up the resources we created to avoid incurring unnecessary costs.


  

**Resources to delete:**

Delete the **Forked GitHub repository**.

Delete the **Secrets Manager secret**.

Delete the **local repository**.

Delete the resources from the **Secret Mission** (optional 💎).

  
  

GitHub ForkSecrets Manager SecretLocal Repository

-   Go back to your terminal.
    
-   Delete the local repository by going back to your parent folder, and then removing the repository folder from there:  
      
    

macOS/LinuxWindowsSecret Mission (Optional)



```bash
cd ../
rm -rf nextwork-security-secretsmanager

```

-----
## 🎉 Mission Accomplished



That's a wrap!

Nice work! 🔐 You've successfully learned how to protect sensitive credentials using AWS Secrets Manager!

![](https://learn.nextwork.org/projects/static/aws-security-secretsmanager/architecture-complete.png)

  

**You've learned how to:**

-   💡 Apply security best practices for managing credentials in code
    
-   🔍 Understand GitHub's secret scanning feature and why it's important
    
-   🔑 Use AWS Secrets Manager to securely store and manage AWS credentials
    
-   🔄 Retrieve secrets programmatically in Python using the AWS SDK (boto3)
    
-   🧹 Clean up sensitive data from Git commit history by rebasing and handling merge conflicts

--------
content owner : **Nextwork**
------


