# Docker Compose Setup for WordPress with Custom Theme Removal

This repository contains a Docker Compose configuration to set up a WordPress site with a MySQL database. It includes a custom command to remove specific themes from the WordPress installation.

## Prerequisites

- Docker
- Docker Compose

## Getting Started

1. **Clone the repository:**

   ```bash
   git clone https://github.com/your-username/your-repo.git
   cd your-repo
   ```

2. **Create a `.env` file:**

   Create a `.env` file in the root directory with the following environment variables:

   ```env
   MYSQL_HOST=database
   MYSQL_DATABASE=your_database
   MYSQL_USER=your_user
   MYSQL_PASSWORD=your_password
   MYSQL_ROOT_PASSWORD=your_root_password
   ```

3. **Run Docker Compose:**

   ```bash
   docker-compose up -d
   ```

   This command starts the WordPress and MySQL containers in detached mode.

## Configuration Details

### WordPress Service

- **Image:** Official WordPress Docker image (`wordpress:latest`).
- **Ports:** Maps container port 80 to host port 8000.
- **Environment Variables:**
  - `WORDPRESS_DB_*`: Configures WordPress to connect to the MySQL database.
  - `WORDPRESS_CONFIG_EXTRA`: Defines custom WordPress configurations.
- **Volumes:**
  - `./wp/wp-content:/var/www/html/wp-content`: Maps the `wp-content` directory for persistence.
  - `wordpress-data:/var/www/html`: Optionally maps the entire WordPress directory for persistence.
- **Custom Command:** Removes themes starting with `twenty` from `wp-content/themes/` before starting the server:

  ```yaml
  command: sh -c "rm -rf /var/www/html/wp-content/themes/twenty* && docker-entrypoint.sh apache2-foreground"
  ```

### MySQL Service

- **Image:** Latest official MySQL Docker image (`mysql:latest`).
- **Ports:** Maps container port 3306 to host port 3306.
- **Environment Variables:**
  - Configures MySQL using environment variables from the `.env` file.
- **Volumes:** `mysql-data:/var/lib/mysql`: Maps the MySQL data directory for persistence.

## Volumes

- **mysql-data:** Named volume for MySQL data.
- **wordpress-data:** Named volume for WordPress data.

## License

This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for details.
