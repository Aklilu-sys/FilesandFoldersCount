# FilesandFoldersCount
Synopsis
   This scripts counts the number of files and folders under a main or root folder.
DESCRIPTION
   This scripts counts the number of files and folders under a main or root folder. It also has an option to see only file counts
   or only folder counts using the -File or Folder switch parameters. In the folder lists shown is a new folder named "root", 
   which is the main folder passed as the path parameter.
   .Parameter Path
   This parameter accepts the location of the main folder in which you want to count the files and folders.
Parameter File
   This parameter lets you see file counts.
Parameter Folder
   This parameter lets you see Folder counts.
EXAMPLE
   To get the file counts under the folder located at "C:\Users\User\Documents", use the below example
   
   .\FileandFolderCount.ps1 -Path C:\Users\User\Documents -File
   
EXAMPLE 
   To get the folder counts under the folder located at "C:\Users\User\Documents", use the below example
   
   .\FileandFolderCount.ps1 -Path C:\Users\User\Documents -Folder
   
EXAMPLE
   To get both the file and folder counts under the folder located at "C:\Users\User\Documents", use the below example (without typing -File or -Folder). The same result will be shown if both the -File and -Folder switch parameters are given to the script.
   
   .\FileandFolderCount.ps1 -Path C:\Users\User\Documents
