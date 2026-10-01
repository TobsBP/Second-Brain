## Firewall
internet -> Parede (Firewall) <- rede local

Um ataque dentro da própria rede, o firewall não tem conhecimento. Dentro da própria rede pode usar para se defender, IDS, EDR, ACL (Lista de controle de acesso).

## ACL
- A ACL no roteador só filtra tráfego que **passa pelo roteador**, ou seja, entre redes/VLANs diferentes. Dois hosts na mesma sub-rede conversam direto pelo switch e a ACL do roteador nem vê esse tráfego.
- Ela não "identifica" o atacante sozinha: ela só permite/nega pacotes. Dá pra registrar quem bateu na regra colocando `log` no fim da linha (ex.: `deny ip any any log`), aí aparece no log do roteador.

Existem alguns tipos de ACLs na cisco.
- ACL padrão
	- Filtra apenas IP de origem
	- Intervalo numérico: 1-99 e 1300-1999
	- Pouco granular
- ACL estendida
	- Filtra IP de origem **e** destino, protocolo (ip, tcp, udp, icmp) e porta
	- Intervalo numérico: 100-199 e 2000-2699
	- Bem granular
- ACL Nomeada (Recomendada)
	- Usa nome em vez de número (ex.: `ACL_GUEST`), pode ser padrão ou estendida
	- Dá pra editar/inserir regras pelo número de sequência sem apagar a ACL inteira

Para no primeiro encontro da tabela, é lida de forma ordenada ou seja se a regra na tabela está abaixo de uma porta que era para ser bloqueada a pessoa vai conseguir acessar pois não chegou lá ainda. Essa leitura de cima pra baixo parando no primeiro match vale para **todos** os tipos de ACL, não só a nomeada. Por isso regra mais específica vem antes da mais genérica.

Config do ACL de convidados
```cisco
enable
config t
ip access-list extended ACL_GUEST
! deixa a rede 192.168.1.0/24 acessar só a rede 192.168.10.0/24
permit ip 192.168.1.0 0.0.0.255 192.168.10.0 0.0.0.255
! é implícito, não precisa escrever (mas escrever deixa claro)
deny ip any any
exit
interface g0/0
! aplicando essa regra na entrada da porta g0/0
ip access-group ACL_GUEST in
```

> [!info] Importante
> - O `permit` não deixa "qualquer pessoa da rede acessar": ele libera só a origem `192.168.1.0/24` indo pro destino `192.168.10.0/24`. Qualquer outra coisa cai no `deny` implícito.

Config de ACL de alunos
```cisco
enable
config t
ip access-list extended ACL_ALUNOS
! rede aluno n pode chegar em financeiro
deny ip 192.168.2.0 0.0.0.255 192.168.1.0 0.0.0.255
permit ip any any
exit
interface g1/0
ip access-group ACL_ALUNOS in
```

## Pontos importantes

> [!note] Complemento
> **Deny implícito:** toda ACL termina com um `deny any` invisível. Uma ACL só com `deny` bloqueia tudo, por isso a de alunos precisa do `permit ip any any` no final.
>
> **Wildcard mask:** é o inverso da máscara de sub-rede. `0` = o bit tem que bater, `1` = tanto faz.
> - `/24` → máscara `255.255.255.0` → wildcard `0.0.0.255`
> - Conta rápida: `255.255.255.255 - máscara`
> - Atalhos: `host 192.168.1.5` = `192.168.1.5 0.0.0.0`; `any` = `0.0.0.0 255.255.255.255`
>
> **Onde aplicar:**
> - Padrão → o mais perto possível do **destino** (só olha a origem, se ficar perto da origem bloqueia o host pra tudo)
> - Estendida → o mais perto possível da **origem** (descarta o tráfego antes de ele andar pela rede)
>
> **in x out:** `in` filtra o pacote entrando na interface (antes do roteamento), `out` filtra saindo. Só pode **uma ACL por interface, por sentido, por protocolo** (IPv4/IPv6).
>
> **Filtrando porta (estendida):**
> ```cisco
> ! bloqueia só HTTP da rede alunos pro servidor
> deny tcp 192.168.2.0 0.0.0.255 host 192.168.1.10 eq 80
> ```
>
> **Editar ACL nomeada** com número de sequência (as regras vêm numeradas de 10 em 10):
> ```cisco
> ip access-list extended ACL_ALUNOS
> 15 deny icmp any any
> no 20
> ```
>
> **Verificar:**
> - `show access-lists` → regras e quantos pacotes bateram em cada uma (matches)
> - `show ip interface g0/0` → qual ACL está aplicada e em qual sentido
> - `show running-config`
