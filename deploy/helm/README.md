## Helm

To test charts using Helm use the below commands

```
helm upgrade --install --dry-run bede-devopsdashboard-api ./apps/. \
  --values ./apps/bede-devopsdashboard-api.yaml \
  --set db.host="en1bdess01sdb.database.windows.net" \
  --set db.name="bde_ss01.devopsdashboard" \
  --set ingress.domain="aks.ss02.bde.bedegaming.com"

helm upgrade --install --dry-run bede-devopsdashboard-web ./apps/. \
  --values ./apps/bede-devopsdashboard-web.yaml \
  --set db.host="en1bdess01sdb.database.windows.net" \
  --set db.name="bde_ss01.devopsdashboard" \
  --set ingress.domain="aks.ss02.bde.bedegaming.com"

helm upgrade --install --dry-run bede-devopsdashboard-codefresh ./cronjobs/. \
  --values ./cronjobs/bede-devopsdashboard-codefresh.yaml \
  --set db.host="en1bdess01sdb.database.windows.net" \
  --set db.name="bde_ss01.devopsdashboard" \

helm upgrade --install --dry-run bede-devopsdashboard-od ./cronjobs/. \
  --values ./cronjobs/bede-devopsdashboard-od.yaml \
  --set db.host="en1bdess01sdb.database.windows.net" \
  --set db.name="bde_ss01.devopsdashboard" \
  ```

## Argo CD

The below commands can be used if for whatever reason the apps need to be recreated in Argo CD

- Authenticate with Argo in SS02 using `argocd login 10.33.255.250`

- Create applications

```
argocd app create bede-devopsdashboard-api \
--repo "https://github.com/BedeGaming/Bede.DevOpsDashboard.git" \
--path "deploy/helm/apps" \
--values bede-devopsdashboard-api.yaml \
--dest-server "https://kubernetes.default.svc" \
--dest-namespace "codefresh" \
-p db.host="en1bdess01sdb.database.windows.net" \
-p db.name="bde_ss01.devopsdashboard" \
-p ingress.domain="aks.ss02.bde.bedegaming.com"

argocd app create bede-devopsdashboard-web \
--repo "https://github.com/BedeGaming/Bede.DevOpsDashboard.git" \
--path "deploy/helm/apps" \
--values bede-devopsdashboard-web.yaml \
--dest-server "https://kubernetes.default.svc" \
--dest-namespace "codefresh" \
-p db.host="een1bdess01sdb.database.windows.net" \
-p db.name="bde_ss01.devopsdashboard" \
-p ingress.domain="aks.ss02.bde.bedegaming.com"

argocd app create bede-devopsdashboard-codefresh \
--repo "https://github.com/BedeGaming/Bede.DevOpsDashboard.git" \
--path "deploy/helm/cronjobs" \
--values bede-devopsdashboard-codefresh.yaml \
--dest-server "https://kubernetes.default.svc" \
--dest-namespace "codefresh" \
-p db.host="en1bdess01sdb.database.windows.net" \
-p db.name="bde_ss01.devopsdashboard"
-p codefresh.pipeline_name="Terraform/TerraformChildApply" \
-p codefresh.build_limit="30"

argocd app create bede-devopsdashboard-od \
--repo "https://github.com/BedeGaming/Bede.DevOpsDashboard.git" \
--path "deploy/helm/cronjobs" \
--values bede-devopsdashboard-od.yaml \
--dest-server "https://kubernetes.default.svc" \
--dest-namespace "codefresh" \
-p db.host="en1bdess01sdb.database.windows.net" \
-p db.name="bde_ss01.devopsdashboard"
```