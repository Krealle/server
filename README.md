<h1>
  <img src="https://user-images.githubusercontent.com/4565223/54240739-2d6e0b00-451f-11e9-8473-d15e78914c9b.png" height="36" valign="bottom" /> WoWAnalyzer.com server
</h1>

> WoWAnalyzer is a tool to help you analyze and improve your World of Warcraft raiding performance through various relevant metrics and gameplay suggestions.

[https://wowanalyzer.com](https://wowanalyzer.com)

This repo containers the server hosting WoWAnalyzer.com. It does not include any analysis code, that can be found on [the main repository](https://github.com/WoWAnalyzer/WoWAnalyzer). This houses our internal API and login mechanisms, such as WCL and Battle.net API proxies and caches.

## Development environment

### Configuration

This is needed for both options. Copy `.env.local.example` (name it `.env.local`) and configure the API keys you're going to need. **You don't need to configure everything.**

- WCL API key can be found at the bottom of this page: https://www.warcraftlogs.com/profile
- Battle.net API client can be created here: https://develop.battle.net/access/clients
- GitHub OAuth can be configured here: https://github.com/settings/developers
- Patreon OAuth can be configured here: https://www.patreon.com/portal/registration/register-clients

Note if you change something in the `.env` files you may need to restart your development server if it's already running.

### Standard (recommended)
Use `docker-compose up -d database cache` to start the database and cache.

#### Dev server
1. Install dependencies: `pnpm`
2. Fire her up: `pnpm start`

Now the development server is available at http://localhost:3001. It will recompile automatically but not refresh.

### Combining with the SPA dev server

See [the main repository](https://github.com/WoWAnalyzer/WoWAnalyzer) for instructions on how to start the SPA development server. For the max development pleasure, run the SPA development like normally and change the server it uses to your server path. In the SPA root make a `.env.development.local` and add the line: `VITE_APP_SERVER_BASE=http://localhost:3001/`. Restart the SPA development server and it will now use your own server for API calls.

Alternaitvely if you want to test the proxy, you should use the standard environment. It will proxy the SPA development server by default. The Docker environment seems to be unable to connect to your SPA development server.

## Production environment

1. `docker build --tag wowanalyzer-server .`
2. `docker run wowanalyzer-server`

Now the production server is available at http://localhost:3001.

## Migrations

New migrations can be created with the `sequelize-cli` tool:

```bash
npx sequelize-cli migration:generate --name=<migration-name>
```

You can run the migrations with:

```bash
npm run migrate
```
