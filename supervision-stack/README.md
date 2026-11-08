# Wazuh + GLPI + Zabbix with Docker Compose

## Prerequisites
- Docker + Docker Compose v2, ~6 GB free RAM
- Wazuh indexer needs (on the Docker host): `sudo sysctl -w vm.max_map_count=262144`
  (persist it in /etc/sysctl.conf)

## First start
1. Edit `.env` and change the GLPI / Zabbix passwords.
2. Generate Wazuh TLS certificates (once):
   `docker compose -f generate-wazuh-certs.yml run --rm generator`
3. `docker compose up -d --build`

## Access
| Tool    | URL                      | Default login                                  |
|---------|--------------------------|------------------------------------------------|
| GLPI    | http://localhost:8080    | web installer; DB host `glpi-db`, user/pass from .env. Then glpi / glpi |
| Zabbix  | http://localhost:8081    | Admin / zabbix                                 |
| Wazuh   | https://localhost:8443   | admin / SecretPassword                         |

Change every default login right after the first connection.

## Notes
- GLPI: delete `install/install.php` after setup (GLPI asks you to).
- Wazuh agents connect to ports 1514/1515 of the host.
- Changing Wazuh passwords requires regenerating the hashes in the indexer's
  internal_users.yml - follow the Wazuh docs, then update `.env` and wazuh.yml.
