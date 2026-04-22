# audiovisual-koa

Site institucional **KOA Audiovisual** — produção audiovisual em Santa Catarina.

- **Live**: https://audiovisual.koaworld.org
- **Cliente**: Guzz Secco (KOA)
- **Hospedagem**: servidor Guzz (Oracle sa-vinhedo-1, Docker Swarm) — stack `audiovisual-koa` em `/opt/stacks/audiovisual-koa/`
- **Serve**: `nginx:alpine` bind-mount de `site/` em `/usr/share/nginx/html`
- **TLS**: Traefik + Let's Encrypt (HTTP-01, cert resolver `letsencrypt`)
- **DNS**: Cloudflare (zone `koaworld.org`, proxied)

## Estrutura

```
site/
├─ index.html           # SPA single-page
├─ favicon.png
├─ koa-logo.webp
├─ robots.txt
├─ sitemap.xml
└─ depoimentos/         # mídia: vídeos MP4 + áudios OGG
   ├─ video1.mp4
   ├─ video2.mp4
   ├─ sitio1.ogg
   ├─ sitio2.ogg
   ├─ sitio3.ogg
   └─ greenbarn.ogg
```

## Rodar local

```bash
cd site/
python3 -m http.server 8080
# abre http://localhost:8080
```

## Deploy

O servidor Guzz tem `/opt/stacks/audiovisual-koa/repo/` como clone desse repo; o volume bind-mount aponta pra `repo/site`. Para atualizar:

```bash
ssh guzz
cd /opt/stacks/audiovisual-koa/repo
sudo git pull
# nginx serve o volume bind-mount em tempo real — não precisa restart
```

Se trocar imagem base ou labels do Traefik, redeployar a stack:

```bash
cd /opt/stacks/audiovisual-koa
sudo docker stack deploy -c docker-compose.yml audiovisual-koa
```

## Tapa melhorias aplicadas (2026-04-22)

Guzz liberou "caso veja que pode dar um tapa a mais no site fique a vontade" — aplicado apenas o que não altera copy visível:

- Open Graph (og:title/description/image/url/site_name/locale) — link rico em WhatsApp/Facebook/LinkedIn
- Twitter Card (summary_large_image)
- Canonical URL + `robots` index/follow + `theme-color`
- Apple touch icon
- `robots.txt` + `sitemap.xml` mínimo

Copy, seções, vídeos e áudios permanecem exatamente como enviados.
