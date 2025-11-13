# Jokalaria 3Dn

Unityn adierazteko diren objektuen artetik nagusia erabitzaileak kontrolatu beharreko hori da: `player` edo jokalaria, euskaraz. Harentzat oinarrizko ezarpenak ezagutzen; gero, hura kontrolatzeko aginte sistema doitzen; era naturalean (rigging) eta animazio segidak elkarlotzen ikasiko duzu, besteak beste. Jakintza guneak laburbildurik dituzu hemen:

1. [**Mugimendu Sistema Aurreratuak**](mugimendu-sistema-aurreratuak/): Rigidbody vs CharacterController
2. [**Talka Sistema**](talka-sistema.md): Collisions eta Triggers
3. [**Animazio Sistema**](animazio-sistema.md): Animator Controller eta State Machines
4. [**Input Sistema Berria**](mugimendu-sistema-aurreratuak/input-system-berria.md): Unity Input System
5. [**VR Garapena**](vr-garapena.md): XR Interaction Toolkit
6. **Optimizazioa**: Profiler eta Performance

Baina hasi orduko...

{% hint style="info" %}
#### Jokalaria zer da?

Jokalaria erabiltzaileak kontrolatzen duen pertsonaia edo objektua da jokoan. Jokalaria da joko-munduaren "begiak" eta "eskuak"; erabiltzaileak aginduko duen objektua; haren avatarra.
{% endhint %}

#### Jokalari Motak

**1. Lehen Pertsonako Jokalaria (First Person)**

* Kamera pertsonaiaren begietatik ikusten da
* Adibideak: Counter-Strike, Minecraft, Half-Life, Escape From Tarkov (Unity)
* Immertsioa handiagoa
* Erabiltzen da: FPS, simulagailu, VR

**2. Hirugarren Pertsonako Jokalaria (Third Person)**

* Kamera pertsonaia kanpotik ikusten du
* Adibideak: Fortnite, Tomb Raider, Zelda
* Pertsonaia eta ingurunea ikusten dira
* Erabiltzen da: Abentura, ekintza, plataforma

**3. 2D Jokalaria**

* Bi dimentsiotan mugitzen da
* Adibideak: Super Mario, Celeste, Hollow Knight (Unity), Cuphead (Unity), Ori (Unity)
* Errazagoa programatzeko
* Erabiltzen da: Plataforma, puzzle, indie jokoak

**4. Ibilgailu Jokalaria**

* Kotxeak, hegazkinak, ontziak, etab.
* Adibideak: Gran Turismo, Flight Simulator, Dredge (Unity)
* Fisika konplexua
* Erabiltzen da: Lasterketa, simulagailu

***

### Unity-ko Jokalariaren Osagaiak

Jokalari bat Unity-n GameObject bat da, osagai (component) desberdinekin hornitua:

#### 1. `Transform`

{% hint style="info" %}
**Zer da?** Objektuaren kokapena, errotazioa eta tamaina.

**Zergatik garrantzitsua?** Transform-ak objektuaren kokapen espazialak kontrolatzen ditu.
{% endhint %}

```csharp
// Posizioa aldatu
transform.position = new Vector3(0, 1, 0);

// Biratu
transform.Rotate(0, 90, 0);

// Tamaina aldatu
transform.localScale = new Vector3(2, 2, 2);
```

#### 2. `Collider`

{% hint style="info" %}
**Zer da?** Objektuaren forma fisikoa, talkak detektatzeko.

**Zergatik garrantzitsua?** Gabe, jokalaria hormetan zehar pasako litzateke.
{% endhint %}

Collider motak:

* **Box Collider**: Kutxak
* **Sphere Collider**: Esferak
* **Capsule Collider**: Pertsonaiak (normalean)
* **Mesh Collider**: Forma konplexuak

<figure><img src="../../.gitbook/assets/Unity - Untitled - Collisions - Web Player_ _DX11_ 2015-04-22 22.41.37.png" alt=""><figcaption></figcaption></figure>

#### 3. `Rigidbody`

{% hint style="info" %}
**Zer da?** Fisikak aplikatzen dituen osagaia (grabitatearen, indarrak, momentua).

**Zergatik garrantzitsua?** Jokalaria errealista mugitzeko eta fisika-munduan integratuta egoteko.
{% endhint %}

```csharp
Rigidbody rb = GetComponent<Rigidbody>();
rb.AddForce(Vector3.forward * 10);
```

#### 4. Script (C# kodea)

**Zer da?** Jokalariaren portaera programatzen duen kodea.

```csharp
public class PlayerController : MonoBehaviour
{
    void Update()
    {
        // Hemen jokalariaren logika
    }
}
```

**Zergatik garrantzitsua?** Script-ak dira jokalaria biziarazten dutenak.

#### 5. `Camera` (Kamera)

**Zer da?** Jokalariaren ikuspegia, jokoa "begien" bidez ikusten dena.

* **Lehen pertsonan**: Kamera jokalari objektuaren barruan
* **Hirugarren pertsonan**: Kamera jokalaria jarraitzen du kanpotik

**Zergatik garrantzitsua?** Kamera gabe ez genuke ezer ikusiko!

***

### Jokalariaren Mugimenduaren Oinarriak

#### Sarrera (Input) Sistema

Unity-k bi input sistema ditu:

**1. Input Manager Zaharra (Legacy)**

```csharp
// Saguen/teklatuko sarrera
float horizontal = Input.GetAxis("Horizontal"); // A/D edo geziak
float vertical = Input.GetAxis("Vertical");     // W/S edo geziak

// Botoi sakatzeak
if (Input.GetKeyDown(KeyCode.Space))
{
    // Jauzia
}

if (Input.GetMouseButtonDown(0)) // Ezker-klik
{
    // Tiroa
}
```

**2. Input System Berria (Gomendatua)**

```csharp
using UnityEngine.InputSystem;

// Action-en bidez konfiguratua
public void OnMove(InputAction.CallbackContext context)
{
    Vector2 movement = context.ReadValue<Vector2>();
}
```

**Abantailak**:

* Gamepad, teklatua, sagua, VR kontrol-ak... guztiak berdin
* Berriz esleitzearen (rebinding) euskarria
* Multiplayer lokalerako errazagoa

***

### Jokalariaren Oinarrizko Egitura

#### Jokalari Sinple baten Adibidea (3D)

```csharp
using UnityEngine;

public class SimplePlayer : MonoBehaviour
{
    [Header("Mugimendu Ezarpenak")]
    public float moveSpeed = 5f;
    public float jumpForce = 5f;
    
    private Rigidbody rb;
    private bool isGrounded;
    
    void Start()
    {
        // Osagaiak lortu
        rb = GetComponent<Rigidbody>();
    }
    
    void Update()
    {
        // Sarrera jaso
        float moveX = Input.GetAxis("Horizontal");
        float moveZ = Input.GetAxis("Vertical");
        
        // Mugimendua kalkulatu
        Vector3 movement = new Vector3(moveX, 0, moveZ);
        
        // Mugitu
        transform.Translate(movement * moveSpeed * Time.deltaTime);
        
        // Jauzia
        if (Input.GetKeyDown(KeyCode.Space) && isGrounded)
        {
            rb.AddForce(Vector3.up * jumpForce, ForceMode.Impulse);
        }
    }
    
    void OnCollisionStay(Collision collision)
    {
        // Lurrean dagoela egiaztatu
        if (collision.gameObject.CompareTag("Ground"))
        {
            isGrounded = true;
        }
    }
    
    void OnCollisionExit(Collision collision)
    {
        if (collision.gameObject.CompareTag("Ground"))
        {
            isGrounded = false;
        }
    }
}
```

***

### Jokalariaren Diseinu Printzipioak

#### 1. **Erantzukortasuna (Responsiveness)**

Jokalariaren kontrolak berehala erantzun behar dute. Atzerapenak (lag) esperientzia hondatzen du.

**Aholkua**: Input-a `Update()`-n irakurri baina fisika-mugimendua `FixedUpdate()`-n egin.

#### 2. **Irakurgarritasuna (Readability)**

Jokalariaren egoera argi ikusi behar da:

* Osasuna
* Energia
* Munizio-kopurua
* Buff/Debuff-ak

#### 3. **Feedback-a**

Jokalariaren ekintzek erantzun argia izan behar dute:

* **Bisuala**: Animazioak, efektuak
* **Soinuzkoa**: Pausoak, tiro-soinuak
* **Haptikoa**: Dardarak (VR, gamepad)

#### 4. **Intuitiboa**

Kontrolak naturalak eta ulerterrazak izan behar dira:

* WASD mugimendurako (PC)
* Stick ezkerrak mugimendurako (Gamepad)
* Espazio-barra jauzirako
* ESC pausarako

#### 5. **Barkagarria (Forgiving)**

Txikiak erroreak penaletza ez handia:

* **Coyote Time**: Jauzi pixka bat lurun ertzean beherago
* **Input Buffering**: Jauzi-sakatze goiztiarra gorde
* **Aim Assist**: Jokalariaren helburua pixka bat lagundu

***

### Jokalariaren Bizi-Zikloa (`MonoBehaviour` Lifecycle)

```markup
        Jokoa Hasi / Objektua Sortu
                  |
         +--------v--------+
         |    **AWAKE()** |  (Behin bakarrik, hasierako erreferentziak)
         +--------+--------+
                  |
         +--------v--------+
         |   **ONENABLE()**|  (Objektua aktibatzean)
         +--------+--------+
                  |
         +--------v--------+
         |    **START()** |  (Behin bakarrik, lehen Update baino lehen)
         +--------+--------+
```

```markup
          +-------------------------------------------------+
          |                                                 |
          v                                                 |
+---------+----------+                            +---------+----------+
|  **FIXEDUPDATE()** |  (Fisika, denbora finkoa)  |  **UPDATE()** |  (Joko-logika, fotograma bakoitzean)
+---------+----------+                            +---------+----------+
          |                                                 |
          +-------------------------------------------------+
                                  |
                        +---------v----------+
                        |  **LATEUPDATE()** |  (Kamera, Update guztiak amaitu ondoren)
                        +---------+----------+
                                  |
                      [ Errenderizazioa eta Pantaila ]
                                  |
                          (Hurrengo Fotograma)
                                  ^
                                  |
```

```markup
                  Objektua Desaktibatzen da (SetActive(false)) / Ezabatzen da
                                  |
                         +--------v--------+
                         |  **ONDISABLE()**|  (Objektua desaktibatzean)
                         +--------+--------+
                                  |
                         +--------v--------+
                         | **ONDESTROY()** |  (Objektua behin betiko ezabatzean)
                         +--------+--------+
                                  |
                                 AMAITU
```

#### 1. Hasiera (Initialization)

```csharp
void Awake()
{
    // Lehenengo deitzen da, beste objektuak prest egon aurretik
    rb = GetComponent<Rigidbody>();
}

void Start()
{
    // Awake ondoren, beste objektu guztiak prest daudenean
    InitializePlayer();
}
```

#### 2. Eguneraketa (Update Loop)

```csharp
void Update()
{
    // Frame bakoitzean deitzen da
    // Input-a, UI, ez-fisika logika
    HandleInput();
    UpdateUI();
}

void FixedUpdate()
{
    // Fisika-frame bakoitzean (0.02s normalean)
    // Fisika mugimendua
    HandleMovement();
}

void LateUpdate()
{
    // Update ondoren
    // Kamera jarraipena
    FollowCamera();
}
```

#### 3. Amaiera (Destruction)

```csharp
void OnDestroy()
{
    // Objektua suntsitzen denean
    // Garbiketa (cleanup)
    SavePlayerData();
}
```

***

### Jokalari Motak Unity-n

#### Jokalari Fisikoa (Physics-Based)

```csharp
// Rigidbody erabiltzen du
rb.AddForce(direction * speed);
```

**Erabilera**: Joko errealista, simulagailuak, fisika-puzzleak

#### Jokalari Kinematikoa (Kinematic)

```csharp
// CharacterController edo Transform zuzena
controller.Move(direction * speed * Time.deltaTime);
```

**Erabilera**: Plataforma, FPS, kontrol zuzena

#### Jokalari Hibridoa

```csharp
// Batzuetan fisika, beste batzuetan kontrol zuzena
if (inAir)
    rb.isKinematic = false; // Fisika
else
    rb.isKinematic = true;  // Kontrol zuzena
```

**Erabilera**: Joko konplexuak, aldakortasun handiko mekanikak

***

### Jokalariaren Egoerak (States)

Jokalari batek egoera desberdinak izan ditzake:

```csharp
public enum PlayerState
{
    Idle,      // Geldirik
    Walking,   // Oinez
    Running,   // Korrika
    Jumping,   // Jauzi egiten
    Falling,   // Erortzen
    Crouching, // Makurtuta
    Dead       // Hilda
}

private PlayerState currentState = PlayerState.Idle;

void UpdateState()
{
    switch (currentState)
    {
        case PlayerState.Idle:
            // Geldirik dagoenean logika
            break;
        case PlayerState.Walking:
            // Oinez dabilen logika
            break;
        // ...
    }
}
```

***

### Jokalariaren Animazioak

#### Animator Controller

Unity-k **Animator Controller** erabiltzen du animazioak kudeatzeko:

```csharp
Animator animator = GetComponent<Animator>();

// Parametroak ezarri
animator.SetFloat("Speed", currentSpeed);
animator.SetBool("IsGrounded", isGrounded);
animator.SetTrigger("Jump");
```

#### Animazio Motak

* **Idle**: Geldirik dago
* **Walk/Run**: Mugitzen
* **Jump**: Jauzi egiten
* **Fall**: Erortzen
* **Attack**: Erasotzen
* **Hit**: Kolpea jasotzen
* **Death**: Hil

***

### VR Jokalariak ([Meta Quest 3](../../gailuak/vr-betaurrekoak/meta-quest-3.md))

VR-n jokalariak beste dimentsio bat du:

#### Ezaugarri Bereziak

1. **Bi eskuak**: Ezker eta eskuin kontrolak
2. **Burua (HMD)**: 6 askatasun graduko jarraipena
3. **Telepatioa**: Leku batetik bestera "salto" egitea
4. **Eskuz hartzea**: Objektuak fisikoki hartu

#### XR Interaction Toolkit

```csharp
using UnityEngine.XR.Interaction.Toolkit;

// VR objektu bat hartzeko
public class GrabbableObject : XRGrabInteractable
{
    protected override void OnSelectEntered(XRBaseInteractor interactor)
    {
        base.OnSelectEntered(interactor);
        Debug.Log("Objektua hartu da!");
    }
}
```

***

### Praktika Onenak

#### 1. Antolakuntza

```
Player (GameObject)
├── Model (3D eredua edo sprite)
├── Camera (lehen pertsonan)
├── Scripts
│   ├── PlayerController.cs
│   ├── PlayerHealth.cs
│   └── PlayerInventory.cs
└── Audio
    └── AudioSource
```

#### 2. Inspector Ezarpenak

```csharp
[Header("Mugimendu")]
[SerializeField] private float walkSpeed = 5f;
[SerializeField] private float runSpeed = 8f;

[Header("Jauzia")]
[SerializeField] private float jumpForce = 5f;
[SerializeField] private LayerMask groundLayer;

[Header("Osasuna")]
[SerializeField] private int maxHealth = 100;
private int currentHealth;
```

#### 3. Script Komunikazioa

```csharp
// TXARRA: GameObject.Find erabili
GameObject player = GameObject.Find("Player"); // Geldoa!

// ONA: Singleton pattern
public class GameManager : MonoBehaviour
{
    public static GameManager Instance;
    
    void Awake()
    {
        Instance = this;
    }
}

// ONENA: Inspector-etik esleitu
[SerializeField] private PlayerController player;
```

#### 4. Optimizazioa

```csharp
// Kacheatu osagaiak Start()-n
private Rigidbody rb;
private Animator animator;

void Start()
{
    rb = GetComponent<Rigidbody>();
    animator = GetComponent<Animator>();
}

// Ez egin GetComponent() Update()-n!
```

***

### Ohiko Akatsak Ekiditeko

#### ❌ **Akatsa 1**: `GetComponent()` `Update()`-n

```csharp
// TXARRA
void Update()
{
    GetComponent<Rigidbody>().AddForce(...); // Frame bakoitzean!
}

// ONA
void Start()
{
    rb = GetComponent<Rigidbody>();
}
void Update()
{
    rb.AddForce(...);
}
```

#### ❌ **Akatsa 2**: `Time.deltaTime` ahaztea

```csharp
// TXARRA - Abiadura FPS-aren araberakoa
transform.position += direction * speed;

// ONA - Abiadura konstantea
transform.position += direction * speed * Time.deltaTime;
```

#### ❌ **Akatsa 3**: Fisika `Update()`-n

```csharp
// TXARRA
void Update()
{
    rb.AddForce(...);
}

// ONA
void FixedUpdate()
{
    rb.AddForce(...);
}
```

#### ❌ **Akatsa 4**: `Null` egiaztatzeak ez egitea

```csharp
// TXARRA
rb.AddForce(...); // Crash egingo du rb null bada!

// ONA
if (rb != null)
{
    rb.AddForce(...);
}
```

***

### Laburpena

#### Gogoratzekoak:

* 🎮 **Jokalaria** = Erabiltzaileak kontrolatzen duen pertsonaia
* 📦 **GameObject** = Unity-ko oinarrizko unitatea
* 🔧 **Osagaiak** = Transform, Collider, Rigidbody, Script
* ⌨️ **Input** = Teklatua, sagua, gamepad, VR kontrolak
* 🏃 **Mugimendu** = Transform edo Rigidbody bidez
* 🎬 **Animazioak** = Animator Controller
* 🎯 **Feedback** = Bisuala, soinuzkoa, haptikoa
* ⚡ **Errendimendua** = Kacheatu, optimizatu, profil

#### Gomendatutako Lerro-ordena:

1. Sortu jokalari sinple bat mugimendua eta jauziz
2. Gehitu talkak eta triggerrak
3. Inplementatu animazioak
4. Gehitu soinua eta efektuak
5. Optimizatu eta probatu
6. Esperimentatu VR-n!

***

Orain prest zaude jokalari sistema bat sortzen hasteko Unity-n! Jarrai hurrengo gai espezifikoetara sakontzeko. 🚀
