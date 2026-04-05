# my-minecraft-server Helm Charts

## Structure

my-minecraft-server/
├── Chart.yaml              # Umbrella chart
├── values.yaml             # Top-level overrides
└── charts/
    ├── minecraft/          # Minecraft server + PVC + service
    ├── exporter/           # Minecraft Prometheus exporter
    └── monitoring/         # Prometheus + Grafana (as dependencies)

## First-time install

### 1. Uninstall existing releases
helm uninstall my-grafana -n monitoring
helm uninstall prometheus -n monitoring

### 2. Update monitoring dependencies

helm dependency update charts/monitoring

### 3. Install everything

# Minecraft + exporter
helm upgrade --install minecraft charts/minecraft
helm upgrade --install exporter charts/exporter

# Monitoring in monitoring namespace
helm upgrade --install monitoring charts/monitoring -n monitoring --create-namespace


## Upgrading
helm upgrade minecraft charts/minecraft
helm upgrade exporter charts/exporter
helm upgrade monitoring charts/monitoring -n monitoring

## Customising

Override any value without editing files:

helm upgrade minecraft charts/minecraft --set rcon.password=mynewpassword

Or edit the relevant `values.yaml` and re-run the upgrade command.