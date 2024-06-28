# Ejercicio
Configurar SonarQube utilizando Docker Compose, para esto necesitas dos servicios:
- Servicio: SonarQube
- Desde el host es necesario acceder a SonarQube por lo que necesitas mapear el puerto correspondiente.
- Servicio: PostgreSQL (existen otras opciones: Microsoft SQL Server, Oracle)
- Coloca un healtcheck para cada uno de los servicios.
- Los dos servicios deben pertenecer a uan red de tipo bridge
- Investiga cuáles son los volúmenes necesarios para cada servicio
- Investiga cuáles son las variables de entorno para que los servicios funcionen de manera adecuada.
  
# Una vez creado tu archivo .yaml realiza la respectiva prueba 
```
version: '3.8'

services:
  sonarqube:
    image: sonarqube:latest
    container_name: sonarqube-container
    environment:
      SONAR_JDBC_URL: jdbc:postgresql://db:5432/sonarqube
      SONAR_JDBC_USERNAME: sonarqube
      SONAR_JDBC_PASSWORD: sonarqube
    ports:
      - "9000:9000"  
    volumes:
      - sonarqube_data:/opt/sonarqube/data
      - sonarqube_logs:/opt/sonarqube/logs
      - sonarqube_extensions:/opt/sonarqube/extensions
    networks:
      - sonarqube-network
    depends_on:
      db:
        condition: service_healthy
    healthcheck:
      test: ["CMD", "curl", "-f", "http://localhost:9000"]
      interval: 30s
      timeout: 10s
      retries: 5
    restart: always

  db:
    image: postgres:latest
    container_name: postgres-container
    environment:
      POSTGRES_USER: sonarqube
      POSTGRES_PASSWORD: sonarqube
      POSTGRES_DB: sonarqube
    volumes:
      - postgresql_data:/var/lib/postgresql/data
    networks:
      - sonarqube-network
    healthcheck:
      test: ["CMD-SHELL", "pg_isready -U sonarqube"]
      interval: 30s
      timeout: 10s
      retries: 5
    restart: always

volumes:
  sonarqube_data:
  sonarqube_logs:
  sonarqube_extensions:
  postgresql_data:

networks:
  sonarqube-network:
    driver: bridge

```
# COMPLETAR CON UNA CAPTURA DE PANTALLA LUEGO DE EJECUTAR EL ARCHIVO
![image](https://github.com/estevan-j/2024A-ISWD633-Practica5/assets/94009206/5766cd5d-e960-4342-9184-b27ff4cceda8)

# ACCEDER A LOCALHOST:puertoDefinido para ingresar a SonarQube
![image](https://github.com/estevan-j/2024A-ISWD633-Practica5/assets/94009206/ea586be7-4f69-48e4-81ad-d94ff662a004)
