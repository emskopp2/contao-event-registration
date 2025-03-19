[![](https://img.shields.io/packagist/v/inspiredminds/contao-event-registration.svg)](https://packagist.org/packages/inspiredminds/contao-event-registration)
[![](https://img.shields.io/packagist/dt/inspiredminds/contao-event-registration.svg)](https://packagist.org/packages/inspiredminds/contao-event-registration)

# Deutsches Handbuch

- **Registrierungsformular:** Wählen Sie aus dem Formulargenerator ein Formular aus, das für die Registrierung verwendet werden soll. Alle Formulardaten werden bei jeder Registrierung gespeichert. Das Formular wird wie gewohnt verarbeitet, d. h. es werden auch Benachrichtigungs-E-Mails versendet.
- **Mehrfachregistrierung von Events:** Es besteht die Möglichkeit, über den Kalender mehrere Events per Checkbox auszuwählen und diese dann dem Registrierungsforular zu übergeben.
- **Bestätigung und Stornierung von Events:** Events als Mitgliederbereich ein neues Frontend-Modul für Veranstaltungsregistrierungen.
- **Mindestteilnehmerzahl:** Sie können optional eine Mindestteilnehmerzahl festlegen, die Sie dann (zusammen mit den aktuell angemeldeten Teilnehmern) im Frontend anzeigen können.
- **Maximale Teilnehmerzahl:** Sie können optional eine maximale Teilnehmerzahl festlegen. Wird diese Zahl erreicht, ist eine Anmeldung nicht mehr möglich.
- **Ende der Registrierung:** Sie können optional ein Datum festlegen, nach dem keine Registrierung mehr möglich ist.
- **Ende der Stornierung : Sie können optional ein Datum festlegen, nach dem eine Stornierung nicht mehr möglich ist.
- **Bestätigung erforderlich:** Wenn aktiviert, werden nur bestätigte Registrierungen zur Gesamtzahl der Registrierungen gezählt.
- **Warteliste aktivieren:** Hält die Anmeldung auch nach Erreichen der maximalen Teilnehmerzahl offen. Alle Anmeldungen werden auf eine Warteliste gesetzt und automatisch weiterverfolgt, wenn vorherige Anmeldungen storniert werden.
- **Benachrichtigung über Vorrücken von der Warteliste:** Diese Benachrichtigung wird gesendet, wenn ein Teilnehmer von der Warteliste vorrückt.

## Module

**Die Erweiterung bietet drei neue Event-Module:**

- Veranstaltungsregistrierungsformular
- Bestätigung der Veranstaltungsregistrierung
- Stornierung der Veranstaltungsregistrierung
- Veranstaltungsregistrierungsliste

Alle diese Module sind optional. 

> [!IMPORTANT]
> Das Veranstaltungsregistrierungsformular muss auf derselben Seite wie das Veranstaltungslesermodul eingefügt werden und zeigt das Veranstaltungsregistrierungsformular an, sofern die Registrierung für die Veranstaltung aktiviert ist. Alternativ ist dieses Formular auch als Vorlagenvariable in Veranstaltungsvorlagen verfügbar.

Die Bestätigungs- und Stornierungsformulare können auf anderen Seiten eingefügt werden. In diesem Fall müssen Sie diese Seiten ebenfalls in den Kalendereinstellungen angeben. Andernfalls wird vorausgesetzt, dass diese Module auch auf der Veranstaltungsanzeigeseite vorhanden sind. Die Module ermöglichen die Definition eines Knotens für detaillierte Inhalte. Dieser Inhalt wird angezeigt, wenn eine Veranstaltungsanmeldung erfolgreich bestätigt oder storniert wurde. Sie können außerdem eine Benachrichtigung auswählen, die nach erfolgreicher Stornierung bzw. Bestätigung versendet wird. Wenn Sie weder die Bestätigungs- noch die Stornierungsfunktion benötigen, müssen diese Module nicht erstellt werden.

Mit dem Modul _Veranstaltungsregistrierungsliste_ können Sie eine Liste der Anmeldungen für die aktuelle Veranstaltung auf der Veranstaltungsseite anzeigen. Die Liste zeigt nur bestätigte Anmeldungen an, sofern die Anmeldebestätigung aktiviert ist. Abgesagte Anmeldungen werden nicht angezeigt. Standardmäßig `mod_event_registration_list` verwendet die Vorlage für jeden Eintrag ein automatisch generiertes Label entsprechend der DCA- `list.label` Konfiguration `tl_event_registration`. Die Standardkonfiguration verwendet die Felder `firstname` und `lastname`. Sollte Ihr eigenes Formular diese Werte jedoch nicht enthalten, müssen Sie eine benutzerdefinierte `mod_even_registration_list` Vorlage erstellen und die korrekten Felder entsprechend ausgeben. Alle übermittelten Werte des Registrierungsformulars für jede Anmeldung sind unter dem `form_data` Schlüssel verfügbar. 

**Beispiel:**

```PHP

<?php $this->extend('block_unsearchable'); ?>

<?php $this->block('content'); ?>
  <ul>
    <?php foreach ($this->registrations as $registration): ?>
      <li><?= $registration->form_data->my_form_field ?></li>
    <?php endforeach; ?>
  </ul>
<?php $this->endblock(); ?>

```
## Kalendereinstellungen

Wie bereits erwähnt, gibt es in Ihren Kalendern zusätzliche Einstellungen. Pro Kalender können Sie die Weiterleitungsseite für Bestätigungen sowie die Weiterleitungsseite für Stornierungen konfigurieren. Auf diesen Seiten fügen Sie dann bei Bedarf entweder das Modul zur Bestätigung der Veranstaltungsregistrierung oder das Modul zur Stornierung der Veranstaltungsregistrierung hinzu.

## Vorlagenvariablen (Template-Variablen)

**Folgende Template-Variablen stehen in den Event-Templates sowie dem Template für das Event-Anmeldeformular zur Verfügung:**

- `$this->canRegister`: Ob für diese Veranstaltung eine Anmeldung möglich ist.
- `$this->registrationForm`: Enthält das Registrierungsformular.
- `$this->isFull`: Ob die maximale Anzahl an Anmeldungen für diese Veranstaltung erreicht wurde.
- `$this->isWaitingList`: Ob die maximale Anzahl an Anmeldungen für diese Veranstaltung erreicht wurde und ob die Warteliste aktiviert ist.
- `$this->registrationCount`: Der aktuelle Registrierungsstand für diese Veranstaltung.
- `$this->reg_min`: Die Mindestanzahl an Anmeldungen für diese Veranstaltung.
- `$this->reg_max`: Die maximale Anzahl an Anmeldungen für diese Veranstaltung.
- `$this->reg_regEnd`: Zeitstempel, nach dem eine Registrierung nicht mehr möglich ist.
- `$this->reg_cancelEnd`: Zeitstempel, nach dem eine Stornierung nicht mehr möglich ist.

## Einfache Token

**Innerhalb von Benachrichtigungen sowie den Knoteninhalten der *Bestätigungs- und Abbruchmodule* stehen folgende einfache Token zur Verfügung:**

- `##event_*##`: Alle Variablen des Ereignisses.
- ##reg_*##`: Alle Variablen der Registrierung (dazu gehören alle Formulardaten).
- `##reg_amount##`: Die Anzahl der Personen für eine Registrierung.
 - `##reg_count##`: Die jeweilige Registrierungsanzahl.
- `##reg_confirm_url##`: Die URL, mit der die Registrierung bestätigt werden kann.
- `##reg_cancel_url##`: Die URL, mit der die Registrierung storniert werden kann.

## Mehrere Sprachen

Diese Erweiterung unterstützt terminal42/contao-changelanguage. Wenn für eine Veranstaltung eine Hauptveranstaltung definiert ist, ist die oben genannte Option nur für die Hauptveranstaltung verfügbar. Alle Anmeldungen werden immer der Hauptveranstaltung zugeordnet und für diese gezählt, sodass die Gesamtzahl der Anmeldungen für alle mit der Hauptveranstaltung verknüpften Veranstaltungen gleich ist.

Dies gilt auch für die oben genannten Vorlagenvariablen. Diese verweisen immer auf das Hauptereignis, sofern verfügbar.

## Registrierungen anzeigen und exportieren

Für jedes Event gibt es im Backend ein zusätzliches Icon in der Eventliste.

- Bild -

Damit können Sie die Anmeldungen zu den einzelnen Veranstaltungen auflisten.

- Bild -

Innerhalb der Liste können Sie über das Info-Symbol die Details der Veranstaltungsanmeldung einsehen. Sie können außerdem einige Eigenschaften der Veranstaltung bearbeiten, beispielsweise die Anzahl der Personen für diese Anmeldung und ob sie bestätigt oder storniert wurde.

- Bild -

`,` Die Übersicht ermöglicht Ihnen außer dem den Export der Registrierungen als CSV. Dabei können Sie das Trennzeichen (entweder oder ) konfigurieren `;`und eine Microsoft Excel-kompatible CSV-Datei exportieren.

- Bild -

## Backend-Konfiguration

Die Veranstaltungsregistrierungsliste im Backend (sowie im Frontend-Modul) verwendet standardmäßig die Felder `firstname`und `lastname`. Sollte Ihr Veranstaltungsregistrierungsformular diese Felder jedoch nicht enthalten, können Sie die DCA-Konfiguration anpassen, damit im Backend eine passende Beschriftung angezeigt wird. Wenn Ihr Formular beispielsweise die Felder und verwendet `vorname`, können Sie die Backend-Beschriftungen wie folgt konfigurieren: `nachname` `email`

````SQL

// contao/dca/tl_event_registration.php
$GLOBALS['TL_DCA']['tl_event_registration']['list']['label']['fields'] = ['vorname', 'nachname', 'email'];
$GLOBALS['TL_DCA']['tl_event_registration']['list']['label']['format'] = '%s %s (%s)';

````
Beachten Sie, dass dies auch Auswirkungen auf die Standardbeschriftungen des Front-End-Moduls der Veranstaltungsregistrierungsliste hat.

## Benutzerdefinierter Betrag

Standardmäßig wird für jede Anmeldung eine Person angenommen. Es ist jedoch auch möglich, dem Besucher, der sich für eine Veranstaltung anmeldet, die Anzahl der Personen für diese Anmeldung selbst zu überlassen. Fügen Sie dazu ein neues Textfeld (vorzugsweise mit numerischer Validierung) mit dem Feldnamen in das Formular ein `amount`. Dadurch wird die Standardanzahl überschrieben und die Gesamtzahl der Anmeldungen erhöht sich ebenfalls um diesen Betrag.

## Mehrfachregistrierungen

Ab der Version `2.2.0` können Sie Besuchern auch die gleichzeitige Anmeldung für mehrere Veranstaltungen ermöglichen. Dafür gibt es zwei neue Funktionen:

- Ein Front-End-Modul für den Veranstaltungsregistrierungskalender, das ein normales Kalendermodul (genau wie der reguläre Kalender) mit einem Kontrollkästchen für jede Veranstaltung im Kalender rendert.
- Ein ausgewähltes Ereignisformularfeld, das die Auswahl in einem Formular des Formulargenerators anzeigt und später verarbeitet.

**Insgesamt muss Folgendes getan werden, um diese Funktion zu nutzen:**

1. Erstellen Sie eine neue Benachrichtigung, die für die Registrierung mehrerer Ereignisse verwendet wird (die Token sind dieselben).
2. Erstellen Sie ein neues Formular, das für die Registrierung mehrerer Ereignisse verwendet wird.
3. Fügen Sie in dieses Formular ein ausgewähltes Ereignisformularfeld ein .
4. Wählen Sie außerdem die entsprechende Benachrichtigung aus.
5. Erstellen Sie eine neue Seite, auf der Sie das neue Anmeldeformular einfügen (alternativ können Sie es auch auf derselben Seite wie den Kalender einfügen).
6. Erstellen Sie ein neues Kalendermodul zur Veranstaltungsregistrierung und wählen Sie die zuvor erstellte Seite als Weiterleitungsseite aus (oder keine).
7. Fügen Sie dieses Modul in eine neue Seite ein.

Wenn Sie diese Seite im Frontend aufrufen, sehen Sie für jede Veranstaltung, für die Sie sich anmelden können, Kontrollkästchen. Sie können auch zwischen den Monaten wechseln – die vorherige Auswahl bleibt erhalten. Sobald Sie auf „Weiter“ klicken, werden Sie zum Anmeldeformular weitergeleitet. Dort sehen Sie eine Liste der Veranstaltungen, die für diese Anmeldung ausgewählt wurden.

Nach dem Absenden des Anmeldeformulars wird die Formularbenachrichtigung wie gewohnt versendet. Sofern `##reg_confirm_url##`Ihre Benachrichtigung einen Token enthält, bestätigt die Bestätigungs-URL automatisch die Anmeldungen für alle ausgewählten Veranstaltungen.

## Mitgliederregistrierungsliste

Zusätzlich zu den oben genannten Funktionen gibt es im Mitgliederbereich ein neues Frontend-Modul für Veranstaltungsregistrierungen. Dieses Modul listet alle Veranstaltungsregistrierungen des aktuell angemeldeten Frontend-Benutzers auf. Außerdem werden Links zum Bestätigen (falls zutreffend) oder Abbrechen der Registrierung angezeigt.
