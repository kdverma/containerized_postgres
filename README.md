
# create service nodeport
kubectl expose deployment postgres --type=NodePort --port=5432 --name=postgres-service --dry-run=client -o yaml > postgres-service.yaml

# get secret 
kubectl get secret postgres-secret-config -o jsonpath="{.data.password}" | base64 --decode

# run a client to test postgres connection
kubectl run postgres-client --rm --tty -i --restart='Never' --image postgres:11 --env="PGPASSWORD=$POSTGRES_PASSWORD" --command -- psql -h postgres-service -U postgres

You will get a db prompt to postgres db as postgres user.