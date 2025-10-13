### Drawbacks of traditional deployments
Manual 'kubectl' commands (hard to manage with this approach when managing multiple clusters)
No Audit trails ( Dificult to understand who deployed the changes)
Deployment Drift (cluster is running with replica 5 and at source control it is 3)
Multi-cluster complexity (need to connect each cluster and deploy)
Show rollbacks (other doesn't know what changes made)

### How argo helps
Automates syncing with Git for consistent deployments
Provide audit trails via Git history
Detects and correct drifts automatically
Simplefies multi-cluster management
Enables faster rollback with get revert

Workflow
```
Git====================>Arogcd====================>Kubernetes
    pull changes                 sync changes

