---

#  Ansible Role: MikroTik IPsec IKEv2 Configuration

![Build Status](https://github.com/M0rtm/ansible-role-mikrotik_ipsec_ikev2/actions/workflows/ansible-galaxy-ci.yml/badge.svg)
[![Galaxy Role](https://img.shields.io/badge/Ansible--Galaxy-M0rtm.mikrotik_ipsec_ikev2-blue.svg?logo=ansible&logoColor=white)](https://galaxy.ansible.com/M0rtm/mikrotik_ipsec_ikev2/)

> Configure secure Site-to-Site IPsec IKEv2 tunnels between MikroTik routers using Ansible and the community.routeros collection.

---

## Requirements

- Ansible **2.12+**
- MikroTik RouterOS **v6.45+**
- API access to MikroTik device
- Collection `community.routeros`

```bash
ansible-galaxy collection install community.routeros
```

---

## 📁 Role Variables

These variables are defined in `defaults/main.yml`:

```yaml
# IPsec Proposal
ipsec_proposal_name: "{{ inventory_hostname }}"
ipsec_proposal_auth_algorithms: sha256
ipsec_proposal_enc_algorithms: aes-256-cbc
ipsec_proposal_lifetime: 1h

# IPsec Profile
ipsec_profile_name: "{{ inventory_hostname }}"
ipsec_profile_dh_group: modp2048
ipsec_profile_enc_algorithm: aes-256
ipsec_profile_hash_algorithm: sha256

# IPsec Peer
ipsec_peer_address: ""
ipsec_peer_exchange_mode: ike2

# IPsec Identity
ipsec_secret: ""

# IPsec Policy
ipsec_policy_name: "{{ inventory_hostname }}"
ipsec_src_address: ""
ipsec_dst_address: ""
ipsec_sa_src_address: ""        
ipsec_policy_tunnel: true
ipsec_policy_proposal: "{{ ipsec_proposal_name }}"

# Firewall rules
ipsec_firewall_ports: "500,4500"
ipsec_firewall_protocols: ["udp", "ipsec-esp"]

# Notifications
ipsec_restart_tunnel: true
```

You can override any variable in your inventory or playbook.

---

## 📅 Tasks Overview

### ⚖️ configure.yml
- Create proposal: AES-256-CBC + SHA256
- Create profile: AES-256 + MODP2048
- Configure peer (IKEv2 mode)
- Configure identity with PSK
- Add tunnel-mode IPsec policy

### 🔒 firewall.yml
- Add accept rules for `ipsec-esp` and `ipsec-ah`

### 🌋 handlers/main.yml
- Restart tunnel by disabling/enabling peer + policy on changes

---

## 📦 Install via Galaxy

```bash
ansible-galaxy install M0rtm.mikrotik_ipsec_ikev2
```

---

## 🔧 Usage Example

```yaml
- hosts: mikrotik
  gather_facts: false
  roles:
    - role: M0rtm.mikrotik_ipsec_ikev2
      vars:
        ipsec_peer_address: "192.168.2.111"
        ipsec_secret: "SuperSecretKey"
        ipsec_src_address: "192.168.88.0/24"
        ipsec_dst_address: "192.168.90.0/24"
        ipsec_sa_src_address: "192.168.1.1"

        ipsec_ike: "aes256-sha256-modp2048"
        ipsec_esp: "aes256-sha256-modp2048"
        ipsec_lifetime: "1h"
        ipsec_dpd_action: "restart"
        ipsec_dpd_timeout: "120s"
        ipsec_dpd_delay: "30s"
```

You can override any variables from `defaults/main.yml` or define additional IKE/DPD options as needed.

---

## 🌐 Tags

- `configure_ipsec`
- `firewall_rules`
- `restart_ipsec`

```bash
ansible-playbook site.yml --tags restart_ipsec
```

---

## 📢 Dependencies

- [community.routeros](https://galaxy.ansible.com/community/routeros)

---

## 📄 License

MIT

---

## 👤 Author

Created by **Dmytro Fedorov**

---

