---
name: add-slack-annotation
description: Add or update the "spotify.net/slack-channel" annotation in a plugin's catalog-info.yaml descriptor file. Sets the value to a specified Slack channel (default: soundcheck-users).
---

# Add Slack Channel Annotation

This skill adds the `spotify.net/slack-channel` annotation to a plugin's `catalog-info.yaml` file.

## Steps

1. **Identify the target.** Accept a plugin name or path from the user. If none provided, ask which plugin to update.

2. **Locate the `catalog-info.yaml`.

3. **Read the file** and check whether a `metadata.annotations` block already exists.

4. **Apply the annotation.** There are three cases:

   - **`annotations` block exists and already has `spotify.net/slack-channel`:** Update the value to the requested channel (default `soundcheck-users`).
   - **`annotations` block exists but lacks `spotify.net/slack-channel`:** Add `spotify.net/slack-channel: <channel>` under the existing `annotations` key.
   - **No `annotations` block exists:** Add an `annotations` block under `metadata` with `spotify.net/slack-channel: <channel>`.

5. **Preserve YAML formatting.** Use the Edit tool for surgical edits — do not rewrite the entire file. Keep existing indentation.

6. **Verify.** Read the file back and confirm the annotation is present and the YAML is well-formed.

## Example Usage

```
/add-slack-annotation my-plugin
/add-slack-annotation my-plugin --channel my-team-channel
```

## Example Result

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
