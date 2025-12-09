services:

  mysql:
    restart: unless-stopped
    image: mysql:8.0
    hostname: mysql
    volumes:
      - semaphore-mysql:/var/lib/mysql
    environment:
      MYSQL_RANDOM_ROOT_PASSWORD: "yes"
      MYSQL_DATABASE: semaphore
      MYSQL_USER: semaphore
      MYSQL_PASSWORD: semaphore

  semaphore:
    restart: unless-stopped
    image: semaphoreui/semaphore:latest
    ports:
      - "3000:3000"

    extra_hosts:
      - "host.docker.internal:host-gateway"

    environment:
      SEMAPHORE_DB_USER: semaphore
      SEMAPHORE_DB_PASS: semaphore
      SEMAPHORE_DB_HOST: mysql
      SEMAPHORE_DB_PORT: 3306
      SEMAPHORE_DB_DIALECT: mysql
      SEMAPHORE_DB: semaphore

      SEMAPHORE_PLAYBOOK_PATH: /tmp/semaphore/
      SEMAPHORE_ADMIN_PASSWORD: changeme
      SEMAPHORE_ADMIN_NAME: admin
      SEMAPHORE_ADMIN_EMAIL: admin@localhost
      SEMAPHORE_ADMIN: admin
      SEMAPHORE_LDAP_ACTIVATED: 'no'
      TZ: UTC

    depends_on:
      - mysql

volumes:
  semaphore-mysql:
