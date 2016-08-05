# Local testing of Apache Mesos

This repository contains Vagrant configuration for provisioning Apache Mesos and running it locally on a single node.


## Provisioning

Run the following to provision the VM, download and install Apache Mesos v1.0.0:

```sh
  vagrant up --provider libvirt
```

Apache Mesos should be up on http://mesos-test:5050

