# sqlinjection
Exploiting SQL Injection vulnerability

# AIM:
To exploit SQL Injection vulnerability using Multidae web application in Metasploitable2

## DESIGN STEPS:

### Step 1:

Install kali linux either in partition or virtual box or in live mode


### Step 2:

Investigate on the various categories of tools as follows:

### Step 3:

Open terminal and try execute some kali linux commands

## EXECUTION STEPS AND ITS OUTPUT:

SQL Injection is a sort of infusion assault that makes it conceivable to execute malicious SQL statements. These statements control a database server behind a web application. Assailants can utilize SQL Injection vulnerabilities to sidestep application safety efforts. They can circumvent authentication and authorization of a page or web application and recover the content of the whole SQL database. Identify IP address using ifconfig in Metasploitable2

![image](https://github.com/chandrumathiyazhagan/sqlinjection/assets/119393023/84312b2a-7396-4236-98a8-1bc6aefed6a2)


Use the above ip address to access the apache webserver of Metasploitable2 from kali linux. In Kali Linux use the ip address in a web browser. 

![image](https://github.com/chandrumathiyazhagan/sqlinjection/assets/119393023/9baf43e1-f4a2-4636-ab2d-3f4025faa591)


Select Multidae from the menu listed as shown above. You will get the page as displayed below:

![image](https://github.com/chandrumathiyazhagan/sqlinjection/assets/119393023/9e8aa2a3-8537-48d6-9b40-571a2d1a4b86)


Click on the menu Login/Register and register for an account

![image](https://github.com/chandrumathiyazhagan/sqlinjection/assets/119393023/a4ff59ce-cb0c-4108-b0b6-02bc031460e9)


Click on the link “Please register here” 

![image](https://github.com/chandrumathiyazhagan/sqlinjection/assets/119393023/0012db04-80e2-408e-8237-fd1de6b8b628)


Click on “Create Account” to display the following page: 

![image](https://github.com/chandrumathiyazhagan/sqlinjection/assets/119393023/053f073f-e3f3-4b17-9c58-97628a8d4870)


The login structure we will use in our examples is straightforward. It contains two input fields (username and password), which are both vulnerable. The back-end content creates a query to approve the username and secret key given by the client. Here is an outline of the page rationale:

($query = “SELECT * FROM users WHERE username=’$_POST[username]’ AND password=’$_POST[password]’“;). For the username put “ganesh” or “anything” and for the password put (anything’ or ‘1’=’1) or (admin’ or ‘1’=’1) then try to log in, and you’ll be presented with an admin login page.

![image](https://github.com/chandrumathiyazhagan/sqlinjection/assets/119393023/ef61e37b-6ab5-4bc6-8992-15622806ca2e)



Click “Login”. The logged in page will show as below: 

![image](https://github.com/chandrumathiyazhagan/sqlinjection/assets/119393023/6e21a99b-210a-4925-b466-5b4ac9fb0ac8)



## Bypassing login field

The username field is vulnerable. Put (ganesh’ #) or (ganesh’--) in the username field and hit “Enter” to log in. We use “#” or “--” to comment everything in the query sentence that comes after the username filed telling the database to disregard the password field: (SELECT * FROM users WHERE username=’admin’ # AND password=’ ‘). By using line commenting, the aggressor eliminates a part of the login condition and gains access. This technique will make the “WHERE” clause true only for one user; in this case, it is “ganesh.”

Now after logging out you will see the login page. In the login page give ganesh’ # . You can see the page now enters into the administrator page as before when giving the password.

![image](https://github.com/chandrumathiyazhagan/sqlinjection/assets/119393023/597d6fa1-c5ea-446f-93d2-6fd66daa81c9)


Click the login button and you will see it enter into the administrator page. 

![image](https://github.com/chandrumathiyazhagan/sqlinjection/assets/119393023/566a302c-c56d-494a-82de-e8b6c54c7d92)


## Union-based SQL injection

UNION-based SQL injection assaults enable the analyzer to extract data from the database effectively. Since the “UNION” operator must be utilized if the two inquiries have precisely the same structure, the attacker must craft a “SELECT” statement like the first inquiry. we will be using the “User Info” page from Mutillidae to perform a Union-Based SQL injection attack. Go to “OWASP Top 10/A1 — Injection/SQLi — Extract-Data/User Info”

After logging out, Now choose the menu as shown below: 

![image](https://github.com/chandrumathiyazhagan/sqlinjection/assets/119393023/c06228bc-9994-49e1-be73-628b03ec1c83)



![image](https://github.com/chandrumathiyazhagan/sqlinjection/assets/119393023/5d475cdc-d20e-46ef-9c2d-30586d61fb02)


![image](https://github.com/chandrumathiyazhagan/sqlinjection/assets/119393023/612d0dbb-037e-4bcc-97ce-38f94880c3d8)


![image](https://github.com/chandrumathiyazhagan/sqlinjection/assets/119393023/04bdb94e-2bb3-4a18-9ab3-c6eeee785d5e)


![image](https://github.com/chandrumathiyazhagan/sqlinjection/assets/119393023/75e08788-1ca0-4436-999b-33166514898d)


From this point, all our attack vectors will be performed in the URL section of the page using the Union-Based technique.There are two different ways to discover how many columns are selected by the original query. The first is to infuse an “ORDER BY” statement indicating a column number. Given the column number specified is higher than the number of columns in the “SELECT” statement, an error will be returned. 

![image](https://github.com/chandrumathiyazhagan/sqlinjection/assets/119393023/958a2082-6e2f-44b9-8187-69dba1feed68)


Since we do not know the number of columns, we start at 1. To find the exact amount of columns, the number is incremented until an error related to the “ORDER BY” clause is returned. In this example, we incremented it to 6 and received an error message, so it means that the number of columns is lower than 6.

The browser url of this info page need to be modified with the url as below:

http://192.168.43.145/mutillidae/index.php?page=user-info.php&username=praveen%27order%20by%206%23&password=&user-info-php-submit-button=View+Account+Details

![image](https://github.com/chandrumathiyazhagan/sqlinjection/assets/119393023/02bbb469-6839-48aa-9aaf-7a69436053f1)


After adding the order by 6 into the existing url , the following error statement will be obtained: 

![image](https://github.com/chandrumathiyazhagan/sqlinjection/assets/119393023/d968b86a-c121-441f-a167-ca5cdd89454a)


When we ordered by 5, it worked and displayed some information. It means there are five columns that we can work with. Following screenshot shows that the url modified to have statement added with ordered by 5 replacing 6. 

![image](https://github.com/chandrumathiyazhagan/sqlinjection/assets/119393023/b532c5ab-2f1e-4acc-b722-031880e1d500)


As it is having 5 columns the query worked fine and it provides the correct result

![image](https://github.com/chandrumathiyazhagan/sqlinjection/assets/119393023/e13a703d-8396-486c-b939-16a87e7e7023)



Instead of using the "order by" option, let’s use the "union select" option and provide all five columns. Ex: (union select 1,2,3,4,5)

![image](https://github.com/chandrumathiyazhagan/sqlinjection/assets/119393023/716c32f2-a19b-41d5-8630-b87699dd3e2d)

As given in the screenshot below columns 2,3,4 are usable in which we can substitute any sql commands to extract necessary information. 

![image](https://github.com/chandrumathiyazhagan/sqlinjection/assets/119393023/20f1d381-cf16-4b87-9650-e320e9bff590)


Now we will substitute some few commands like database(), user(), version() to obtain the information regarding the database name, username and version of the database.

![image](https://github.com/chandrumathiyazhagan/sqlinjection/assets/119393023/1c30f701-58f2-434e-889a-e501a29ef8bc)


The url when executed, we obtain the necessary information about the database name owasp10, username as root@localhost and version as 5.0.51a-3ubuntu5. In MySQL, the table “information_schema.tables” contains all the metadata identified with table items. Below is listed the most useful information on this table.

Replace the query in the url with the following one: union select 1,table_name,null,null,5 from information_schema.tables where table_schema = ‘owasp10’

![image](https://github.com/chandrumathiyazhagan/sqlinjection/assets/119393023/dbfeb09b-d77c-4f12-972e-41f40f042e5f)


The url once executed will retrieve table names from the “owasp 10” database. ##Extracting sensitive data such as passwords

When the attacker knows table names, he needs to discover what the column names are to extract data.

In MySQL, the table “information_schema.columns” gives data about columns in tables. One of the most useful columns to extract is called “column_name.”

Ex: (union select 1,colunm_name,null,null,5 from information_schema.columns where table_name = ‘accounts’).

Here we are trying to extract column names from the “accounts” table.

![image](https://github.com/chandrumathiyazhagan/sqlinjection/assets/119393023/d24356bd-1169-42f6-83f1-33efe7af39e5)


![image](https://github.com/chandrumathiyazhagan/sqlinjection/assets/119393023/8bf257ea-c15f-4448-9859-9a5d9a87b30a)


Once we discovered all available column names, we can extract information from them by just adding those column names in our query sentence.

Ex: (union select 1,username,password,is_admin,5 from accounts).

![image](https://github.com/chandrumathiyazhagan/sqlinjection/assets/119393023/a00b12b3-ded1-4103-aa96-8dfa0ea12f87)


## Reading and writing files on the web-server

We can use the “LOAD_FILE()” operator to peruse the contents of any file contained within the web-server. We will typically check for the “/etc/password” file to see if we get lucky and scoop usernames and passwords to possible use in brute force attacks later.

Ex: (union select null,load_file(‘/etc/passwd’),null,null,null).

![image](https://github.com/chandrumathiyazhagan/sqlinjection/assets/119393023/6b4da461-e757-408c-a1ef-01044feca199)


the “INTO_OUTFILE()” operator for all that they offer and attempt to root the objective server by transferring a shell-code through SQL infusion. we will write a “Hello World!” sentence and output it in the “/tmp/” directory as a “hello.txt” file. This “Hello World!” sentence can be substituted with any PHP shell-code that you want to execute in the target server. Ex: (union select null,’Hello World!’,null,null,null into outfile ‘/tmp/hello.txt’).

## RESULT:
The SQL Injection vulnerability is successfully exploited using the Multidae web application in Metasploitable2.
