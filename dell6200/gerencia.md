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
## Upgrade firmware para 3.3.18.1 remove acesso HTTP
É um comportamento esperado já que o HTTP contém uma vulnerabilidade nesse equipamento. A alternativa é habilitar o HTTPS
Comando para garantir que o HTTP esteja desativado:
```shell
sw-a14superior#show ip http

HTTP Server is Disabled.  Port :  80


```
## Habilitar o HTTPS
Segue os comando para gerar certificado e chave e ativar o HTTPs no Dell 6200
```shell
sw-a14superior#configure

sw-a14superior(config)#crypto Certificate 2 generate

sw-a14superior(config-crypto-cert)#key-generate

sw-a14superior(config-crypto-cert)#exit

sw-a14superior(config)#ip https certificate 2

sw-a14superior(config)#ip https server

```
