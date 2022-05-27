## Пример структуры репозитория используемый для базового бутстрапа кластеров k8s  

Список сервисов:  
- Kube prometheus stack
- Nginx Ingress Controller

Так же присутствуют дополнительные ресурсы:  
- Prometheus resources -> Pod monitor for Flux  

В файле infrastructure.yaml описаны все сервисы которые должны быть "задеплоены" в кластер  

При помощи директивы ```dependsOn``` мы управляем зависимостями  
В качестве хеслчека идет проверка всех ресурсов, для "более тонкой" настройки хеслчеков можно использовать директиву ```healthChecks```  

Общий порядок деплоя выглядит следующим образом:  

```bash
Kube prometheus stack -> Prometheus resources
                      -> Nginx Ingress Controller
```  

Для бутстрапа кластера можно выполнить следующий набор команд:  

```bash
export GITHUB_TOKEN=*********** #Token для доступа в Github, нужны права на запись в репозиторий  
flux bootstrap github --owner=REPO_OWNER --repository=REPO_NAME --path=FOLDER_PATH
```

Проверить результат можно следующий командой:  

```bash
flux get kustomizations
```

Ожидаемый вывод:  

```bash
NAME                    	REVISION    	SUSPENDED	READY	MESSAGE
flux-system             	main/69644c7	False    	True 	Applied revision: main/69644c7
nginx-ingress-controller	main/69644c7	False    	True 	Applied revision: main/69644c7
prometheus              	main/69644c7	False    	True 	Applied revision: main/69644c7
prometheus-resources    	main/69644c7	False    	True 	Applied revision: main/69644c7
```
