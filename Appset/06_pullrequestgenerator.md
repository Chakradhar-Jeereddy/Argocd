## Pull request generator

- Discovers open pull requests in your source code provider (such as GitHub) repos.
- Generates new application and create dynamic environments for reviewing the pull reuest changes.
- After the pull request is merged or deleted, the application is deleted.

                  watch for pull requests                  deploy k8s resources
***Github***<================================***Argocd***===========================>***k8s***

Supported repositories
- GitHub
- Gittea
- Bitbucket Server
- More in future releases.

Filters
- Controls which pull requests you need to generate applications for.
- If no filters are specified, all pull requests will be processed.

***Example:***
  - branchMatch: "*-web". Any branch ends with web.
  - Adding prview label into PRs, and use it to generate application by this label only.

Parameters emited by pullrequest generator, can be used in application template
- number: The ID number of the pull request
- branch: The name of the branch of the pull request head.
- head_sha: This is the SHA of the head of the pull request.

Default pooling
- The ApplicationSet contoller polls every (defaulting every 30 minutes) to detect changes

How to reduce delay:
- This can be changed by updating requestAfterSeconds property
- or a better approach is to configure Argo CD webhook in git provider, each PR event such as created/closed or
- updated will trigger Argo CD web hook, then ArgoCD update changes accordingly.
