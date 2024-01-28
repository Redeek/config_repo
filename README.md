## upewnienie się czy kubectl wykorzystuje właściwy context
## kubectl config get-context



## ustawienie context minikube na namespace argocd
## kubectl config set-context --current --namespace=argocd



## uruchomienie wszystkich potrzebnych repo
## argocd app create zad2-app --repo https://github.com/Redeek/config_repo --path . --dest-server https://kubernetes.default.svc --dest-namespace default



## sprawdzenie czy argocd widzi uruchomioną aplikację
## argocd app list
NAME             CLUSTER                         NAMESPACE  PROJECT  STATUS     HEALTH   SYNCPOLICY  CONDITIONS  REPO                                   PATH  TARGET
argocd/zad2-app  https://kubernetes.default.svc  default    default  OutOfSync  Missing  <none>      <none>      https://github.com/Redeek/config_repo  .~



## Wylisotwanie dostępnych workflow
## gh workflow list
Zad2 - GHAction example solution  active  83860872    


## uruchomienie workflow o podanym numerze
## gh workflow run 83860872
✓ Created workflow_dispatch event for gha_zad2.yml at main

To see runs for this workflow, try: gh run list --workflow=gha_zad2.yml


## Obejrzenie statusu workflow
## gh run watch
? Select a workflow run * Update index.html, Zad2 - GHAction example solution (main) 8s ago


Refreshing run status every 3 seconds. Press Ctrl+C to quit.

✓ main Zad2 - GHAction example solution · 7682520603
Triggered via workflow_dispatch about 1 minute ago

JOBS
✓ Build, tag and push Docker image to DockerHub in 15s (ID 20936934741)
  ✓ Set up job
  ✓ Check out the source_repo
  ✓ Docker metadata definitions
  ✓ QEMU set-up
  ✓ Buildx set-up
  ✓ Login to DockerHub
  ✓ Build and push Docker image
  ✓ Interjobs output declaration
  ✓ Post Build and push Docker image
  ✓ Post Login to DockerHub
  ✓ Post Buildx set-up
  ✓ Post Check out the source_repo
  ✓ Complete job
✓ Modify K8s deployment and configmaps manifests in 5s (ID 20936938841)
  ✓ Set up job
  ✓ Check out the config_repo
  ✓ Configure Git
  ✓ Modify the K8s manifests
  ✓ Commit and push to config repo
  ✓ Post Check out the config_repo
  ✓ Complete job

ANNOTATIONS
! Node.js 16 actions are deprecated. Please update the following actions to use Node.js 20: docker/build-push-action@v3. For more information see: https://github.blog/changelog/2023-09-22-github-actions-transitioning-from-node-16-to-node-20/.
Build, tag and push Docker image to DockerHub: .github#1


✓ Run Zad2 - GHAction example solution (7682520603) completed with 'success'


## Sprawdzenie zmian w aplikacji
## argocd app diff zad2-app --refresh

===== /ConfigMap default/zad2-cm ======
4c4
<     </h1>\n<p> \nImage: \nredek/zad2:sha-44a3a6c\n</p>\n\n</body>\n</html>"
---
>     </h1>\n<p> \nImage: \nredek/zad2:sha-0725a75\n</p>\n\n</body>\n</html>"

===== apps/Deployment default/zad2-app ======
145c145
<       - image: redek/zad2:sha-44a3a6c
---
>       - image: redek/zad2:sha-0725a75


## Sprawdzenie informacji oraz statusów plików
## argocd app get zad2-app
Name:               argocd/zad2-app
Project:            default
Server:             https://kubernetes.default.svc
Namespace:          default
URL:                https://127.0.0.1:8080/applications/zad2-app
Repo:               https://github.com/Redeek/config_repo
Target:             
Path:               .
SyncWindow:         Sync Allowed
Sync Policy:        <none>
Sync Status:        OutOfSync from  (c6a4277)
Health Status:      Degraded

GROUP              KIND        NAMESPACE  NAME          STATUS     HEALTH    HOOK  MESSAGE
                   ConfigMap   default    zad2-cm       OutOfSync                  configmap/zad2-cm created
                   Service     default    zad2-service  Synced     Healthy         service/zad2-service created
apps               Deployment  default    zad2-app      OutOfSync  Degraded        deployment.apps/zad2-app created
networking.k8s.io  Ingress     default    zad2-ingress  Synced     Healthy         ingress.networking.k8s.io/zad2-ingress created


## Synchronizacja danych aplikacji zad2-app
## argocd app sync zad2-app
TIMESTAMP                  GROUP                    KIND   NAMESPACE                  NAME    STATUS    HEALTH         HOOK  MESSAGE
2024-01-28T02:36:58+01:00                      ConfigMap     default               zad2-cm  OutOfSync                        
2024-01-28T02:36:58+01:00                        Service     default          zad2-service    Synced   Healthy               
2024-01-28T02:36:58+01:00   apps              Deployment     default              zad2-app  OutOfSync  Degraded              
2024-01-28T02:36:58+01:00  networking.k8s.io     Ingress     default          zad2-ingress    Synced   Healthy               
2024-01-28T02:36:59+01:00          ConfigMap     default               zad2-cm    Synced                       
2024-01-28T02:36:59+01:00  networking.k8s.io     Ingress     default          zad2-ingress    Synced   Healthy               ingress.networking.k8s.io/zad2-ingress unchanged
2024-01-28T02:36:59+01:00                      ConfigMap     default               zad2-cm    Synced                         configmap/zad2-cm configured
2024-01-28T02:36:59+01:00                        Service     default          zad2-service    Synced   Healthy               service/zad2-service configured
2024-01-28T02:36:59+01:00   apps              Deployment     default              zad2-app  OutOfSync  Degraded              deployment.apps/zad2-app configured
2024-01-28T02:36:59+01:00   apps  Deployment     default              zad2-app    Synced  Progressing              deployment.apps/zad2-app configured

Name:               argocd/zad2-app
Project:            default
Server:             https://kubernetes.default.svc
Namespace:          default
URL:                https://127.0.0.1:8080/applications/zad2-app
Repo:               https://github.com/Redeek/config_repo
Target:             
Path:               .
SyncWindow:         Sync Allowed
Sync Policy:        <none>
Sync Status:        Synced to  (c6a4277)
Health Status:      Progressing

Operation:          Sync
Sync Revision:      c6a42779fe05e2cfb353a274bbe838b8b474bf34
Phase:              Succeeded
Start:              2024-01-28 02:36:58 +0100 CET
Finished:           2024-01-28 02:36:59 +0100 CET
Duration:           1s
Message:            successfully synced (all tasks run)

GROUP              KIND        NAMESPACE  NAME          STATUS  HEALTH       HOOK  MESSAGE
                   ConfigMap   default    zad2-cm       Synced                     configmap/zad2-cm configured
                   Service     default    zad2-service  Synced  Healthy            service/zad2-service configured
apps               Deployment  default    zad2-app      Synced  Progressing        deployment.apps/zad2-app configured
networking.k8s.io  Ingress     default    zad2-ingress  Synced  Healthy            ingress.networking.k8s.io/zad2-ingress unchanged

## Otagowanie aplikacji własnym tagiem
## git tag -a "v1.1.1" -m "wersja ArgoCD"
## Spushowanie zmian (nowy tag) za pomocą gita
## git push origin v1.1.1
Enumerating objects: 1, done.
Counting objects: 100% (1/1), done.
Writing objects: 100% (1/1), 162 bytes | 162.00 KiB/s, done.
Total 1 (delta 0), reused 0 (delta 0), pack-reused 0
To https://github.com/Redeek/source_repo.git
 * [new tag]         v1.1.1 -> v1.1.1


## Sprawdzenie workflow
## gh run watch
? Select a workflow run * Update index.html, Zad2 - GHAction example solution (v1.1.1) 13s ago


Refreshing run status every 3 seconds. Press Ctrl+C to quit.

* v1.1.1 Zad2 - GHAction example solution · 7682569185
Triggered via push less than a minute ago

JOBS
✓ Build, tag and push Docker image to DockerHub in 21s (ID 20937038552)
Refreshing run status every 3 seconds. Press Ctrl+C to quit.

* v1.1.1 Zad2 - GHAction example solution · 7682569185
Triggered via push less than a minute ago

JOBS
✓ Build, tag and push Docker image to DockerHub in 21s (ID 20937038552)
  ✓ Set up job
  ✓ Check out the source_repo
  ✓ Docker metadata definitions
  ✓ QEMU set-up
  ✓ Buildx set-up
  ✓ Login to DockerHub
Refreshing run status every 3 seconds. Press Ctrl+C to quit.

✓ v1.1.1 Zad2 - GHAction example solution · 7682569185
Triggered via push less than a minute ago

JOBS
✓ Build, tag and push Docker image to DockerHub in 21s (ID 20937038552)
  ✓ Set up job
  ✓ Check out the source_repo
  ✓ Docker metadata definitions
  ✓ QEMU set-up
  ✓ Buildx set-up
  ✓ Login to DockerHub
  ✓ Build and push Docker image
  ✓ Interjobs output declaration
  ✓ Post Build and push Docker image
  ✓ Post Login to DockerHub
  ✓ Post Buildx set-up
  ✓ Post Check out the source_repo
  ✓ Complete job
✓ Modify K8s deployment and configmaps manifests in 3s (ID 20937044312)
  ✓ Set up job
  ✓ Check out the config_repo
  ✓ Configure Git
  ✓ Modify the K8s manifests
  ✓ Commit and push to config repo
  ✓ Post Check out the config_repo
  ✓ Complete job

ANNOTATIONS
! Node.js 16 actions are deprecated. Please update the following actions to use Node.js 20: docker/build-push-action@v3. For more information see: https://github.blog/changelog/2023-09-22-github-actions-transitioning-from-node-16-to-node-20/.
Build, tag and push Docker image to DockerHub: .github#1


✓ Run Zad2 - GHAction example solution (7682569185) completed with 'success'



## Sprawdzenie zmian w aplikacji zad2-app
## argocd app diff zad2-app --refresh

===== /ConfigMap default/zad2-cm ======
4c4
<     </h1>\n<p> \nImage: \nredek/zad2:sha-0725a75\n</p>\n\n</body>\n</html>"
---
>     </h1>\n<p> \nImage: \nredek/zad2:1.1.1\n</p>\n\n</body>\n</html>"

===== apps/Deployment default/zad2-app ======
145c145
<       - image: redek/zad2:sha-0725a75
---
>       - image: redek/zad2:1.1.1



## Synchronizacja danych
## argocd app sync zad2-app
TIMESTAMP                  GROUP                    KIND   NAMESPACE                  NAME    STATUS    HEALTH            HOOK  MESSAGE
2024-01-28T02:43:58+01:00                      ConfigMap     default               zad2-cm  OutOfSync                           
2024-01-28T02:43:58+01:00                        Service     default          zad2-service    Synced   Healthy                  
2024-01-28T02:43:58+01:00   apps              Deployment     default              zad2-app  OutOfSync  Progressing              
2024-01-28T02:43:58+01:00  networking.k8s.io     Ingress     default          zad2-ingress    Synced   Healthy                  
2024-01-28T02:43:58+01:00  networking.k8s.io     Ingress     default          zad2-ingress    Synced   Healthy                  ingress.networking.k8s.io/zad2-ingress unchanged
2024-01-28T02:43:58+01:00                      ConfigMap     default               zad2-cm  OutOfSync                           configmap/zad2-cm configured
2024-01-28T02:43:58+01:00                        Service     default          zad2-service    Synced   Healthy                  service/zad2-service configured
2024-01-28T02:43:58+01:00   apps              Deployment     default              zad2-app  OutOfSync  Progressing              deployment.apps/zad2-app configured
2024-01-28T02:43:58+01:00          ConfigMap     default               zad2-cm    Synced                       configmap/zad2-cm configured

Name:               argocd/zad2-app
Project:            default
Server:             https://kubernetes.default.svc
Namespace:          default
URL:                https://127.0.0.1:8080/applications/zad2-app
Repo:               https://github.com/Redeek/config_repo
Target:             
Path:               .
SyncWindow:         Sync Allowed
Sync Policy:        <none>
Sync Status:        Synced to  (be63d68)
Health Status:      Progressing

Operation:          Sync
Sync Revision:      be63d688df4590d09b355c8e2dd514289ddee655
Phase:              Succeeded
Start:              2024-01-28 02:43:58 +0100 CET
Finished:           2024-01-28 02:43:58 +0100 CET
Duration:           0s
Message:            successfully synced (all tasks run)

GROUP              KIND        NAMESPACE  NAME          STATUS  HEALTH       HOOK  MESSAGE
                   ConfigMap   default    zad2-cm       Synced                     configmap/zad2-cm configured
                   Service     default    zad2-service  Synced  Healthy            service/zad2-service configured
apps               Deployment  default    zad2-app      Synced  Progressing        deployment.apps/zad2-app configured
networking.k8s.io  Ingress     default    zad2-ingress  Synced  Healthy            ingress.networking.k8s.io/zad2-ingress unchanged



## Dodanie sekcji data do configmap ArgoCD by synchronizował dane co 2 sekundy
apiVersion: v1
kind: ConfigMap
metadata:
  annotations:
    kubectl.kubernetes.io/last-applied-configuration: |
      {"apiVersion":"v1","kind":"ConfigMap","metadata":{"annotations":{},"labels":{"app.kubernetes.io/name":"argocd-cm","app.kubernetes.io/part-of":"argocd"},"name":"argocd-cm","namespace":"argocd"}}
  creationTimestamp: "2024-01-26T19:42:33Z"
  labels:
    app.kubernetes.io/name: argocd-cm
    app.kubernetes.io/part-of: argocd
  name: argocd-cm         
  namespace: argocd
  resourceVersion: "1301"
  uid: df413f60-d7c5-4218-bc44-b50177d26cf1
data:
  application.config: |
    autoSyncInterval: 2s

## Od tego momentu nie potrzeba już wpisywać komendy "argocd app sync zad2-app" ponieważ jest to robione automatycznie przez operatora argoCD. Tak jak poniżej pokazano



gh workflow list
Zad2 - GHAction example solution  active  83860872
redek@redek:~/poprawka/source_repo$ gh workflow run 83860872
✓ Created workflow_dispatch event for gha_zad2.yml at main

To see runs for this workflow, try: gh run list --workflow=gha_zad2.yml
redek@redek:~/poprawka/source_repo$ gh run watch
? Select a workflow run * Update index.html, Zad2 - GHAction example solution (main) 16s ago


Refreshing run status every 3 seconds. Press Ctrl+C to quit.

* main Zad2 - GHAction example solution · 7682692453
Triggered via workflow_dispatch less than a minute ago

JOBS
✓ Build, tag and push Docker image to DockerHub in 13s (ID 20937268188)
Refreshing run status every 3 seconds. Press Ctrl+C to quit.

* main Zad2 - GHAction example solution · 7682692453
Triggered via workflow_dispatch less than a minute ago

JOBS
✓ Build, tag and push Docker image to DockerHub in 13s (ID 20937268188)
Refreshing run status every 3 seconds. Press Ctrl+C to quit.

* main Zad2 - GHAction example solution · 7682692453
Triggered via workflow_dispatch less than a minute ago

JOBS
✓ Build, tag and push Docker image to DockerHub in 13s (ID 20937268188)
  ✓ Set up job
  ✓ Check out the source_repo
  ✓ Docker metadata definitions
  ✓ QEMU set-up
  ✓ Buildx set-up
  ✓ Login to DockerHub
Refreshing run status every 3 seconds. Press Ctrl+C to quit.

* main Zad2 - GHAction example solution · 7682692453
Triggered via workflow_dispatch less than a minute ago

JOBS
✓ Build, tag and push Docker image to DockerHub in 13s (ID 20937268188)
  ✓ Set up job
  ✓ Check out the source_repo
  ✓ Docker metadata definitions
  ✓ QEMU set-up
  ✓ Buildx set-up
  ✓ Login to DockerHub
Refreshing run status every 3 seconds. Press Ctrl+C to quit.

* main Zad2 - GHAction example solution · 7682692453
Triggered via workflow_dispatch less than a minute ago

JOBS
✓ Build, tag and push Docker image to DockerHub in 13s (ID 20937268188)
  ✓ Set up job
  ✓ Check out the source_repo
  ✓ Docker metadata definitions
  ✓ QEMU set-up
  ✓ Buildx set-up
  ✓ Login to DockerHub
  ✓ Build and push Docker image
Refreshing run status every 3 seconds. Press Ctrl+C to quit.

✓ main Zad2 - GHAction example solution · 7682692453
Triggered via workflow_dispatch less than a minute ago

JOBS
✓ Build, tag and push Docker image to DockerHub in 13s (ID 20937268188)
  ✓ Set up job
  ✓ Check out the source_repo
  ✓ Docker metadata definitions
  ✓ QEMU set-up
  ✓ Buildx set-up
  ✓ Login to DockerHub
  ✓ Build and push Docker image
  ✓ Interjobs output declaration
  ✓ Post Build and push Docker image
  ✓ Post Login to DockerHub
  ✓ Post Buildx set-up
  ✓ Post Check out the source_repo
  ✓ Complete job
✓ Modify K8s deployment and configmaps manifests in 7s (ID 20937272441)
  ✓ Set up job
  ✓ Check out the config_repo
  ✓ Configure Git
  ✓ Modify the K8s manifests
  ✓ Commit and push to config repo
  ✓ Post Check out the config_repo
  ✓ Complete job

ANNOTATIONS
! Node.js 16 actions are deprecated. Please update the following actions to use Node.js 20: docker/build-push-action@v3. For more information see: https://github.blog/changelog/2023-09-22-github-actions-transitioning-from-node-16-to-node-20/.
Build, tag and push Docker image to DockerHub: .github#1


✓ Run Zad2 - GHAction example solution (7682692453) completed with 'success'
redek@redek:~/poprawka/source_repo$ argocd app get zad2-app
Name:               argocd/zad2-app
Project:            default
Server:             https://kubernetes.default.svc
Namespace:          default
URL:                https://127.0.0.1:8080/applications/zad2-app
Repo:               https://github.com/Redeek/config_repo
Target:             
Path:               .
SyncWindow:         Sync Allowed
Sync Policy:        <none>
Sync Status:        Synced to  (be63d68)
Health Status:      Degraded

GROUP              KIND        NAMESPACE  NAME          STATUS  HEALTH    HOOK  MESSAGE
                   ConfigMap   default    zad2-cm       Synced                  configmap/zad2-cm configured
                   Service     default    zad2-service  Synced  Healthy         service/zad2-service configured
apps               Deployment  default    zad2-app      Synced  Degraded        deployment.apps/zad2-app configured
networking.k8s.io  Ingress     default    zad2-ingress  Synced  Healthy         ingress.networking.k8s.io/zad2-ingress unchanged


