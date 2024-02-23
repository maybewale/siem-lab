# siem lab using WAZUH + THEHIVE

### Skills Gained

- Advanced understanding of SIEM concepts and practical application.
- Proficiency in analyzing and interpreting network logs.
- Ability to generate and recognize attack signatures and patterns.
- Enhanced knowledge of network protocols and security vulnerabilities.
- Development of critical thinking and problem-solving skills in cybersecurity.

### Tools Used

- Security Information and Event Management (SIEM) system for log ingestion and analysis.
- Network analysis tools (such as Wireshark) for capturing and examining network traffic.
- Telemetry generation tools to create realistic network traffic and attack scenarios.


## Steps

*Ref 1: Network Diagram*

![image](https://github.com/maybewale/siem-lab/assets/78131867/1dbfc263-34bb-4e0d-adb8-857da97072eb)

*made use of 3 machines for this lab. 2 Ubuntu cloud machine and 1 on-prem windows machine.*

**visit this repo for more indepth steps - https://github.com/MyDFIR/SOC-Automation-Project/blob/main/mac-M1-M2-M3-Instructions** 



lets begin 😈


**Wazuh cloud instance** 

> no dependencies required
> install wazuh and extract credentials 
  
> ![image](https://github.com/maybewale/siem-lab/assets/78131867/fe5d40f5-bcc9-4522-8b23-4425782632fc) 



**TheHive cloud instance**

> install java
> 
> install cassandra
> >![image](https://github.com/maybewale/siem-lab/assets/78131867/3714c990-a435-437b-9615-e952991fe8e9)
>
> install elastic search
> >![image](https://github.com/maybewale/siem-lab/assets/78131867/012b9726-1573-4bf6-9272-7d8b3f175d21)
>
> install the hive
> >![image](https://github.com/maybewale/siem-lab/assets/78131867/73339c0b-90af-4e82-aa37-6b1b2dd25f79)


**Windows VM**

> setup sysmon and configure
> >![image](https://github.com/maybewale/siem-lab/assets/78131867/0022b3bb-abaf-4be7-ab8d-60cf9eee1070)

## -

**Now lets configure the hive**

> starting with cassandra
> > nano /etc/casssandra/cassandra.yaml
> > 
> > > edit cluster name
> > > 
> > > >![image](https://github.com/maybewale/siem-lab/assets/78131867/c26af92f-78b5-43c8-87bf-68ac418546d2)
> > >
> > > edit listen, rpc and seed provider -this is the same as the ip address of your machine
> > > 
> > > > ![image](https://github.com/maybewale/siem-lab/assets/78131867/b8e395a8-bff3-4ca9-a82f-f3f643443bd0)
> > > > ![image](https://github.com/maybewale/siem-lab/assets/78131867/31b643e8-cbc9-4f7e-9c93-5b95984854ec)
> > > > ![image](https://github.com/maybewale/siem-lab/assets/78131867/f2cd775f-96dd-4f25-84ce-7c4e3ed46fa9)
> > > >
> > > 



