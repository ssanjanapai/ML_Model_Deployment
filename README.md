# ML_Model_Deployment
Deploying Iris Model on AWS by containerizing flask app using docker

Here, the 'hello world' program of Machine Learning ie; Iris built in dataset is processed using Logistic Regression Model.
This model gave the accuracy of 93%.

Later ,this model was fed into Flask. Docker was used to containerize this Flask app.

Docker daemon used to build the docker image:

```
FROM python:3.6-slim
COPY ./app.py /deploy/
COPY ./requirements.txt /deploy/
COPY ./iris_trained_model.pkl /deploy/
WORKDIR /deploy/
RUN pip install -r requirements.txt
EXPOSE 80
ENTRYPOINT ["python", "app.py"]
```

Build and run this image by commands:

```
docker build -t app-iris .
docker run -p 80:80 app-iris .
```

After the image is run, an AWS EC2 instance is created under security group that has HTTP port allowed. And a curl request is sent using public DNS name created.

The output will be similar to the output obtained in local machine.

```
curl -X POST \
public-dns-name:80/predict \
-H 'Content-Type: application/json' \
-d '[5.9,3.0,5.1,1.8]' 
```
Output:
There is more probability of species of flower belonging to the first category(ie; Setosa)
