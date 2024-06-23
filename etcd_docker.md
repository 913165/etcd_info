    docker run -d \
      -p 2379:2379 \
      -p 2380:2380 \
      --name etcd \
      --volume=/tmp/etcd-data.tmp:/etcd-data \
      quay.io/coreos/etcd:latest \
      /usr/local/bin/etcd \
      --name s1 \
      --data-dir /etcd-data \
      --listen-client-urls http://0.0.0.0:2379 \
      --advertise-client-urls http://0.0.0.0:2379 \
      --listen-peer-urls http://0.0.0.0:2380 \
      --initial-advertise-peer-urls http://0.0.0.0:2380 \
      --initial-cluster s1=http://0.0.0.0:2380 \
      --initial-cluster-token tkn

1.  `docker run -d`: This starts a new Docker container in detached mode (runs in the background).
2.  `-p 2379:2379` and `-p 2380:2380`: These map ports from the host to the container. Port 2379 is used for client communication, and 2380 for peer communication in etcd.
3.  `--name etcd`: This assigns the name "etcd" to the container.
4.  `--volume=/tmp/etcd-data.tmp:/etcd-data`: This creates a volume mount, linking the `/tmp/etcd-data.tmp` directory on the host to `/etcd-data` in the container. This is where etcd will store its data.
5.  `quay.io/coreos/etcd:latest`: This specifies the Docker image to use, pulling the latest version of etcd from the Quay.io registry.
6.  `/usr/local/bin/etcd`: This is the command that will be run inside the container to start etcd.
7.  `--name s1`: This sets the name of this etcd instance to "s1".
8.  `--data-dir /etcd-data`: This tells etcd where to store its data inside the container.
9.  `--listen-client-urls http://0.0.0.0:2379`: This configures etcd to listen for client requests on all network interfaces on port 2379.
10.  `--advertise-client-urls http://0.0.0.0:2379`: This is the URL that etcd will advertise to clients.
11.  `--listen-peer-urls http://0.0.0.0:2380`: This configures etcd to listen for peer requests on all network interfaces on port 2380.
12.  `--initial-advertise-peer-urls http://0.0.0.0:2380`: This is the URL that etcd will advertise to peers.
13.  `--initial-cluster s1=http://0.0.0.0:2380`: This defines the initial cluster configuration. Here, it's set up for a single-node cluster.
14.  `--initial-cluster-token tkn`: This sets the initial cluster token, which is used to identify unique cluster during bootstrapping.

This command sets up a single-node etcd cluster in a Docker container, with data persistence and proper networking configuration. It's suitable for development or testing purposes, but for production use, you'd typically want a multi-node cluster for high availability.
