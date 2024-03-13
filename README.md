## Getting Started with Elasticsearch and Kibana using Docker Compose

This guide walks you through setting up Elasticsearch and Kibana using Docker Compose. It provides a quick and convenient way to get these tools running locally for development and exploration.

**Prerequisites:**

- Docker installed: [https://www.docker.com/products/docker-desktop/](https://www.docker.com/products/docker-desktop/)
- Docker Compose installed: [https://docs.docker.com/compose/install/](https://docs.docker.com/compose/install/)

**Steps:**

1. **Update `.env` File (Optional):**

   This file can store environment variables for easier configuration management. You can define variables for:

   - `STACK_VERSION`: Version of Elasticsearch and Kibana (e.g., `8.4.0`)
   - `ES_PORT`: Port to expose Elasticsearch HTTP API (default: 9200)
   - `KIBANA_PORT`: Port to expose Kibana (default: 5601)
      ```
      STACK_VERSION=8.4.0
      ES_PORT=9200
      KIBANA_PORT=5601
      ```

2. **Start the Containers:**

   ```bash
   docker-compose up -d
   ```

   This command brings up both Elasticsearch (`es01`) and Kibana (`kib01`) containers in detached mode (-d).

3. **(Optional) Regenerate Enrollment Token:**

   If you've lost or need to replace the initial enrollment token (used for early access after enabling security), follow these steps:

   a. Access the running Elasticsearch container's shell:

      ```bash
      docker-compose exec -it es01 /bin/bash
      ```

   b. Regenerate the token using `elasticsearch-create-enrollment-token -s kibana`:

      ```bash
      elasticsearch-create-enrollment-token -s kibana
      ```

      This command generates a new token and prints it to the console. Copy and store it securely (e.g., password manager).

4. **Set Up Kibana Authentication:**

   **Warning:** Resetting the "elastic" user password is **not recommended** for production environments. Prioritize secure methods like API keys.

   Open Kibana in your web browser: `http://localhost:<kibana_port>` (replace `<kibana_port>` with your Kibana port, usually 5601).

   a. **If Enrollment Token is Used:**

      Enter the regenerated enrollment token (from step 4b) when prompted during login.

   b. **Recommended: Secure Authentication (API Key):**

      - Navigate to **Security** -> **Users** in Kibana.
      - Create a new user with an appropriate username and password.
      - Assign roles to the user that define their access level within Elasticsearch.
      - Generate an API key for the newly created user. Store the API key securely (e.g., environment variables).

      **Remember:** Don't utilize enrollment tokens for long-term access. Use the generated API key for subsequent Kibana connections.

   c. **Reset Password for elastic User**

    i. Access the running Elasticsearch container's shell:

    ```bash
      docker-compose exec -it es01 /bin/bash
    ```

    ii. Regenerate the token using `elasticsearch-reset-password -u elastic`:

    ```bash
    elasticsearch-reset-password -u elastic
    ```

2. **Stop the Containers:**
   ```bash
   docker compose down
   ```

   This command brings down both Elasticsearch (`es01`) and Kibana (`kib01`) containers.
