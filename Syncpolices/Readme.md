```
syncPolicy:
        automated: {}
         prune: true
        syncOptions:
          - CreateNamespace=true
          - ApplyOutofSyncOnly=true
          - Replace=true
          - FailOnSharedResource=true  #only at application
annotations:          #Resource level
 argocd.argoproj.io/sync-options: PruneLast=true
 argocd.argoproj.io/sync-options: Prune=false
 argocd.argoproj.io/sync-options: Replace=true
```
***Automated sync***
- My defualt Argocd polls git repository every 3 minutes to detect changes to the manifests.
- It automatically sync if any commits happens in git
- CI/CD pipelines no longer need direct access.

***Conditions***
- Autosync only performed when the application is outofsync.
- Autosync will not reattempt if pervious sync for same commit-sha failed.
- Rollback cannot be peroformed for an application when autosync is enabled.

***Example***
argocd app history <app-name>
argocd app rollback <app-name> <revision-number>
git revert <bad-commit-sha>
git push

| Method                        | Steps                                               | Auto-sync reaction                | Best use case               |
| ----------------------------- | --------------------------------------------------- | --------------------------------- | --------------------------- |
| Revert in Git                 | `git revert` → push                                 | Auto-sync applies automatically   | Always recommended          |
| Temporarily disable auto-sync | `app set --sync-policy none` → rollback → re-enable | Manual rollback allowed           | Emergency rollback          |
| Change `targetRevision`       | Update to old commit/tag                            | Auto-sync switches to that commit | Controlled version rollback |

***Automatic pruning***
Default - no prune: 
For safety reason, when a resouse is deleted in git, the resource wont be deleted in cluster through autosync.
Pruning can be enabled to delete resources automatically as part of the automated sync.

***Automatic selfhealing***
Autosync apply changes from git to cluster
selfhealing detects any manual changes in cluster and overwrites it with those in git.

***Sync Options at application and resorce level***
Sync options are controlled at application level and at resource level
1) validation=false, will not validate resource manifests, wont do --dry-run, no checks on existance of CRDs and CRs, no schema checks.
2) Prune: false or application level or resource level using anotations
3) Selectivesync, ApplyOutofSyncOnly=true, it saves time when you have many resource manifest, if not set argocd will sync all manifests.
4) PruneLast=true using anotations, those resources will be deleted at last.
