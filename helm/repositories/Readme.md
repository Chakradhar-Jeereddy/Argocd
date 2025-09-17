## Setting up local repository
- Create a new folder chartsrepo
  ```
  mkdir chartsrepo
  ```
- The repo should have index.yaml file
  ```
  helm repo index chartsrepo/
  ```
- Add first chart to the repository
 ```
 helm package myfirstchart -d chartsrepo
 helm repo index chartsrepo
 ```

  
- 
