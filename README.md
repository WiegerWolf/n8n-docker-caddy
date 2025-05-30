# n8n with Caddy and PostgreSQL using Docker Compose                                                                                                                                                   
                                                                                                                                                                                                       
This project provides a Docker Compose setup to run [n8n](https://n8n.io/), a workflow automation tool, behind a [Caddy](https://caddyserver.com/) reverse proxy with automatic HTTPS, using a         
[PostgreSQL](https://www.postgresql.org/) database for persistence.                                                                                                                                    
                                                                                                                                                                                                       
## Prerequisites                                                                                                                                                                                       
                                                                                                                                                                                                       
*   [Docker](https://docs.docker.com/get-docker/)                                                                                                                                                      
*   [Docker Compose](https://docs.docker.com/compose/install/)                                                                                                                                         
                                                                                                                                                                                                       
## Configuration                                                                                                                                                                                       
                                                                                                                                                                                                       
1.  **Copy the example environment file:**                                                                                                                                                             
    
    ```bash                                                                                                                                                                                            
    cp .env.example .env                                                                                                                                                                               
    ```

2.  **Edit the `.env` file:**                                                                                                                                                                          
    Open the `.env` file in a text editor and replace the placeholder values with your actual configuration:                                                                                           
    *   `DOMAIN_NAME`: Your top-level domain (e.g., `example.com`).                                                                                                                                    
    *   `SUBDOMAIN`: The subdomain for n8n (e.g., `n8n`, resulting in `n8n.example.com`).                                                                                                              
    *   `GENERIC_TIMEZONE`: Your preferred timezone (e.g., `Europe/Berlin`). Defaults to `America/New_York` if commented out or removed.                                                               
    *   `SSL_EMAIL`: The email address for Let's Encrypt registration.                                                                                                                                 
    *   `DB_POSTGRESDB_HOST`: Your PostgreSQL database host.                                                                                                                                           
    *   `DB_POSTGRESDB_PORT`: Your PostgreSQL database port (usually `5432`).                                                                                                                          
    *   `DB_POSTGRESDB_USER`: Your PostgreSQL database username.                                                                                                                                       
    *   `DB_POSTGRESDB_PASSWORD`: Your PostgreSQL database password.                                                                                                                                   
    *   `DB_POSTGRESDB_DATABASE`: The name of the PostgreSQL database for n8n.                                                                                                                         
                                                                                                                                                                                                       
    *Note:* The example `.env` file includes placeholders suitable for a database hosted on [Neon.tech](https://neon.tech/), but you can use any PostgreSQL provider. Ensure                           
`DB_POSTGRESDB_SSL_ENABLED=true` is set correctly based on your database provider's requirements (it's enabled by default in `docker-compose.yml` for Neon).                                           
                                                                                                                                                                                                       
## Usage                                                                                                                                                                                               
                                                                                                                                                                                                       
1.  **Start the services:**                                                                                                                                                                            
    Run the following command in the project's root directory:                                                                                                                                         
    
    ```bash                                                                                                                                                                                            
    docker-compose up -d                                                                                                                                                                               
    ```                                                                                                                                                                                                
    
    This will build (if necessary) and start the `n8n` and `caddy` services in detached mode.                                                                                                          
                                                                                                                                                                                                       
2.  **Access n8n:**                                                                                                                                                                                    
    Once the containers are running, you can access your n8n instance in your web browser at `https://<SUBDOMAIN>.<DOMAIN_NAME>` (e.g., `https://n8n.example.com`).                                    
                                                                                                                                                                                                       
3.  **Stop the services:**                                                                                                                                                                             
    To stop the running containers:  

    ```bash                                                                                                                                                                                            
    docker-compose down                                                                                                                                                                                
    ```                                                                                                                                                                                                
