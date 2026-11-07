# Linux Homelab — Infraestrutura Self-Hosted

Homelab desenvolvido com hardware reaproveitado, visando aprendizado prático em administração de sistemas Linux, redes, armazenamento e automação, com baixo custo de implantação.

## Visão geral

![Visão geral do homelab](imagens/homelab-overview.jpg)

O ambiente é composto por dois servidores físicos — um desktop e um notebook — conectados a um switch Gigabit Ethernet na rede doméstica principal.

A infraestrutura hospeda serviços de DNS, compartilhamento de arquivos, mídia e um servidor Minecraft de uso familiar.

O projeto prioriza a reutilização de equipamentos, a redução de custos operacionais e a experimentação com tecnologias de infraestrutura.

## Infraestrutura

![Visão geral da infraestrutura](<imagens/arquitetura geral.svg>)

### Servidor principal — Desktop

**Sistema operacional:** Ubuntu Desktop, com interface gráfica desativada via `systemctl`.

Hardware parcialmente reaproveitado, incluindo processador e módulos de memória recuperados de descarte eletrônico.

**Armazenamento:**

- Disco independente para o sistema operacional.
- Dois HDDs de 4 TB em ZFS mirror.
- Pool ZFS denominado `hdd-4tb`.
- Verificação de integridade por scrub, com último resultado informado sem erros.

### Servidor secundário — Notebook Reaproveitado

**Sistema operacional:** Ubuntu Server.


## Tecnologias utilizadas

| Categoria | Tecnologias |
|---|---|
| Sistemas operacionais | Ubuntu Server, Ubuntu Desktop |
| Redes | TCP/IP, DHCP, DNS, SSH, Ethernet |
| Serviços | Pi-hole, Unbound, Samba, Jellyfin |
| Containers | Docker, Docker Compose |
| Armazenamento | ZFS mirror |
| Automação | cron, rsync, SSH |

## Objetivos e aprendizados

- Administrar servidores Linux em equipamentos físicos.
- Implantar e manter serviços de rede.
- Experimentar redundância de DNS.
- Configurar armazenamento com tolerância à falha de disco.
- Administrar aplicações containerizadas.
- Automatizar tarefas de sincronização e backup.
- Diagnosticar problemas de infraestrutura.
- Reaproveitar hardware para reduzir custos.

## Melhorias futuras

- Documentar procedimentos de recuperação de serviços.
- Implementar e testar backups versionados.
- Adicionar monitoramento centralizado.
- Registrar testes de restauração.
- Expandir a documentação de rede e armazenamento.

## Documentação técnica

- [Arquitetura de rede e DNS](docs/network.md)
- [Armazenamento ZFS](docs/storage.md)
- [Serviços hospedados](docs/services.md)
- [Automação de backups](docs/backups.md)
