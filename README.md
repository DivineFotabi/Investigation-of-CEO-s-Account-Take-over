# Investigation-of-CEO-s-Account-Take-over

<img src= "https://github.com/DivineFotabi/Investigation-of-CEO-s-Account-Take-over/blob/fcd1d84c40223d7888a76c83984bfeefadd37679/Cloudora%20setup.png" />

# Objective

In this project I investigated an executive account take over during a client engagement, traced the intial access, found the persistence, scope the victim and deliverd instant report.

# Skills Learned

- KQL
- Entra ID Signin Analysis
- Account Take over Analysis
- MITRE ATT&CK Mapping
- Incident Reporting

# Tools
- Azure Data explore
- Sigin logs
- Audit logs

# Setup/Steps

<img src= "https://github.com/DivineFotabi/Investigation-of-CEO-s-Account-Take-over/blob/2a91fe94e28ebe5d1f7cf7f9956e19fe519e0f10/Ticket.png" /> 

- Ingested in Azure data explorer both the Sigin and the Audit logs.
- Set the time frame for the attack from the 2026-08-10 to 2026-11-10
- Queried the sigin logs to know who came in and from which location.
- Queried the Audit logs to know what the attacker did.
- Check the scope of the attack to see other victims.
- Wrote an incident report.

<img src= "https://github.com/DivineFotabi/Investigation-of-CEO-s-Account-Take-over/blob/7acb121f484405a643ae68d3ea78e4f4a025b4db/Sigin_CL1.png" /> 

The attempt on the CEO's account (daniel.reeve) started on 08/10/2026 at 12:35:48 am from Lagos Nigeria using a Windows 10 device with IP address 102.89.47.17 and had a couple of failed attempt but finally got in on the 10/10/2026 at 3:12:05 am from Lagos Nigeria using a windows 10 device. The original user sigined on the 10/08/2026 at 8:41:00 am. Same CEO's account and two signed ins from two location within a very short time interval.

<img src= "https://github.com/DivineFotabi/Investigation-of-CEO-s-Account-Take-over/blob/ee62848942a60c81fef64c008b5354cd27ea869c/Sigin_CL2%20NTP.png" /> 

From the above command we noticed a whole lot of sigin ins in London and three different Lagos Ip addresses within two hours interval which does not look like a usual between Lagos and London.
Looking at another account with almost thesame sign in locations we noticed the time is within range of travel and possibly the user might be on a business trip.

<img src= "https://github.com/DivineFotabi/Investigation-of-CEO-s-Account-Take-over/blob/4b413e60556f98fc808930be889a5e82c4e17486/omar.png" /> 

Analyzing the failed attempt from the sign in logs we notice 3 different Ip's from Nigeria attempting ten accounts and more. This show a possibile password spray attack by the attacker and not a brute force.
<img src= "https://github.com/DivineFotabi/Investigation-of-CEO-s-Account-Take-over/blob/eeaacec7c35ab734d9f35b9a4c10ad14d9c6ef21/Failed%20Logins.png" /> 

Confirming this we run the command below within a 24hours time frame and recorded the following below.

<img src= "https://github.com/DivineFotabi/Investigation-of-CEO-s-Account-Take-over/blob/153413a7eb6fc8d6e3f066f40805e9d505403652/confirming%20psswd%20Spary.png" /> 

When the attacker got in they created two persistence methods that is using the Pixel 6 device for authentication and establishing a RSS Subscriptions rule.

<img src= "https://github.com/DivineFotabi/Investigation-of-CEO-s-Account-Take-over/blob/b2bdf01e3dd7006c843f719454ddb9ceffb92177/what%20they%20did.png" />

Determinig the scope of the accounts to see other victms I noticed that the attacker had access to two accounts and carried attempted breached on 24 others

<img src= "https://github.com/DivineFotabi/Investigation-of-CEO-s-Account-Take-over/blob/fd7e3db8fbf92f6d9dd0f79476513b3a4cba4639/Scope.png" /> 


<img src= "https://github.com/user-attachments/assets/8ae33e43-2aed-461b-a33e-a885f1cf0499" /> 


More analysis on the other victim priya.nair to check on more potential backdoors and which other persistence method the victim used. 

<img src= "https://github.com/DivineFotabi/Investigation-of-CEO-s-Account-Take-over/blob/08a90bbcad9a1e619fe936618b86c8d7cc926e1a/More%20Persistence.png" />

The results show no Multifacator authentication method was used or Rule set. This doesnot imply the victim is totally free it shows the lack of facts from the logs hance all the methods of containments need to to be applied still on the priya.nair victim.

## Incident Report

# SUMMARY
From the 8 to the 10 August 2926, an external attacker tried commonly used password to access 26 accounts owned by Cloudora staff. The three IP address used for this attacker was verified to come from Nigeria. On the 10 of August at 3:12am the attacked got a correct password to an account that was owned by Daniel Reeve who happened to be the CEO and at 3:47am he got access to another staff account Priya Nair. On the CEO'S account the attacker set uo thier own authentication method ( a pixel device) and created a rule to enable them divert invoice related emails.
The breached was discovered the same morning and both accounts were kept under control, the device and the rule was removed and the attacker's network address blocked. No data theft of data or payments was recorded. The other 24 accounts that were targeted but not breached were all flaged for password reset 

# Timeline

| Time (UTC) | Source | Event |
|------------|--------|-------|
| Aug 08, 00:00-05:00 | Sign-in | Password spray night 1:44 failed sign-ins from 102.89 IPs across 20 accounts |
| Aug 09, 00:00-05:00 | Sign-in | Password spray night 2: 36 failed sign-ins, same source IPs |
| Aug 10, 00:35;48 | Sign-in | Spray night 3 begins; Failed attempt against daniel reeve from 102.89.44.17 |
| Aug 10, 03:09:12 | Sign-in | Failed sign-in, daniel.reeve 102.89.44.17 lagos |
| Aug 10, 03:10:41 | Sign-in | Failed sign-in, daniel.reeve, 102.89.44.17 |
| Aug 10, 03:12:05 | Sign-in | Successful sign-in, daniel.reeve, 102.89.44.17, Windows 10 / Chrome 125 - initial access |
| Aug 10, 03:14:30 | Sign-in | daniel.reeve mailbox accessed via Outlook Web from 102.89.44.17 |
| Aug 10, 03:18:44 | Sign-in | Attacker registers own authenticator app on daniel.reeve, device 'Pixel 6' |
| Aug 10, 03:26:02 | Sign-in | daniel.reeve session opens Azure Portal from 102.89.44.17 |
| Aug 10, 03:31:09 | Audit |  Inbox rule 'RSS Subscriptions' created on daniel.reeve: mail from finance*cloudora.io or containing 'invoice' moved to RSS Feeds and marked read |
| Aug 10, 03:44:55 | Sign-in | Failed sign-in, priya.nair, 102.89.45.101 | 
| Aug 10, 03:47:18 | Sign-in | Successful sign-in, priya.nair, 102.89.45.101 - second account compromised |
| Aug 10, 03:52:40 | Sign-in | priya.nair session accesses SharePoint Online from 102.89.45.101 |
| Aug 10, 08:41:00 | Sign-in | Daniel Reeve's genuine sign-in from London (203.0.113.11, macOS/Safari) - account in use from two countries 5.5 hours apart |
| Aug 10, 08:55 | Ticket | IT admin flags impossible travel on the CEO account; ticket CLD-0001 raised, investigation starts |
| Aug 10, ~10:30 | Response | Containment executed on both accounts |

# Findings

1) The CEO's account daniel reeve access was obtained at 03:12:05 UTC on the 10 Aug from a window 10 device that happened to be in Lagos Nigeria having a network address of 102.89.44.17. The success was immediately followed by multiple fail attemptsat 03:09 and 03:10 and mailbox access (Outlook web). Aure Portal access at 03:26). The accounts original location is United Kingdom where Daniel sign's in from London at 08:41 the same morning usingd a macOS device. Travel and VPN's where not used in the failure pattern as the unfamiliar devices were on the same day as the London activity.
2) The lagos IPs (3) produced 114 failed sign-in (ResultType 50126) across the 26 accounts from the 8 to 10 Aug. Between 00:00 and 05:00 UTC 102.89.44.17 registered 48 failures over 23 accounts, 102.89.45.101 registered 38 failures over 20 accounts and 102.89.44.23 registered 28 failures over 18 accounts.The pattern seen in this failure signifies password spraying (T1110.003 as per the MITRE ATT&CK).
3) Multifactor Authentication a persistent method was recorded at 03:31:09 that is six minutes after the intial access. The Audit logs showed that the attacker used a Pixel 6 device as register security tool on daniel reeve. With this the attacker will survive a password reset which is why credntial reset is not the ideal method to deal with this.
4) A Business Email Compromise was done by the attacker through the setting up of a 'RSS Subscription Rule' to divert payment invoices from the finance departments to the RSS feeds folder and marked read. The main reason for this is to hide finance transactions from the mail owner but no finance theft was recorded in the logs provided.
5) Not just daniel was successfully compromised. The attacker also had access to a member of staff's account Priya Nair from 102.89.45.101 at 03:47:18 with a shared point access. The account was based in London with no foreign activity. No persistence was noticed in the account but the account still needed to be contained.
6) Omar Farah shows 12 sign-in from Dubai from the 8-10 Aug. All these activities took place during the day and a successful sign-in on first trial and no failures all coming from thesame iOS 17 device. This show a real travel travel history but since still among the breached a password reset will be carried out.

# Indicators of Compromise

| Type | Value | First Seen | Context |
|------|-------|------------|---------|
| IPV4 | 102.89.44.17 | Aug 08, at 00:00 | Lagos, Nigeria Spray source; CEO initial access,MFA registration, inbox rule |
| IPV4 | 102.89.44.23 | Aug 08, at 00:00 | Lagos, Nigeria Spray source (28 failures, 18
accounts |
| IPv4 | 102.89.45.101 | Aug 08, at 00:00 | Lagos, NG. Spray source; priya.nair compromise |
| Device fingerprint | Windows 10 / Chrome 125 | Aug 08 | Consistent attacker fingerprint on all 102.89.x.x activity |
| MFA device name | 'Pixel 6' (authenticator app)'Pixel 6' (authenticator app) | Aug 10, 03:18:44 | Attacker-registered MFA on daniel.reeve removed |
| Inbox rule name | 'RSS Subscriptions' | Aug 10, 03:31:09 | BEC mail-hiding rule on daniel.reeve - deleted |

# MITRE ATT&CK; mapping

| Tactic | Technique ID | Technique name | Evidenced by |
|--------|--------------|----------------|--------------|
| Credential Access | T1110.003 | Brute Force: Password Spraying | Finding 2 - 114 failures, 26 accounts, 3 nights, 3 IPs |
| Initial Access | T1078 | Valid Accounts | Findings 1 and 5 - successful logins with sprayed credentials |
| Persistence | T1098.005 | Account Manipulation: Device Registration | Finding 3 - attacker authenticator 'Pixel 6' |
| Defense Evasion | T1564.008 | Hide Artifacts: Email Hiding Rules | Finding 4 - inbox rule 'RSS Subscriptions' | 
# Scope
1) Daniel.reeve&cloudora.io (03:12:05 UTC) and priya.nair&cloudora.io (03:47:18 UTC), both 10 Aug, both from
102.89.x.x attacker infrastructure, evidenced by successful sign-ins in CloudoraSignIn_CL.
2) The spray Targeted 26 distinct account; excluding the two breached accounts, 24 recieved failed attempts and required password reset
alba.vega, amelia.frost, aria.reid, cole.burke, dina.said, emma.hayes, ethan.wells, freya.lynn, gwen.muir, isla.grant, joel.kerr, jude.ross, kian.patel, leah.stone, lena.voss, liam.doyle, mira.shah, nina.cole, omar.farah, rhys.owen, ruth.dean, ryan.boyd, seth.lane, sofia.marino (all @cloudora.io).
3) omar.farah&cloudora.io. Dubai sign-ins assessed as legitimate travel . Remains in the reset list above as a spray target.

# Action Taken

| order | Action | Why this matters |
|-------|--------|------------------|
| 1 | Revoked all active sessions and refresh token for daniel.reeve and priya.nair | Kills live attacker session immediately; a reset without revocation leaves existing session working |
| 2 | Reset credentials for both compromised accounts | New credentials only matter once old session are dead |
| 3 | Removed attacker MFA method 'pixel 6' from daniel.reeve; reviewed priya.nair MFA methods (no attacker method found) | New credentials only matter once old sessions are dead |
| 4 | Deleted inbox rule 'RSS Subscription' from daniel.reeve mailbox; audited both mailboxes for futher rules | Removes the Business Email Compromise staging; rules persist independently of credentials |
| 5 | Blocked 102.89.44.17, 102.89.44.23, 102.89.45.101 via conditional Access / named locations | Cuts the know attacker infrastructure off from the tenant |
| 6 | Verified redo Step 1 and Step 5 queries -no 102.89 activity after containment; forced re-authentication on both accounts | Containment is a claim until you prove it in the logs|

# Recommendations

1) Force password resets for the 24 targeted-but-not-breached accounts listed in section 6 - the spray may have come closer than the logs show.
2)  Require MFA on all accounts and disable legacy authentication protocols that bypass it; two accounts fell to password-only guessing.
3)   Alert on new MFA method registration and new inbox rules for executive accounts - both attacker persistence actions were loggable but unmonitored.
4)    Deploy a password-spray detection rule (failures per IP across distinct accounts in a rolling window) this attack wasvisible from night one, two days before the breach.
5)  Review Conditional Access: block or step-up authentication from countries with no business presence.
6)   Brief the finance team on BEC risk before the enterprise deal closes: verify any payment-detail change by phone on a known number.

# Lesson Learned

Detection relied on a human noticing impossible travel five and a half hours after initial access; a basic spray rule would have alerted on night one, roughly 51 hours earlier. The audit log turned a password incident into the real story - without it, containment would have missed the Multifactor Authentication back door and the business email compromise rule. Scoping past the first victim found the second
account within minutes of the same query. Baseline comparison (Daniel vs Omar) prevented both a missed detection and a false accusation, and belongs in every identity investigation.
