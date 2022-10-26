---
title: How To Run Prism Scripts To Automate Analysis
author: Nicole Whealtey
date: 20221021
---
# How To Run Prism Scripts To Automate Analysis 😊

Friday October 21

<!-- - [How To Run Prism Scripts To Automate Analysis](#how-to-run-prism-scripts-to-automate-analysis) -->
- [How To Run Prism Scripts To Automate Analysis 😊](#how-to-run-prism-scripts-to-automate-analysis-)
- [Intro](#intro)
  - [What the scripts do](#what-the-scripts-do)
  - [What you do](#what-you-do)
- [✋ Before You Begin](#-before-you-begin)
  - [Files To Import](#files-to-import)
    - [Formatting](#formatting)
    - [Naming](#naming)
    - [Saved in a Single Folder](#saved-in-a-single-folder)
    - [Know How Many](#know-how-many)
  - [Save These 2 Scripts](#save-these-2-scripts)
    - [1. `DuplicateFamilyNTimes.pcz`](#1-duplicatefamilyntimespcz)
    - [2. `ImportDataTables.pcz`](#2-importdatatablespcz)
- [Instructions](#instructions)
  - [Create First Prism Family](#create-first-prism-family)
  - [Run `DuplicateFamilyNTimes` Script](#run-duplicatefamilyntimes-script)
  - [Run `ImportDataTables` Script](#run-importdatatables-script)
  - [🎉 Congratulations! 🎉](#-congratulations-)
- [Techy Tips 👓](#techy-tips-)
  - [Get Folder Path](#get-folder-path)
    - [On Mac](#on-mac)
      - [Finder](#finder)
      - [Terminal](#terminal)
    - [On Windows](#on-windows)
  - [Find Number Of `.csv` Files In Folder](#find-number-of-csv-files-in-folder)


# Intro

## What the scripts do

  🤖 Duplicate Family of DataTable `X` Times

  🤖 Clear Each DataTable

  🤖 Import all `.csv` files from `Folder` into consequitive DataTables, one after another

  🤖 Updates Table Names and Graph Titles To the corresponding Filename

## What you do 

  👉 You create a single DataTable + Analysis + Plot Family in Prism

  👉 You decide `N` # of duplicate families to want to create

  👉 You save the `N` # of `.csv` files in a `Folder` on your computer

  👉 You properly format the `.csv` file data


# ✋ Before You Begin

## Files To Import
  ### Formatting
   
   One File = DataTable.

The script will directly import one DataTable from one `.csv` file, so it must already be in correct format.

Here is <b>one</b> example of properly formatted data for a XY data analysis where Y has 3 replicates. But, the proper format will vary project by project.


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


The first data value (here it is `XValues`) cell must be in the `A1` position.

### Naming

The file name will become the name of the DataTable, the graph table, and the graph title.

### Saved in a Single Folder

All the files must be saved in the same folder. There must not be any other `.csv` files in this folder, because the Prism script does not know which ones to import.

For example, save the files in a folder called MyFiles on your Desktop.

### Know How Many

If you have a lot of files, you might not know how many there are.

See How to [Find the Number of `.csv` Files in your Folder](#Find-Number-Of-`.csv`-Files-In-Folder) Below

## Save These 2 Scripts

### 1. `DuplicateFamilyNTimes.pcz`
  - Open GraphPad Prism
  - File > Run Script > New Script
    - Select `Blank Script`
    - Name of Script: `DuplicateFamilyNTimes` 
    - Press `Create`
    - Copy paste the script below into the window labeled `Your Script`

```
ForEach 10
GoTo D 1
DuplicateFamily "Dup %N "
Next
```
  - Press `Save`


###  2. `ImportDataTables.pcz`
  - Open GraphPad Prism
  - File > Run Script > New Script
    - Select `Blank Script`
    - Name of Script: `ImportDataTables` 
    - Press `Create`
    - Copy paste the script below into the window labeled `Your Script`
  
```
SetPath "/the/path/to/your/folder/goes/here" //tell prism where your folder is that contains `.csv` files before you run!

OpenOutput ScriptLog

ForEach *.csv 
    WText "file =%F.csv"
    WText %N
    GoTo D %N
    ClearTable
    Import
    WText "import success"
    Next

WText "DataTables are Imported!"

GoTo G

ForEachSheet
	WText "Setting Graph Title to %S"
	SetGraphTitle %S 
	Next

WText "Graph Names are Updated!"

CloseOutput
```
  - Press `Save`

# Instructions
 
## Create First Prism Family 
- Import single DataTable
- Do analysis
- Format plots
- Add a layout if you want
- <b>Save A Copy</b> of this file, in case you mess up
- 
## Run `DuplicateFamilyNTimes` Script

First, tell the script how many times to "Duplicate Family..."

  1. File > Run Script > Open Script File
  2. Click on `DuplicateFamilyNTimes.pzc` 
  3. Change the number after `ForEach` to the number of Duplicates you want.

So if you have 30 DataTables, then N = 29.

To make 29 duplicates (for final 30 DataTables)
```
ForEach 29
GoTo D 1
DuplicateFamily "Dup %N "
Next
```

To make 15 duplicates (for final 16 DataTables)
```
ForEach 15
GoTo D 1
DuplicateFamily "Dup %N "
Next
```
  1. Press `Run` 
  

## Run `ImportDataTables` Script

First, tell the script where to find your files to Import

  1. File > Run Script > Open Script File
  2. Click on `ImportDataTables.pzc` 
  3. Paste your Path in quotes after `SetPath `
    - See How To [Get Folder Path](#Get-Folder-Path) below

  Windows Example
  ```
  SetPath "C:\Users\syoder\Desktop\Data"
  ```
  Mac Example
  ```
  SetPath "/Users/nicolewheatley/Documents/PrismData/Files"
  ```

  4. Press `Run`



## 🎉 Congratulations! 🎉

The script should take ~10 seconds to run.

Then you will see all of your newly imported data, and updated analysis and plots!

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

## Find Number Of `.csv` Files In Folder
On Mac
1.  Go to your folder in Finder
    1.  Right click > menu select `New Terminal At Folder`
2.  In the terminal window that appears, paste the command below <i>using right click, not control-v</i>
    ```bash
    ls *`.csv` | wc -l
    ```
3. Press `Enter`
4.  The number of files ending in `.csv` in that folder will print out

On Windows
1. Go to your Folder in Windows Explorer
2. Open the Folder
3. In the Windows Explorer Address Bar, type "cmd" (but don't include the quotes) and press Enter.
4. This takes you to command promt, where you can paste this garbage:
  ```bash
  (for /r %f in (*`.csv`) do @type "%f") | find /c /v ""
  ```
5.  The number of files ending in `.csv` in that folder will print out
