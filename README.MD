---
title: How To Run Prism Scripts To Automate Analysis
author: Nicole Whealtey
date: 20221021
---
# How To Run Prism Scripts To Automate Analysis 😊

Friday October 21

<!-- - [How To Run Prism Scripts To Automate Analysis](#how-to-run-prism-scripts-to-automate-analysis) -->
- [How To Run Prism Scripts To Automate Analysis 😊](#how-to-run-prism-scripts-to-automate-analysis-)
- [What do the Script Do?](#what-do-the-script-do)
  - [What the scripts do for you](#what-the-scripts-do-for-you)
  - [What you do for the scripts](#what-you-do-for-the-scripts)
- [What you need prior to starting..](#what-you-need-prior-to-starting)
  - [1. Properly formatted .CSV files.](#1-properly-formatted-csv-files)
    - [One File = DataTable.](#one-file--datatable)
  - [2. Properly Named Files](#2-properly-named-files)
  - [3. Files Saved in a Folder](#3-files-saved-in-a-folder)
  - [4. Know the Total Number of CSV Files you wish to import](#4-know-the-total-number-of-csv-files-you-wish-to-import)
  - [5. Save These 2 Scripts](#5-save-these-2-scripts)
    - [`DuplicateFamilyNTimes.pcz`](#duplicatefamilyntimespcz)
    - [`ImportDataTables.pcz`](#importdatatablespcz)
- [Instructions](#instructions)
  - [Create First Prism Family](#create-first-prism-family)
  - [Edit One Number In `DuplicateFamilyNTimes` Script](#edit-one-number-in-duplicatefamilyntimes-script)
  - [Edit One Line in `ImportDataTables` Script](#edit-one-line-in-importdatatables-script)
  - [🎉 Congratulations! 🎉](#-congratulations-)
- [Techy Tips 👓](#techy-tips-)
  - [Get Folder Path](#get-folder-path)
    - [On Mac](#on-mac)
      - [Finder](#finder)
      - [Terminal](#terminal)
    - [On Windows](#on-windows)


# What do the Script Do?

## What the scripts do for you

  🤖 Duplicate Family of DataTable `X` Times

  🤖 Clear Each Data Table

  🤖 Import all CSV files from `Folder` into consequitive DataTables, one after another

  🤖 Updates Table Names and Graph Titles To the corresponding Filename

## What you do for the scripts

  👉 You create the first DataTable + Analysis + Plot Family in Prism

  👉 You decide `X` # of duplicate families to create

  👉 You save the `X` # of csv files in a `Folder` on your computer

  👉 You properly format the csv files


# What you need prior to starting..

## 1. Properly formatted .CSV files.
  ### One File = DataTable.

The script will directly import the data from csv file, so it must already be in correct format.

Here is <b>one</b> example of properly formatted data for a XY data analysis where Y has 3 replicates in the analysis. But, the proper format will vary project by project.


| XValues | Sample1 |       |       | Sample2 |       |       | Sample3 |       |       |
| ------- | ------- | ----- | ----- | ------- | ----- | ----- | ------- | ----- | ----- |
| 0.0014  | 0       | 0     | 0     | 0.1     | 0     | 0     | 0       | 0     | 1.5   |
| 0.0041  | 0       | 0     | 0     | 1.82    | 0     | 0     | 0       | 3.97  | 1.68  |
| 0.012   | 0       | 0     | 0     | 0.41    | 3.76  | 0     | 0       | 6.16  | 1.65  |
| 0.037   | 0       | 0     | 0     | 0.63    | 0     | 0     | 0       | 0     | 0     |
| 0.11    | 0       | 0     | 0     | 19.97   | 20.42 | 18.95 | 15.39   | 14.8  | 17.27 |
| 0.33    | 0       | 0     | 0     | 58.85   | 60.49 | 58.08 | 56.28   | 57.86 | 60.44 |
| 1       | 12.73   | 12.48 | 14.4  | 95.33   | 94.2  | 95.38 | 94.36   | 94.73 | 93.91 |
| 3       | 45.83   | 44.2  | 47.33 | 99.93   | 99.91 | 99.97 | 99.91   | 99.95 | 99.93 |


The first data value (here it is `XValues`) cell must be in the `A1` position/cell.

## 2. Properly Named Files

The file name will become the name of the data table, the graph table, and the graph title.

## 3. Files Saved in a Folder

All the files must be in the same folder. There must not be any other CSV files in this folder, because the Prism script will try to import them all.

## 4. Know the Total Number of CSV Files you wish to import

If you want to double-check the number of CSV files in your folder.

On Mac
1.  Go to your folder in Finder, right click, and select `New Terminal At Folder`
2.  In the terminal window, just copy paste the command <i>using right click paste, not control-v</i>
    ```bash
    ls *.csv | wc -l
    ```
3.  The number of files ending in .csv in that folder will print out

On Windows
1. Go to your Folder in Windows Explorer
2. Open the Folder
3. In the Windows Explorer Address Bar, type "cmd" (but don't include the quotes) and press Enter.
4. This takes you to command promt, where you can paste this garbage:
  ```bash
  (for /r %f in (*.csv) do @type "%f") | find /c /v ""
  ```
5.  The number of files ending in .csv in that folder will print out

## 5. Save These 2 Scripts

### `DuplicateFamilyNTimes.pcz`
  - Open GraphPad Prism
  - File > Run Script > New Script
    - Select `Blank Script`
    - Name of Script: `DuplicateFamilyNTimes` 
    - Press `Create`
    - Copy paste the script below into the window labeled `Your Script`
    - Press `Save`
  
```
ForEach 10
GoTo D 1
DuplicateFamily "Dup %N "
Next
```

###  `ImportDataTables.pcz`
  - Open GraphPad Prism
  - File > Run Script > New Script
    - Select `Blank Script`
    - Name of Script: `ImportDataTables` 
    - Press `Create`
    - Copy paste the script below into the window labeled `Your Script`
    - Press `Save`
  
```
SetPath "/the/path/to/your/folder/goes/here" //tell prism where your folder is that contains CSV files before you run!

OpenOutput ScriptLog

ForEach *.csv 
    WText "file =%F.csv"
    WText %N
    GoTo D %N
    ClearTable
    Import
    WText "import success"
    Next

WText "Datatables are Imported!"

GoTo G

ForEachSheet
	WText "Setting Graph Title to %S"
	SetGraphTitle %S 
	Next

WText "Graph Names are Updated!"

CloseOutput
```

# Instructions

## Create First Prism Family 
- Import single data table
- Do analysis
- Format plots
- Add a layout if you want
- <b>Save A Copy</b> of this file, in case you mess up

## Edit One Number In `DuplicateFamilyNTimes` Script 
  1. File > Run Script > Open Script File
  2. Click on `DuplicateFamilyNTimes.pzc` 
  3. Change the number after `ForEach` to the number of Duplicates you want.
  4. Press `Run` 

To make 30 duplicates
```
ForEach 30
GoTo D 1
DuplicateFamily "Dup %N "
Next
```
To make 15 duplicates
```
ForEach 15
GoTo D 1
DuplicateFamily "Dup %N "
Next
```
 - After the script is finished, you should see all the duplicated familes.

## Edit One Line in `ImportDataTables` Script
  1. File > Run Script > Open Script File
  2. Click on `ImportDataTables.pzc` 
  3. Copy your folder path that contains your CSV Files
    - See How To [Get Folder Path](#Get-Folder-Path) below
  4. Paste your Path in quotes after `SetPath `

  Windows Example
  ```
  SetPath "C:\Users\syoder\Desktop\Data"
  ```
  Mac Example
  ```
  SetPath "/Users/nicolewheatley/Documents/PrismData/Files"
  ```

  5. Press `Run`



## 🎉 Congratulations! 🎉

The script should take ~10 seconds to run.

Then you will see all of your newly imported data.

<br/>
<br>

# Techy Tips 👓

## Get Folder Path

### On Mac 
#### Finder
1. Open Finder
2. Go to Folder that contains your data
3. Right click on the folder
4. You will see there is an option to 'copy' the folder
5. Press Alt, and the Copy option will become 'Copy Folder Path'
6. Click Copy Folder Path while holding down Alt.

#### Terminal
1. Go to your folder in Finder, right click, and select `New Terminal At Folder`
2. type `pwd` and press enter in the terminal.
   - `pwd` stands for 'print working directory'
3. Copy the printed directory.

### On Windows
1. Go to your Folder in Windows Explorer
2. `Shift` + right-click on Folder 
3. Select `Copy as path` on drop down menu.

