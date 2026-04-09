# README_ORACLE

Base enxuta para Oracle Linux ou Ubuntu, rodando sem Docker no fluxo principal.

## Objetivo

- Node direto
- Baileys
- multi-instancia sem limite artificial de codigo
- QR disponivel ate ser substituido por QR novo, conexao aberta, logout explicito ou expiracao real
- persistencia obrigatoria de instancias, contatos, chats, historico, mensagens novas e updates

## Requisitos

- Node 20
- npm 10+
- PostgreSQL ou MySQL acessivel
- systemd

## Arquivos oficiais para Oracle

- `.env.oracle.example`
- `systemd/evolution.service`

## Fluxo oficial de producao

```bash
npm ci
npm run build
npm run db:generate
npm run db:deploy
npm run start:prod
```

## Setup rapido

```bash
cp .env.oracle.example .env
mkdir -p instances store store/auth
npm ci
npm run build
npm run db:generate
npm run db:deploy
```

## Start manual

```bash
npm run start:prod
```

## systemd

```bash
sudo mkdir -p /opt/evolution
sudo rsync -av --delete ./ /opt/evolution/
cd /opt/evolution
cp .env.oracle.example .env
npm ci
npm run build
npm run db:generate
npm run db:deploy
sudo cp systemd/evolution.service /etc/systemd/system/evolution.service
sudo systemctl daemon-reload
sudo systemctl enable evolution
sudo systemctl start evolution
sudo systemctl status evolution
```

## Observacoes operacionais

- `PROVIDER_ENABLED=false` no fluxo Oracle para nao depender de file server externo.
- `CACHE_REDIS_ENABLED=false` por padrao no exemplo Oracle.
- `EVENT_EMITTER_MAX_LISTENERS=0` remove teto artificial de listeners no runtime.
- `QRCODE_LIMIT=0` desliga o limite artificial de geracao de QR.
- Os dados obrigatorios do cliente continuam persistidos pelo banco e pelos artefatos locais de auth em `instances/`.

## Update de deploy

```bash
cd /opt/evolution
git pull
npm ci
npm run build
npm run db:generate
npm run db:deploy
sudo systemctl restart evolution
```
