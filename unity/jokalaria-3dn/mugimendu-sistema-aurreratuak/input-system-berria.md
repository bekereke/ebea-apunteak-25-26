---
description: Erabiltzailearen agintea
---

# Sarrera kudeaketa

Erabiltzaileak **bideo-jokoari agintzeko** hainbat sarrera gailu izan ditzake: teklatua, sagua, gamepada, VR kontrolak, ukipen-pantaila, etab.&#x20;

Seinale edo agindu horiek (**inputs**) ulertzeko eta **mapeatzeko** (i.e. nahi eran berrantolatzeko) barne sistema bat izan behar du motore grafikoak. Hiru sistema desberdin erabili daitezke Unityn.

> Input System-ak **ENTZUTEN du** zer egiten duen erabiltzaileak.

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
