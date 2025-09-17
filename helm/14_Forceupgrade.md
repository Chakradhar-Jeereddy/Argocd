## Forceupgrade 
- Normally helm restarts only the pods whose config changed during upgrade.
- To force the restart of all pods of the deployment use --force option.
```
helm upgrade mydb bitnami/apache --force --set imagepullPolicy=never
```

## Cleanup on failure
- helm upgrade mydb bitnami/apache --force --set imagepullPolicy=test --cleanup-on-fail
