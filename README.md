# Active-Directory-Configure-Group-Policy-Lab
Windows Server AD DS lab configuring Group Policy Objects (GPOs) for enterprise security baselines, automated drive mapping, and software deployment.

## Enviroment & Specifications:
* **Goal**: In this lab, I will configure Group Policy Objects (GPOs) within a Windows Server Active Directory environment to automate network settings and enforce compliance. Specifically, I will be applying a domain-wide password policy, mapping a shared network drive for domain users, and changing the default desktop wallpaper across client machines.
* **Platform**: VMware Workstation (VMnet4 Host-Only) using Windows Server 2022 and Windows 11 Pro.
* **Network Types**: Host-Only (VMnet4) treated as a “sandbox” for isolation.
* **Subnet**: 192.168.100.0/24 with DHCP disabled.

---

## 1. Creating Wallpaper Group Policy Object
Within the Group Policy Management console, I created a new GPO designed to standardize the desktop background across all client machines. Next, inside the Group Policy Management Editor, I navigated to the Desktop section under User Configuration to enable the wallpaper policy and specify the network path of the image asset. Once configured, I linked the GPO directly to the `lab.nate.com` domain. To verify that the policy took effect, I opened a command prompt on the client machine and executed `gpupdate /force`. The initial background refresh was unsuccessful because User Configuration changes often require a complete session reload to apply; after signing out and signing back into the user account, I re-ran the `gpupdate /force` command, which successfully updated the background environment.

![](Creation%20of%20GPO-Wallpaper.png)
![](Hierarcy%20Veiw%20of%20GPO-Wallpaper.png)
![](Edit%20of%20GPO-Wallpaper.png)
![](Enabling%20GPO-Wallpaper.png)
![](Linking%20GPO-Wallpaper.png)
![](Unsuccessful%20gpupdate%20command.png)
![](Successful%20gpupdate%20command.png)
![](Result%20of%20GPO-Wallpaper.png)

## 2. Apply Password Policies
Within the Group Policy Management Editor console, I navigated to the Password Policy section and changed the default password policy settings to a more secure enterprise baseline. Once the changes were configured, I navigated to the client machine and forced an immediate Group Policy update by running the command `gpupdate /force` in the command prompt. To verify that the new security baselines applied successfully, I executed the `net accounts` command to inspect the active password policies, and further validated the configuration by attempting to change a user's password to a weak fallback string, which was successfully rejected by the domain.

![](Password%20Policy%20Settings.png)
![](Changed%20Password%20Policy%20Settings.png)
![](Successful%20gpupdate%20command%20for%20Password%20Policy.png)
![](Password%20Policy%20Settings%20on%20W11C.png)
![](Testing%20Password%20Policy.png)
![](Result%20of%20Password%20Policy%20Test.png)

## 3. Mapping a Network Drive
In File Explorer on the Windows Server, I created a shared network directory named `CorporateShare` to serve as a centralized file repository for the domain. Next, I navigated to the Group Policy Management console and created a new GPO dedicated to mapping this shared asset. Inside the Group Policy Management Editor, I navigated to the Drive Maps section under User Preferences to configure the target share path and assign a local drive letter. Once configured, I linked the GPO directly to the `lab.nate.com` domain. To apply and verify the settings on the client side, I opened a command prompt on the client machine and executed `gpupdate /force` to pull down the updated preferences. Finally, I opened File Explorer on the client machine to confirm that the new network drive was successfully mounted and accessible then i ran a `gpresult /r` command to comfirm both GPOs are activated on the client machine.

![](Creating%20Shared%20Folder%20on%20Server.png)
![](Creation%20of%20GPO-Map-Network-Drive.png)
![](Veiw%20of%20Drive%20Maps.png)
![](Creation%20of%20Mapped%20Drive.png)
![](Drive%20Maps%20Veiw%20of%20Created%20Drive.png)
![](Linking%20GPO%20Mapped%20Drive.png)
![](Veiw%20ofLinked%20GPO-Map-Network-Drive.png)
![](Successful%20gpupdate%20of%20GPO-Map-Network-Drive.png)
![](Result%20of%20GPO-Map-Network-Drive.png)
![](gpresult%20command%201.png)
![](gpresult%20command%202.png)

---

## Summary & Key Takeaways
This lab demonstrated the practical application of centralized environment management and security enforcement through Active Directory Group Policy Objects (GPOs). By deploying a robust domain-wide password policy, automating network resource deployment via drive mapping, and enforcing user-space compliance with a desktop wallpaper GPO, I mastered the primary tools used to maintain consistency across enterprise endpoints. Ultimately, this lab underscored how Group Policy minimizes administrative overhead, ensures baseline security hygiene, and automates resource provisioning dynamically at scale, while highlighting critical troubleshooting factors like policy replication delays and session sign-out behaviors during client-side application.
