`kubectl create ns monitoring`

installing prometheus

`helm repo add prometheus-community https://prometheus-community.github.io/helm-charts`

`helm repo update`

`helm install prometheus prometheus-community/prometheus -n monitoring`

Expose the prometheus service
This is required to access prometheus-server using your browser.

`kubectl edit svc prometheus-server -n monitoring` ---> edit the type from CLusterIP to NodePort and save it

or

`kubectl expose service prometheus-server -n monitoring --type=NodePort --target-port=9090 --name=prometheus-server-ext`

`sudo -E kubectl port-forward -n monitoring svc/prometheus-server 80:80`

Installing Grafana

`helm repo add grafana https://grafana.github.io/helm-charts`

`helm repo update`

`helm install grafana grafana/grafana`

Expose Grafana service

`kubectl edit svc grafana -n monitoring` ---> edit the type from CLusterIP to NodePort and save it

or

`kubectl expose service grafana — type=NodePort — target-port=3000 — name=grafana-ext`

`sudo -E kubectl port-forward -n monitoring svc/grafana 80:80`

Get your 'admin' user password of GRAFANA by running:

`kubectl get secret --namespace monitoring grafana -o jsonpath="{.data.admin-password}" | base64 --decode ; echo`

Login --> Click on data source --> Add http://prometheus-server.monitoring.svc.cluster.local:8080 --> click save&test

Click on dashboard --> add dashboard --> give 3662 and click on go
