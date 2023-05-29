There are 8 and those are split into 2 sections; Data Orientated and Process Orientated, as shown respectively:

Data Orientated deals with the processing of data
1. Minimise - limit as much as possible the processing of data (deleting data ...?)
2. Separate - separate the processing of data as much as possible (separating does not mean data becomes inaccessible)
3. Abstract - limit as much as possible the detail in which personal data is processed 
4. Hide - make personal data unlink-able and unobservable
-----
Process Orientated deals with organisational aspects
1. Inform - Inform subjects about the processing of their data in a timely manner
2. Control - Provide data subjects with adequate control over the processing of their data.
3. Enforce - Process data in a privacy-friendly manner and enforce this behaviour
4. Demonstrate - Demonstrate that you are enforcing these principles.

-------
##### Minimise
- Just collect less data !
- Collect only what is absolutely necessary
- Remove data as soon as it's not relevant -  this includes backup data.
- This also extends to when sharing and analysing data, try to minimise this as well!
- When deleting data there are several methods which include writing garbage information to where the data is stored or simply destroying the key used to encrypt the data so that it's not recoverable.

##### Separate
- Logically or Physically separating data on different databases 
- For example, a privacy focused social media would allow users to store all posts and images locally instead of centrally.

##### Abstract
- This address the question of the level of detail in which we process the personal data (collecting data)
- We can generalise data slightly to make it less personal - asking for age instead of birthday, City instead of address, ...
- We can group people to make average data - the less personal the better !
- You can even add random noise to data - for example adding noise to a person's location.
- ==Homomorphic Encryption== allows one to perform calculations on encrypted data without learning the real data. Once the sum has been computed your can decrypt the value.

##### Hide
- Restricting access to the actual personal data - 'need-to-know' basis. So it's hard to accidentally leak the data when access to it internally is hard.
- Make data hard to understand - encrypt it !
- ==Attribute-based Credentials (ABCs)== - allow a privacy-friendly form of identity management. Attributes are personal quantities (name, age, ...) Using ABCs you can prove you have a certain attribute without revealing any other information, for instance you can prove you are over 18, 100 times but to the provider it looks like a 100 different people have proved they are over 18.

-------

##### Inform
- Informing users about what data is processed allows them to make informed decisions and you can be verified as to whether you are processing the data responsibly.
- Supply information as to which data, how it's being processed and why. The more information the better - how long you will have it, who you share it with, ...
- Provide simple and detailed explanations as for the reasons for the data collection to all backgrounds of users - novice, experts, authorities, ...
- Notify users immediately as to anything - processing, sharing, leaks - and allow users to control whether they want to receive a notification.
- Process user requests with good response times.

##### Control
- Ask for consent to process their data - after informing exactly how and what will be processed - and allow them to withdraw consent.
- The platform should still have a basic function for those who do not consent to their data being processed.
- Keep data up to date - this can be done by allowing users to keep track of what data you have on them.
- Allow users to delete the data you have on them.

##### Enforce
- The organisation itself should take responsibility, provide resources to execute it's stated privacy policy.
- All personnel should be trained, privacy policies should be maintained and updated regularly.
- One method is called =='sticky policies'== - data that is collected under certain conditions has labels stating those conditions, so that we know how to use such data and when to delete it, and so on...

##### Demonstrate
- Document steps taken and Record decisions - make sure you documentation corresponds with reality.
- Audit logs regularly.
- Report Results to a Data Protection Authority and Consult them whenever necessary.

goto: [[SWE 2 Home Page|Home Page]]


