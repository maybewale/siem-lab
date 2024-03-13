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

~visit this repo for more indepth steps - https://github.com/MyDFIR/SOC-Automation-Project/blob/main/mac-M1-M2-M3-Instructions

lets begin 😈

### Wazuh cloud instance 

no dependencies required
install wazuh and extract credentials 
  
![image](https://github.com/maybewale/siem-lab/assets/78131867/fe5d40f5-bcc9-4522-8b23-4425782632fc) 


### TheHive cloud instance

install java
install cassandra
![image](https://github.com/maybewale/siem-lab/assets/78131867/3714c990-a435-437b-9615-e952991fe8e9)

install elastic search
![image](https://github.com/maybewale/siem-lab/assets/78131867/012b9726-1573-4bf6-9272-7d8b3f175d21)

install the hive
![image](https://github.com/maybewale/siem-lab/assets/78131867/73339c0b-90af-4e82-aa37-6b1b2dd25f79)


### Windows VM

setup sysmon and configure
![image](https://github.com/maybewale/siem-lab/assets/78131867/0022b3bb-abaf-4be7-ab8d-60cf9eee1070)

## -

### Now lets configure the hive

starting with cassandra
`nano /etc/casssandra/cassandra.yaml`

edit cluster name
 
![image](https://github.com/maybewale/siem-lab/assets/78131867/c26af92f-78b5-43c8-87bf-68ac418546d2)

edit listen, rpc and seed provider -this is the same as the ip address of your machine
 
![image](https://github.com/maybewale/siem-lab/assets/78131867/b8e395a8-bff3-4ca9-a82f-f3f643443bd0)
![image](https://github.com/maybewale/siem-lab/assets/78131867/31b643e8-cbc9-4f7e-9c93-5b95984854ec)
![image](https://github.com/maybewale/siem-lab/assets/78131867/f2cd775f-96dd-4f25-84ce-7c4e3ed46fa9)

stop cassandra

`systemctl stop cassandra.service`

delete old files and restart

`rm -rf /var/lib/cassandra/*`

`systemctl start cassandra.service`
  
![image](https://github.com/maybewale/siem-lab/assets/78131867/42ee75b4-e6ac-4721-a3fb-8439280422f4)

run same process for elasticsearch

`nano /etc/elasticsearch/elasticsearch.yml`

edit cluster name, rpc & seed provider 

![image](https://github.com/maybewale/siem-lab/assets/78131867/b9fb720d-8516-41a2-813b-21b3b0f3a5b1)

start and emable elastic search 

`systemctl start elasticsearch.service`
`systemctl enable elasticsearch.service`

up & running 
![image](https://github.com/maybewale/siem-lab/assets/78131867/ee48323a-0136-4058-bc9d-33581ee262d1) 


now, make sure thehive user and group has access to a file path

`ls -la /opt/thp`

we can see that root user has access here
![image](https://github.com/maybewale/siem-lab/assets/78131867/0336b937-e70e-4686-8aba-11f7742f1191)


now, lets make thehive owner 

`chown -R thehive:thehive /opt/thp`

check again, nice 
![image](https://github.com/maybewale/siem-lab/assets/78131867/bd268e21-a6e5-4bb3-9972-a9f443d9d0bc)


now lets configure thehive configuration file 

`nano /etc/thehive/application.conf`

edit hostmane, IP address & base url 

start and enable thehive.service 
![image](https://github.com/maybewale/siem-lab/assets/78131867/dff80aaa-dcfb-493b-8829-17a362c8831a)


we login with default credntials and got hit with an error

![image](https://github.com/maybewale/siem-lab/assets/78131867/d2416e25-15dd-434b-9ed2-688d43047170)

apparently elasticsearch went down

![image](https://github.com/maybewale/siem-lab/assets/78131867/63d76455-a149-4502-87a2-695e0c1dfbd5)

lets create a custom jvl file

`nano /etc/elasticsearch/jvm.options.d/jvm.options`

paste a command that tells java to limit its memory 4gsb

![image](https://github.com/maybewale/siem-lab/assets/78131867/8b2d2082-7db7-4ccf-a77e-df68b0a0018c)

restart elasticsearch

now we have access to thehive console 

![image](https://github.com/maybewale/siem-lab/assets/78131867/6c8198d1-92ca-400d-8cb8-150fa458c3c5)


### Adding agents into Wazuh

lets add agents into Wazuh, as we can see we have 0 agents 

![image](https://github.com/maybewale/siem-lab/assets/78131867/11991df2-94ae-4697-ac16-d585d8c6a23e)

i'll be deploying a Windows agent...i'll input the IP add of the wazuh and then copy the command in green circle to my windows powershell 

![image](https://github.com/maybewale/siem-lab/assets/78131867/c87add15-29e3-48ce-acae-14c4885bb9c3)

wazuh is installed 

![image](https://github.com/maybewale/siem-lab/assets/78131867/b96d5767-808e-4a40-988b-dba1f67d4ea3)

start the service 

![image](https://github.com/maybewale/siem-lab/assets/78131867/a2bbbc2c-3e8c-428a-83ae-833dcb922114)

back on the wazuh dashboard, we can see that our agent has been synced. ggs 

![image](https://github.com/maybewale/siem-lab/assets/78131867/254dc4b3-3eb1-4886-8588-c7956c330af0)

### Lets generate telemetry from our agent and ingest into Wazuh

lets find out wazuh configuration file on the Win10 agent 
c:/Program Files (x86)/ossec-agent/ossec.conf

![image](https://github.com/maybewale/siem-lab/assets/78131867/59014739-6d97-4f8b-9013-09220bce217a)

i integrate previously installed sysmon and powershell into wazuh

![image](https://github.com/maybewale/siem-lab/assets/78131867/32ff21fc-fc7a-4400-9482-67ab6bab6a01)

then restart wazuh in the services tab. 

`any time you make a change, always restart the service`

*back into wazuh*

we an see that logs are already being sent and ingested 

![image](https://github.com/maybewale/siem-lab/assets/78131867/0ace3c48-a7d4-4098-a9d8-9b71f3346896)

now lets download a know credential extractor mimikatz
execute the .exe file 

![image](https://github.com/maybewale/siem-lab/assets/78131867/59cc9ea3-927a-4450-bb14-6ab6334b6f21)

but our wazuh did not log anything because `sysmon` didnt repoort it

![image](https://github.com/maybewale/siem-lab/assets/78131867/9863fc99-9d45-4d8e-875f-54bd862405f7)

lets fix that :(  -  we're gonna modify the ossec.conf file  
make a backup

![image](https://github.com/maybewale/siem-lab/assets/78131867/51d7d953-c85d-4fe9-a7d9-f6ab8e084cfb)

edit the log all section to yes 

![image](https://github.com/maybewale/siem-lab/assets/78131867/0de05f34-7929-4ebb-81d8-e9715118bf26)

restart the wazuh-manager.service. what i just did will force wazuh to archive all the logs into this dir.

![image](https://github.com/maybewale/siem-lab/assets/78131867/8cbaa656-ee1e-4073-b1d7-7b1bae63eaab)

to start injestion, lets change the config of filebeat 

![image](https://github.com/maybewale/siem-lab/assets/78131867/e67fc12d-6bf8-4777-a353-8a7fa0642236)

restart filebeat.service

*back to wazuh* lets update our indexer to view these logs 

goto stack management, index patterns, create index pattern
create new index wazuh-archives-**
save 

![image](https://github.com/maybewale/siem-lab/assets/78131867/16d493cf-47bc-4828-a489-d1b8b964c2e1)

back to discover 
select the new index

![image](https://github.com/maybewale/siem-lab/assets/78131867/2f1990a9-b763-4a4d-8a0f-80f5c5de7065)


