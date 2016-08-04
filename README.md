# Local testing of Apache Mesos

This repository contains Vagrant configuration for provisioning Apache Mesos and running it locally on a single node.

## Provisioning

Run the following to provision the VM, download, build and install Apache Mesos v1.0.0:

```sh
  vagrant up --provider libvirt
```

## Testing

**Step 1.** SSH into the machine:

```sh
  vagrant ssh
```

**Step 2.** Change into build directory.

```sh
  cd build
```

**Step 3.** Start mesos master (Ensure work directory exists and has proper permissions).

```sh
  ./bin/mesos-master.sh --ip=127.0.0.1 --work_dir=/var/lib/mesos
```

**Step 4.** Start mesos agent (Ensure work directory exists and has proper permissions).

```sh
  ./bin/mesos-agent.sh --master=127.0.0.1:5050 --work_dir=/var/lib/mesos
```

**Step 5.** Visit the mesos web page (http://127.0.0.1:5050)

**Step 6.** Run C++ framework (Exits after successfully running some tasks.).

```sh
  ./src/test-framework --master=127.0.0.1:5050
```

**Step 7.** Run Java framework (Exits after successfully running some tasks.).

```sh
  ./src/examples/java/test-framework 127.0.0.1:5050
```

**Step 8.** Run Python framework (Exits after successfully running some tasks.).

```sh
  ./src/examples/python/test-framework 127.0.0.1:5050
```

