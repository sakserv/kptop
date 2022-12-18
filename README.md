# Kube-Prometheus-Top [ kptop ]

A Python tool that provides Monitoring for Kubernetes Nodes/Pods/Containers resources on the terminal through Prometheus metircs

<br>

## Motivation

The resources metrics provided by the K8s APIs are very limited compared what what's scraped by Prometheus
This tool is using Prometheus as a data source for metrics to display all the needed informations right on the terminal.

<br>

## Project Status
<br>

>  [[ In the Development Phase ]]
- [ ] [Top Nodes](#top_nodes)
- [x] [Live monitoring for Nodes](#monitor_node) `#done#`
- [x] [Top Pods](#top_pods) `#done#`
- [x] [Live monitoring for Pods/Containers](#monitor_pod) `#done#`
- [x] [Top PVCs](#top_pvcs)


<br>

---

<br>

## Installation
<a id=installation></a>

> Compatible with Python 3.6+

[on PyPi](https://pypi.org/project/kptop/0.0.0/#description)

```bash
pip3 install kptop --upgrade
```

<br>

---

## Environment Variables
<a id=env></a>

| ENV                            | Description                                                  | Default | Required |
| ------------------------------ | ------------------------------------------------------------ | ------- | -------- |
| `KUBE_PTOP_PROMETHEUS_SERVER`    | Prometheus server URL                                        |         | Yes      |
| `KPTOP_BASIC_AUTH_ENABLED`       | Whether basic authentication is needed to connect to Prometheus | False   | No       |
| `KPTOP_PROMETHEUS_USERNAME`      | Prometheus username                                          |         | No       |
| `KPTOP_PROMETHEUS_PASSWORD`      | Prometheus password                                          |         | No       |
| `KPTOP_INSECURE`                 | Verify SSL certificate                                       | False   | No       |
| `KPTOP_NODE_EXPORTER_NODE_LABEL` | node exporter "node label"                                   | "node"  | NO       |
| `KPTOP_START_GRAPHS_WITH_ZERO`   | By default graphs begin with '0'  to let the graph take its full hight | True    | NO       |



<br>

## CLI Arguments
<a id=cli></a>


| ENV                      | Description                                                  | Default                                                      |
| ------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| `--namespace`,  `-n`           | Specify a Kubernetes Namespace                               | default                                                      |
| `--all-namespaces`,  -A      |                                                              |                                                              |
| `--container`,  `-c`           | Specify a container                                          |                                                              |
| `--interval`,  `-i`            | Live monitoring update interval                              | 8  <br> <sub>[NOTE: the actuall update depends on the Prometheus scaping interval (15s by default)]</sub> |
| `--debug`,  `-d`               | Enable debugging logging mode                                | False                                                        |
| `--verify-prometheus`,  `-V`   | Verify connectivity to Prometheus server & check the existence of the needed exporters |                                                              |
| `--sort-by-mem-usage`,  `-s` | Sort top result by memory usage                              | False                                                        |


<br>

---

<br>

## Usage with Examples

<br>

Different ways to connect to Prometheus server:
1. You have direct access to it (Like in dev environments)
2. Prometheus is exposed publically/over-vpn (mostly with an Ingress)
3. You can use kubectl port-forward command
4. You also can run it as a Kubernetes pod



<br>

```bash
export KPTOP_PROMETHEUS_SERVER="http://prometheus.home-lab.com"
```

<br>

### Top nodes
<a id=top_nodes></a>

```bash
kptop nodes
```

<br>


### Live monitoring for Nodes
<a id=monitor_node></a>

```bash
kptop node <NODE>
```


![image](https://user-images.githubusercontent.com/33789516/208190533-cb719cef-e76c-4c6d-8686-2f32edae15c6.png)



<br>

### Top pods
<a id=top_pods></a>

```bash
kptop pods -n <NAMESPACE>
```

```
kptop pods -n elk-stack

NAMESPACE    POD                                      MEM LIMIT    MEM USAGE    MEM USAGE %    MEM USAGE MAX    MEM FREE    CPU LIMIT    CPU USAGE
elk-stack    elasticsearch-master-0                   2.0 gb       1.38 gb      68%            2.0 gb           635.25 mb   1000m        0.04m
elk-stack    elasticsearch-master-1                   2.0 gb       1.49 gb      74%            2.0 gb           522.05 mb   1000m        0.03m
elk-stack    strimzi-filebeat-filebeat-f8ms7          200.0 mb     85.73 mb     42%            174.16 mb        114.27 mb   1000m        0.02m
elk-stack    haproxy-ingress-filebeat-filebeat-pq2wf  200.0 mb     87.73 mb     43%            171.31 mb        112.27 mb   1000m        0.03m
elk-stack    strimzi-filebeat-filebeat-r7dht          200.0 mb     119.12 mb    59%            199.52 mb        80.88 mb    1000m        0.02m
elk-stack    haproxy-ingress-filebeat-filebeat-lzqdt  200.0 mb     98.66 mb     49%            199.57 mb        101.34 mb   1000m        0.02m
elk-stack    my-kibana-kibana-79448f7fb7-wf4t6        2.0 gb       342.87 mb    16%            618.07 mb        1.67 gb     1000m        0.02m
elk-stack    my-logstash-logstash-0                   1.5 gb       1008.22 mb   65%            1.21 gb          527.78 mb   1000m        0.02m
```

```
kptop pod -n kube-system

NAMESPACE    POD                                              MEM LIMIT    MEM USAGE    MEM USAGE %    MEM USAGE MAX    MEM FREE    CPU LIMIT    CPU USAGE
kube-system  coredns-558bd4d5db-nfcjq                         170.0 mb     26.65 mb     15%            42.77 mb         143.35 mb   ---          0.0m
kube-system  coredns-558bd4d5db-vcstr                         170.0 mb     17.41 mb     10%            23.22 mb         152.59 mb   ---          0.0m
kube-system  etcd-master                                      ---          71.4 mb                     390.23 mb                    ---          0.02m
kube-system  kube-apiserver-master                            ---          639.01 mb                   731.13 mb                    ---          0.09m
kube-system  kube-controller-manager-master                   ---          100.29 mb                   145.41 mb                    ---          0.02m
kube-system  kube-proxy-q6nr7                                 ---          30.32 mb                    58.91 mb                     ---          0.0m
kube-system  kube-proxy-q489q                                 ---          22.43 mb                    63.0 mb                      ---          0.0m
kube-system  kube-proxy-bghp6                                 ---          22.1 mb                     64.1 mb                      ---          0.0m
kube-system  kube-scheduler-master                            ---          38.75 mb                    61.68 mb                     ---          0.0m
kube-system  nfs-subdir-external-provisioner-b97f4d9f5-bjp2h  ---          9.37 mb                     37.38 mb                     ---          0.0m
```


<br>

### Live monitoring for Pods
<a id=monitor_pod></a>

```bash
kptop pod <POD> -n <NAMESPACE>
```

![image](https://user-images.githubusercontent.com/33789516/208233405-f07bab04-896e-4d80-bed5-f92deae91e19.png)  

![image](https://user-images.githubusercontent.com/33789516/208233544-e51e1ec2-a4c9-4615-9f9f-32156937a18c.png)

![image](https://user-images.githubusercontent.com/33789516/208233652-ed26534f-f5f1-486f-a3df-87800ebc8b73.png)

![image](https://user-images.githubusercontent.com/33789516/208233865-9026acfa-ad64-475a-8337-8e8edab1fcb9.png)



<br>

### Live monitoring for Containers
<a id=monitor_container></a>

```bash
kptop pod <POD> -n <NAMESPACE> -c <CONTAINER>
```

![image](https://user-images.githubusercontent.com/33789516/208234009-e3656b8b-6fdb-4f72-bb87-15332c67e3d7.png)



<br>

### Top PVCs
<a id=top_pvcs></a>


🚩 Currently `kptop pvc` can take time depending on the latency between you and the Prometheus server (many requests have to be made to get the PVCs data).
 Will work on improving the speed in the future releases.


```bash
kptop pvcs <NAMESPACE>
```
 
> **NOTE:** in this example, all VPCs have the same capacity because this is a testing environment (using [nfs-provisioner](https://github.com/kubernetes-sigs/nfs-subdir-external-provisioner))


```bash
kptop pvcs --all-namespaces
```

```
NAMESPACE    PVC                                          VOLUME                CAPACITY    USED      USED %    FREE      FREE %
elk-stack    elasticsearch-master-elasticsearch-master-0  elasticsearch-master  123.14 gb   21.42 gb  17%       95.43 gb  77%
elk-stack    elasticsearch-master-elasticsearch-master-1  elasticsearch-master  123.14 gb   21.42 gb  17%       95.43 gb  77%
elk-stack    elasticsearch-master-elasticsearch-master-2  elasticsearch-master  ?           ?         ?         ?         ?
kafka        data-0-kafka-cluster-region1-kafka-0         data-0                123.14 gb   21.42 gb  17%       95.43 gb  77%
kafka        data-0-kafka-cluster-region1-kafka-1         data-0                123.14 gb   21.42 gb  17%       95.43 gb  77%
kafka        data-0-kafka-cluster-region1-kafka-2         data-0                123.14 gb   21.42 gb  17%       95.43 gb  77%
kafka        data-0-kafka-cluster-region2-kafka-0         data-0                123.14 gb   21.42 gb  17%       95.43 gb  77%
kafka        data-0-kafka-cluster-region2-kafka-1         data-0                123.14 gb   21.42 gb  17%       95.43 gb  77%
kafka        data-0-kafka-cluster-region2-kafka-2         data-0                123.14 gb   21.42 gb  17%       95.43 gb  77%
kafka        data-1-kafka-cluster-region1-kafka-0         data-1                123.14 gb   21.42 gb  17%       95.43 gb  77%
kafka        data-1-kafka-cluster-region1-kafka-1         data-1                123.14 gb   21.42 gb  17%       95.43 gb  77%
kafka        data-1-kafka-cluster-region1-kafka-2         data-1                123.14 gb   21.42 gb  17%       95.43 gb  77%
kafka        data-2-kafka-cluster-region1-kafka-0         data-2                123.14 gb   21.42 gb  17%       95.43 gb  77%
kafka        data-2-kafka-cluster-region1-kafka-1         data-2                123.14 gb   21.42 gb  17%       95.43 gb  77%
kafka        data-2-kafka-cluster-region1-kafka-2         data-2                123.14 gb   21.42 gb  17%       95.43 gb  77%
kafka        data-3-kafka-cluster-region1-kafka-0         data-3                123.14 gb   21.42 gb  17%       95.43 gb  77%
kafka        data-3-kafka-cluster-region1-kafka-1         data-3                123.14 gb   21.42 gb  17%       95.43 gb  77%
kafka        data-3-kafka-cluster-region1-kafka-2         data-3                123.14 gb   21.42 gb  17%       95.43 gb  77%
kafka        data-kafka-cluster-region1-zookeeper-0       data                  123.14 gb   21.42 gb  17%       95.43 gb  77%
kafka        data-kafka-cluster-region2-zookeeper-0       data                  123.14 gb   21.42 gb  17%       95.43 gb  77%
prometheus   my-prometheus-alertmanager                   storage-volume        123.14 gb   21.42 gb  17%       95.43 gb  77%
prometheus   my-prometheus-server                         storage-volume        123.14 gb   21.42 gb  17%       95.43 gb  77%
```


<br>


### Verify Prometheus connectivity
<a id=verify_prometheus></a>

```bash
kptop --verify-prometheus
```

<details>
    <summary>
        <b style="font-size:14px" >Sample output</b>
    </summary>
    <br>

```bash 
Verifying Prometheus connection: Connected                     
{
  "connected": true,
  "status_code": 200,
  "reason": "",
  "fail_reason": ""
}

Verifying Prometheus Exporters:

* Node Exporter:  Found             
{
  "success": true,
  "fail_reason": "",
  "result": {
    "found_versions": {
      "1.3.1": "2"
    }
  }
}

* Kubernetes Exporter:  Found           
{
  "success": true,
  "fail_reason": "",
  "result": {
    "found_git_versions": {
      "v1.21.0": "3",
      "v1.21.14": "1"
    }
  }
}
```
    
</details>




<br>


---

<br>

## Logging

Default log file location is "`/tmp/kptop.log`"


<br>

---

<br>


## Known Issues


### 

<details>
    <summary>
        <b style="font-size:22px" > [1] Node Exporter metrics don't return data</b>
    </summary>
    <br>
    
![image](https://user-images.githubusercontent.com/33789516/208234711-bf46a36b-db60-4fba-943c-792b68f721e6.png)
    
</details>




- This is NOT an issue, the node exporter NODE label change from version to another, currently we encountered only "kubernetes_node" or "node"
- "node" is the default, to fix the it you can change it with the "[KPTOP_NODE_EXPORTER_NODE_LABEL](#env)" Environment variables

```bash
export KPTOP_NODE_EXPORTER_NODE_LABEL="node" # default
export KPTOP_NODE_EXPORTER_NODE_LABEL="kubernetes_node"
```

> auto detection of exporters verstions can be implemented later (if needed).

<br>

---



<br>

Reach me anytime on [Linkedin](https://www.linkedin.com/in/eslam-gomaa/)

