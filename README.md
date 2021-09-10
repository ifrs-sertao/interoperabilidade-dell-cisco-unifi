# interoperabilidade-dell-cisco-unifi
Anotações sobre Interoperailidade entre swicthes Dell, Cisco e Unifi

# Para acessar os switches Cisco 2950
```shell
 ssh -oKexAlgorithms=+diffie-hellman-group1-sha1 admin@172.18.0.31
```

# Enlace Dell N2000 + Cisco 2960S


### No lado do switch de distribuição DELL N2000
```shell
show interfaces switchport gigabitethernet 1/0/6 
git 
Port: Gi1/0/6
VLAN Membership Mode: Trunk Mode
Member of VLANs : (900),1-899,901-4093
Access Mode VLAN: 1 (default)
General Mode PVID: 900
General Mode Ingress Filtering: Enabled
General Mode Acceptable Frame Type: Admit All
General Mode Dynamically Added VLANs:
General Mode Untagged VLANs: 900
General Mode Tagged VLANs: 1,25-26,40-41,50-51,53,
                           200,301-308,331,340,525,540,600,610,700,800,911-912,
                           920
General Mode Forbidden VLANs:
Trunking Mode Native VLAN: 900
Trunking Mode Native VLAN Tagging: Disabled
Trunking Mode VLANs Enabled: All
Private VLAN Host Association: none
Private VLAN Mapping:
Private VLAN Operational Bindings:
Default Priority: 0
Protected: Disabled

```

### Porta trunk(uplink) do Cisco 2960S
```shell
sw-agri2#show interfaces gigabitEthernet 0/24 switchport 
Name: Gi0/24
Switchport: Enabled
Administrative Mode: trunk
Operational Mode: trunk
Administrative Trunking Encapsulation: dot1q
Operational Trunking Encapsulation: dot1q
Negotiation of Trunking: On
Access Mode VLAN: 1 (default)
Trunking Native Mode VLAN: 900 (GERENCIA)
Administrative Native VLAN tagging: enabled
Voice VLAN: none
Administrative private-vlan host-association: none 
Administrative private-vlan mapping: none 
Administrative private-vlan trunk native VLAN: none
Administrative private-vlan trunk Native VLAN tagging: enabled
Administrative private-vlan trunk encapsulation: dot1q
Administrative private-vlan trunk normal VLANs: none
Administrative private-vlan trunk associations: none
Administrative private-vlan trunk mappings: none
Operational private-vlan: none
Trunking VLANs Enabled: 1-2000
Pruning VLANs Enabled: 2-1001
Capture Mode Disabled
Capture VLANs Allowed: ALL
          
Protected: false
Unknown unicast blocked: disabled
Unknown multicast blocked: disabled
Appliance trust: none

```

### Porta trunk(uplink) do Unifi US-8-150W

A configuração pelo console Web dá entender que a porta fica como Trunk.

# Spanning-tree

### Configuração do RSTP no switch de distribuição Dell N2000

```shell
show spanning-tree 

Spanning Tree            : Enabled
BPDU Flooding            : Disabled
Portfast BPDU Filtering  : Enabled
Mode                     : rstp
Portfast BPDU Guard      : Disabled
CST Regional Root        : 10:00:F8:B1:56:7D:6B:AF
Regional Root Path Cost  : 0
ROOT ID
              Priority        0
              Address         E4F0.04D8.8F1E
              Path Cost       22000
              Root Port       Gi1/0/48
              Hello Time: 2s Max Age: 20s Forward Delay: 15s Transmit Hold Count: 6s
              Bridge Max Hops: 20
Bridge ID
              Priority        4096
              Address         F8B1.567D.6BAF
              Hello Time: 2s Max Age: 20s Forward Delay: 15s
Interfaces

```
### Configuração do RSTP no switch de acesso Cisco 2960S
```shell
show spanning-tree 

VLAN0001
  Spanning tree enabled protocol rstp
  Root ID    Priority    0
             Address     e4f0.04d8.8f1e
             Cost        22019
             Port        24 (GigabitEthernet0/24)
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec

  Bridge ID  Priority    8193   (priority 8192 sys-id-ext 1)
             Address     5897.1ef7.3c80
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec
             Aging Time  300 sec
```

# Enlace Dell N1524 e Cisco 2960S

Neste cenário os switches só interoperaram alterando o modo spanning-tree do Dell N1524 para PVRST, após seguir orientações da própria Dell. No entanto, essas alterações foram desfeitas e os equipamentos no fim das contas interoperaram simplesmente usando o modo RSTP - Rapid Spanning-tree.

Resta monitorar e estudar mais esses padrões.
