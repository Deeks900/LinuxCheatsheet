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

   
   



 
 
