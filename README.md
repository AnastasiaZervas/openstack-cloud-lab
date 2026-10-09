# OpenStack Cloud Lab: administering a tenant end to end

A single-node OpenStack cloud (DevStack) built on a Google Cloud VM with nested virtualization, then administered as a cloud operator: identity, quotas, networking, security groups and instances, ending with a working two-tier application running inside the tenant.

## What was built

| Resource | Role |
|---|---|
| `BrickProject` | The tenant (project) |
| `BrickAdmin` | Project administrator, with its own `openrc` credentials file |
| `lab.small`, `lab.medium` | Custom flavors (1 vCPU / 1 GB and 1 vCPU / 2 GB, 10 GB disk) |
| Quotas | 4 cores, 3 instances, 4 GB RAM for the project |
| `BrickNet` / `BrickSubnet` | Private network 192.168.100.0/24 with DHCP |
| `BrickRouter` | Router from the private network to the external `public` network |
| `BrickSG` | Security group allowing SSH, HTTP, ICMP and PostgreSQL |
| `BrickVM-Postgres` | Database tier: PostgreSQL on Debian 12 |
| `BrickVM-Flask` | Application tier: Flask REST API served by Gunicorn (systemd) behind Nginx |

Both instances use key pairs and floating IPs. The Horizon dashboard is reached through an SSH tunnel to the GCP host.

## Steps covered

1. **Platform** — GCP VM with nested virtualization, `stack` user, `local.conf`, `stack.sh`, NAT rule so instances reach the internet.
2. **Identity and flavors** — project, user, role assignment, flavors, `openrc`.
3. **Quotas** — sized for the two-VM scenario and verified in Horizon.
4. **Networking** — network, subnet, router, external gateway.
5. **Security groups** — per-port ingress rules.
6. **Instances** — key pairs, two Debian 12 VMs, floating IPs.
7. **Verification** — connectivity checks, PostgreSQL setup and remote access from the application VM, a Glance snapshot of the database VM and a GCP disk snapshot, the Flask API under Gunicorn and Nginx, and an end-to-end test from the browser.

Each step was done with the `openstack` CLI and checked in Horizon. The [full report](docs/report.el.md) (in Greek) contains every command with its screenshot.

![Network topology](screenshots/34.png)

## Stack

OpenStack (DevStack: Keystone, Nova, Neutron, Glance, Horizon) · Google Cloud Compute Engine · Debian 12 · PostgreSQL · Flask · Gunicorn · Nginx · iptables

## Repository layout

```
docs/report.el.md   Full walkthrough with commands and figures
screenshots/        Screenshots referenced by the report
```

The environment was a disposable lab. The passwords and addresses that appear in the report belonged to it and no longer exist.

## Context

Coursework for the Cloud Computing course (DIT266), Department of Informatics and Telematics, Harokopio University of Athens (2026).
