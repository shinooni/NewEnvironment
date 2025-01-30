### Hotpatch (preview)

Hotpatch is now available for Windows Server 2025 machines connected to Azure Arc after Hotpatch is enabled in the Azure Arc portal. You can use Hotpatch to apply OS security updates without restarting your machine. To learn more, see [Hotpatch](hotpatch.md).

> [!IMPORTANT]
> Azure Arc-enabled Hotpatch is currently in preview. See the [Supplemental Terms of Use for Microsoft Azure Previews](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) for legal terms that apply to Azure features that are in beta, preview, or otherwise not yet released into general availability.

| What to Check         | Checked |
| :-------------------- | ------: |
| Checked.For.NPS:      | Y       |
| NPS.No.Risk:          | N       |
| Checked.For.DC:       | N       |
| DC.No.Risk:           | N       |
| Checked.For.WiderUse: | N       |
| WiderUse.No.Risk:     | N       |
| Notes: | This has not been vetted for NPS use and we do not use Azure Arc. Testing is still required for when the Hotpatch goes mainstream. There are some risks with this feature however this will also hit other version of windows server (2022)

- **Active Directory schema updates**: Three new log database files are introduced that extend the Active Directory schema: `sch89.ldf`, `sch90.ldf`, and `sch91.ldf`. The AD LDS equivalent schema updates are in `MS-ADAM-Upgrade3.ldf`. To learn more about previous schema updates, see [Windows Server Active Directory schema updates](../identity/ad-ds/deploy/Schema-Updates.md).
- **Active Directory object repair**: Enterprise administrators can now repair objects with the missing core attributes `SamAccountType` and `ObjectCategory`. Enterprise administrators can reset the `LastLogonTimeStamp` attribute on an object to the current time. These operations are achieved through a new [RootDSE](/openspecs/windows_protocols/ms-adts/fc74972f-b267-4c1a-8716-0f5b48cf52b9) modify operation feature on the affected object called `fixupObjectState`.
- **Channel binding audit support**: You can now enable events 3074 and 3075 for Lightweight Directory Access Protocol (LDAP) channel binding. When the channel binding policy is modified to a more secure setting, an administrator can identify devices in the environment that don't support or fail channel binding. These audit events are also available in Windows Server 2022 and later via [KB4520412](https://support.microsoft.com/topic/2020-2023-and-2024-ldap-channel-binding-and-ldap-signing-requirements-for-windows-kb4520412-ef185fb8-00f7-167d-744c-f299a66fc00a).
- **DC-location algorithm improvements**: The DC discovery algorithm provides new functionality with improvements to mapping of short NetBIOS-style domain names to DNS-style domain names. To learn more, see [Locating domain controllers in Windows and Windows Server](../identity/ad-ds/manage/dc-locator.md).


| What to Check         | Checked |
| :-------------------- | ------: |
| Checked.For.NPS:      | N       |
| NPS.No.Risk:          | N       |
| Checked.For.DC:       | N       |
| DC.No.Risk:           | N       |
| Checked.For.WiderUse: | N       |
| WiderUse.No.Risk:     | N       |
| Notes: | If viewing an older AD from the a 2025 machine it may flag as requiring a repair for AD objects. Administrators should not "repair" the object. Specifically "Enterprise administrators can now repair objects with the missing core attributes `SamAccountType` and `ObjectCategory`. Enterprise administrators can reset the `LastLogonTimeStamp` attribute on an object to the current time." This should not be done on a 2025 machine.



Windows Local Administrator Password Solution (LAPS) helps organizations manage local administrator passwords on their domain-joined computers. It automatically generates unique passwords for each computer's local administrator account. It then stores them securely in Active Directory and updates them regularly. Automatically generated passwords help to improve security. They reduce the risk of attackers gaining access to sensitive systems by using compromised or easily guessable passwords.

Several features new to Microsoft LAPS introduce the following improvements:

- **New automatic account management**: IT admins can now create a managed local account with ease. With this feature, you can customize the account name and enable or disable the account. You can even randomize the account name for enhanced security. The update also includes improved integration with existing local account management policies from Microsoft. To learn more about this feature, see [Windows LAPS account management modes](/windows-server/identity/laps/laps-concepts-account-management-modes).
- **New image rollback detection**: Windows LAPS now detects when an image rollback occurs. If a rollback does happen, the password stored in Active Directory might no longer match the password stored locally on the device. Rollbacks can result in a *torn state*. In this case, the IT admin is unable to sign in to the device by using the persisted Windows LAPS password.

  To address this issue, a new feature was added that includes an Active Directory attribute called `msLAPS-CurrentPasswordVersion`. This attribute contains a random globally unique identifier (GUID) written by Windows LAPS every time a new password is persisted in Active Directory and saved locally. During every processing cycle, the GUID stored in `msLAPS-CurrentPasswordVersion` is queried and compared to the locally persisted copy. If they're different, the password is immediately rotated.

  To enable this feature, run the latest version of the `Update-LapsADSchema` cmdlet. Windows LAPS then recognizes the new attribute and begins to use it. If you don't run the updated version of the `Update-LapsADSchema` cmdlet, Windows LAPS logs a 10108 warning event in the event log but continues to function normally in all other respects.

  No policy settings are used to enable or configure this feature. The feature is always enabled after the new schema attribute is added.

- **New passphrase**: IT admins can now use a new feature in Windows LAPS that enables the generation of less-complex passphrases. An example is a passphrase such as **EatYummyCaramelCandy**. This phrase is easier to read, remember, and type compared to a traditional password like **V3r_b4tim#963?**.

  With this new feature, you can configure the `PasswordComplexity` policy setting to select one of three different word lists for passphrases. All of the lists are included in Windows and don't require a separate download. A new policy setting called `PassphraseLength` controls the number of words used in the passphrase.Windows Local Administrator Password Solution (LAPS) helps organizations manage local administrator passwords on their domain-joined computers. It automatically generates unique passwords for each computer's local administrator account. It then stores them securely in Active Directory and updates them regularly. Automatically generated passwords help to improve security. They reduce the risk of attackers gaining access to sensitive systems by using compromised or easily guessable passwords.

Several features new to Microsoft LAPS introduce the following improvements:

- **New automatic account management**: IT admins can now create a managed local account with ease. With this feature, you can customize the account name and enable or disable the account. You can even randomize the account name for enhanced security. The update also includes improved integration with existing local account management policies from Microsoft. To learn more about this feature, see [Windows LAPS account management modes](/windows-server/identity/laps/laps-concepts-account-management-modes).
- **New image rollback detection**: Windows LAPS now detects when an image rollback occurs. If a rollback does happen, the password stored in Active Directory might no longer match the password stored locally on the device. Rollbacks can result in a *torn state*. In this case, the IT admin is unable to sign in to the device by using the persisted Windows LAPS password.

  To address this issue, a new feature was added that includes an Active Directory attribute called `msLAPS-CurrentPasswordVersion`. This attribute contains a random globally unique identifier (GUID) written by Windows LAPS every time a new password is persisted in Active Directory and saved locally. During every processing cycle, the GUID stored in `msLAPS-CurrentPasswordVersion` is queried and compared to the locally persisted copy. If they're different, the password is immediately rotated.

  To enable this feature, run the latest version of the `Update-LapsADSchema` cmdlet. Windows LAPS then recognizes the new attribute and begins to use it. If you don't run the updated version of the `Update-LapsADSchema` cmdlet, Windows LAPS logs a 10108 warning event in the event log but continues to function normally in all other respects.

  No policy settings are used to enable or configure this feature. The feature is always enabled after the new schema attribute is added.

- **New passphrase**: IT admins can now use a new feature in Windows LAPS that enables the generation of less-complex passphrases. An example is a passphrase such as **EatYummyCaramelCandy**. This phrase is easier to read, remember, and type compared to a traditional password like **V3r_b4tim#963?**.

  With this new feature, you can configure the `PasswordComplexity` policy setting to select one of three different word lists for passphrases. All of the lists are included in Windows and don't require a separate download. A new policy setting called `PassphraseLength` controls the number of words used in the passphrase.

  When you create a passphrase, the specified number of words are randomly selected from the chosen word list and concatenated. The first letter of each word is capitalized to enhance readability. This feature also fully supports backing up passwords to either Active Directory or Microsoft Entra ID.

  The passphrase word lists used in the three new `PasswordComplexity` passphrase settings are sourced from the Electronic Frontier Foundation article [Deep Dive: EFF's New Wordlists for Random Passphrases](https://www.eff.org/deeplinks/2016/07/new-wordlists-random-passphrases). The [Windows LAPS Passphrase Word Lists](https://go.microsoft.com/fwlink/?linkid=2255471) is licensed under the CC-BY-3.0 attribution license and is available for download.

  > [!NOTE]
  > Windows LAPS doesn't allow for customization of the built-in word lists or the use of customer-configured word lists.

- **Improved readability password dictionary**: Windows LAPS introduces a new `PasswordComplexity` setting that enables IT admins to create less complex passwords. You can use this feature to customize LAPS to use all four character categories (uppercase letters, lowercase letters, numbers, and special characters) like the existing complexity setting of `4`. With the new setting of `5`, the more complex characters are excluded to enhance password readability and minimize confusion. For example, the numeral **1** and the letter **I** are never used with the new setting.

  When `PasswordComplexity` is configured to `5`, the following changes are made to the default password dictionary character set:

  - **Don't use:** The letters **I**, **O**, **Q**, **l**, **o**
  - **Don't use:** The numbers **0**, **1**
  - **Don't use:** The special characters **,**, **.**, **&**, **{**, **}**, **[**, **]**, **(**, **)**, **;**
  - **Use:** The special characters **:**, **=**, **?**, **\***

  The ADUC snap-in (via Microsoft Management Console) now features an improved Windows LAPS tab. The Windows LAPS password now appears in a new font that enhances its readability when it appears in plain text.

- **Post-authentication action support for terminating individual processes**: A new option is added to the **Post-authentication actions** (PAA) Group Policy setting, `Reset the password, sign out the managed account, and terminate any remaining processes`, which is located in **Computer Configuration** > **Administrative Templates** > **System** > **LAPS** > **Post-authentication actions**.

  This new option is an extension of the previous option, `Reset the password and log off the managed account`. After configuration, the PAA notifies and then terminates any interactive sign-in sessions. It enumerates and terminates any remaining processes that are still running under the local account identity managed by Windows LAPS. No notification precedes this termination.
  
  The expansion of logging events during the execution of PAA provides deeper insights into the operation.

To learn more about Windows LAPS, see [What is Windows LAPS?](/windows-server/identity/laps/laps-overview).

| What to Check         | Checked |
| :-------------------- | ------: |
| Checked.For.NPS:      | N       |
| NPS.No.Risk:          | N       |
| Checked.For.DC:       | N       |
| DC.No.Risk:           | N       |
| Checked.For.WiderUse: | N       |
| WiderUse.No.Risk:     | N       |
| Notes: | Need to test if the new Group Policy setting seeds to DC when access from Server 2025.

  When you create a passphrase, the specified number of words are randomly selected from the chosen word list and concatenated. The first letter of each word is capitalized to enhance readability. This feature also fully supports backing up passwords to either Active Directory or Microsoft Entra ID.

  The passphrase word lists used in the three new `PasswordComplexity` passphrase settings are sourced from the Electronic Frontier Foundation article [Deep Dive: EFF's New Wordlists for Random Passphrases](https://www.eff.org/deeplinks/2016/07/new-wordlists-random-passphrases). The [Windows LAPS Passphrase Word Lists](https://go.microsoft.com/fwlink/?linkid=2255471) is licensed under the CC-BY-3.0 attribution license and is available for download.

  > [!NOTE]
  > Windows LAPS doesn't allow for customization of the built-in word lists or the use of customer-configured word lists.

- **Improved readability password dictionary**: Windows LAPS introduces a new `PasswordComplexity` setting that enables IT admins to create less complex passwords. You can use this feature to customize LAPS to use all four character categories (uppercase letters, lowercase letters, numbers, and special characters) like the existing complexity setting of `4`. With the new setting of `5`, the more complex characters are excluded to enhance password readability and minimize confusion. For example, the numeral **1** and the letter **I** are never used with the new setting.

  When `PasswordComplexity` is configured to `5`, the following changes are made to the default password dictionary character set:

  - **Don't use:** The letters **I**, **O**, **Q**, **l**, **o**
  - **Don't use:** The numbers **0**, **1**
  - **Don't use:** The special characters **,**, **.**, **&**, **{**, **}**, **[**, **]**, **(**, **)**, **;**
  - **Use:** The special characters **:**, **=**, **?**, **\***

  The ADUC snap-in (via Microsoft Management Console) now features an improved Windows LAPS tab. The Windows LAPS password now appears in a new font that enhances its readability when it appears in plain text.


________________________________


- **Post-authentication action support for terminating individual processes**: A new option is added to the **Post-authentication actions** (PAA) Group Policy setting, `Reset the password, sign out the managed account, and terminate any remaining processes`, which is located in **Computer Configuration** > **Administrative Templates** > **System** > **LAPS** > **Post-authentication actions**.

  This new option is an extension of the previous option, `Reset the password and log off the managed account`. After configuration, the PAA notifies and then terminates any interactive sign-in sessions. It enumerates and terminates any remaining processes that are still running under the local account identity managed by Windows LAPS. No notification precedes this termination.
  
  The expansion of logging events during the execution of PAA provides deeper insights into the operation.

To learn more about Windows LAPS, see [What is Windows LAPS?](/windows-server/identity/laps/laps-overview).



________________________________

^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^


| What to Check         | Checked |
| :-------------------- | ------: |
| Checked.For.NPS:      | N       |
| NPS.No.Risk:          | N       |
| Checked.For.DC:       | N       |
| DC.No.Risk:           | N       |
| Checked.For.WiderUse: | N       |
| WiderUse.No.Risk:     | N       |
| Notes: | Need to test if the new Group Policy setting seeds to DC when access from Server 2025.


### Security baseline

By implementing a customized security baseline, you can establish security measures right from the beginning for your device or VM role based on the recommended security posture. This baseline comes equipped with more than 350 preconfigured Windows security settings. You can use the settings to apply and enforce specific security settings that align with the best practices recommended by Microsoft and industry standards. To learn more, see [OSConfig overview](../security/osconfig/osconfig-overview.md).

| What to Check         | Checked |
| :-------------------- | ------: |
| Checked.For.NPS:      | N       |
| NPS.No.Risk:          | N       |
| Checked.For.DC:       | N       |
| DC.No.Risk:           | N       |
| Checked.For.WiderUse: | N       |
| WiderUse.No.Risk:     | N       |
| Notes: | This has not been vetted for NPS use. This shouldn't effect any other machines and shouldn't interfere unless we enable it. 


#### SMB signing

SMB signing is now required by default for all SMB outbound connections. Previously, it was required only when you connected to shares named **SYSVOL** and **NETLOGON** on Active Directory DCs. To learn more, see [How signing works](../storage/file-server/smb-signing-overview.md#how-signing-works).


| What to Check         | Checked |
| :-------------------- | ------: |
| Checked.For.NPS:      | N       |
| NPS.No.Risk:          | N       |
| Checked.For.DC:       | N       |
| DC.No.Risk:           | N       |
| Checked.For.WiderUse: | N       |
| WiderUse.No.Risk:     | N       |
| Notes: | Will need to be confirmed as not a risk.