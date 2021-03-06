# Kubernetes Deployment Example

This example runs CloverDX Server with PostreSQL database as a deployment in Kubernetes.
It contains a sample echo data service, which simply prints the string passed as a path parameter.

### Requirements
* bash + envsubst
* Java
* Docker + docker CLI
* Kubernetes + kubectl
* Docker image registry accessible from Kubernetes
* ``clover.war``
* ``license.dat``

---

### Running the example

* Put ``clover.war`` into the project root directory.
* Switch to ``examples/kubernetes`` directory and put your ``license.dat`` file there.
* Execute `run.sh` and pass the hostname and port of your Docker registry as a parameter. The script will build and deploy the example and start port forwarding to localhost:8090.

    ```
    ./run.sh my-docker-registry:5000
    ```

Thanks to port forwarding, you can now access the application at the following URLs:

* <http://localhost:8090/data-service/echo/Hello+World!> - sample Data Service
* <http://localhost:8090/clover> - CloverDX Server Console (clover/clover)
* <http://localhost:8090/monitoring> - Grafana monitoring dashboard
* <http://localhost:8090/gravitee/> - Gravitee UI (admin/admin)

---

### Architecture

At the core of the example, there is a CloverDX Server container together with a PostgreSQL container, running in the same pod.

Then there is a Grafana pod providing monitoring dashboard. Grafana uses Prometheus and cAdvisor to collect the monitoring data.

Finally, all the services are proxied via Gravitee.io Gateway. The gateway is split into a number of microservices - the proxy itself, its Management UI that uses Management API, and MongoDB and Elasticsearch used as a storage. In addition, there are Gravitee Access Management API and UI pods, but we have left them out of the schema for the sake of simplicity.


![Architecture of the Example](architecture.png)
