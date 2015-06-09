etcd-ca
=======

A role to create/use an etcd-ca instance to generate certificates for hosts

Role Variables
--------------

- etcd_ca: etcd-ca binary location
- etcd_ca_url: url to download (compressed) etcd-ca from, if need be
- etcd_ca_depot: etcd-ca depot location
- etcd_ca_crt_group: group of hosts to generate certificates from
- etcd_ca_passphrase: passphrase for CA certificate
- etcd_ca_host_passphrase: passphrase for host certificate (read from hostvars)
- etcd_ca_host_ip: ip to generate certificate for (read from hostvars)

Example Playbook
----------------

Example for bootstraping a SSL-protected etcd cluster with CoreOS

    - name: bootstrap coreos hosts
      hosts: core
      gather_facts: False
      roles:
        - role: "sigma.coreos-bootstrap"
    - name: ca
      hosts: localhost
      roles:
        - role: "sigma.etcd-ca"
          etcd_ca_crt_group: "core"
          etcd_ca_depot: "/tmp/etcd-ca-depot"
    - name: bootstrap etcd cluster
      hosts: core
      roles:
        - role: "sigma.etcd-cluster"
          etcd_ca_host: "localhost"
          etcd_ca_depot: "/tmp/etcd-ca-depot"
          local_certs: "certs"

License
-------

MIT
