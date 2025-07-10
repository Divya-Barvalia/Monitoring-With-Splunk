<h1 align="center">📊 Monitoring Active Directory with Splunk</h1>

<p align="center">
  All done on a Windows Server 2022 VM.
</p>

<hr/>

<h2>🎮 Side Quest: Setting up Active Directory Domain</h2>

<p>
Before configuring Splunk, a basic Active Directory domain was created on the same Windows Server 2022 instance. A test user was added to verify login functionality. Initially, logging in failed because the user lacked administrative privileges (since I was using the same Windows 2022 Server VM to login with the test user). To resolve this, the test user was added to the <strong>“Domain Admins”</strong> group via Group Policy. After this change, login succeeded. The user was also set to reset their password upon first login which worked as intended.
</p>

<hr/>

<h2>🛠️ Deployment & Integration Processes</h2>

<ol>
  <li>
    Made a Windows 2022 Server to host the Splunk enterprise server and the Splunk Universal Forwarder.<br/>
    <img src="screenshots/step1.png" alt="Step 1 Screenshot" width="600"/>
  </li>

  <li>
    Put the user in Domain Admins group so I can use same computer for testing purposes of Splunk enterprise and forwarder.<br/>
    <img src="screenshots/step2.png" alt="Step 2 Screenshot" width="600"/>
  </li>

  <li>
     Set up the Splunk Enterprise Server on <code>localhost:8000</code> and configured a receiving rule on port <code>9997</code>.<br/>
    <img src="screenshots/step3.png" alt="Step 3 Screenshot" width="600"/>
  </li>

  <li>
    Setup the Splunk Universal Forwarder's receiving indexer to send data provided the Windows 2022 Server IP and port <code>9997.</code><br/>
    <img src="screenshots/step4.png" alt="Step 4 Screenshot" width="600"/>
  </li>

  <li>
    Created a new <code>local</code> folder in:<br/>
    <code>C:\Program Files\SplunkUniversalForwarder\etc\apps\search</code><br/>
    <img src="screenshots/step5.png" alt="Step 5 Screenshot" width="600"/>
  </li>

  <li>
    Set up <code>inputs.conf</code> file in this folder.<br/>
    <img src="screenshots/step6.png" alt="Step 6 Screenshot" width="600"/>
  </li>

  <li>
    Restarted Splunk from within the <code>C:\Program Files\SplunkUniversalForwarder\bin</code> directory to save and apply changes made to these configuration files.<br/>
    <img src="screenshots/step7.png" alt="Step 7 Screenshot" width="600"/>
  </li>

  <li>
    Created an index on the Splunk Server's web interface to match name specified <code>inputs.conf</code> file.<br/>
    <img src="screenshots/step8.png" alt="Step 8 Screenshot" width="600"/>
  </li>

  <li>
    Created user in Active Directory to test monitoring on Splunk.<br/>
    <img src="screenshots/step9.png" alt="Step 9 Screenshot" width="600"/>
  </li>

  <li>
    After some messing around in AD and creating a user we were able to see that 5 events were logged in the <code>ad_index</code>index that was created earlier.<br/>
    <img src="screenshots/step10.png" alt="Step 10 Screenshot" width="600"/>
  </li>

  <li>
    Queried all logs within <code>ad_index</code> and saw the 5 events were all sourced from Active Directory. This indicated the active monitoring of AD through Splunk and that it was configured correctly.<br/>
    <img src="screenshots/step11.png" alt="Step 11 Screenshot" width="600"/>
  </li>

  <li>
    Upon deeper inspection of the latest event, detailed information was shown — including user settings, password expiration status, etc.<br/>
    <img src="screenshots/step12.png" alt="Step 12 Screenshot" width="600"/>
  </li>

  <li>
    Narrowed the search to user <strong>John</strong> and found that 4 of the 5 AD events were associated with this user/creation of this user.<br/>
    <img src="screenshots/step13.png" alt="Step 13 Screenshot" width="600"/>
  </li>

  <li>
     Gave user <strong>John Jay</strong> Domain Admin privileges, then logged in with that account and confirmed new logs were generated. These included the user's new group/privleges.<br/>
    <img src="screenshots/step14.png" alt="Step 14 Screenshot" width="600"/>
  </li>

  <li>
    Used a PowerShell script to automate creating <strong>1,000 new users</strong> and verified that a lot more logs were generated in Splunk, specifically for all the new users that were created via the PS script.<br/>
    <img src="screenshots/step15.png" alt="Step 15 Screenshot" width="600"/>
  </li>
</ol>

<hr/>
