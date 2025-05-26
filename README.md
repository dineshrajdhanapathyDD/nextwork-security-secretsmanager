<img src="https://cdn.prod.website-files.com/677c400686e724409a5a7409/6790ad949cf622dc8dcd9fe4_nextwork-logo-leather.svg" alt="NextWork" width="300" />

# Secure Secrets with Secrets Manager

**Project Link:** [View Project](http://learn.nextwork.org/projects/aws-security-secretsmanager)

**Author:** Dineshraj Dhanapathy  
**Email:** dineshrajdhanapathy@gmail.com

---

![](https://learn.nextwork.org/projects/static/aws-security-secretsmanager/architecture-complete.png)

---

## Introducing Today's Project!

In this project, I will demonstrate how to securely manage AWS credentials in a web application. I'm doing this project to learn about the risks of hardcoded credentials, how to use AWS Secrets Manager, and how to ensure sensitive data is not exposed in Git repositories.

First, I’ll clone a simple web app that lists S3 buckets and hardcode AWS credentials into it. This will highlight a common security mistake. Next, I’ll push the vulnerable code to a public GitHub repository to see the risks of exposed secrets.

To fix this, I’ll integrate AWS Secrets Manager to securely store credentials. I’ll modify the app to fetch them dynamically instead of hardcoding them. Then, I’ll push the updated, secured code to GitHub and ensure secrets are properly managed.

Finally, I’ll clean the repository history to remove any previously exposed credentials. This hands-on approach will help me understand secure AWS credential management and best practices for cloud security.

----


![Image](http://learn.nextwork.org/positive_purple_innocent_lemon/uploads/aws-security-secretsmanager_r7s8t9u0)


### Tools and concepts

Services I used were AWS Secrets Manager, S3, and GitHub. Key concepts I learned include secure credential management using Secrets Manager, automating secret retrieval in config.py, and Git best practices, such as interactive rebasing to remove sensitive data. I also explored GitHub's secret scanning, handling merge conflicts, and improving security by eliminating hardcoded credentials while keeping the repository clean.

### Project reflection

This project took me approximately 3 hours to complete. The most challenging part was resolving the Git rebase conflicts and ensuring that my hardcoded credentials were fully removed from the repository history. It was most rewarding to successfully integrate AWS Secrets Manager, making my application more secure by dynamically retrieving credentials instead of storing them in plain text. This experience improved my security awareness and Git skills. 

I did this project today to strengthen my AWS security skills and practice secure credential management using Secrets Manager. My goal was to eliminate hardcoded credentials, improve my Git workflow, and understand GitHub’s secret scanning. This project met my goals by enhancing my security awareness, reinforcing best practices, and giving me hands-on experience with AWS security features.

---

## Hardcoding credentials

In this project, a sample web app is exposing AWS credentials in config.py, making them visible to anyone with access to the code. It is unsafe to hardcode credentials because attackers can misuse them to access AWS resources, delete data, or incur high costs. Publicly shared credentials can be scraped by bots in seconds, leading to security breaches. Instead, use environment variables, AWS Secrets Manager, or IAM roles to manage secrets securely and protect sensitive information.

I've set up the initial configuration with example AWS access keys in config.py. These credentials are just examples because using real credentials in code is a security risk. If they were real, anyone with access could use my AWS account, potentially leading to unauthorized access or high costs.

![Image](http://learn.nextwork.org/positive_purple_innocent_lemon/uploads/aws-security-secretsmanager_j2k3l4m5)

---

## Using my own AWS credentials

As an extension for this project, I also decided to ensure a clean environment by using a virtual environment. To set up my virtual environment, I installed boto3 for AWS service interactions, fastapi for building the web API, and uvicorn as the ASGI server. These packages are essential for handling API requests, managing AWS resources, and running the application efficiently.

When I first ran the app, I ran into an error because the AWS Access Key ID provided was not valid. This happened because the app was using placeholder credentials in config.py, which are not real AWS credentials. As a result, the app couldn't authenticate with AWS, leading to the InvalidAccessKeyId error when calling the ListBuckets operation. Updating config.py with valid AWS credentials will fix this.

To resolve the 'InvalidAccessKeyId' error, I updated the config.py file with my actual AWS credentials. It now contains my AWS_ACCESS_KEY_ID and AWS_SECRET_ACCESS_KEY, which allow the app to authenticate with AWS.

![Image](http://learn.nextwork.org/positive_purple_innocent_lemon/uploads/aws-security-secretsmanager_wghjteykut)

---

## Pushing Insecure Code to GitHub

Once I updated the web app code with credentials, I forked the repository because I wanted my own online copy in my GitHub account. A fork is different from a clone because a fork allows me to make changes without affecting the original repository, while a clone only creates a local copy on my computer. Forking is useful for contributing to open-source projects, experimenting with code, or showcasing my own version of the project. It also enables me to push changes online, collaborate, and open pull requests to suggest improvements to the original project.

To connect my local repository to the forked repository, I first initialized Git with git init. Then, I used git remote set-url origin <your-forked-repo-url> to update the remote repository link to my forked version. I verified this with git remote -v.

Then, I used git add . to stage all modified files and git commit -m "Updated config.py with hardcoded credentials" to create a snapshot of my changes.

Finally, git push -u origin main attempted to upload my local commits to GitHub, but GitHub’s secret scanning blocked the push due to detected AWS credentials. This security feature prevents accidental exposure of sensitive data, ensuring that API keys and credentials are not publicly accessible. To proceed, I need to remove the credentials from config.py, use environment variables or AWS Secrets Manager, and then push my code safely.

GitHub blocked my push because it detected hardcoded AWS credentials in config.py. This is a good security feature because it prevents accidental exposure of sensitive information, such as API keys, which could be exploited by malicious users. Even though I intended to share my code publicly, GitHub ensures that secrets are not unintentionally leaked, protecting my AWS account from unauthorized access and potential security risks.

![Image](http://learn.nextwork.org/positive_purple_innocent_lemon/uploads/aws-security-secretsmanager_o2p3q4r5)

---

## Secrets Manager

Secrets Manager is an AWS service that securely stores and manages sensitive information like API keys, database credentials, and authentication tokens. It encrypts secrets, controls access through IAM policies, and supports automatic rotation. I'm using it to store my AWS credentials securely and retrieve them dynamically in my application. Other common use cases include managing database passwords, third-party API keys, and encryption keys to enhance security and compliance. 

Another feature in Secrets Manager is secret rotation, which means automatically updating and replacing credentials on a set schedule. This reduces the risk of compromised credentials by ensuring they remain valid only for a limited time. It's useful in situations where high-risk credentials, like database passwords or privileged API keys, need frequent updates to prevent unauthorized access. However, it may not be suitable for legacy systems that can't handle frequent changes. 

Secrets Manager provides sample code in various languages, like Python, Java, and Node.js, to help developers easily retrieve secrets in their applications. This is helpful because it eliminates the need to hardcode credentials, reducing security risks. By using the provided code snippets, developers can securely access secrets programmatically, ensuring seamless integration while following best security practices for managing sensitive information.

![Image](http://learn.nextwork.org/positive_purple_innocent_lemon/uploads/aws-security-secretsmanager_h2i3j4k5)

---

## Updating the web app code

I updated the config.py file to retrieve AWS credentials securely from Secrets Manager instead of hardcoding them. The get_secret() function will connect to AWS Secrets Manager using boto3, fetch the stored secret, and return the credentials dynamically. This ensures that sensitive data is not exposed in the code. The function also includes a try...except block to handle potential errors, making the application more secure and resilient.

I also added code to config.py to extract the AWS credentials from the retrieved secret and assign them to the required variables. The get_secret() function fetches the secret from Secrets Manager, and json.loads(get_secret()) converts it into a dictionary. Then, AWS_ACCESS_KEY_ID and AWS_SECRET_ACCESS_KEY are extracted from this dictionary, while AWS_REGION is set to the predefined region. This is important because it ensures that credentials are securely managed and dynamically retrieved, preventing accidental exposure and enhancing security best practices. 

![Image](http://learn.nextwork.org/positive_purple_innocent_lemon/uploads/aws-security-secretsmanager_v0w1x2y3)

---

## Rebasing the repository

Git rebasing is a process of rewriting commit history by moving or modifying commits. I used it to clean up my repository and remove the commit containing sensitive AWS credentials. This was necessary because pushing secrets to GitHub poses a security risk, and simply deleting them in a later commit wouldn’t remove them from the history. By using git rebase -i --root, I was able to drop the commit entirely, ensuring the credentials were never part of the repository’s history.

A merge conflict occurred during rebasing because Git detected conflicting changes between my branch and the rewritten commit history. I resolved the merge conflict by manually editing the affected file, choosing the correct version to keep, and removing the conflict markers (<<<<<<<, =======, >>>>>>>). Then, I staged the resolved file with git add . and continued the rebase using git rebase --continue. This ensured a clean commit history.

Once the merge conflict was resolved, I verified that my hardcoded credentials were removed by checking the Git history using git log to ensure the sensitive commit was gone. I also used git grep 'AWS_ACCESS_KEY_ID' to confirm no credentials remained in the code. Finally, I pushed the cleaned branch to GitHub and reviewed the repository to ensure Secrets Manager was used instead. 

![Image](http://learn.nextwork.org/positive_purple_innocent_lemon/uploads/aws-security-secretsmanager_t5u6v7w8)

---

---
