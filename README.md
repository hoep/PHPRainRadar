# PHPRainRadar

## Übersicht

Die `WeatherRadar`-Klasse ist ein leistungsstarkes PHP-Tool zur Darstellung und Auswertung von Wetterradardaten. Sie ermöglicht das Herunterladen, Verarbeiten und Visualisieren von Regenradardaten mit automatischer Erkennung von Niederschlagsintensitäten und bietet verschiedene Ausgabeformate inklusive animierter GIFs und HTML-Vorhersagetabellen.

**Version:** 2.3.7  
**Datum:** 2025-04-03

## Funktionen

- Automatisches Herunterladen und Verarbeiten von Radarbildern
- Erkennung von Regenintensitäten an definierten Koordinaten
- Überlagerung von Radarbildern auf einer Basiskarte
- Erstellung von animierten GIFs aus einer Sequenz von Radarbildern
- Generierung von HTML-Vorhersagetabellen
- Natürlichsprachliche Interpretation von Regendaten
- Anpassbare Visualisierungsoptionen mit umfangreichen Konfigurationsmöglichkeiten

## Voraussetzungen

- PHP 7.0 oder höher
- GD-Bibliothek für die Bildverarbeitung
- ImageMagick für die Erstellung von animierten GIFs
- cURL-Erweiterung für das Herunterladen von Radarbildern
- Eine Basiskarte im PNG-Format

## Installation

1. Kopieren Sie die `WeatherRadar.php`-Datei in Ihr Projektverzeichnis
2. Stellen Sie sicher, dass die erforderlichen Abhängigkeiten installiert sind
3. Importieren Sie die Klasse in Ihr Projekt:

```php
require_once 'WeatherRadar.php';
```

## Beispiele

### Grundlegende Verwendung

```php
// Instanz erstellen mit Standardkonfiguration
$radar = new WeatherRadar();

// Radarbilder verarbeiten
$radar->processRadarImages();

// HTML-Code für die Anzeige generieren
echo $radar->getHTML();
```

### Anpassung der Konfiguration

```php
// Konfiguration erstellen
$config = [
    'baseMapPath' => '/pfad/zu/ihrer/basiskarte.png',
    'outputDir' => './radar_ausgabe',
    'coordinates' => [[250, 300], [400, 350]], // X/Y-Koordinaten zur Prüfung
    'hours' => 48, // Anzahl der zu verarbeitenden Stunden
    'forecastTableHours' => 24, // Stunden für die Tabelle
    'forecastTextHours' => 48, // Stunden für die Textvorhersage
    'textColor' => '#FFFFFF', // Weiße Textfarbe
    'cellBorderColor' => '#555555' // Graue Rahmenfarbe
];

// Instanz mit angepasster Konfiguration erstellen
$radar = new WeatherRadar($config);

// Oder Konfiguration nach der Instanziierung aktualisieren
$radar->setConfig(['coordinates' => [[300, 350], [450, 400]]]);

// Radarbilder verarbeiten
$radar->processRadarImages();
```

### Regendaten abrufen und analysieren

```php
// Instanz erstellen
$radar = new WeatherRadar();

// Radarbilder verarbeiten
$radar->processRadarImages();

// Regendaten für alle Koordinaten abrufen
$allRainData = $radar->getRain();

// Regendaten für spezifische Koordinate abrufen
$specificRainData = $radar->getRain(0); // Index 0

// Vorhersage generieren für die nächsten 24 Stunden
$forecast24h = $radar->getRainForecast(0, 24);

// Vorhersage generieren für die nächsten 48 Stunden
$forecast48h = $radar->getRainForecast(0, 48);

// Textausgabe
echo "Regenvorhersage (24h): " . $forecast24h . "\n";
echo "Regenvorhersage (48h): " . $forecast48h . "\n";
```

### Angepasste HTML-Ausgabe

```php
// HTML-Ausgabe mit angepassten Parametern
$html = $radar->getHTML(
    "my-weather-container", // Container-ID
    "800px",                // Breite
    "600px",                // Höhe
    true,                   // Responsives Design
    0,                      // Koordinaten-Index
    "#FFFFFF",              // Textfarbe
    "#333333",              // Rahmenfarbe
    true,                   // Vorhersagetext anzeigen
    12,                     // 12 Stunden in der Tabelle
    48                      // 48 Stunden für die Textvorhersage
);

echo $html;
```

## Konfigurationsparameter

Die `WeatherRadar`-Klasse bietet eine umfangreiche Konfiguration. Hier sind alle verfügbaren Optionen:

### Basispfade und URLs

| Parameter | Typ | Standard | Beschreibung |
|-----------|-----|----------|--------------|
| `baseMapPath` | String | `/usr/share/symcon/preview/kremsmuenster/austria.png` | Pfad zur Basiskarte im PNG-Format |
| `outputDir` | String | `./radar_images` | Ausgabeverzeichnis für verarbeitete Bilder |
| `radarBaseUrl` | String | `https://portale.geosphere.at/hpAT/portallib/factory.php?pu=default&op=getNoCacheImg&a=INCAL_VW1398&p=HP_RR_AT&i=` | Basis-URL für Radarbilder |
| `cleanOutputDir` | Boolean | `true` | Ausgabeverzeichnis vor der Verarbeitung bereinigen |

### Radar-Einstellungen

| Parameter | Typ | Standard | Beschreibung |
|-----------|-----|----------|--------------|
| `hours` | Integer | `48` | Anzahl der Stunden für Radarbilder |
| `coordinates` | Array | `[]` | X/Y-Koordinaten zur Prüfung, Format: `[[x1,y1], [x2,y2], ...]` |
| `radius` | Integer | `5` | Radius um die Koordinaten für Regenprüfung |
| `hourOffset` | Integer | `null` | Stunden-Offset für Radar-URL (null = automatisch berechnen) |
| `hoursInterval` | Integer | `3` | Stunden-Intervall für die Anzeige der Uhrzeiten in der Tabelle |
| `forecastTableHours` | Integer | `24` | Anzahl der Stunden für die Vorhersagetabelle |
| `forecastTextHours` | Integer | `24` | Anzahl der Stunden für die Textvorhersage |

### HTML-Ausgabe

| Parameter | Typ | Standard | Beschreibung |
|-----------|-----|----------|--------------|
| `textColor` | String | `#FFFFFF` | Textfarbe für die HTML-Ausgabe |
| `cellBorderColor` | String | `#555555` | Rahmenfarbe für die Zellen der Vorhersagetabelle |
| `showForecastText` | Boolean | `true` | Vorhersagetext anzeigen |

### Bildverarbeitung

| Parameter | Typ | Standard | Beschreibung |
|-----------|-----|----------|--------------|
| `drawMarkers` | Boolean | `true` | Rote Kreuze an den Koordinaten zeichnen |
| `markerColor` | Array | `[255, 0, 0]` | Farbe der Marker (RGB) |
| `markerSize` | Integer | `5` | Größe der Marker in Pixeln |
| `

### Konfigurationsmethoden

#### `setConfig($config)`
Aktualisiert die Konfiguration.
- `$config`: Neue Konfigurationsparameter
- Rückgabe: Boolean (Erfolg des Vorgangs)

#### `getConfig($key = null)`
Ruft die aktuelle Konfiguration ab.
- `$key`: Konfigurationsschlüssel (optional, wenn null wird die gesamte Konfiguration zurückgegeben)
- Rückgabe: Konfigurationswert oder gesamte Konfiguration

### Hilfsmethoden

#### `getImageSequence()`
Gibt die Sequenz der generierten Bilder zurück.
- Rückgabe: Array mit Bildsequenz

#### `getDebugInfo()`
Gibt Debug-Informationen zurück.
- Rückgabe: Array mit Debug-Informationen

#### `cleanOutputDirectory($directory = null, $filePatterns = ['*.png', '*.jpg', '*.jpeg', '*.gif'])`
Bereinigt das Ausgabeverzeichnis.
- `$directory`: Das zu bereinigende Verzeichnis (optional, Standard ist der konfigurierte Ausgabepfad)
- `$filePatterns`: Array mit Dateimustern, die gelöscht werden sollen
- Rückgabe: Anzahl der gelöschten Dateien

#### `createAnimation($outputFile = null, $delay = null)`
Erstellt ein animiertes GIF aus den Sequenzbildern.
- `$outputFile`: Pfad zur Ausgabedatei (optional, Standard ist der konfigurierte Animationsdateiname)
- `$delay`: Verzögerung zwischen den Bildern in Millisekunden (optional, Standard ist der konfigurierte Wert)
- Rückgabe: Boolean (Erfolg des Vorgangs)# WeatherRadar Dokumentation
