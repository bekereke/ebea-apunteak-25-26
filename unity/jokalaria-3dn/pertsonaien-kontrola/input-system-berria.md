---
description: Erabiltzailearen agintea
---

# Sarrera Sistema

<details>

<summary>Kontzeptu batzuk</summary>

<figure><img src="https://docs.unity3d.com/Packages/com.unity.inputsystem@1.16/manual/images/ConceptsOverview.png" alt=""><figcaption></figcaption></figure>

<table><thead><tr><th width="173.20001220703125">Kontzeptua</th><th>Deskribapena</th></tr></thead><tbody><tr><td><strong>Erabiltzailea (User)</strong></td><td>Sarrera gailua (input device) eusten edo ukitzen duen pertsona da, eta jokoari edo aplikazioari sarrera ematen diona.</td></tr><tr><td><strong>Sarrera Gailua (Input Device)</strong></td><td>Sarritan "gailua" bezala aipatzen da sarrera testuinguruan. Hardware fisiko bat da (adibidez, teklatua, gamepada, sagua edo ukipen-pantaila), erabiltzaileari sarrera Unity-ra bidaltzeko aukera ematen diona.</td></tr><tr><td><strong>Kontrola (Control)</strong></td><td>Sarrera Gailu baten zati indibidual eta bereiziak dira. Zati bakoitzak sarrera-balioak bidaltzen ditu Unity-ra. Adibidez, gamepad baten kontrolak botoiak, stick-ak eta trigger-ak dira; saguaren kontrolak, berriz, X eta Y sentsoreak, botoiak eta scroll gurpilak.</td></tr><tr><td><strong>Ekintza (Action)</strong></td><td>Goiko mailako kontzeptu bat da, erabiltzaileak jokoan edo aplikazioan egin nahi dituen gauza indibidualak deskribatzen dituena (adibidez, "Salto egin", "Korrika egin", "Hautatu"). Sarrera gailu edo kontrol espezifikoa edozein dela ere egiten diren gauzak dira. Izen kontzeptualak izan ohi dituzte eta normalean <strong>aditzak</strong> izaten dira.</td></tr><tr><td><strong>Ekintza Mapa (Action Map)</strong></td><td>Ekintzak taldeetan antolatzeko aukera ematen du, Ekintza multzo batek zentzua duen egoera zehatzen arabera. Oso erabilgarria da testuinguruaren arabera Ekintza multzo osoak aldi berean gaitzeko edo desgaitzeko (adibidez, Jokalaria kontrolatzeko mapa bat eta UI-arekin interakzioa kontrolatzeko beste bat).</td></tr><tr><td><strong>Lotura (Binding)</strong></td><td><strong>Ekintza</strong> baten eta gailu-kontrol zehatzen artean definitzen den konexioa da. Lotura anitzek jokoari sarrera multi-plataforma onartzeko aukera ematen diote. Adibidez, "Mugitu" ekintza teklatuko gezi-teklekin, WSAD teklekin eta joypad bateko ezkerreko stick-arekin lotu daiteke.</td></tr><tr><td><strong>Zure Ekintza Kodea (Your Action Code)</strong></td><td>Konfiguratutako ekintzetan oinarrituta exekutatzen den script-aren zatia da. Kode horretan, ekintzen uneko balioa edo egoera irakurri daiteke ("polling" izenez ere ezaguna) edo, bestela, callback funtzio bat konfigura daiteke ekintzak burutzen direnean zure metodo propioa deitzeko.</td></tr><tr><td><strong>Ekintza Aktiboa (Action Asset)</strong></td><td>Ekintza Mapak, Ekintzak eta Loturak gordetzen dituen konfigurazioa duen aktibo mota bat da. Proiektu-mailako ekintzak zehazteko aukera ematen du, gero kodean <code>InputSystem.actions</code> erabiliz erraz erreferentzia daitezen.</td></tr></tbody></table>

</details>

Erabiltzaileak **bideo-jokoari agintzeko** hainbat sarrera gailu izan ditzake: teklatua, sagua, gamepada, VR kontrolak, ukipen-pantaila, etab. Seinale edo agindu horiek (**inputs**) ulertzeko eta **mapeatzeko** (i.e. nahi eran berrantolatzeko) barne sistema bat izan behar du motore grafikoa: hainbat gailu (teklatua, VR, aginteak) eta etxe deberdinetako aginte (Nintendo Switch, Play Station, XBOX) bakoitzaren xehetasunak baliatzeko mapeatzen dira botoiak eta haien ondoriozko ekintzak; **binding edo lotura** osatzen da orduan.&#x20;

Unity bi sistema egonagatik, gero praktikan, konbinaketa desberdinak onartzen dituzte programatzeko garairako.

{% hint style="info" %}
Input System-ak **ENTZUTEN du** zer egiten duen erabiltzaileak.
{% endhint %}

#### Zer egiten dute?

* 🎮 **Irakurtzen dute** zer sakatu duen erabiltzaileak
* 🔄 **Itzultzen dute** sarrera horiek zure kodera
* ⚙️ **Konfiguratzen dituzte** kontrolak (rebinding)

#### Adibide xumea

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

## Sarrera Sistemaren Lan-fluxu Nagusiak

Sarrera Sistema Berriak hainbat modu (workflow edo lan-fluxu) eskaintzen ditu erabiltzailearen sarrerak kudeatzeko.

| Workflow                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    | Interakzioa                                                                                              |
| --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------- |
| <p><a href="https://docs.unity3d.com/Packages/com.unity.inputsystem@1.16/manual/Workflow-Actions.html"><strong>Ekintzak erabiliz</strong></a> <strong>(Actions)</strong><br></p><p>Hau da <strong>lan-fluxu nagusia eta gomendagarriena</strong> egoera gehienetarako.</p><ul><li><strong>Konfigurazioa:</strong> Lan-fluxu honetan, <strong>Input Actions panel</strong>-a erabiltzen da <strong>Project Settings window</strong>-n Ekintza eta Lotura (Bindings) multzoak konfiguratzeko. Ekintza hauek proiektu osoan erabiltzeko pentsatuta daude.</li><li><strong>Kodea:</strong> Kodean, ekintza horien erreferentziak lortzen dira <code>Start</code> metodoan, eta ekintza horien balioak irakurtzen dira <code>Update</code> metodoan.</li></ul>                                                                   | ![](https://docs.unity3d.com/Packages/com.unity.inputsystem@1.16/manual/images/Workflow-Actions.png)     |
| <p><a href="https://docs.unity3d.com/Packages/com.unity.inputsystem@1.16/manual/Workflow-PlayerInput.html"><strong>Ekintzak (Actions) eta </strong><kbd><strong>PlayerInput</strong></kbd><strong> Osagaia erabiliz</strong></a><br></p><p>Lan-fluxu honek abstrakzio-geruza gehigarri bat eskaintzen du eta ezaugarri osagarriak gehitzen ditu.</p><ul><li><strong>Abstrakzioa:</strong> Ekintzetatik zuzenean zure callback (deialdi) kudeatzaile metodoetara konektatzeko aukera ematen du, horrela ez da beharrezkoa kodean ekintza erreferentziak kudeatzea.</li><li><strong>Aplikazioa:</strong> Bereziki erabilgarria da <strong>tokiko jokalari anitzeko</strong> (local multiplayer) egoeretan, hala nola gailuen esleipena (device assignment) eta pantaila zatituaren (split-screen) funtzionaltasuna.</li></ul> | ![](https://docs.unity3d.com/Packages/com.unity.inputsystem@1.16/manual/images/Workflow-PlayerInput.png) |
| <p><a href="https://docs.unity3d.com/Packages/com.unity.inputsystem@1.16/manual/Workflow-Direct.html"><strong>Gailuen Egoerak Zuzenean Irakurtzea</strong></a><br><br></p><p>Lan-fluxu hau Ekintzak eta Loturak guztiz saihesten dituen <strong>ikuspegi sinplifikatua eta script-ean oinarritutakoa</strong> da.</p><ul><li><strong>Metodologia:</strong> Zure script-ak berariaz egiten die erreferentzia gailu-kontrol zehatzei (adibidez, "ezkerreko gamepad stick") eta balioak zuzenean irakurtzen ditu.</li><li><strong>Egokitasuna:</strong> <strong>Prototipo azkarretarako</strong> edo plataforma finko bakarreko egoeretarako egokia da.</li><li><strong>Malgutasuna:</strong> <strong>Malgutasun gutxiagokoa</strong> da, sarrera sistema nagusiaren ezaugarri batzuk saihesten baititu.</li></ul>             | ![](https://docs.unity3d.com/Packages/com.unity.inputsystem@1.16/manual/images/Workflow-Direct.png)      |

{% hint style="warning" %}
Sarrera Sistemak lan-fluxu ugari dituenez, dokumentazioan erabilitako kode-adibideak askotarikoak izan daitezke, batzuetan Ekintza erreferentziak erabiliz eta besteetan sarrera gailuetatik zuzenean irakurriz.
{% endhint %}

***

Lan-fluxu hauek, tresna ezberdinez hornitutako erreminta-kutxa baten antzera, garatzaileari aukera ematen diote proiektu bakoitzaren beharren arabera sarrera kudeatzeko modu egokiena aukeratzeko: malgutasun handieneko Ekintzak, jokalari anitzeko kasuetarako PlayerInput, edo sinpletasun handieneko irakurketa zuzena.
