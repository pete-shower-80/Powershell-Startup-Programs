# Funktion zum Anzeigen eines Popup-Fensters ohne Bestätigung
function Show-Message {
    param (
        [string]$message,
        [string]$title = "Programmstart",
        [int]$duration = 5  # Dauer in Sekunden, wie lange das Fenster angezeigt wird
    )

    # XAML um das Fenster zu definieren
    $xaml = @"
<Window xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        Title="$title" SizeToContent="WidthAndHeight" WindowStartupLocation="CenterScreen"
        Topmost="True" ShowInTaskbar="False" WindowStyle="None" AllowsTransparency="True"
        Background="Transparent">
    <Border Background="#FFDDDDDD" Padding="20" CornerRadius="20">
        <StackPanel>
            <TextBlock Text="$message" FontSize="20" Foreground="Black" HorizontalAlignment="Center" Margin="10"/>
        </StackPanel>
    </Border>
</Window>
"@

    # XAML in PowerShell-Objekt konvertieren
    Add-Type -AssemblyName PresentationFramework
    $reader = New-Object System.Xml.XmlNodeReader ([xml]$xaml)
    $window = [Windows.Markup.XamlReader]::Load($reader)

    # Fenster anzeigen
    $window.Show()
    Start-Sleep -Seconds $duration
    $window.Close()
}

# Abfragen des aktuellen Wochentags
$dayOfWeek = (Get-Date).DayOfWeek
$Date = (Get-Date).ToString("dd-MM-yyyy")

if ($dayOfWeek -eq 'Saturday' -or $dayOfWeek -eq 'Sunday') {
   # Anzeigen einer Pop Up Message für das Wochenende
    Show-Message -message "Thank God it`s $dayOfWeek $Date ... Chilling time" -title "Willkommen" -duration 5
    exit
}

# Begrüßung anzeigen
Show-Message -message "It is $dayOfWeek -- $Date" -title "Willkommen" -duration 3

# Programme aus der XML-Datei einlesen und in einer Schleife starten
$xmlPath = "ProgrammName.xml"
[xml]$programme = Get-Content -Path $xmlPath

foreach ($programm in $programme.Programme.Programm) {
    $name = $programm.Name
    $pfad = $programm.Pfad

    # Programm starten
    $proc = Start-Process -FilePath $pfad -PassThru
    Show-Message -message "Starting $name" -title "Status" -duration 3

    # Warten, bis das Programm gestartet ist
    Wait-Process -Id $proc.Id
}

exit
