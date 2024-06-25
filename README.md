# OpenShift Lab Management Tool - Hetzner

This script provides a set of commands to create, destroy, manage, and monitor a cluster of virtual machines using `libvirt`. It is designed to be used as an OpenShift client plugin.

## Installation

Copy the script to the `/usr/local/bin/` directory and name it `oc-lab`:

```bash
sudo cp oc-lab /usr/local/bin/oc-lab
sudo chmod +x /usr/local/bin/oc-lab
```

## Usage

```bash
oc lab --command --name <name_of_cluster> [options]
```

### Commands

- `--create`: Create a new cluster.
- `--destroy`: Destroy an existing cluster.
- `--status`: Check the status of the VMs.
- `--start`: Start all the VMs.
- `--stop`: Stop all the VMs.
- `--attach`: Attach an ISO image to all the VMs.

### Options

- `--name, -n <name_of_cluster>`: Name of the cluster (required).
- `--workers <number_of_worker_nodes>`: Number of worker nodes (default is 2).
- `--airgap`: Disconnect the network from the internet (default is connected).
- `--iso <path_to_iso>`: Path to the ISO image (required for `--attach`).
- `--domain, -d <domain_name>`: Domain name (default is `superdemo.live`).

## Examples

### Creating the Lab

1. **Create the Cluster**

   To create a cluster named `ocp` with the default number of worker nodes (2) and connected to the internet:

   ```bash
   oc lab -n ocp --create
   ```

   To create a cluster named `ocp` with 4 worker nodes and airgapped (disconnected from the internet):

   ```bash
   oc lab -n ocp --create --workers 4 --airgap
   ```

   To create a cluster named `ocp` with a custom domain:

   ```bash
   oc lab -n ocp --create --domain customdomain.com
   ```

2. **Attach an ISO Image**

   To attach an ISO image to all VMs in the cluster named `ocp`:

   ```bash
   oc lab -n ocp --attach --iso /path/to/iso
   ```

3. **Start All VMs**

   To start all VMs in the cluster named `ocp`:

   ```bash
   oc lab -n ocp --start
   ```

4. **Check the Status of VMs**

   To check the status of VMs in the cluster named `ocp`:

   ```bash
   oc lab -n ocp --status
   ```

### Destroying the Lab

To destroy a cluster named `ocp`:

```bash
oc lab -n ocp --destroy
```

This tool simplifies the management of your OpenShift lab environment, making it easy to create, manage, and destroy clusters with a single command.
