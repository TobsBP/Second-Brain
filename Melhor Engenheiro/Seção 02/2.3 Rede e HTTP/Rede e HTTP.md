![[Rede e HTTP Draw]]

Toda API que a gente escreve é, no fundo, texto (ou bytes) indo de uma máquina pra outra. Entender as camadas por baixo explica a maioria dos problemas "misteriosos": timeout, CORS, latência alta, conexão recusada, certificado inválido.

### Camadas
O modelo que importa na prática é o **TCP/IP** (o OSI de 7 camadas é mais didático do que real):

| Camada | O que faz | Exemplos |
|---|---|---|
| Aplicação | O protocolo que o programa fala | HTTP, DNS, SMTP, WebSocket |
| Transporte | Conexão entre processos (portas) | TCP, UDP |
| Rede | Endereçar e rotear entre máquinas | IP |
| Enlace/Física | Levar o bit até o próximo salto | Ethernet, Wi-Fi |

Cada camada só conversa com a de baixo e encapsula os dados da de cima: a requisição HTTP vai dentro de um segmento TCP, que vai dentro de um pacote IP, que vai dentro de um frame Ethernet.

### IP e portas
- **IP** identifica a máquina (`192.168.0.10`, ou IPv6 `2001:db8::1`). Não garante nada: o pacote pode se perder, duplicar ou chegar fora de ordem.
- **Porta** identifica o processo dentro da máquina (0–65535). Padrões: 80 HTTP, 443 HTTPS, 5432 Postgres, 22 SSH.
- Uma conexão é identificada por `(IP origem, porta origem, IP destino, porta destino, protocolo)`.
- `localhost`/`127.0.0.1` é a própria máquina. Servidor ouvindo em `127.0.0.1` só aceita conexão local; em `0.0.0.0` aceita de qualquer interface. Esse é um erro clássico com Docker.
- **NAT**: vários dispositivos da rede interna saem pela internet com o mesmo IP público.

### DNS
Traduz nome em IP. `api.exemplo.com` → resolver local → servidores raiz → `.com` → servidor autoritativo de `exemplo.com` → IP.

- Registros principais: `A` (IPv4), `AAAA` (IPv6), `CNAME` (apelido para outro nome), `MX` (e-mail), `TXT` (verificações, SPF).
- Cada resposta tem um **TTL** e fica em cache. Por isso uma mudança de DNS "demora pra propagar": na verdade o cache é que ainda não expirou.

```bash
dig api.github.com +short
```

### TCP vs UDP
#### TCP
Orientado a conexão, **confiável e ordenado**: retransmite o que se perde, reordena, controla fluxo e congestionamento.

Abrir conexão custa o **3-way handshake** (`SYN` → `SYN-ACK` → `ACK`), ou seja, 1 RTT antes de mandar qualquer dado. Por isso reaproveitar conexão (keep-alive, connection pool no banco) faz tanta diferença.

**Head-of-line blocking**: se um pacote se perde, tudo o que veio depois espera a retransmissão, mesmo que já tenha chegado.

#### UDP
Sem conexão, sem garantia de entrega nem de ordem. Só manda. É mais leve e tem menos latência. Usado em DNS, jogos, voz/vídeo em tempo real e no QUIC (HTTP/3).

### TLS (o S do HTTPS)
Fica entre o TCP e o HTTP e garante três coisas:
- **Confidencialidade**: o tráfego vai criptografado.
- **Integridade**: ninguém altera no meio do caminho sem ser detectado.
- **Autenticidade**: o servidor prova quem é com um **certificado** assinado por uma CA em que o sistema confia.

O handshake negocia as chaves com criptografia assimétrica e depois tudo segue com uma chave simétrica, que é muito mais rápida. O TLS 1.3 faz isso em 1 RTT. O nome do domínio vai no **SNI**, e é assim que um mesmo IP serve vários sites com certificados diferentes.

### HTTP
Protocolo de **requisição/resposta** e **stateless**: cada requisição é independente e o servidor não "lembra" do cliente. Estado vem por fora (cookie, token).

#### Anatomia
```http
POST /users HTTP/1.1
Host: api.exemplo.com
Content-Type: application/json
Authorization: Bearer eyJ...

{"name": "Tobias"}
```
```http
HTTP/1.1 201 Created
Content-Type: application/json
Location: /users/42

{"id": 42, "name": "Tobias"}
```
Linha inicial, headers, linha em branco e body.

#### Métodos
| Método | Uso | Seguro | Idempotente |
|---|---|---|---|
| GET | Ler | ✅ | ✅ |
| POST | Criar / ação | ❌ | ❌ |
| PUT | Substituir inteiro | ❌ | ✅ |
| PATCH | Alterar parcial | ❌ | ❌* |
| DELETE | Remover | ❌ | ✅ |

- **Seguro**: não altera estado no servidor.
- **Idempotente**: repetir N vezes tem o mesmo efeito que uma vez. É isso que define se dá pra fazer **retry** automático sem medo. POST repetido cria duas vezes; por isso APIs de pagamento usam um header `Idempotency-Key`.

\*PATCH pode ser idempotente, depende de como foi implementado.

#### Status codes
- **2xx sucesso**: `200 OK`, `201 Created`, `204 No Content`
- **3xx redirecionamento**: `301` (permanente), `302`/`307` (temporário), `304 Not Modified` (use o cache)
- **4xx erro do cliente**: `400` (payload inválido), `401` (não autenticado, "quem é você?"), `403` (autenticado mas sem permissão), `404`, `409 Conflict`, `422` (validação), `429 Too Many Requests`
- **5xx erro do servidor**: `500`, `502 Bad Gateway` (o proxy não conseguiu falar com o upstream), `503` (indisponível/sobrecarregado), `504` (o upstream demorou demais)

Regra prática: 4xx não adianta repetir igual; 5xx e 429 podem ter retry (com backoff).

#### Headers importantes
- `Content-Type` / `Accept`: formato do body enviado / esperado.
- `Authorization`: credenciais (`Bearer <token>`).
- `Cookie` / `Set-Cookie`: estado no navegador. Flags de segurança: `HttpOnly` (JS não lê), `Secure` (só HTTPS), `SameSite` (protege contra CSRF).
- `Cache-Control`, `ETag`, `If-None-Match`: cache.
- `User-Agent`, `Origin`, `Referer`: de onde veio a requisição.

### Versões do HTTP
- **HTTP/1.1**: texto, uma requisição por vez por conexão. Navegadores abrem ~6 conexões por domínio para compensar. Keep-alive reaproveita a conexão.
- **HTTP/2**: binário, **multiplexa** várias requisições numa conexão só e comprime headers (HPACK). Ainda roda sobre TCP, então o head-of-line blocking de TCP continua.
- **HTTP/3**: roda sobre **QUIC** (UDP). Cada stream é independente, então perder um pacote não trava as outras. Handshake de transporte e TLS juntos e troca de rede (Wi-Fi → 4G) sem derrubar a conexão.

### Cache HTTP
```http
Cache-Control: public, max-age=3600
ETag: "abc123"
```
- `max-age`: por quantos segundos a resposta vale sem precisar perguntar ao servidor.
- `no-cache`: pode guardar, mas precisa revalidar antes de usar. `no-store`: não guarda nada.
- **Revalidação**: o cliente manda `If-None-Match: "abc123"`; se nada mudou, o servidor responde `304` sem body, o que economiza banda.
- `private` vs `public`: se CDN/proxy compartilhado pode guardar. Resposta com dado do usuário é `private`.

### CORS
É uma regra do **navegador**, não do servidor. Por padrão uma página em `app.com` não pode ler a resposta de `api.com` (**Same-Origin Policy**; origem = protocolo + domínio + porta).

O servidor libera com headers:
```http
Access-Control-Allow-Origin: https://app.com
Access-Control-Allow-Methods: GET, POST
Access-Control-Allow-Headers: Content-Type, Authorization
```
Requisições "não simples" (com `Authorization`, `Content-Type: application/json`, `PUT`/`DELETE`...) disparam antes um **preflight** `OPTIONS`. Por isso o `curl` e o Postman funcionam e o navegador dá erro: eles não aplicam CORS. CORS não é segurança da API; é proteção do usuário no navegador.

### Na prática com TypeScript
```typescript
const res = await fetch("https://api.exemplo.com/users/42", {
	headers: { Accept: "application/json" },
	signal: AbortSignal.timeout(5000), // sem timeout, fetch pode esperar pra sempre
});

if (!res.ok) throw new Error(`HTTP ${res.status}`); // fetch NÃO lança em 4xx/5xx
const user: unknown = await res.json(); // chega sem tipo, valida na fronteira
```
Duas pegadinhas do `fetch`:
- Ele só rejeita a Promise em falha de **rede** (DNS, conexão recusada, timeout). Um 404 ou 500 volta como resposta normal, então é preciso checar `res.ok`.
- O `res.json()` devolve dado sem garantia de formato. O `as User` só engana o compilador (ver [[TypeScript#Consequência do type erasure]]).

### O que acontece ao digitar uma URL
1. O navegador faz o parse da URL: `https` → porta 443, host `exemplo.com`, path `/`.
2. **DNS** resolve o host para um IP (checando os caches antes).
3. **TCP handshake** com o IP na porta 443.
4. **TLS handshake**: valida o certificado e negocia as chaves.
5. Envia a **requisição HTTP** `GET /`.
6. O servidor (normalmente passando antes por load balancer/CDN/reverse proxy) processa e responde.
7. O navegador renderiza o HTML e dispara novas requisições para CSS, JS e imagens, reaproveitando a conexão.

Latência total ≈ DNS + 1 RTT (TCP) + 1 RTT (TLS 1.3) + 1 RTT (requisição) + tempo do servidor. Longe do servidor, cada RTT pesa. É por isso que CDN, keep-alive e HTTP/3 existem.
