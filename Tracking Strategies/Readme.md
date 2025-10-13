### Tracking strategies
- There are several options to track manifests whether its in Git repos or helm repos.

- Git repos tracking:
* Commit SHA(good for production).
* Tags(good for production).
* Branch tracking(ex: main branch)
* Symbolic refrence(HEAD)

- Helm repos tracking: Helm always use semantic versioning
  * Sepecific version v1.2
  * Range 1.2* or >=1.2.0<1.3.0
  * Latest* or >=0.0.0

*** In application defination***
- When source is github
```
  source:
   path: guestbook
   repoURL: "https://github.com/mabusaa/argocd-course-apps-definitions.git"
   targetRevision: v1                        # asking to track specific tag
   targetRevision: 24455bb6                  # commit id
   targetRevision: main                      # branch
   targetRevision: HEAD                      # Symbolic refrence
```
- When source is helm
```
  source:
   path: guestbook
   repoURL: "https://github.com/mabusaa/argocd-course-apps-definitions.git"
   targetRevision: 1.16.1                    # asking to specific version
   targetRevision: '>3.0.0<4.1.0             # Recent version that is smaller than 4.1.0
   targetRevision: *                         # Latest version
```

