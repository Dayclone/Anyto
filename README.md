# iMyFone AnyTo Trial Reset Script

This project contains a batch script that modifies the registry to reset the trial limit of iMyFone AnyTo, effectively enabling unlimited trials. The script generates a new GUID and updates the relevant registry entries to bypass trial restrictions.

## Features
- Resets the trial period for iMyFone AnyTo by changing the `guid` registry value.
- Disables `ABTest_Countdown` and `ABTestVersionCollect` registry entries.
- Generates a new GUID automatically and updates the registry.

## How It Works
1. The script modifies the registry path:  
   `HKEY_LOCAL_MACHINE\SOFTWARE\WOW6432Node\iMyfone\AnyTo`.
2. It disables `ABTest_Countdown` and `ABTestVersionCollect`.
3. It retrieves the current GUID and generates a new GUID using random hexadecimal values.
4. The script updates the registry with the new GUID, resetting the trial period.

## Script
Here is the batch script used in this project:

```batch
@echo off
setlocal enabledelayedexpansion

REM Define the registry path
set "regPath=HKEY_LOCAL_MACHINE\SOFTWARE\WOW6432Node\iMyfone\AnyTo"

REM Disable ABTest_Countdown
reg add "%regPath%" /v ABTest_Countdown /t REG_SZ /d false /f
REM Disable ABTestVersionCollect
reg add "%regPath%" /v ABTestVersionCollect /t REG_SZ /d false /f

REM Get the current GUID value (for display purposes)
for /f "tokens=2*" %%A in ('reg query "%regPath%" /v guid') do set "currentGUID=%%B"

REM Generate a new GUID
set "hex=0123456789ABCDEF"
set "newGUID="

REM Generate the first 8 characters
for /L %%i in (1,1,8) do (
    set /a "r=!random! %% 16"
    for %%j in (!r!) do set "newGUID=!newGUID!!hex:~%%j,1!"
)

REM Add first hyphen
set "newGUID=!newGUID!-"

REM Generate the next 4 characters
for /L %%i in (1,1,4) do (
    set /a "r=!random! %% 16"
    for %%j in (!r!) do set "newGUID=!newGUID!!hex:~%%j,1!"
)

REM Add second hyphen
set "newGUID=!newGUID!-"

REM Generate the next 4 characters
for /L %%i in (1,1,4) do (
    set /a "r=!random! %% 16"
    for %%j in (!r!) do set "newGUID=!newGUID!!hex:~%%j,1!"
)

REM Add third hyphen
set "newGUID=!newGUID!-"

REM Generate the next 4 characters
for /L %%i in (1,1,4) do (
    set /a "r=!random! %% 16"
    for %%j in (!r!) do set "newGUID=!newGUID!!hex:~%%j,1!"
)

REM Add fourth hyphen
set "newGUID=!newGUID!-"

REM Generate the final 12 characters
for /L %%i in (1,1,12) do (
    set /a "r=!random! %% 16"
    for %%j in (!r!) do set "newGUID=!newGUID!!hex:~%%j,1!"
)

REM Update the registry
reg add "%regPath%" /v guid /t REG_SZ /d "!newGUID!" /f

echo Old GUID: !currentGUID!
echo New GUID: !newGUID!
echo Registry updated successfully.
pause
