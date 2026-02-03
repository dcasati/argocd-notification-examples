# argocd-notification-examples
a handful examples of how to use ArgoCD Notifications


## What is ArgoCD Notifications?
ArgoCD Notifications is a built-in component that comes with ArgoCD. It:

- Monitors ArgoCD Application events (sync failures, health changes, etc.)
- Sends notifications to various channels (Slack, email, webhooks, etc.)
- Runs as a separate controller: argocd-notifications-controller

## What We Configured

1. ArgoCD Notifications Controller - Already installed with ArgoCD
  - Watches ArgoCD Applications for events
  - Configured via ConfigMap: argocd-notifications-cm
  - Credentials stored in Secret: argocd-notifications-secret

2. Triggers - When to send notifications

  ```yaml
  trigger.on-sync-failed: when sync fails
  trigger.on-health-degraded: when health becomes degraded
  ```
3. Templates - what to send
  ```yaml
  template.sync-failed-webhook: the JSON payload with error details
  ```
4. Services - where to send

  ```yaml
  service.webhook.github-webhook: GitHub repository_dispatch endpoint
  ```

## The Full Stack

```bash
ArgoCD Notifications Controller
    ↓ (detects failure)
    ↓ (triggers webhook)
GitHub repository_dispatch API
    ↓ (triggers workflow)
GitHub Actions
    ↓ (creates issue)
GitHub Issues
```
