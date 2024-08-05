if errors found when creating services/registry:
``error creating external connectivity network: cannot restrict inter-container communication: ensure that the br_netfilter kernel module is loaded``

0. RUN:
``apt install build-essential dkms``

1. Check if ``br_netfilter`` is loaded:

``lsmod | grep br_netfilter``

If there's no output, it means the module is not loaded.

2. Load the ``br_netfilter`` module:
Run the following command to load the module:
``sudo modprobe br_netfilter``

3. Ensure the module loads on boot:
To make sure the module loads automatically on system boot, add it to the /etc/modules-load.d/ configuration:
``echo "br_netfilter" | sudo tee /etc/modules-load.d/br_netfilter.conf``

4. Enable bridge-nf-call-iptables:
You may also need to enable the bridge-nf-call-iptables to ensure that the iptables rules are applied to bridged traffic:
``sysctl -w net.bridge.bridge-nf-call-iptables=1``

5. Persist the sysctl setting:
To make this setting persistent across reboots, add it to /etc/sysctl.conf:
``echo "net.bridge.bridge-nf-call-iptables = 1" | sudo tee -a /etc/sysctl.conf
sudo sysctl -p``




