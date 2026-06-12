---
name: add-slack-annotation
description: Add or update the Slack channel annotation in a plugin's catalog-info.yaml or service-info.yaml descriptor file. Sets the value to a specified Slack channel (default: soundcheck-users).
---

# Add Slack Channel Annotation

This skill adds a Slack channel reference to a plugin's descriptor file — either `catalog-info.yaml` or `service-info.yaml`.

## Steps

1. **Identify the target.** Accept a component name or path from the user. If none provided, ask which component to update.

2. **Locate `catalog-info.yaml`.** Search for the file in the plugin directory.

3. **If `catalog-info.yaml` exists**, read it and apply the annotation:

   - **`annotations` block exists and already has `spotify.net/slack-channel`:** Update the value to the requested channel (default `soundcheck-users`).
   - **`annotations` block exists but lacks `spotify.net/slack-channel`:** Add `spotify.net/slack-channel: <channel>` under the existing `annotations` key.
   - **No `annotations` block exists:** Add an `annotations` block under `metadata` with `spotify.net/slack-channel: <channel>`.

4. **If `catalog-info.yaml` is NOT found**, fall back to `service-info.yaml`:

   a. Locate and read `service-info.yaml` in the plugin/service directory.
   b. Check whether a `facts` block exists.
   c. Apply the channel. There are three cases:
      - **`facts` block exists and already has `slack_channel`:** Update the value to the requested channel (default `soundcheck-users`).
      - **`facts` block exists but lacks `slack_channel`:** Add `slack_channel: <channel>` under the existing `facts` key.
      - **No `facts` block exists:** Add a `facts` block with `slack_channel: <channel>`.

5. **Preserve YAML formatting.** Use the Edit tool for surgical edits — do not rewrite the entire file. Keep existing indentation.

6. **Verify.** Read the file back and confirm the annotation/fact is present and the YAML is well-formed.

## Example Usage

/add-slack-annotation my-plugin
/add-slack-annotation my-plugin --channel my-team-channel

## Example Result — catalog-info.yaml

```yaml
apiVersion: backstage.io/v1alpha1
kind: Component
metadata:
 name: internal-plugin-my-plugin
 title: '@internal/plugin-my-plugin'
 annotations:
   spotify.net/slack-channel: soundcheck-users
spec:
 lifecycle: production
 type: backstage-frontend-plugin
 owner: my-team
```

Example Result — service-info.yaml (fallback)

```yaml
id: a-la-carte
description: One Check Emitter to rule them all. Does both scheduled and responsive checks.
system: soundcheck
owner: toast-infra
visibility: private
dependencies:
 - blanket
 - data-endpoints-service

facts:
 service_discovery: [a-la-carte]
 role: alacarte
 lifecycle: production
 component_type: service
 slack_channel: soundcheck-users
 reliability_tier: 3
```
