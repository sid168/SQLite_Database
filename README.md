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
