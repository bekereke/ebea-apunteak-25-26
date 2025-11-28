---
description: Erabiltzailearen agintea
---

# Sarrera Sistema ()

<details>

<summary>Kontzeptu batzuk</summary>

<figure><img src="https://docs.unity3d.com/Packages/com.unity.inputsystem@1.16/manual/images/ConceptsOverview.png" alt=""><figcaption></figcaption></figure>

<table><thead><tr><th width="173.20001220703125">Kontzeptua</th><th>Deskribapena</th></tr></thead><tbody><tr><td><strong>Erabiltzailea (User)</strong></td><td>Sarrera gailua (input device) eusten edo ukitzen duen pertsona da, eta jokoari edo aplikazioari sarrera ematen diona.</td></tr><tr><td><strong>Sarrera Gailua (Input Device)</strong></td><td>Sarritan "gailua" bezala aipatzen da sarrera testuinguruan. Hardware fisiko bat da (adibidez, teklatua, gamepada, sagua edo ukipen-pantaila), erabiltzaileari sarrera Unity-ra bidaltzeko aukera ematen diona.</td></tr><tr><td><strong>Kontrola (Control)</strong></td><td>Sarrera Gailu baten zati indibidual eta bereiziak dira. Zati bakoitzak sarrera-balioak bidaltzen ditu Unity-ra. Adibidez, gamepad baten kontrolak botoiak, stick-ak eta trigger-ak dira; saguaren kontrolak, berriz, X eta Y sentsoreak, botoiak eta scroll gurpilak.</td></tr><tr><td><strong>Ekintza (Action)</strong></td><td>Goiko mailako kontzeptu bat da, erabiltzaileak jokoan edo aplikazioan egin nahi dituen gauza indibidualak deskribatzen dituena (adibidez, "Salto egin", "Korrika egin", "Hautatu"). Sarrera gailu edo kontrol espezifikoa edozein dela ere egiten diren gauzak dira. Izen kontzeptualak izan ohi dituzte eta normalean <strong>aditzak</strong> izaten dira.</td></tr><tr><td><strong>Ekintza Mapa (Action Map)</strong></td><td>Ekintzak taldeetan antolatzeko aukera ematen du, Ekintza multzo batek zentzua duen egoera zehatzen arabera. Oso erabilgarria da testuinguruaren arabera Ekintza multzo osoak aldi berean gaitzeko edo desgaitzeko (adibidez, Jokalaria kontrolatzeko mapa bat eta UI-arekin interakzioa kontrolatzeko beste bat).</td></tr><tr><td><strong>Lotura (Binding)</strong></td><td><strong>Ekintza</strong> baten eta gailu-kontrol zehatzen artean definitzen den konexioa da. Lotura anitzek jokoari sarrera multi-plataforma onartzeko aukera ematen diote. Adibidez, "Mugitu" ekintza teklatuko gezi-teklekin, WSAD teklekin eta joypad bateko ezkerreko stick-arekin lotu daiteke.</td></tr><tr><td><strong>Zure Ekintza Kodea (Your Action Code)</strong></td><td>Konfiguratutako ekintzetan oinarrituta exekutatzen den script-aren zatia da. Kode horretan, ekintzen uneko balioa edo egoera irakurri daiteke ("polling" izenez ere ezaguna) edo, bestela, callback funtzio bat konfigura daiteke ekintzak burutzen direnean zure metodo propioa deitzeko.</td></tr><tr><td><strong>Ekintza Aktiboa (Action Asset)</strong></td><td>Ekintza Mapak, Ekintzak eta Loturak gordetzen dituen konfigurazioa duen aktibo mota bat da. Proiektu-mailako ekintzak zehazteko aukera ematen du, gero kodean <code>InputSystem.actions</code> erabiliz erraz erreferentzia daitezen.</td></tr></tbody></table>

</details>

Erabiltzaileak **bideo-jokoari agintzeko** hainbat sarrera gailu izan ditzake: teklatua, sagua, gamepada, VR kontrolak, ukipen-pantaila, etab.&#x20;

Seinale edo agindu horiek (**inputs**) ulertzeko eta **mapeatzeko** (i.e. nahi eran berrantolatzeko) barne sistema bat izan behar du motore grafikoak. Unity bi sistema egonagatik, gero praktikan, konbinaketa desberdinak onartzen dituzte programatzeko garairako.

{% hint style="info" %}
Input System-ak **ENTZUTEN du** zer egiten duen erabiltzaileak.
{% endhint %}

#### Zer egiten dute?

* 🎮 **Irakurtzen dute** zer sakatu duen erabiltzaileak
* 🔄 **Itzultzen dute** sarrera horiek zure kodera
* ⚙️ **Konfiguratzen dituzte** kontrolak (rebinding)

#### Adibidea

```csharp
using UnityEngine.InputSystem;

public class InputExample : MonoBehaviour
{
    // Input System-ek IRAKURTZEN du zer tekla sakatu duzun
    void Update()
    {
        // Erabiltzaileak "W" sakatu du?
        if (Keyboard.current.wKey.isPressed)
        {
            Debug.Log("W sakatu da!");
        }
        
        // Gamepad-aren joystick-a mugitu da?
        Vector2 stick = Gamepad.current.leftStick.ReadValue();
    }
}
```
