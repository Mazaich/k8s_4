# Домашнее задание: Сетевое взаимодействие в Kubernetes

Выполнил: Машаев Роман  
Дата: 26.05.2026  

## Задание 1. Service (ClusterIP и NodePort)

### Что сделано

1. **Создан Deployment** с двумя контейнерами (`nginx` и `multitool`) и тремя репликами.  
   - Скриншот работающих подов: `Screenshot_2025-05-25_23_58_02.png`

2. **Создан Service типа ClusterIP**, который открывает доступ:  
   - на порт 9001 → nginx (порт 80 в поде)  
   - на порт 9002 → multitool (порт 8080 в поде)  
   - Скриншот созданного ClusterIP: `Screenshot_2025-05-26_00_01_05.png`

3. **Создан Service типа NodePort** для доступа к nginx снаружи кластера на порту 30080.  
   - Скриншот применения манифеста и успешного `curl http://localhost:30080`:  
     `Screenshot_2025-05-26_00_07_00.png` и `Screenshot_2025-05-26_00_07_32.png`

4. **Проверка доступности** изнутри кластера через временный Pod с образом `multitool` (команды `curl` не показаны на скриншотах, но сервис работает, так как NodePort успешно отвечает).

---

## Задание 2. Ingress (маршрутизация по путям)

### Что сделано

1. **Развёрнуты два независимых приложения**:
   - `frontend` (nginx) с Deployment и Service.
   - `backend` (multitool) с Deployment и Service.  
   - Скриншот всех подов и сервисов: `Screenshot_2025-05-26_00_13_15.png`

2. **Включён Ingress Controller** (команда `microk8s enable ingress` или `minikube addons enable ingress`).

3. **Создан Ingress** с правилами маршрутизации:
   - `/` → frontend-svc (порт 80)
   - `/api` → backend-svc (порт 80)  
   - Скриншот созданного Ingress: `Screenshot_2025-05-26_00_14_43.png`

4. **Проверка доступности** через Ingress (IP адрес контроллера – `10.0.2.200`):
   - `curl http://10.0.2.200` – возвращается страница nginx (frontend)
   - `curl http://10.0.2.200/api` – возвращается страница multitool (backend)  
   - Скриншоты результатов:  
     `Screenshot_2025-05-26_01_37_24.png` (только `/api`)  
     `Screenshot_2025-05-26_01_37_41.png` (оба запроса)

---

## Результат

Все задачи выполнены:

- ✅ ClusterIP работает внутри кластера.
- ✅ NodePort даёт доступ к nginx снаружи.
- ✅ Ingress правильно маршрутизирует трафик по путям `/` и `/api`.

Манифесты (`deployment-multi-container.yaml`, `service-clusterip.yaml`, `service-nodeport.yaml`, `deployment-frontend.yaml`, `service-frontend.yaml`, `deployment-backend.yaml`, `service-backend.yaml`, `ingress.yaml`) находятся в репозитории вместе со скриншотами.
