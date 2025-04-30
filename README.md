# n8n Deployment with Docker Compose and Traefik                                                                           
                                                                                                                           
This project provides a Docker Compose setup to deploy n8n, using Traefik as a reverse proxy and PostgreSQL as the database
backend. It's configured to use an external PostgreSQL database (like Neon.tech) and assumes HTTPS is handled externally   
(e.g., via Cloudflare DNS Proxy).                                                                                          
                                                                                                                           
## Prerequisites                                                                                                           
                                                                                                                           
*   **Docker:** Install Docker from [docker.com](https://www.docker.com/get-started).                                      
*   **Docker Compose:** Usually included with Docker Desktop. If not, follow the installation guide                        
[here](https://docs.docker.com/compose/install/).                                                                          
*   **Domain Name:** You need a domain name pointed to your server's IP address.                                           
*   **External PostgreSQL Database:** A running PostgreSQL instance accessible from your server (e.g., Neon.tech, AWS RDS).
                                                                                                                           
## Configuration                                                                                                           
                                                                                                                           
1.  **Clone the repository (if you haven't already):**                                                                     
    ```bash                                                                                                                
    git clone <your-repository-url>                                                                                        
    cd <repository-directory>                                                                                              
    ```                                                                                                                    
                                                                                                                           
2.  **Create the environment file:**                                                                                       
    Copy the example environment file to `.env`:                                                                           
    ```bash                                                                                                                
    cp .env.example .env                                                                                                   
    ```                                                                                                                    
                                                                                                                           
3.  **Edit `.env`:**                                                                                                       
    Open the `.env` file and replace the placeholder values with your actual configuration:                                
    *   `DOMAIN_NAME`: Your top-level domain (e.g., `example.com`).                                                        
    *   `SUBDOMAIN`: The subdomain for n8n (e.g., `n8n`, resulting in `n8n.example.com`).                                  
    *   `GENERIC_TIMEZONE`: Your timezone (e.g., `Europe/Amsterdam`).                                                      
good practice).                                                                                                            
    *   `DB_POSTGRESDB_HOST`: Hostname of your PostgreSQL database.                                                        
    *   `DB_POSTGRESDB_PORT`: Port of your PostgreSQL database (usually `5432`).                                           
    *   `POSTGRES_USER`: Username for your PostgreSQL database.                                                            
    *   `POSTGRES_PASSWORD`: Password for your PostgreSQL database.                                                        
    *   `POSTGRES_DB`: Name of the database to use for n8n.                                                                
                                                                                                                           
## Usage                                                                                                                   
                                                                                                                           
1.  **Start the services:**                                                                                                
    Run the following command in the project directory:                                                                    
    ```bash                                                                                                                
    docker-compose up -d                                                                                                   
    ```                                                                                                                    
    This will build (if necessary) and start the n8n and Traefik containers in detached mode.                              
                                                                                                                           
2.  **Access n8n:**                                                                                                        
    Open your web browser and navigate to `http://<SUBDOMAIN>.<DOMAIN_NAME>` (e.g., `http://n8n.example.com`). Traefik will
route the request to the n8n container.                                                                                    
    *Note: This setup expects HTTPS to be handled externally. If accessing directly via HTTP, ensure your external         
proxy/firewall allows it.*                                                                                                 
                                                                                                                           
3.  **Stop the services:**                                                                                                 
    To stop the containers, run:                                                                                           
    ```bash                                                                                                                
    docker-compose down                                                                                                    
    ```                                                                                                                    
    This stops and removes the containers but preserves the volumes (`n8n_data`, `puppeteer_data`). To remove volumes as   
well, use `docker-compose down -v`.                                                                                        
                                                                                                                           
## Components                                                                                                              
                                                                                                                           
*   **n8n:** The workflow automation tool. Data is persisted in the `n8n_data` volume. Puppeteer cache is stored in        
`puppeteer_data`.                                                                                                          
*   **Traefik:** A modern reverse proxy and load balancer. It automatically discovers the n8n service and routes traffic   
based on the hostname defined in the labels. The Traefik dashboard can be accessed (insecurely) for debugging if needed,   
typically via port 8080 if exposed, but it's not exposed by default in this configuration.                                 
*   **PostgreSQL:** The database backend for n8n. This configuration uses an *external* PostgreSQL database specified in   
the `.env` file.                                                                                                           
                                                                                                                           
## License                                                                                                                 
                                                                                                                           
Refer to the LICENSE file. 