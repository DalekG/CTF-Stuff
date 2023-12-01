# Insider Insidiousness

### Background
Your pentesting company has been asked by the Security Manager of Questron, Inc. to demonstrate the post-exploitation capabilities of an APT or an insider threat. With this, Questron's IT department and network security team can improve their detection abilities and overall security posture.

### Getting Started
You have been given access to a Windows 10 host on the Questron network as a domain administrator. You must complete a series of tasks related to files in a shared directory on the machine named FS01. Standard access to the file share has been disabled, so you should use remote queries to find the answer to each task question (WMI and/or PSRemoting are recommended tactics).

`nslookup FS01`

IP Address: 10.10.10.213
Computer Name: FS01.questron.com

### Question 1: Which of the top-level directories in the Questron file share is a hidden directory?

```
Enter-PSSession -computer FS01.questron.com

dir

cd C:\

cd .\Shares\

cd .\Questron\

dir -h
```

Headquarters

Directory = Headquarters

### Question 2: Based on the resources found under the Planning directory, name one of the Questron partner organizations.
```
cd .\Management\

cd .\Planning\

cd .\Partners\

dir
```

Digimon\
Midway\
Techmore

Partners = ^listed above^

### Question 3: Name the Questron project that is not named after a President (excluding the STONEHENGE project).
```
cd ../../../

cd .\Questron\

cd .\Operations\

cd .\Projects\

dir
```

Find the project not names after a President (Gladiator)

### Question 4: You have been asked to identify information within the Project Charter of the STONEHENGE project. View the project documents and give the last name of the Project Lead.

Open new powershell window

```
$MYSESSION = Start-PSSession -computer FS01.questron.com

Copy-Item -FromSession $MYSESSION <file-path>\ProjectCharter_STONEHENGE.docx .\Desktop\
```

Open file from desktop and find project lead name

Project Lead = Anderson

### Question 5: Locate an excel spreadsheet containing a roster of Questron personnel. According to the roster, in what year did COO, Charles Lewis, begin working for Questron? (Hint, this is a hidden file )

On FS01 make sure the directory is C:\Shares\Questron\
```
cd .\Administration\'Human Resources'\Personnel\

dir -h
```

See Roster.xls as a hidden file

`atrrib .\Roster.xls\ -h`

On local powershell session do:

`Copy-Item -FromSession $MYSESSION <filepath>\Roster.xls .\Desktop\`

Open file on desktop and find Charles Lewis, look at the StartDate

Year Started = 1996
