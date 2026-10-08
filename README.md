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


