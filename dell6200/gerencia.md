## Configurações de VLAN de Gerência e Roteamento no Dell 6248
Um aprendizado recente foi que nesse modelo( que é camada 2 e 3) a VLAN de gerência não aceita a definição de um default gateway. A solução que encontramos até agora foi criar uma VLAN aleatória e habilitar o routing nela,
para que as configurações de IP da VLAN de gerência funcionem.

### Ao tentar definir o default gateway recebemos um erro:
```shell
sw-a14superior(config)#ip default-gateway 172.18.0.254

IP address and gateway do not reside on the same subnet!

```

### A solução encontrada:

```shell
sw-a14superior(config)#vlan database

sw-a14superior(config-vlan)#vlan 4054
Warning: The use of large numbers of VLANs or interfaces may cause significant
delays in applying the configuration.

sw-a14superior(config-vlan)#exit

sw-a14superior(config)#ip address vlan 4054

sw-a14superior(config)#ip routing

```
