## <b>Production</b> <!-- {docsify-ignore} -->

### On Premise
#### Prerequisites
1. **Kubernetes**: A Kubernetes cluster is required to run NTCore in production. Kubernetes is an open source container orchestration engine for automating deployment, scaling, and management of containerized applications. You can setup a Kubernetes cluster manually following this [tutorial](https://kubernetes.io/docs/setup/production-environment/).

2. **Postgres**: NTCore uses Postgres SQL to store data in production. Download and install Postgres SQL database following this [guidance](https://www.postgresql.org/download/).

#### Installation
1. Install kubectl following this [guidance](https://kubernetes.io/docs/tasks/tools/). kubectl is the Kubenetes command line tool that allows you to run commands against Kubernetes clusters. Run the below command to confirm if kubectl is installed successfully.
```
kubectl version
```

2. Download NTCore repository from github.
```
git clone https://github.com/nantutech/ntcore.git
```
3. Provide the Postgres database config in <ins>kube/ntcore.yml</ins> as below.
```
database:
    provider: 
    type: postgres
    config: 
        host: <postgres_host>
        port: <postgres_port>
        user: <postgres_user>
        database: <postgres_database>
        password: <postgres_password>
```

4. <ins>Inside</ins> the NTCore repository, run the below command.
```
kubectl apply -f kube/
```

5. Launch NTCore from your browser at the below url.
>http://{hostname}:8000/dsp/console/workspaces