# Splunk App Navigation with `default.xml`

This guide explains how the Splunk app navigation (`default.xml`) relates to the dashboards stored in the app folders.

---

## 1. Location of `default.xml`

The file that defines the app's menu is located at:

```
$SPLUNK_HOME/etc/apps/<your_app_name>/default/data/ui/nav/default.xml
```

This XML file defines:
- App landing page
- Tabs and sub-tabs
- The display label for each dashboard

---

## 2. Example of a navigation file

```xml
<nav version="1.1">
  <view name="dashboard_home" default="true" label="RAP Home" />

  <collection label="Web">
    <view name="web_traffic_overview" label="Traffic Overview" />
    <view name="web_proxy_errors" label="Proxy Errors" />
  </collection>
</nav>
```

- `<collection>` = a top-level menu section
- `<view name="...">` = a dashboard reference
- `default="true"` = sets the landing page when app opens
- `label="..."` = UI label for the menu item

---

## 3. Where dashboards are stored

When you reference a view like:
```xml
<view name="web_traffic_overview" />
```

Splunk looks for:

- Studio dashboard:
  ```
  $SPLUNK_HOME/etc/apps/<your_app>/appserver/studio/web_traffic_overview.json
  ```

- Classic dashboard:
  ```
  $SPLUNK_HOME/etc/apps/<your_app>/local/data/ui/views/web_traffic_overview.xml
  ```

> The `name` in the view tag **must match** the filename (without extension).

---

## 4. Folder structure summary

```
splunk_rap/
├── default/
│   └── data/ui/nav/default.xml          # navigation structure
├── local/
│   └── data/ui/views/                   # classic dashboards (.xml)
└── appserver/studio/                    # studio dashboards (.json)
```

---

## 5. Tips for consistency

- When creating dashboards via the UI, set the ID to match what you'll use in `default.xml`
- Only use dashboards that exist, or the app UI may break (404)
- Use this command to reload app navigation without restart:
  ```bash
  $SPLUNK_HOME/bin/splunk refresh app <your_app>
  ```

---

## 6. Verifying dashboard IDs

You can:
- Check the filename in `views/` or `studio/`
- Go to **Settings > User interface > Views** and check the `Name` column
