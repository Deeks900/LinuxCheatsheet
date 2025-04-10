**📌Transferring Files from Windows to Linux Machine**

    :: stop printing on the terminal
    @echo off
    
    ::Get the Inputs from the user
    set "src_folder=%1" 
    set "year=%2" 
    set "dest_user=%3" 
    set "dest_pass_b64=%4" 
    set "dest_server=%5" 
    set "dest_path=%6"
    
    :: CONVERT B64 
    FOR /F "tokens=*" %%a in ('powershell -Command "[System.Text.Encoding]::ASCII.GetString([System.Convert]::FromBase64String('%%dest_pass_b64%%'))"') do SET dest_pass=%%a 
    :: echo %dest_pass%
    
    :: Folder path trailing slash 
    if not "%src_folder:~-1%" == "" ( 
          set "src_slash=\" 
          ) else ( 
          set "src_slash="
    )
    
    ::Now loop on the desired folder 
    for %%F in (%src_folder%%year%*.zip) do (
            pscp -pw "%dest_pass%" 
    "%%F" %dest_user%@%dest_server%:%dest_path%/%year%
    )
    
    📍Now arguments to this can be passed like:
    scriptName.bat "pathOnWindowsMachine" "argLikeYearCanBePassedToIdentifyFolder" "linuxMachineUsername" "linuxMachinePasswordInBase64" "linuMachineName" "targetPathOnLinux" >"pathWhereLogsShouldBeCreated" 2>&1
    
    📍pscp is primarily a Windows Utility for secure file transfer over SSH. It’s a part of Putty. 
    
    📍If you wish to transfer a folder over pscp then use recursive flag: 
      pscp -r sourceWindowsPath remoteMachineUsername@remoteMachineName:targetPathOnRemoteMachine

📌**Transferring file between Two Linux Machines** 

Using **scp** -> Securely copies between 2 machines over SSH

📍From your local machine to remote Machine 

    scp pathOnyourMachine remoteUsername@remoteHost:targetPathOnRemoteMachine

📍From remote machine to your local Machine 

    scp remoteUsername@remoteHost:/pathToRemoteFile /pathToLocalDirectory

📍Copying an entire directory Recursively 

    scp -r /sourcePath username@remoteHost:/destinationPath

Using **sftp** ->Interactively transfers files over SSH

📍starting the session - 

    sftp username@machineName 

📍uploading a file – 

    put /pathToFile /pathOnRemote (-r flag for Recursive in case folder is there) 

📍downloading a file – 

    get /pathOnRemote /pathOnyourMachine

📌**Nohup** -> No hang up .Can use this in case where you want to run a process in the background and keep it running even after you close the terminal or log out. 

    nohup command > /dev/null 2>&1 & 

`nohup command > output.log 2>&1 
&` 

 `& -> make sure to run the process in background and give you terminal back instantly.` 

    2>&1 -> Redirection mechanism in linux. 
    1 -> Standard Output
    2 -> Standard Error 
     This says that send both the things to the sample place like output.log 

    /dev/null -> Special file in linux that discards any data written to it. Use it when you don’t care about the output of your command.


📌Suppose you are in situation where you have space issues and want to run a script that can cleanup some specific named style files like every 24 hours. You can use :

     #!/bin/bash
    while true; do 
        find /home/folderName -name 'krb5cc_*' -mmin +10 -delete 
        sleep 86400 
    done

📌Suppose you have a file which you want that should get triggered every 1 second then also you can write a script like: 

    #!/bin/bash
    while true; do
          /pathToYourFileLocation/abc.cpp > /dev/null 2>&1 
          sleep 2
    done
    
    And then can run this shell script itself with no hup - nohup ./def.sh > /dev/null 2>&1 &

📌Suppose you want to check that on your linux machine which process is consuming how much CPU, memory etc. 



    Write top -> Press M(sort Output on base of memory consumption) -> Press q(quit)`

📌Now let’s see some more useful commands : 
| Command | Use |
|--|--|
📍find /home/pathToDirectory -name 'fileNamePattern_*' -mmin +10 \| wc -l | Find command is usedto search for files and directories based on conditions you give.wc -l is used to count number of lines.
📍find /home/pathToDirectory -name 'fileNamePattern_*' -mmin +10 -delete |Find [path] [expression]
📍find /home/pathToDirectory -name 'fileNamePattern_*' -mmin +10 \| head -n 10 | head and tail -> Display beginning and end respectively
📍find /home/pathToDirectory -name 'fileNamePattern_*' -mmin +10 \| tail -n 10 | head and tail -> Display beginning and end respectively
📍df -h .  | Display the disk space for the current directory(if . is used) 
📍du -sh * | Display disk usage of each file and directory in the current directory.h basically says human readable. 
📍ps aux \| grep processName \| grep -v grep | Displays Process named processName. x make sure to show even the background processes.grep -v grep will be removing the line having term grep in it, otherwise grep processName line will also appear in your results as it have word processName in it. 
📍nohup rm -rf /data/folderName > delete.log 2>&1 & | You can delete a folder and its contents using nohup like this. 
📍nohup tar -xzvf /path /abc.tar.gz -C /data/folderName > untar.log 2>&1 &  |This command can extract the contents of a compressed archive file in linux-C flag ensures to specify directory where contents should be extracted.If directory will not be there, it will create that for you. 
📍nohup tar -czvf abc.tar.gz abc/ > tar.log 2>&1 &| It will be creating a compressed archive file for you.Note : \/after abc ensures that only contents of this particular folder will be archived.The folder itself will not be included. 
📍find . -maxdepth 1 -name "_.pdf" \| wc -l  | Gives count of all pdf documents in Current Folder 
📍nohup find . -maxdepth 1 -type f -name "_.pdf" -exec rm {} + > /dev/null 2>&1 & | Delete all pdf documents from current folder 
📍find . -name "_Deeksha_" ! -name "_Deeksha Sharma_" -exec cp '{}' "/path/folderName" \\; | This command is first finding all the files in current directory which have substring Deeksha in their names but Deeksha Sharma as a whole substring should not be there in those file names.Then we are copying all those files to destined folder 
📍ls -1 \| wc -l |Does not return extra header row 
📍ls -l \| wc -l | Returns one extra header row 
📍find . -name "_2015 Deeksha_" -exec ls '{}' \\; |Use of ls command with find
📍chmod -R u+w folderName | To deal with Write protected error 
📍nohup bash -c 'for zip in *.zip; do unzip "$zip" -d /outerFolder/folderName/others/; done' & | This unzip all the zip files in a directory 
📍unset TMOUT | Disable the TMOUT environment variable.Shell will remain open indefinitely even if there is no activity.
|--|--|
   
   



 
 
