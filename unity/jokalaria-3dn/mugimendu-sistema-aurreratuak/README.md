---
layout:
  width: default
  title:
    visible: true
  description:
    visible: true
  tableOfContents:
    visible: true
  outline:
    visible: true
  pagination:
    visible: true
  metadata:
    visible: true
---

# Mugimendu Sistema Aurreratuak

3D Unity jokoetan jokalariak mugitzeko hiru hurbilpen nagusi daude:

1. [**Rigidbody Mugimendua** ](./#id-1.-rigidbody-mugimendua)- Fisikan oinarritua
2. [**CharacterController**](./#id-2.-charactercontroller-mugimendua) - Pertsonaiak mugitzeko osagai espezializatua
3. [**Transform Mugimendua**](./#id-3.-transform-mugimendua-ez-da-gomendatzen) - Manipulazio zuzena (ez da gomendatzen jokalarientzat)

***

## <kbd>Rigidbody</kbd> Mugimendua

> Rigidbody Unity-ren fisika-osagaia da, objektuak fisika-motorrarekin elkarreragiten dituen osagaia. Grabitatearen, talken, indarra eta momentuaren kudeaketa automatikoki kudeatzen du.

### Noiz erabili <kbd>Rigidbody</kbd>

* Fisikan oinarritutako jokoak (objektuak bultzatzea, mugimendu errealistak)
* Hirugarren pertsonako ekintza-jokoak
* Fisika-elkarrekintzak dituzten plataforma-jokoak
* Fisika-erantzun errealistak behar dituen edozein joko
* Beste fisika-objektuekin elkarreragiteko beharra dagoenean
* 2D jokoak (Rigidbody2D erabili behar da nahitaez)

{% columns %}
{% column %}
#### Abantailak

✅ Fisika-integrazio osoa\
✅ Talka errealistak eta erantzunak\
✅ Indarrak, grabitatearekin, loturak funtzionatzen du\
✅ Unity fisika-ezaugarri guztiekin bateragarria\
✅ 2D eta 3D bietan funtzionatzen du\
✅ Hirugarren aldeen ondasunen euskarri onena
{% endcolumn %}

{% column %}
#### Desabantailak

❌ "Irristakorra" sentitu daiteke edo kontrolatzeko zaila\
❌ Fisikaren doikuntza behar du\
❌ Mugimendu soilerako konplexuagoa\
❌ Fisika-kalkuluen errendimendu-kostua
{% endcolumn %}
{% endcolumns %}

{% tabs %}
{% tab title="Rigibody Oinarrizkoa" %}
{% code overflow="wrap" expandable="true" %}
```csharp
using UnityEngine;

public class RigidbodyPlayerMovement : MonoBehaviour
{
    [Header("Mugimendu Ezarpenak")]
    [SerializeField] private float moveSpeed = 5f;
    [SerializeField] private float jumpForce = 5f;
    
    [Header("Lurra Egiaztatzea")]
    [SerializeField] private Transform groundCheck;
    [SerializeField] private float groundDistance = 0.4f;
    [SerializeField] private LayerMask groundMask;
    
    private Rigidbody rb;
    private bool isGrounded;
    
    void Start()
    {
        rb = GetComponent<Rigidbody>();
        
        // Gomendatutako Rigidbody ezarpenak pertsonaientzat
        rb.freezeRotation = true; // Pertsonaia irauli ez dadin
        rb.interpolation = RigidbodyInterpolation.Interpolate; // Mugimendu leuna
    }
    
    void Update()
    {
        // Lurra egiaztatzea
        isGrounded = Physics.CheckSphere(groundCheck.position, groundDistance, groundMask);
        
        // Jauzi sarrera
        if (Input.GetButtonDown("Jump") && isGrounded)
        {
            rb.AddForce(Vector3.up * jumpForce, ForceMode.Impulse);
        }
    }
    
    void FixedUpdate()
    {
        // GARRANTZITSUA: Fisika-mugimendua FixedUpdate-n gertatu behar da
        Move();
    }
    
    void Move()
    {
        // Sarrera lortu
        float horizontal = Input.GetAxis("Horizontal");
        float vertical = Input.GetAxis("Vertical");
        
        Vector3 direction = new Vector3(horizontal, 0f, vertical).normalized;
        
        if (direction.magnitude >= 0.1f)
        {
            // 1. Metodoa: Abiaduraren bidez (kontrol zuzenagoa)
            Vector3 moveVelocity = direction * moveSpeed;
            rb.velocity = new Vector3(moveVelocity.x, rb.velocity.y, moveVelocity.z);
            
            // 2. Metodoa: Indarren bidez (fisika errealistagoa)
            // rb.AddForce(direction * moveSpeed, ForceMode.Force);
        }
    }
}
```
{% endcode %}
{% endtab %}

{% tab title="Rigibody Aurreratua" %}
```csharp
// Aireko kontrola (mugimendu murriztua jauzi egitean)
public class AdvancedRigidbodyMovement : MonoBehaviour
{
    [SerializeField] private float groundMoveSpeed = 5f;
    [SerializeField] private float airMoveSpeed = 2f;
    [SerializeField] private float groundDrag = 5f;
    [SerializeField] private float airDrag = 0.5f;
    
    private Rigidbody rb;
    private bool isGrounded;
    
    void FixedUpdate()
    {
        // Drag-a doitu lurra-egoeraren arabera
        rb.drag = isGrounded ? groundDrag : airDrag;
        
        float currentSpeed = isGrounded ? groundMoveSpeed : airMoveSpeed;
        
        // Hemen mugimendu-kodea currentSpeed erabiliz
    }
}
```
{% endtab %}

{% tab title="Rigibody Ezarpen garrantzitsuak" %}
```csharp
// Start() edo Awake()-n
Rigidbody rb = GetComponent<Rigidbody>();

// Biraketa galarazi (pertsonaiak ez luke irauli behar)
rb.freezeRotation = true;

// Edo ardatz zehatzak izoztu
rb.constraints = RigidbodyConstraints.FreezeRotation;

// Mugimendu leuna fisika-eguneratzen artean
rb.interpolation = RigidbodyInterpolation.Interpolate;

// Talka-detekzio modua
rb.collisionDetectionMode = CollisionDetectionMode.ContinuousDynamic; // Objektu azkarrentzat

// Masa (indarrek objektua nola mugitzen duten eragiten du)
rb.mass = 1f; // Lehenetsita normalean ondo dago

// Drag (airearen erresistentzia)
rb.drag = 0f; // Handitu mugimenduarekiko erresistentzia gehiagotzeko
rb.angularDrag = 0.05f; // Biraketa-erresistentzia
```
{% endtab %}
{% endtabs %}

{% hint style="success" %}
#### Praktika onak <kbd>Rigidbody</kbd> Mugimendurako

1. Beti mugitu `FixedUpdate()`-n, ez `Update()`-n
2. Erabili `rb.freezeRotation = true` iraultzea galarazteko
3. Ezarri drag balio egokiak irristatzea kontrolatzeko
4. Erabili `Interpolation` mugimendu leunerako
5. Inplementatu lurra-egiaztatzea raycast edo esfera egiaztapen-ekin
6. Kontuan izan kinematika modua erabili behar kontrolak gehiago behar badituzue
{% endhint %}

<details>

<summary><kbd>Rigidbody</kbd> Arazo Arruntak eta Konponketak</summary>

Arazoa: Pertsonaia irristatzen da gelditzen denean

```csharp
// Konponketa: Gehitu drag-a edo abiadura berrezarri eskuz
rb.drag = 5f; // Edo handiagoa

// Edo kodean:
if (moveInput == Vector3.zero)
{
    rb.velocity = new Vector3(0, rb.velocity.y, 0);
}
```

Arazoa: Pertsonaia irauli egiten da

```csharp
// Konponketa: Izoztu biraketa
rb.freezeRotation = true;
```

Arazoa: Astunegi Mugitzea

```csharp
// Konponketa: Erabili interpolazioa
rb.interpolation = RigidbodyInterpolation.Interpolate;
```

</details>

***

## <kbd>**CharacterController**</kbd> Mugimendua

> CharacterController Unity-ren osagai espezializatu bat da, espresuki pertsonaien mugimenduetarako diseinatua. Ez du fisika-indarrik erabiltzen baina talka-detekzioa eta irristatzeko portaera eskaintzen ditu.

### Noiz erabili <kbd>CharacterController</kbd>

* Lehen pertsonako tiratzaileak (FPS)
* Mugimendu zehatza, arkade-estilokoa behar duzunean
* Fisika-elkarrekintzak ez direnean garrantzitsuak
* Fisika "sentipenik" gabe kontrol osoa nahi duzunean
* Fisika-puzzlerik gabeko plataforma-joko soilak

{% columns %}
{% column %}
#### Abantailak

✅ Kontrol zuzenagoa (irristatzerik/momenturik ez)\
✅ Lurra-detekzio integratua (`isGrounded`)\
✅ Malda eta eskailerak automatikoki kudeatzen ditu\
✅ Fisika-kosturik ez\
✅ Oinarrizko mugimendurako errazagoa\
✅ Fisika-ezarpenen doikuntzarik ez behar
{% endcolumn %}

{% column %}
#### Desabantailak

❌ **3D-n soilik funtzionatzen du** (ez dago 2D bertsiorik)\
❌ Ez du Rigidbody fisikarekin ondo funtzionatzen\
❌ Ezin ditu fisika-indarrak, loturak edo murrizketak erabili\
❌ Talka-erantzun aukera mugatuak\
❌ Arazoak izan ditzake trigger-ekin\
❌ Hirugarren aldeen ondasunen euskarri gutxiago
{% endcolumn %}
{% endcolumns %}

{% tabs %}
{% tab title="Oinarrizko konfigurazioa" %}
{% code overflow="wrap" expandable="true" %}
```csharp
using UnityEngine;

public class CharacterControllerMovement : MonoBehaviour
{
    [Header("Mugimendu Ezarpenak")]
    [SerializeField] private float moveSpeed = 5f;
    [SerializeField] private float jumpHeight = 2f;
    [SerializeField] private float gravity = -9.81f;
    
    [Header("Lurra Egiaztatzea")]
    [SerializeField] private Transform groundCheck;
    [SerializeField] private float groundDistance = 0.4f;
    [SerializeField] private LayerMask groundMask;
    
    private CharacterController controller;
    private Vector3 velocity;
    private bool isGrounded;
    
    void Start()
    {
        controller = GetComponent<CharacterController>();
    }
    
    void Update()
    {
        // Lurra egiaztatzea
        isGrounded = Physics.CheckSphere(groundCheck.position, groundDistance, groundMask);
        
        // Abiadura berrezarri lurrean dagoenean
        if (isGrounded && velocity.y < 0)
        {
            velocity.y = -2f; // Balio negatiboa txikia lurrean mantentzeko
        }
        
        // Sarrera lortu
        float horizontal = Input.GetAxis("Horizontal");
        float vertical = Input.GetAxis("Vertical");
        
        // Mugimendu-norabidea kalkulatu
        Vector3 move = transform.right * horizontal + transform.forward * vertical;
        
        // Pertsonaia mugitu
        controller.Move(move * moveSpeed * Time.deltaTime);
        
        // Jauzia
        if (Input.GetButtonDown("Jump") && isGrounded)
        {
            velocity.y = Mathf.Sqrt(jumpHeight * -2f * gravity);
        }
        
        // Grabitatearen aplikazioa
        velocity.y += gravity * Time.deltaTime;
        
        // Abiadura bertikala aplikatu
        controller.Move(velocity * Time.deltaTime);
    }
}
```
{% endcode %}
{% endtab %}

{% tab title="CharacterController Kamera Biraketa-rekin" %}
```csharp
using UnityEngine;

public class FPSController : MonoBehaviour
{
    [Header("Mugimendua")]
    [SerializeField] private float moveSpeed = 5f;
    [SerializeField] private float sprintSpeed = 8f;
    [SerializeField] private float jumpHeight = 2f;
    [SerializeField] private float gravity = -9.81f;
    
    [Header("Begiratzea")]
    [SerializeField] private Transform cameraTransform;
    [SerializeField] private float mouseSensitivity = 100f;
    
    private CharacterController controller;
    private Vector3 velocity;
    private float xRotation = 0f;
    
    void Start()
    {
        controller = GetComponent<CharacterController>();
        Cursor.lockState = CursorLockMode.Locked;
    }
    
    void Update()
    {
        HandleMovement();
        HandleLook();
    }
    
    void HandleMovement()
    {
        bool isGrounded = controller.isGrounded;
        
        if (isGrounded && velocity.y < 0)
        {
            velocity.y = -2f;
        }
        
        float x = Input.GetAxis("Horizontal");
        float z = Input.GetAxis("Vertical");
        
        Vector3 move = transform.right * x + transform.forward * z;
        
        // Sprint
        float currentSpeed = Input.GetKey(KeyCode.LeftShift) ? sprintSpeed : moveSpeed;
        
        controller.Move(move * currentSpeed * Time.deltaTime);
        
        if (Input.GetButtonDown("Jump") && isGrounded)
        {
            velocity.y = Mathf.Sqrt(jumpHeight * -2f * gravity);
        }
        
        velocity.y += gravity * Time.deltaTime;
        controller.Move(velocity * Time.deltaTime);
    }
    
    void HandleLook()
    {
        float mouseX = Input.GetAxis("Mouse X") * mouseSensitivity * Time.deltaTime;
        float mouseY = Input.GetAxis("Mouse Y") * mouseSensitivity * Time.deltaTime;
        
        xRotation -= mouseY;
        xRotation = Mathf.Clamp(xRotation, -90f, 90f);
        
        cameraTransform.localRotation = Quaternion.Euler(xRotation, 0f, 0f);
        transform.Rotate(Vector3.up * mouseX);
    }
}
```
{% endtab %}
{% endtabs %}

#### <kbd>CharacterController</kbd> Ezarpen Garrantzitsuak

* **Slope Limit**: Pertsonaiak igo dezakeen gehienezko malda-angelua (lehenetsia: 45°)
* **Step Offset**: Pertsonaiak eman dezakeen urrats bertikalaren muga; hau da, igo ditzakeen eskaileren/koxken gehienezko altuera (lehenetsia: 0.3m)
* **Skin Width**: Kolisionatzaileak elkarrengandik zenbateko urrun mantentzen diren (lehenetsia: 0.08m)
* **Min Move Distance**: Pertsonaiak mugitu behar duen gutxieneko distantzia (lehenetsia: 0.001m)
* **Center**: Kapsula-kolisionatzailearen zentroa
* **Radius**: Kapsularen erradioa
* **Height**: Kapsularen altuera

<figure><img src="../../../.gitbook/assets/irudia (6).png" alt=""><figcaption></figcaption></figure>

{% hint style="success" %}
#### Praktika onak <kbd>CharacterController</kbd> Mugimendurako

1. Mugitu `Update()`-n `Time.deltaTime` erabiliz
2. Beti biderkatu mugimendua `Time.deltaTime`-z
3. Sortu GameObject huts bat oinetatik lurra egiaztatzeko
4. Kudeatu grabitatearen eskuz
5. Mantendu `Skin Width` txikia (0.08 lehenetsia ondo dago)
6. Doitu `Step Offset` zure jokoko urrats/eskaileraren arabera
{% endhint %}

<details>

<summary><kbd>CharacterController</kbd> Arazo Arruntak eta Konponketak</summary>

Arazoa: Ezin da berehala jauzi egin lurreratu ondoren

```csharp
// Konponketa: Gehitu buffer edo doitu lurra-egiaztatzea
if (isGrounded && velocity.y < 0)
{
    velocity.y = -2f; // Mantendu pixka bat negatiboa
}
```

Arazoa: Hormekin trabatzen da

```
// Konponketa: Doitu skin width (Inspector-ean)
// Edo ziurtatu kolisionatzaileak ondo konfiguratuta daudela
```

</details>

***

## <kbd>Transform</kbd> Mugimendua (Ez da gomendatzen)

```csharp
// Sinplea baina TXARRA jokalarientzat - talka-detekziorik ez
void Update()
{
    float horizontal = Input.GetAxis("Horizontal");
    float vertical = Input.GetAxis("Vertical");
    
    Vector3 move = new Vector3(horizontal, 0, vertical);
    transform.position += move * moveSpeed * Time.deltaTime;
}
```

#### Zergatik ez erabili <kbd>Transform</kbd> Mugimendua?

❌ Talka-detekzio automatikorik ez\
❌ Hormak zeharkatzen ditu\
❌ Fisika-elkarrekintzarik ez\
❌ Talka-kudeaketa manuala behar du\
❌ Ez da egokia jokalari-pertsonaientzat

#### <kbd>**Transform**</kbd>**&#x20;mugimendua soilik erabili:**

* Objektu ez-fisikoetarako (mundu-espazioko UI elementuak, efektuak)
* Talkak ignoratu behar dituzten objetuentzat
* Cutscene mugimenduentzat

***

{% hint style="warning" %}
### Gomendio orokorrak

* Erabili `Input.GetAxis()` sarrera leunerako (ez `GetKey()`)
* Normalizatu mugimendu diagonala abiadura azkarrago diagonala saihesteko
* Inplementatu azelerazioa/deszelerazioa sentimentu hoberako
* Gehitu coyote time jauzi barkatsuagorako
* Kontuan izan sarrera-buffering-a kontrol erantzuleentzat
{% endhint %}

***

## Laburpena

| Ezaugarria                  | Rigidbody                      | CharacterController | Transform           |
| --------------------------- | ------------------------------ | ------------------- | ------------------- |
| **Fisika Integrazioa**      | ✅ Osoa                         | ❌ Bat ere ez        | ❌ Bat ere ez        |
| **Talka Detekzioa**         | ✅ Automatikoa                  | ✅ Automatikoa       | ❌ Eskuzkoa soilik   |
| **2D Euskarria**            | ✅ Bai (`Rigidbody2D`)          | ❌ Ez                | ✅ Bai               |
| **Indar/Inpulsu Euskarria** | ✅ Bai                          | ❌ Ez                | ❌ Ez                |
| **Lurraren Detekzioa**      | ⚠️ Manuala                     | ✅ Integratua        | ❌ Manuala           |
| **Malden Kudeaketa**        | ⚠️ Fisikan oinarritua          | ✅ Integratua        | ❌ Bat ere ez        |
| **Kontrolen Zehaztasuna**   | ⚠️ Ertaina                     | ✅ Altua             | ✅ Altua             |
| **Fisiken Errealismoa**     | ✅ Altua                        | ❌ Bat ere ez        | ❌ Bat ere ez        |
| **Errendimendua**           | ⚠️ Ertaina                     | ✅ Ona               | ✅ Onena             |
| **Konplexutasuna**          | ⚠️ Erdi-Altua                  | ✅ Baxu-Ertaina      | ✅ Baxua             |
| **Jomuga**                  | Fisika jokoak, 3. pertsonakoak | FPS, arkade jokoak  | Objektu ez-fisikoak |

### Gomendatutako Hurbilpena Joko Mota Arabera

* **Lehen Pertsonako Tiratzailea (FPS)**: CharacterController
* **Hirugarren Pertsonako Ekintza/Abentura**: Rigidbody
* **Fisika Puzzle Jokoa**: Rigidbody
* **2D Plataforma**: Rigidbody2D (ez dago aukerarik)
* **Lasterketa Jokoa**: Rigidbody fisika pertsonalizatuarekin
* **Arkade Joko Sinplea**: Biak balio, CharacterController errazagoa da

**Aukeratuko duzu&#x20;**<kbd>**Rigidbody**</kbd>**&#x20;...**

* Fisika-elkarrekintzak behar dituzunean
* 2D joko bat egiten ari zarenean
* Mugimendu eta talka errealistak nahi dituzunean
* Fisikan oinarritutako joko bat eraikitzen ari zarenean

**Aukeratuko duzu&#x20;**<kbd>**CharacterController**</kbd>**&#x20;...**

* FPS bat egiten ari zarenean
* Kontrol zehatza, arkade-estilokoa nahi duzunean
* Ez dituzu fisika-elkarrekintzak behar
* Inplementazio errazagoa nahi duzunean
