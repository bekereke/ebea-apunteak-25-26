# Input System Berria

> Input System-ak **ENTZUTEN du** zer egiten duen erabiltzaileak.

{% hint style="info" %}
#### Zer da?

Sistema bat **sarrerak (inputs) kudeatzeko** eta batetik bestera errazago **mapeatzeko**: teklatua, sagua, gamepada, VR kontrolak, ukipen-pantaila, etab.
{% endhint %}

#### Zer egiten du?

* 🎮 **Irakurtzen du** zer sakatu duen erabiltzaileak
* 🔄 **Itzultzen du** sarrera horiek zure kodera
* ⚙️ **Konfiguratzen ditu** kontrolak (rebinding)

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

