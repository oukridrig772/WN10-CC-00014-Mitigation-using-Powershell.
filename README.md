# WN10-CC-00014-Mitigation-using-Powershell.
#WN10-CC-000145 - Users must be prompted for a password on resume from sleep (on battery).
# This script must be run with Administrator privileges

# Define the registry path and value
$regPath = "HKLM:\SOFTWARE\Policies\Microsoft\Power\PowerSettings\0e796bdb-100d-47d6-a2d5-f7d2daa51f51"
$valueName = "DCSettingIndex"
$valueData = 1

try {
    # Check if the registry path exists, if not, create it.
    if (-not (Test-Path -Path $regPath)) {
        Write-Host "Registry path does not exist. Creating it now..."
        New-Item -Path $regPath -Force | Out-Null
    }

    # Set the registry key to require a password on resume from sleep (on battery)
    Write-Host "Setting registry key to require a password on wake from sleep (on battery)..."
    Set-ItemProperty -Path $regPath -Name $valueName -Value $valueData -Type DWord -Force
    
    # Confirm the change
    Write-Host "The 'DCSettingIndex' registry value has been set to 1."
    Write-Host "Users will now be prompted for a password on resume from sleep on battery power."
    
    # Optional: Verify the change by getting the item property
    $currentValue = (Get-ItemProperty -Path $regPath -Name $valueName).$valueName
    Write-Host "Current value of $valueName is: $currentValue"

} catch {
    Write-Error "Failed to set the registry value. Please ensure you are running as an administrator."
    Write-Error $_.Exception.Message
}
