# Armazenamento — ZFS

## Arquitetura de armazenamento

![Visão geral do armazenamento](<../imagens/overview-storage.svg>)

O servidor principal do homelab utiliza três discos rígidos, organizados de forma a separar o sistema operacional dos dados armazenados.

- **HDD de 500 GB:** utilizado exclusivamente para o sistema operacional Ubuntu.
- **2 HDDs de 4 TB:** utilizados em um pool ZFS configurado em espelhamento (*mirror*).

Essa organização permite manter o armazenamento dos dados independente do disco utilizado pelo sistema operacional.

## ZFS

O [OpenZFS](https://openzfs.org/) é uma plataforma de armazenamento que combina funcionalidades de sistema de arquivos e gerenciamento de volumes.

Entre seus recursos estão a verificação de integridade por checksums, snapshots, espelhamento de discos e mecanismos de detecção e correção de corrupção de dados, quando há redundância adequada.

No homelab, o ZFS foi configurado diretamente pela linha de comando do Linux para gerenciar os dois HDDs destinados ao armazenamento.

## Espelhamento — ZFS Mirror

O pool `hdd-4tb` utiliza dois discos físicos configurados em mirror.

Nesse arranjo, os dados são armazenados de forma redundante nos dois discos, permitindo que o pool continue operando caso uma das unidades apresente falha, desde que a outra permaneça funcional.

A capacidade bruta combinada é de 8 TB, enquanto a capacidade útil nominal do espelhamento corresponde aproximadamente à capacidade de um disco de 4 TB, antes dos ajustes de unidades e do espaço utilizado pelo sistema de arquivos.

O espelhamento oferece tolerância à falha de uma unidade, mas não protege contra exclusões acidentais, falhas simultâneas ou outras formas de perda de dados.

## Integridade e manutenção

O ZFS utiliza checksums para verificar a integridade dos dados armazenados.

Uma das ferramentas de manutenção disponíveis é o **scrub**, procedimento que percorre os dados do pool para verificar sua integridade e, quando possível, corrigir inconsistências utilizando as cópias redundantes.

O estado do armazenamento pode ser consultado com:

`zpool status`

O último estado verificado do pool `hdd-4tb` apresentou:

- Pool online.
- Dois discos online.
- Nenhum erro de leitura, escrita ou checksum registrado.
- Último scrub concluído em 11/10/2026, sem erros e sem necessidade de reparos.

Essas informações representam o estado observado no momento da consulta, não uma garantia permanente de integridade.

## Backups e limitações

Embora o ZFS mirror ofereça redundância, **espelhamento não substitui backup**.

A exclusão ou alteração acidental de arquivos, por exemplo, é refletida no armazenamento espelhado.

O servidor também recebe sincronizações periódicas dos arquivos do servidor Minecraft, utilizando rsync sobre SSH.

A documentação dessas rotinas, incluindo suas limitações e possibilidades de recuperação, está disponível em [Backups automatizados](backups.md).

## Possíveis melhorias

- Verificar e documentar a periodicidade dos scrubs.
- Experimentar snapshots ZFS para manter versões anteriores dos dados.
- Implementar monitoramento e alertas de falhas nos discos.
- Documentar procedimentos de substituição de discos e recuperação do pool.
