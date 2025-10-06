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

















