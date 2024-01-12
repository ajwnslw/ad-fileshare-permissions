<p align="center">
<img src="https://helpdeskgeek.com/wp-content/pictures/2019/04/share-files.png" height = 30% width = 30%/>
</p>

<h1 align = "center">Network File Shares and Permissions in Active Directory</h1>
Organizing resources and ensuring proper user permissions and file access are crucial aspects of business structure. This tutorial illustrates the functioning of file sharing and permissions within the Active Directory domain.

<br />

<h2>Environments and Technologies Used</h2>
<ul>
  <li>Microsoft Azure (Virtual Machines)</li>
  <li>Remote Desktop</li>
  <li>Active Directory</li>
</ul>

<br />

<h2>Operating Systems Used</h2>
<ul>
  <li>Windows Server 2022</li>
  <li>Windows 10 Pro (21H2)</li>
</ul>

<br />

<h2>Procedure</h2>

<h3>Creating File Shares and setting Permissions</h3>

<p>
  <ul>
    <li>Log into your Domain Controller VM (DC-1) as an admin (mydomain.com\jane_admin). Separately, log into your Client VM (Client-1) as a random user (generated using Powershell script).</li>
    <li>In DC-1, create the four following folders in the C:\ Drive and set their permissions (right-click -> Properties -> Sharing -> Share)</li>
    <ul>
      <li><b>read_access</b> - add the group Domain Users and set Permissions to Read</li>
      <li><b>write_access</b> - add the group Domain Users and set Permissions to Read/Write</li>
      <li><b>no_access</b> - add the group Domain Admins and set Permissions to Read/Write</li>
      <li><b>accounting</b> - skip, set permissions later</li>
    </ul>
    <br />
    <li>Screenshot Example</li>
      <img src ="https://github.com/ColtonTrauCC/network-fileshare/assets/147654000/0942d01d-f85f-4b10-a257-f399c3fc5e26" width = 80% height = 80%/>
  </ul>
</p>

<br/>

<h3>Accessing File Shares</h3>

<p>
  <ul>
    <li>Open File Explorer in Client-1 VM. At the top, type <b>\\dc-1</b> to access the previously created folders.</li>
    <ul>
      <img src ="https://github.com/ColtonTrauCC/network-fileshare/assets/147654000/3144b792-b6bf-4f27-b798-d2d8256308ff" width = 80% height = 80%/></li>
    </ul>
    <br />
    <li>Based on the permissions that were set by the admin in DC-1, all folder should be accessible by the random user except for "no_access".</li>
    <ul>
      <img src ="https://github.com/ColtonTrauCC/network-fileshare/assets/147654000/bbc34c03-d505-43b9-aadc-9b3b73db65be" width = 80% height = 80%/>
    </ul>
  </ul>
</p>

<br/>

<h3>Creating the "ACCOUNTANTS" Security Group</h3>

<p>
  <ul>
    <li>Return to the DC-1 VM, navigate to the Server Manager Dashboard, access Active Directory Users and Computers, and establish a new Organizational Unit (OU) named "_SECURITY_GROUPS".</li>
    <ul>
      <img src = "https://github.com/ColtonTrauCC/network-fileshare/assets/147654000/af27e138-36de-44b0-85e9-a2b37088d496" width = 80% height = 80%/>
    </ul>
    <br />
    <li>Create a new Group called "ACCOUNTANTS" within "_SECURITY_GROUPS".</li>
    <ul>
      <img src = "https://github.com/ColtonTrauCC/network-fileshare/assets/147654000/12e4bde2-5aad-4981-b1b9-4fe847355b1c" width = 80% height = 80%/>
    </ul>
    <br />
    <li>Find the accounting folder within the C:\ Drive, include the ACCOUNTANTS group, and configure its permissions to Read/Write.</li>
    <ul>
      <img src = "https://github.com/ColtonTrauCC/network-fileshare/assets/147654000/567b094a-da96-49aa-ba2f-9f70d9c26844" width = 80% height = 80%/>
    </ul>
    <br />
    <li>The user logged into the Client-1 VM should be denied access to the accounting folder because they are not a member of the Group. Log out of the Client-1 VM, and make note of the username used for logging in, as it will be added to the ACCOUNTANTS group.</li>
    <li>Within the DC-1 VM, navigate to the "_SECURITY_GROUPS" Organizational Unit, right-click on ACCOUNTANTS to access Properties, proceed to the Members tab, and include the user as a member of the Group.</li>
    <ul>
      <img src = "https://github.com/ColtonTrauCC/network-fileshare/assets/147654000/9ccc7a04-e3eb-4145-8485-2431280b86e7" width = 80% height = 80%/>
    </ul>
    <li>Re-login to the Client-1 VM using the user who has been added to the ACCOUNTANTS group; now, they should have access to the accounting folder.</li>
  </ul>
</p>

<br/>
