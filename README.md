# FilesandFoldersCount

<#
.Synopsis
   This scripts counts the number of files and folders under a main or root folder.
.DESCRIPTION
   This scripts counts the number of files and folders under a main or root folder. It also has an option to see only file counts
   or only folder counts using the -File or Folder switch parameters. In the folder lists shown is a new folder named "root", 
   which is the main folder passed as the path parameter.
   .Parameter Path
   This parameter accepts the location of the main folder in which you want to count the files and folders.
.Parameter File
   This parameter lets you see file counts.
.Parameter Folder
   This parameter lets you see Folder counts.
.EXAMPLE
   To get the file counts under the folder located at "C:\Users\User\Documents", use the below example
   .\FileandFolderCount.ps1 -Path C:\Users\User\Documents -File
.EXAMPLE 
   To get the folder counts under the folder located at "C:\Users\User\Documents", use the below example
   .\FileandFolderCount.ps1 -Path C:\Users\User\Documents -Folder
.EXAMPLE
   To get both the file and folder counts under the folder located at "C:\Users\User\Documents", use the below example (without typing -File or -Folder). The same result will be shown if both the -File and -Folder switch parameters are given to the script.
   .\FileandFolderCount.ps1 -Path C:\Users\User\Documents
.INPUTS
   Inputs to this cmdlet (if any)
.OUTPUTS
   Output from this cmdlet (if any)
.NOTES
   General notes
.COMPONENT
   The component this cmdlet belongs to
.ROLE
   The role this cmdlet belongs to
.FUNCTIONALITY
   The functionality that best describes this cmdlet
#>

Param(
    [cmdletBinding()]
    [Parameter(Mandatory=$true)]
    [ValidateScript({test-path $_})]
    $Path = (Read-Host 'What is the filepath?'),
    [switch]$Folder,
    [switch]$File

)

Begin{

$ChildDirectories = @(Get-ChildItem -LiteralPath $Path -Directory)
$ChildFiles = @(Get-ChildItem -LiteralPath $Path -File)

}

Process{

    if(($File -eq $true) -and ($Folder -eq $false)){
        $objFile=@()
        foreach ($Item in $ChildDirectories)
        {

            [String]$ItemName = $Item.Name
            [String]$Folderpath = Join-Path -path "$Path" -childpath "$Item"
            $FileCount=(Get-ChildItem -LiteralPath $Folderpath -File -Recurse).count
            $RootFolderFileCount = $ChildFiles.count

            $Prop=@{   'FolderName'=$ItemName;
                       'FileCount'=$FileCount;
            }

            $ObjFile += New-Object -TypeName PSObject -Property $Prop
 
        }
 
 
        $Prop1=@{      'FolderName'="RootFolder";
                       'FileCount'=$RootFolderFileCount;
        }

        $ObjFile += New-Object -TypeName PSObject -Property $Prop1   
  
        Write-Output $ObjFile | Select-Object FolderName,FileCount | Sort-Object FileCount -Descending

    }

    if(($File -eq $false) -and ($Folder -eq $true)){
        $objFolder=@()
        foreach ($ChildFolder in $ChildDirectories){

            [String]$Foldername = $ChildFolder.Name
            [String]$Folderpath = Join-Path -path "$Path" -childpath  "$ChildFolder"
            $FolderCount=(Get-ChildItem -LiteralPath $Folderpath -Directory -Recurse).count
               
            $Prop=@{   'FolderName'=$Foldername;
                       'FolderCount'=$FolderCount;
           
            }
            $ObjFolder += New-Object -TypeName PSObject -Property $Prop
 
        }

        Write-Output $objFolder | Select-Object FolderName,FolderCount | Sort-Object FolderCount -Descending
    }


    if(($File -eq $false) -and ($Folder -eq $false)){
        $objItem=@()
        foreach ($ChildItem in $ChildDirectories){

            [String]$Foldername = $ChildItem.Name
            [String]$Folderpath = Join-Path -path "$Path" -childpath  "$ChildItem"
            $FileCount1=(Get-ChildItem -LiteralPath $Folderpath -File -Recurse).count
            $FolderCount=(Get-ChildItem -LiteralPath $Folderpath -Directory -Recurse).count

            $Prop=@{   'ItemName'=$Foldername;
                       'FolderCount'=$FolderCount;
                       'TotalFileCount'=$FileCount1;
                       'FileType'="Folder"
            }
            $ObjItem += New-Object -TypeName PSObject -Property $Prop
        }

        $Prop1=@{      'ItemName'="RootFolder";
                       'FolderCount'=(Get-ChildItem -LiteralPath $Path -Directory).count;
                       'TotalFileCount'=(Get-ChildItem -LiteralPath $Path -File).count;
        }

        $ObjItem += New-Object -TypeName PSObject -Property $Prop1

        Write-Output $ObjItem | Select-Object ItemName,FolderCount,TotalFileCount | Sort-Object TotalFileCount -Descending

    }

    if($File -and $Folder){
        $objItem1=@()
        foreach ($ChildItem in $ChildDirectories){

            [String]$Foldername = $ChildItem.Name
            [String]$Folderpath = Join-Path -path "$Path" -childpath  "$ChildItem"
            $FileCount1=(Get-ChildItem -LiteralPath $Folderpath -File -Recurse).count
            $FolderCount=(Get-ChildItem -LiteralPath $Folderpath -Directory -Recurse).count

            $Prop=@{   'ItemName'=$Foldername;
                       'FolderCount'=$FolderCount;
                       'TotalFileCount'=$FileCount1;
                       'FileType'="Folder"
            }
            $ObjItem1 += New-Object -TypeName PSObject -Property $Prop
        }

        $Prop1=@{      'ItemName'="RootFolder";
                       'FolderCount'=(Get-ChildItem -LiteralPath $Path -Directory).count;
                       'TotalFileCount'=(Get-ChildItem -LiteralPath $Path -File).count;
        }

        $ObjItem1 += New-Object -TypeName PSObject -Property $Prop1

        Write-Output $ObjItem1 | Select-Object ItemName,FolderCount,TotalFileCount | Sort-Object TotalFileCount -Descending

    }

}
End
{
}
