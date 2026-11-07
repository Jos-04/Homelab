# Rede e DNS

## Arquitetura de rede

![Visão geral da arquitetura](<../imagens/arquitetura dns.svg>)

Os dois servidores do homelab estão conectados à rede principal por meio de um switch Gigabit Ethernet.

O roteador principal distribui os endereços dos dois servidores Pi-hole via DHCP. Cada Pi-hole utiliza sua própria instância do Unbound, permitindo resolução DNS independente entre as máquinas. A redundância DNS foi testada na rede principal.

## Pi-hole

O [Pi-hole](https://pi-hole.net/) é uma ferramenta de código aberto que atua como um **DNS sinkhole**, permitindo bloquear a resolução de domínios associados a anúncios, rastreadores, malware e outros conteúdos indesejados na rede.

A filtragem utiliza **blocklists**: listas de domínios que são incorporadas à base local de bloqueio. Um exemplo de fonte é o repositório [HaGeZi DNS Blocklists](https://github.com/hagezi/dns-blocklists). O Pi-hole não precisa consultar essas listas na internet a cada requisição DNS.

### Funcionamento

Quando um dispositivo faz uma consulta DNS, o Pi-hole aplica suas regras de filtragem. Se o domínio estiver bloqueado, retorna uma resposta de bloqueio. Caso seja permitido, pode responder com dados válidos em cache; caso contrário, encaminha a consulta ao resolvedor configurado — neste homelab, o Unbound.

![Fluxo de filtragem DNS do Pi-hole](../imagens/pihole.svg)

### Limitações

A filtragem DNS atua sobre nomes de domínio, não sobre elementos específicos de uma página. Por isso, anúncios servidos pelo mesmo domínio do conteúdo legítimo podem não ser bloqueados sem afetar também esse conteúdo.

## Unbound

O [Unbound](https://nlnetlabs.nl/projects/unbound/about/) é um resolvedor DNS recursivo de código aberto desenvolvido pela NLnet Labs.

Quando configurado para resolução recursiva, consulta a hierarquia DNS diretamente, sem encaminhar todas as consultas a um resolvedor público único. Isso oferece maior controle sobre a resolução de nomes e reduz a concentração das consultas em um provedor externo.

### Funcionamento

Quando recebe uma consulta do Pi-hole, o Unbound verifica se há uma resposta válida em seu cache. Se não houver, pode iniciar uma resolução recursiva: consulta servidores raiz, servidores do domínio de topo (TLD) e servidores autoritativos, conforme necessário, até obter a resposta. Respostas podem ser armazenadas em cache de acordo com o TTL.

![Fluxo de resolução DNS recursiva do Unbound](../imagens/unbound.svg)

### Privacidade e segurança

O Unbound oferece mecanismos como cache, validação DNSSEC, QNAME minimisation e suporte a DNS-over-TLS (DoT), dependendo da configuração utilizada. **Esses recursos não estão aqui declarados como habilitados no homelab.**

A recursão DNS tradicional não criptografa automaticamente todas as consultas. A QNAME minimisation reduz a informação enviada a diferentes servidores durante a resolução, enquanto DNSSEC pode validar a autenticidade e integridade das respostas de zonas assinadas.

### Integração com Pi-hole

Cada servidor físico executa sua própria instância de Pi-hole e Unbound. O Pi-hole filtra as consultas; o Unbound resolve as consultas permitidas quando necessário. Essa configuração mantém os dois caminhos de resolução independentes entre os servidores e contribui para a redundância DNS testada na rede principal.
