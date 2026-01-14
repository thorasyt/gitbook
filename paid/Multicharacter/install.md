# Installation

Installation Steps of Multicharacter

## Step 1
Drag and Drop 'Multicharacter' script which has been download from cfx.re portal

## Step 2 
if there is any sql file then need run in phpmyadmin or Heildisql, so of our script use auto update method

## Step 3 – Start the Resource

To start the script, open the **txAdmin CFG Editor** and add the resources in the following order:

```cfg
start ox_lib
start framework
start appearance
start tg-multicharacter
```

> ⚠️ Make sure all dependencies are started **before** `tg-multicharacter`,
> otherwise the script may not work correctly.