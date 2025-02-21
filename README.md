# Advance-Real-Estate-Validation-using-ensemble-methods

This Machine Learning project series walks through step by step process of how to build a real estate price prediction website. We will first build a model using sklearn and linear regression using banglore home prices dataset from kaggle.com. Second step would be to write a python flask server that uses the saved model to serve http requests. Third component is the website built in html, css and javascript that allows user to enter home square ft area, bedrooms etc and it will call python flask server to retrieve the predicted price. During model building we will cover almost all data science concepts such as data load and cleaning, outlier detection and removal, feature engineering, dimensionality reduction, gridsearch_cv for hyperparameter tunning, k fold cross validation etc. Technology and tools wise this project covers,

1.Python

2.Numpy and Pandas for Data Cleaning

3.Matplotlib for Data Visualization

4.Sklearn for Model Building

5.Jupyter notebook, Visual studio code and pycharm as IDE

6.Python Flask for http server

7.HTML/CSS/Javascript for UI

## Dataset
The project utilizes real estate datasets from various sources:
- Kaggle real estate datasets
- Public government property databases

## Model Evaluation Metrics
- Mean Absolute Error (MAE)
- Root Mean Squared Error (RMSE)
- R-squared (R²)

## Deploy this app to cloud (AWS EC2)

1. Create EC2 instance using amazon console, also in security group add a rule to allow HTTP incoming traffic
2. Now connect to your instance using a command like this,
```
ssh -i "C:\Users\Viral\.ssh\Banglore.pem" ubuntu@ec2-3-133-88-210.us-east-2.compute.amazonaws.com
```
3. nginx setup
   1. Install nginx on EC2 instance using these commands,
   ```
   sudo apt-get update
   sudo apt-get install nginx
   ```
   2. Above will install nginx as well as run it. Check status of nginx using
   ```
   sudo service nginx status
   ```
   3. Here are the commands to start/stop/restart nginx
   ```
   sudo service nginx start
   sudo service nginx stop
   sudo service nginx restart
   ```
   4. Now when you load cloud url in browser you will see a message saying "welcome to nginx" This means your nginx is setup and running.
4. Now you need to copy all your code to EC2 instance. You can do this either using git or copy files using winscp. We will use winscp. You can download winscp from here: https://winscp.net/eng/download.php
5. Once you connect to EC2 instance from winscp (instruction in a youtube video), you can now copy all code files into /home/ubuntu/ folder. The full path of your root folder is now: **/home/ubuntu/BangloreHomePrices**
6.  After copying code on EC2 server now we can point nginx to load our property website by default. For below steps,
    1. Create this file /etc/nginx/sites-available/bhp.conf. The file content looks like this,
    ```
    server {
	    listen 80;
            server_name bhp;
            root /home/ubuntu/BangloreHomePrices/client;
            index app.html;
            location /api/ {
                 rewrite ^/api(.*) $1 break;
                 proxy_pass http://127.0.0.1:5000;
            }
    }
    ```
    2. Create symlink for this file in /etc/nginx/sites-enabled by running this command,
    ```
    sudo ln -v -s /etc/nginx/sites-available/bhp.conf
    ```
    3. Remove symlink for default file in /etc/nginx/sites-enabled directory,
    ```
    sudo unlink default
    ```
    4. Restart nginx,
    ```
    sudo service nginx restart
    ```
7. Now install python packages and start flask server
```
sudo apt-get install python3-pip
sudo pip3 install -r /home/ubuntu/BangloreHomePrices/server/requirements.txt
python3 /home/ubuntu/BangloreHomePrices/client/server.py
```
Running last command above will prompt that server is running on port 5000.
8. Now just load your cloud url in browser (for me it was http://ec2-3-133-88-210.us-east-2.compute.amazonaws.com/) and this will be fully functional website running in production cloud environmen
## Contributing
Contributions are welcome! Feel free to submit issues and pull requests.

## License
This project is licensed under the MIT License.

## Contact
For inquiries and collaboration:
- **Email:**  vruthvikyadav1348@gmail.com
- **GitHub:** https://github.com/Vruthvik48

**Project Requirements Document**

## **1. Required Dependencies**
To ensure smooth execution, the following Python libraries and tools must be installed:

### **Python Libraries**
| Library | Version | Purpose |
|---------|---------|---------|
| Flask | 3.0.0 | To create the API server for model deployment |
| numpy | 1.26.2 | For numerical computations |
| pandas | 2.1.3 | For data manipulation and analysis |
| scikit-learn | 1.5.1 | For machine learning model training and evaluation |
| joblib | 1.3.2 | For saving and loading the trained model efficiently |
| matplotlib | 3.8.2 | For data visualization |
| seaborn | 0.13.0 | For statistical data visualization |
| requests | 2.31.0 | To send HTTP requests for API interaction |
| gunicorn | 21.2.0 | For running the Flask server in production |

### **Additional Tools Used**
- **Postman** → For testing API endpoints (GET & POST requests).
- **PyCharm** → Used as the Integrated Development Environment (IDE) for project development.

---

## **2. Installation Guide**
### **3.1. Install Python**
Ensure that **Python 3.10+** is installed on your system. You can download it from the [official Python website](https://www.python.org/downloads/).

### **2.2. Install Dependencies**
After cloning the project repository, navigate to the project folder and run:
```bash
pip install -r requirements.txt
```
This will install all the required libraries mentioned above.

### **2.3. Verify Installation**
To check if all dependencies are installed correctly, run:
```bash
python -c "import flask, numpy, pandas, sklearn, joblib, matplotlib, seaborn, requests; print('All dependencies installed successfully!')"
```
If no errors appear, the setup is successful.

---

## **3. Project Setup & Execution**
### **3.1. Running the Flask Server**
After installing the dependencies, start the Flask server using:
```bash
python app.py
```
This will launch the API, which can be tested using **Postman**.

### **3.2. Testing API with Postman**
- Open Postman and make **GET** and **POST** requests to verify the API endpoints.
- Example **GET request**: `http://127.0.0.1:5000/get_location_names`
- Example **POST request** for price prediction:
```json
{
  "total_sqft": 1200,
  "location": "Indira Nagar",
  "bhk": 3,
  "bath": 2
}
```

---

