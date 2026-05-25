# Домашнее задание: Сетевое взаимодействие в Kubernetes

**Выполнил:** Машаев Роман  
**Дата:** 26.05.2026  

---

## Задание 1. Service (ClusterIP и NodePort)

### Что сделано

1. **Создан Deployment** с двумя контейнерами (`nginx` и `multitool`) и тремя репликами.  
   ![Работающие поды](task1/Screenshot_2026-05-25_23_58_02.png)

2. **Создан Service типа ClusterIP**, открывающий доступ:  
   - порт 9001 → nginx  
   - порт 9002 → multitool  
   ![ClusterIP сервис](task1/Screenshot_2026-05-26_00_01_05.png)

3. **Создан Service типа NodePort** для доступа к nginx снаружи на порту 30080.  
   ![Применение NodePort и curl](task1/Screenshot_2026-05-26_00_07_00.png)  
   ![Проверка через curl localhost:30080](task1/Screenshot_2026-05-26_00_07_32.png)

4. **Проверена доступность** изнутри кластера (сервис работает, NodePort отвечает).

---

## Задание 2. Ingress (маршрутизация по путям)

### Что сделано

1. **Развёрнуты два независимых приложения**:  
   - `frontend` (nginx)  
   - `backend` (multitool)  
   ![Все поды и сервисы](task2/Screenshot_2026-05-26_00_13_15.png)

2. **Включён Ingress Controller** (`microk8s enable ingress`).

3. **Создан Ingress** с правилами:  
   - `/` → frontend  
   - `/api` → backend  
   ![Ingress создан](task2/Screenshot_2026-05-26_00_14_43.png)

4. **Проверен доступ через Ingress** (IP контроллера `10.0.2.200`):  
   - `curl http://10.0.2.200` → страница nginx  
   - `curl http://10.0.2.200/api` → страница multitool  
   ![Только /api](task2/Screenshot_2026-05-26_01_37_24.png)  
   ![Оба запроса](task2/Screenshot_2026-05-26_01_37_41.png)

---

## Результат

✅ ClusterIP работает внутри кластера  
✅ NodePort даёт доступ к nginx снаружи  
✅ Ingress правильно маршрутизирует трафик по путям `/` и `/api`

**Манифесты** находятся в папках `task1/` и `task2/` репозитория.
