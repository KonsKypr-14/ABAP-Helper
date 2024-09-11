# How to add a connection
<div class="note">
    <strong>NOTE:</strong> The theme that is used is SAP Signature theme.
</div>

In order to create a connection in SAP Logon we have to clear on the new file icon or shortcut (CTRL + N). After that we click on the Connection.

![image](https://github.com/user-attachments/assets/6809f55a-2c7a-42c1-a4ab-2f90eb45ff79)

1. On the first screen select the User Specified System. Is the most commonly used. In case you want to connect on a specific host (displayed after being on same network + changing the Service and Host file), there will be a different icon (second icon).

![image](https://github.com/user-attachments/assets/6af5f1c7-6200-476d-8368-2cfb8af7b377)

2. On the next screen we write:

![image](https://github.com/user-attachments/assets/2a46a555-996a-4fef-9d32-3fad3bae7009)

a. A Description of the connection. This could be the company name and/or the system "development/quality/production".

b. The application server you want to connect. This can be an IP address, with or without a port (1.2.3.4, 5.6.7.8:12345), or a domain name or host name (after configuring the host file) "example.dev_system".

c. The instance number. Here you can only insert number from 00 - 99.

d. The system ID. The ID (DEV, S4D, etc.) could be the same for different companies/clients but the combination with application server is unique.

e. SAProuter String. In case is needed it will provided to you by the company/client or the IT.

3. After finishing the screen you click on the Finish button at bottom right.

4. You are ready to connect to the system!


# Keep the connection with you.

### Don't lose your connection after changing machine.

In order to avoid inserting 1 by 1 all the connections that you may have, when you change machine, we can transfer all the connection.

On our old machine we have to go to the following path and copy the common folder.

<div class="note">
    <strong>NOTE:</strong> Where ### is your PC user. <br> C:\Users\###\AppData\Roaming\SAP
</div>

After that paste the common folder into you new PC and you are ready to go!

![image](https://github.com/user-attachments/assets/da4ab710-f501-457b-8054-0538935979eb)
