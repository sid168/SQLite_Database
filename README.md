# SQLite_Database
Just experimenting with the most basic database 



1. Open the terminal and type:
   ```bash
   script my_terminal_session.txt
   ```
2. Run the commands:
   ```bash
   sqlite3 my_database.db
   ```
3. Inside the SQLite shell, enter:
   ```sql
   CREATE TABLE users (id INTEGER PRIMARY KEY, name TEXT, age INTEGER);
   INSERT INTO users (name, age) VALUES ('Alice', 30);
   SELECT * FROM users;
   .exit
   ```
4. Type `exit` to stop recording:
   ```bash
   exit
   ```

### Summary
- The `script` command is used **outside** of the SQLite shell in the regular terminal.
- Use `script my_terminal_session.txt` to start recording.
- Perform all SQLite operations after starting the recording.
- Stop the recording with `exit` or `Ctrl+D`.


# Creating a CSV and appending to the Database

To generate a CSV file with user data including `Name` and `Age`, you can follow these steps. 
### Option 1: Using Shell Commands

1. **Create a CSV File Manually**:

   - Open a terminal and use a text editor to create a CSV file. For example, using `nano`:
     ```bash
     nano users.csv
     ```

   - Enter the data in CSV format:
     ```
     Name,Age
     Alice,30
     Bob,25
     Charlie,35
     ```

   - Save the file by pressing `Ctrl+O`, then `Enter`, and exit `nano` by pressing `Ctrl+X`.

2. **Alternatively, Use a Shell Command**:

   - You can also create a CSV file directly from the terminal:
     ```bash
     echo "Name,Age" > users.csv
     echo "Alice,30" >> users.csv
     echo "Bob,25" >> users.csv
     echo "Charlie,35" >> users.csv
     ```

## Shell script for importing csv to db: 

touch import_csv.sh

nano import_csv.sh

Write the following script inside import_csv.sh:


#!/bin/bash

# Define SQLite database file and CSV file
DB_FILE="my_database.db"
CSV_FILE="users.csv"
TEMP_CSV="temp_users.csv"

# Remove the header row from the CSV file and save to a temporary file
tail -n +2 $CSV_FILE > $TEMP_CSV

# Create a temporary table and import data
sqlite3 $DB_FILE <<EOF
-- Drop the temporary table if it exists
DROP TABLE IF EXISTS temp_users;

-- Create the temporary table
CREATE TEMP TABLE temp_users (Name TEXT, Age INTEGER);

-- Set the mode to CSV and import data
.mode csv
.import $TEMP_CSV temp_users

-- Insert unique records into the main table
INSERT INTO users (Name, Age)
SELECT Name, Age
FROM temp_users
WHERE (Name, Age) NOT IN (SELECT Name, Age FROM users);

-- Drop the temporary table
DROP TABLE temp_users;
EOF

# Remove the temporary CSV file
rm $TEMP_CSV

echo "Import process completed."

Then, Make the Script Executable

chmod +x import_csv.sh

Run the Script:

./import_csv.sh
