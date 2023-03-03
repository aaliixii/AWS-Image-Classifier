# Image Classifier on Cloud

For our Cloud Computing project we deployed an Image Classifier on AWS's EC2 instance.

### **Image Classifier**:
The Image Classifier was trained on a self sourced, web scraped dataset for facial recognition of F1 drivers. We trained SVM, RandomForestClassifier, LogisticRegression on the dataset. After Hyperparameter tuning using GridSearchCV, we found SVM to be the most proficient with an accuracy of 83%.

We then created a .pkl file to deploy this model. The steps followed for deployment are as follows:

<br/>

### **Step 1:** **Flask Deployment**

<br/>

**Step 1.1**

Deployed the model on port 5000 of the local machine using Flask

<br/>

**Step 1.2**

Designed a UI using base HTML & CSS

<br/>

**Step 1.3**

Now we make Flask API calls in JavaScript to call the Python functions.


>Now we have set up a local Flask server which is ready for Cloud Deployment


<br/>

### **Step 2:** **Cloud Deployment**

<br/>

For this project we are using AWS's free EC2 instance and the steps followed for deployment

<br/>

**Step 2.1**

Create an EC2 instance using the AWS console, also in the security group add a rule to allow HTTP incoming traffic

<br/>

**Step 2.2**

Open gitbash or powershell on Windows and connect to your EC2 instance using your ssh client key

```console
ssh -i "YOUR/CLIENT/KEY/FILE/PATH" ubuntu@ec2-54-89-133-0.compute-1.amazonaws.com
```

*The following steps must be carried out on the **EC2 instance CLI***

<br/>

**Step 2.3**

Download and install nginx on the EC2 instance with:

```console
sudo apt update
sudo apt install nginx

```

To check if nginx was correctly installed
Run:

```console
sudo service nginx status
```
<br/>

**Step 2.4**

Using WinSCP, we transfer files from our local machine to the EC2 instance

<br/>

**Step 2.5**

Change your pwd to that of nginx:
```console
    cd etc/nginx
```

Unlink the default configuration file in 'sites-enabled'
```console
cd sites-enabled
sudo unlink deafult
```

Create a new configuration file in 'sites-available'
```console
sudo vim cc.conf
```

Add the following to the cc.conf:
```json
server {
    listen 80;
        server_name cc;
        root "/home/ubuntu/ImageClassifier/UI";
        index index.html index.htm;
        location /api/ {
             rewrite ^/api(.*) $1 break;
             proxy_pass http://127.0.0.1:5000;
        }
}
```
<br/>

**Step 2.6**

Add a soft link for 'cc.conf' to 'sites-enabled'

```console
sudo ln -v -s /etc/nginx/sites-available/cc.conf /etc/nginx/sites-enabled
```

Now, restart the nginx server
```console
sudo service nginx restart
```

<br/>

**Step 2.7**

Install pip and requirements.txt
```console
sudo apt install pip

pip install -r /path/to/requirements.txt
```

Run server.py to start the Flask server
```console
python3 /path/to/server.py
```