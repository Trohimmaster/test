#!/usr/bin/env bash

# Configuration
GRAFANA_URL="http://grafana.example.com"    # Your Grafana instance URL
API_KEY="YOUR_API_KEY_HERE"                 # API key with Admin or Editor role
FOLDER_NAME="prod"                          # Folder name to export dashboards from
EXPORT_DIR="./dashboards"                   # Directory to save JSON files

mkdir -p "$EXPORT_DIR"

# 1. Get the folder ID by name
echo "Looking up folder '$FOLDER_NAME'..."
FOLDER_ID=$(curl -s -H "Authorization: Bearer $API_KEY" "$GRAFANA_URL/api/folders" \
  | jq -r ".[] | select(.title==\"$FOLDER_NAME\") | .id")

if [[ -z "$FOLDER_ID" ]]; then
  echo "Folder '$FOLDER_NAME' not found."
  exit 1
fi

echo "Found folder_id: $FOLDER_ID"

# 2. Get the list of dashboards in that folder (compact JSON objects, one per line)
echo "Fetching dashboards from folder..."
DASHBOARDS=$(curl -s -H "Authorization: Bearer $API_KEY" \
  "$GRAFANA_URL/api/search?folderIds=$FOLDER_ID&type=dash-db" \
  | jq -c '.[]')

if [[ -z "$DASHBOARDS" ]]; then
  echo "No dashboards found in folder '$FOLDER_NAME'."
  exit 0
fi

# 3. Export each dashboard to a JSON file
echo "Exporting dashboards..."
while read -r DASH; do
  DASH_UID=$(echo "$DASH" | jq -r '.uid')
  TITLE=$(echo "$DASH" | jq -r '.title' | tr ' ' '_' | tr -dc '[:alnum:]_')
  FILE="$EXPORT_DIR/${FOLDER_NAME}-${TITLE}.json"

  curl -s -H "Authorization: Bearer $API_KEY" \
    "$GRAFANA_URL/api/dashboards/uid/$DASH_UID" \
    | jq '.dashboard' > "$FILE" || {
      echo "Failed to download dashboard uid=${DASH_UID}"
      continue
    }

  echo "Saved: $FILE"
done <<< "$DASHBOARDS"

echo "Export complete."








{{ range .Alerts }}
<b>Status:</b> {{ .Status }}<br>
<b>Alert:</b> {{ .Labels.alertname }}<br>
<b>Env:</b> {{ .Labels.env }}<br>
<b>Host:</b> {{ .Labels.hostalias }}<br>
<b>Severity:</b> {{ .Labels.severity }}<br>

<b>Annotations:</b><br>
- {{ .Annotations.summary }}<br>
- {{ .Annotations.description }}<br>
- <a href="{{ .Annotations.dashboard }}">Open Dashboard</a><br>
<hr>
{{ end }}


<b>Dashboard:</b> <a href="{{ index .Annotations "dashboard" }}">{{ index .Annotations "dashboard" }}</a><br>












{{ range .Alerts }}
  {{ if eq .Status "firing" }}
    <div style="background-color:#FFA500; color:white; padding:8px; font-weight:bold; border-radius:5px;">
      🚨 Alert Firing
    </div>
  {{ else if eq .Status "resolved" }}
    <div style="background-color:#32CD32; color:white; padding:8px; font-weight:bold; border-radius:5px;">
      ✅ Alert Resolved
    </div>
  {{ end }}

  <br>

  <table style="border-collapse:collapse;">
    <tr><td><b>Alert:</b></td><td>{{ .Labels.alertname }}</td></tr>
    <tr><td><b>Env:</b></td><td>{{ .Labels.env }}</td></tr>
    <tr><td><b>Host:</b></td><td>{{ .Labels.hostalias }}</td></tr>
    <tr><td><b>Severity:</b></td><td>{{ .Labels.severity }}</td></tr>
  </table>

  <br>

  <b>Details:</b>
  <ul>
    {{ if .Annotations.summary }}<li>{{ .Annotations.summary }}</li>{{ end }}
    {{ if .Annotations.description }}<li>{{ .Annotations.description }}</li>{{ end }}
  </ul>

  {{ if .Annotations.dashboard }}
    <a href="{{ .Annotations.dashboard }}" style="display:inline-block; padding:6px 12px; background:#1f78c1; color:white; text-decoration:none; border-radius:4px;">
      🔎 Open Dashboard
    </a>
  {{ end }}

  <hr>
{{ end }}








{{ range .Alerts }}
Status: {{ .Status }} | Alert: {{ .Labels.alertname }}
{{ end }}






{{ range .Alerts }}
<b>Status:</b> {{ .Status }}<br>
<b>Alert:</b> {{ .Labels.alertname }}<br>
<hr>
{{ end }}










{{ range .Alerts }}
{{ if eq .Status "firing" }}
<div style="background:#FFA500;color:#fff;padding:8px;font-weight:bold;border-radius:5px;">
  🚨 Alert Firing
</div>
{{ else if eq .Status "resolved" }}
<div style="background:#2E8B57;color:#fff;padding:8px;font-weight:bold;border-radius:5px;">
  ✅ Alert Resolved
</div>
{{ else }}
<div style="background:#808080;color:#fff;padding:8px;font-weight:bold;border-radius:5px;">
  ℹ️ Alert {{ .Status }}
</div>
{{ end }}

<b>Alert:</b> {{ .Labels.alertname }}<br>
<b>Env:</b> {{ .Labels.env }}<br>
<b>Host:</b> {{ .Labels.hostalias }}<br>
<b>Severity:</b> {{ .Labels.severity }}<br>

{{ if .Annotations.summary }}<b>Summary:</b> {{ .Annotations.summary }}<br>{{ end }}
{{ if .Annotations.description }}<b>Description:</b> {{ .Annotations.description }}<br>{{ end }}
{{ if .Annotations.dashboard }}
<a href="{{ .Annotations.dashboard }}">🔎 Open Dashboard</a><br>
{{ end }}

<hr>
{{ end }}








{{ range .Alerts }}
{{ if eq .Status "firing" }}
<b>🚨 Alert Firing</b>
{{ else if eq .Status "resolved" }}
<b>✅ Alert Resolved</b>
{{ else }}
<b>ℹ️ Alert {{ .Status }}</b>
{{ end }}

<b>Alert:</b> {{ .Labels.alertname }}<br>
<b>Env:</b> {{ .Labels.env }}<br>
<b>Host:</b> {{ .Labels.hostalias }}<br>
<b>Severity:</b> {{ .Labels.severity }}<br>

{{ if .Annotations.summary }}<b>Summary:</b> {{ .Annotations.summary }}<br>{{ end }}
{{ if .Annotations.description }}<b>Description:</b> {{ .Annotations.description }}<br>{{ end }}
{{ if .Annotations.dashboard }}<a href="{{ .Annotations.dashboard }}">🔎 Open Dashboard</a><br>{{ end }}

<hr>
{{ end }}







    Created by Michal Bodansky (EXT), last modified about an hour ago

nginx setup

custom configuration file for nginx extending app definition. Destination is setup by gitlab.rb
gitlab.rb
nginx['custom_gitlab_server_config'] = "include /etc/gitlab/nginx/{{ endpoint_sub_path }}/*.conf;"


gitlab_gitaly_tls - it has ilb endpoint as alternative. the naming is misleading.
verify proxy_set_header setup so it correctly provide real ip address of client not haproxy ip 

location /-/{{ endpoint_sub_path }} {
    proxy_cache off;
 
    proxy_set_header       X-Forwarded-Host $host;
    proxy_set_header       X-Forwarded-Server $host;
    proxy_set_header       X-Forwarded-For $proxy_add_x_forwarded_for;
 
    proxy_ssl_certificate {{ gitlab_gitaly_tls.directory }}/{{ gitlab_gitaly_tls.filename }}.crt;
    proxy_ssl_certificate_key {{ gitlab_gitaly_tls.directory }}/{{ gitlab_gitaly_tls.filename }}.key;
 
    rewrite ^/-/{{ endpoint_sub_path }}(.*)$ $1 break;
    proxy_pass {{ ilb_dns }}:{{ endpoint_port }};
}
ilb setup

transparent load balancing. only tcp mode. no need to do tls termination 
frontend internal-{{ endpoint_sub_path }}-tcp-in
    bind *:{{ endpoint_port }}
    mode tcp
    option tcplog
    option clitcpka
 
    default_backend internal-{{ endpoint_sub_path }}-backend
 
backend internal-{{ endpoint_sub_path }}-backend
    mode tcp
    option tcp-check
    option srvtcpka
 
    server dev-sero-run1 {{ full_fqdn_target_server }}:{{ target_app_endpoint_port }} check
    server dev-sero-run2 {{ full_fqdn_target_server }}:{{ target_app_endpoint_port }} check
without primary dns

just configure ilb. nginx is only use in case you need to hide it behind primary dns.












