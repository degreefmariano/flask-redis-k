# Trabajo Integrador de Kurbenetes

## Descripción
En este trabajo integrador de Kubernetes, tomo como punto de partida lo entregado en la anterior actividad (Flask y Redis) como dice el enunciado.

Creo los archivos YAML necesarios para definir los recursos que se van a crear en Kubernetes.

- flask-app-deployment.yaml *(Deployment de Flask)*
- redis-deployment.yaml *(Deployment o StatefulSet para Redis)*
- postgres-statefulset.yaml *(StatefulSet para PostgreSQL)*
- flask-app-service.yaml *(Service para la aplicación Flask)*
- redis-service.yaml *(Service para Redis)*
- postgres-service.yaml *(Service para PostgreSQL)*

## Desarrollo

La infraestructura en Kubernetes se organiza en tres componentes principales: Redis, Aplicación Flask y PostgreSQL.

**Redis**: Se despliega como un StatefulSet para garantizar la persistencia de datos, y se expone mediante un Service ClusterIP en el puerto 6379 para que Flask pueda comunicarse con él.

**Aplicación Flask**: Desplegada como un Deployment para asegurar alta disponibilidad. Se expone mediante un Service LoadBalancer en el puerto 8000 para que los usuarios externos puedan acceder.

**PostgreSQL**: Se implementa como StatefulSet para mantener la persistencia de datos, y se comunica internamente a través de un Service en el puerto 5432.

Se utilizan Persistent Volume Claims (PVCs) para garantizar que tanto Redis como PostgreSQL tengan almacenamiento **persistente**. La conectividad entre los componentes se gestiona mediante Services, facilitando la comunicación sin la necesidad de gestionar manualmente IPs o puertos.

## Pasos para Desplegar

1. **Creamos y aplicamos los archivos YAML** para definir los Deployments, StatefulSets y Services.

2. **Verificamos el despliegue** usando kubectl get pods y kubectl get services.

3. **Acceso**: Flask será accesible externamente a través del LoadBalancer, mientras que Redis y PostgreSQL se manejan internamente.

## Mejora para Escalabilidad
Recomendaría utilizar bases de datos gestionadas en la nube (como Amazon RDS para PostgreSQL y ElastiCache para Redis) para reducir la carga operativa, simplificar la administración y asegurar escalabilidad automática y mayor tolerancia a fallos.


## Pasos para correr el proyecto en Kubernetes

1. Habilito Kubernetes en las configuraciones de Docker Desktop.

   En las preferencias de Docker Desktop (ubicada en la parte superior superior derecha), busca la sección de Kubernetes y activa la opción para habilitarlo. Luego, habilita los cambios y espera a que Docker Desktop se reinicie.


2. Verifico que el clúster de Kubernetes esté corriendo correctamente

   ```bash
   kubectl get nodes
   ```

3. Aplico todos los archivos YAML

   ```bash
      kubectl apply -f flask-app-deployment.yaml
      kubectl apply -f redis-deployment.yaml
      kubectl apply -f postgres-statefulset.yaml   
      kubectl apply -f flask-app-service.yaml
      kubectl apply -f redis-service.yaml
      kubectl apply -f postgres-service.yaml
   ```

4. Verifico el estado de los Pods y Services

   ```bash
      kubectl get pods
      kubectl get services
   ```

5. Accedo a la aplicación Flask

   ```bash
      curl http://localhost:8000
   ```

6. Escalo la aplicación Flask:

   ```bash
      kubectl scale deployment flask-app --replicas=3
   ```
   
- [Proyecto flask-redis-k en Github(https://github.com/degreefmariano/flask-redis-k).
