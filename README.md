# Setting Up this Local Dockerized Repository

# Pre-requisites

 1. **Docker**: Ensure Docker is installed and running on your machine.
 2. **MySQL 5.7 Container**: A container running MySQL 5.7, with a pre-existing schema called **deepdata** that is populated. This schema must include all tables, stored procedures, indexes, etc. You can find the SQL dump file here.

<img width="1152" alt="Screenshot 2024-05-28 at 5 46 36 PM (1)" src="https://github.com/micaelafrancisco13/sample-readme/assets/82253776/9a8ac4c4-ca74-46db-839a-371d9dcb2c62">

## Setting Up MySQL 5.7 Container

If you don't have the aforementioned container, open a terminal and run the script below, otherwise, run this MySQL 5.7 container then proceed with running the multi-container application.

    docker run -d \ 
    --name deepdata_db \ 
    -e MYSQL_ROOT_PASSWORD=root \ 
    -e MYSQL_DATABASE=deepdata \ 
    -v deepdata:/var/lib/mysql \ 
    -p 3307:3306 \ 
    --health-cmd="exit 0" \ 
    --restart unless-stopped \ 
    mysql:5.7

## Importing the *deepdata* Schema

 1. Download the SQL dump file here.
 2. Open MySQL Workbench and set up a new connection using the below connection details:
	 - Connection Name: **DeepData**
	 - Hostname: **127.0.0.1** 
	 - Port: **3307** 
	 - Username: **root** 
	 - Password: **root** 
	 Ensure your connection setup looks like the screenshot below:
	 some image
 3. Now, open the created connection and you should see the **deepdata** schema.
 4. To import the schema:
	 - Click "Server" located on the top bar -> "Data Import". 
	 - Select "Import from self-contained file" and choose the directory where the dump file is downloaded. 
	 - Select **deepdata** from the "Default Target Schema" dropdown. 
	 - Click "Start Import" at the bottom right of the screen.


# Running the multi-container application

## Installing Frontend Dependencies

In the root project, open a terminal in the **frontend** directory and run:

    npm install

Wait for the installation to complete.

## Building and Running Containers

Open a terminal in the root project directory and run:

    docker-compose up -d --build

This will take a few minutes. Wait for all containers in the multi-container app to be running.

## Configuring Keycloak

 1. Open http://localhost:8000 in your browser. 
 2. Log in with "admin" as both the username and password. 
 3. Navigate to the "DeepData" realm, then to "Clients" on the left sidebar. 
 4. Click "Import client".  
 5. Browse to the "realm-clients" directory inside the "keycloak" directory of the root project. 
 6. Select the "deepdata-spring-backend.json" file. 
 7. Wait for the import to complete.

## Running the Completion Module

 1. Go to the **deepdata_msa** repository. 
 2. Checkout the code revamp branch via Sourcetree or by running the following:

    git checkout develop 
    git pull origin 
    git checkout -b feature/DD-2780-code-base---upgrade-fe

	 Remove the *-b* flag if this branch already exists on your local machine.
 3. Run the **Completion** module as per the repository's instructions.

## Frontend Access

 1. Open http://localhost:5175 in your browser. 
 2. Log in using any federated user credentials: 
	 - For example, use **demo.operator@deepdata.com** as the email/username and **ganymede2019** as the password. 
 4. Upon successful login, you should be redirected to the main user interface.

## Functionality Check

 1. Click the "Get Pads" button in the UI. 
 2. Verify that no errors are displayed and the list of pad names from your database is shown.

Make sure the import of the Keycloak client and all the steps mentioned above are completed correctly.

# Conclusion

Following the above steps will ensure that your local Dockerized repository is set up and running smoothly.
