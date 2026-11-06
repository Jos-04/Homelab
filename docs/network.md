## Arquitetura de rede

![Visão geral da arquitetura](<imagens/arquitetura dns.svg>)

Os dois servidores estão conectados à rede principal através de um switch Gigabit Ethernet.

O roteador principal distribui os endereços dos dois servidores Pi-hole via DHCP.

Cada Pi-hole possui sua própria instância do Unbound, permitindo resolução DNS independente entre as duas máquinas.

A redundância DNS foi testada na rede principal.

### Sub-rede adicional

A infraestrutura doméstica também possui uma segunda sub-rede, conectada à rede principal por meio da interface WAN de outro roteador.

Esse roteador permite configurar apenas um servidor DNS via DHCP, limitando a redundância automática para seus clientes.

Essa limitação é conhecida e faz parte das características atuais do ambiente.
