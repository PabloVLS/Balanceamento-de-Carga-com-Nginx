# Alta Disponibilidade com Nginx (Exemplo Docker)

![Docker](https://img.shields.io/badge/Docker-ready-2496ED)
![Nginx](https://img.shields.io/badge/Nginx-Load%20Balancer-009639)

**Resumo**
- **Projeto:** Demonstração simples de balanceamento de carga com Nginx usando Docker Compose.
- **Objetivo:** Executar um load balancer (Nginx) que distribui requisições entre dois servidores web (Nginx) com um index.html compartilhado.

**Pré-requisitos**
- **Docker** instalado e em execução.
- **Docker Compose** (integrado no Docker Desktop ou via plugin `docker compose`).

**Início rápido**
- Subir os containers:

```bash
docker compose up -d
```

- Verificar status:

```bash
docker compose ps
```

- Testar no navegador: acesse `http://localhost:8080` ou via terminal:

```bash
curl -v http://localhost:8080
```

**Arquitetura**
- **Load Balancer:** serviço `lb` usando a imagem `nginx:alpine`, expõe a porta `80` para a porta `8080` da máquina host.
- **Servidores Web:** `web1` e `web2` executando `nginx:alpine`, ambos servem o arquivo `web/index.html` via bind-mount.
- **Rede:** os serviços estão conectados via a network `lb-network` definida em `docker-compose.yml`.

**Arquivos importantes**
- **docker-compose.yml:** configuração dos serviços e rede. Veja [docker-compose.yml](docker-compose.yml)
- **nginx/nginx.conf:** configuração do upstream e regras de proxy do balanceador. Veja [nginx/nginx.conf](nginx/nginx.conf)
- **web/index.html:** página servida pelos servidores web. Veja [web/index.html](web/index.html)

**Solução de problemas comuns**
- Página em branco no `localhost:8080`:
	- Verifique se o arquivo `web/index.html` no host contém conteúdo (não esteja vazio).
	- Confirme que os containers estão rodando: `docker compose ps`.
	- Reinicie serviços após alterar arquivos bind-mounted: `docker compose restart web1 web2 lb`.
- Logs do Nginx (balanceador):

```bash
docker compose logs lb --tail 200
```

- Se o `index.html` foi salvo vazio por engano, restaure o conteúdo local e execute `docker compose restart`.

**Contribuição**
- Sinta-se à vontade para abrir issues, propor melhorias no `nginx.conf` (ex.: health checks, sticky sessions) ou adicionar conteúdo dinâmico aos servidores web.

**Licença**
- MIT — use à vontade para fins educacionais.

Obrigado — divirta-se testando balanceamento de carga!

