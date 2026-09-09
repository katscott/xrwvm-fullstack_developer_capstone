# fullstack_developer_capstone

Fullstack developer capstop project

## Setup Notes

Mongo DB
```
# new terminal
cd /home/project/xrwvm-fullstack_developer_capstone/server/database

docker build . -t nodeapp
docker-compose up
```
start port 3030 for backend url (config in server/djangoapp/.env)

Django
```
# new terminal
cd /home/project/xrwvm-fullstack_developer_capstone/server

pip install virtualenv
virtualenv djangoenv
source djangoenv/bin/activate

python3 -m pip install -U -r requirements.txt

python3 manage.py makemigrations
python3 manage.py migrate
python3 manage.py runserver
```

Sentiment Engine
```
# code engine CLI
cd xrwvm-fullstack_developer_capstone/server/djangoapp/microservices

docker build . -t us.icr.io/${SN_ICR_NAMESPACE}/senti_analyzer
docker push us.icr.io/${SN_ICR_NAMESPACE}/senti_analyzer

ibmcloud ce application create --name sentianalyzer --image us.icr.io/${SN_ICR_NAMESPACE}/senti_analyzer --registry-secret icr-secret --port 5000
```
set sentiment_analyzer_url to deployed url (config in server/djangoapp/.env)
```
ibmcloud ce application get --name sentianalyzer --output url > /home/project
```

Frontend
```
# new terminal
cd xrwvm-fullstack_developer_capstone/server/frontend

npm install
npm run build

cd xrwvm-fullstack_developer_capstone/server

MY_NAMESPACE=$(ibmcloud cr namespaces | grep sn-labs-)
echo $MY_NAMESPACE

docker build -t us.icr.io/$MY_NAMESPACE/dealership .
docker push us.icr.io/$MY_NAMESPACE/dealership

kubectl apply -f deployment.yaml
kubectl port-forward deployment.apps/dealership 8000:8000
```

Frontend (local)
```
python3 manage.py runserver
```
