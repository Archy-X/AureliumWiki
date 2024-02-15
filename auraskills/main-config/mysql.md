---
description: Guide to setting up an configuring MySQL for storage
---

# MySQL

The options under the `mysql` section of the main `config.yml` file allow the storage of player data in a MySQL database instead of the default YAML flatfile format. Using MySQL for data storage enables better performance at high player counts, allows syncing of data between multiple servers, and makes it easier for third-party applications to interact with data.

## Basic Setup

Before configuring the Aurelium Skills config, a MySQL database must already be created separately (either through the server panel or terminal). This database should have a host (usually an IP address), database name, port, username, and password.

The options for configuring MySQL are under the `mysql` section of `config.yml`. To enable MySQL, set the `enabled` option to true. Enter the host address under `host` and the name of your database under `database`. Then, enter the username and password of the database under their corresponding options. Enter the port number of the database under `port`. If desired, SSL can be enabled under the `ssl` option.

Once the server is restarted, MySQL should be successfully working. Aurelium Skills will automatically create tables in the database. If MySQL is not working, check the server console for any errors on startup and double check your database credentials are correct.&#x20;

## Migrating Data From YAML

Enabling MySQL alone will not migrate player data from the default YAML storage method. To migrate data, a few extra steps are required:

1. While still on YAML, create a backup of the player data using `/skills backup save`
2. Shut down the server
3. Follow the instructions in the Basic Setup section and switch to MySQL (make sure you restart the server after)
4. Run `/skills backup load [fileName]`, with fileName being the name of the backup file you saved before (seen in the backups folder)
