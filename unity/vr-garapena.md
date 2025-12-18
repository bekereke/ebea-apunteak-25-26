# VR Garapena

Azalpen orokorra Building Blocks edo hutsetik egitearena

### MRUK Manager

{% embed url="https://developers.meta.com/horizon/documentation/unity/unity-mr-utility-kit-overview/" %}

{% embed url="https://github.com/oculus-samples/Unity-MRUtilityKitSample?tab=readme-ov-file" %}

### Gelaren sareta eredua

Mailak edo _Scene/Room Model/Mesh_, era askotariko izenak hartzen ditu ingurune fisikoaren eskaneoak eta haren grafiko bidezko irudikapen birtualak.

### Oklusioa

_Occlusion_ edo objektu birtualak mundu errealeko objektuen atzean ezkutatzea (pareta, mahaia, monitorea...) MR ingurunetan ezinbestekoa den eta aldi berean zailenetarikoa den ezaugarria da.&#x20;

{% hint style="danger" %}
#### GARRANTZITSUA:

Ingurunean eskaneoa egiteko (Entorno) _Meta Horizon Link_ programaren bidez bisoretako ezarpenetan BETA aukera batzuk aktibatu behar dira:&#x20;

![](<../.gitbook/assets/irudia (3).png>)

**Scene Model** edo Ingurunearen Eredua izatea beharrezkoa da paretak eta objektuak identifikaturik izateko eta haiekin interaktuatu ahal izateko EMan.&#x20;

Ondoren, nahiz eta aurreragoko ADIBIDE PRAKTIKOko aukeretan **Scene Support** Required jarri, ez du automatikoki ingurunearen irakurketa egingo. Beraz, onena da gauden lekuaren irakurketa egitea jarduera hasi aurretik. Bisoreetako ezarpenetan, egin ere: &#x20;

![](<../.gitbook/assets/irudia (8).png>)
{% endhint %}

<details>

<summary><del>ADIBIDE PRAKTIKOA (zaharkitua 2025/12/11)</del></summary>

## EM Kubo irristagarriarena

<kbd>Meta XR All-in-One Building Block</kbd>a erabiliko da Unityren 6 bertsioan. Ezertan hasi orduko XR pluginak instalatu: Open XR aktibatu (<kbd>Meta XR feature group</kbd> aukeratu) bai Windows zein Androiderako, eta, azken hau aktibatu Build Profile gisa.&#x20;

### 1. Oinarrizko Setup-a (Meta XR All-in-One SDK)

Kamera eta Passthrough eskuz konfiguratu daitezkeen edo bloke bat erabili  (`[BuildingBlock] Meta XR All-in-One` ). Lehena egingo dugu, eskuz.&#x20;

1. Eszena garbi batean (ezabatu `Main Camera` ).
2. Bilatu Project atalean <kbd>OVRCameraRig</kbd> prefaba (<kbd>in Packages</kbd> barruan) eta eraman eszenako errora.&#x20;
3. <kbd>OVRPassthroughLayer</kbd> bilatu Project ataleko paketeetan berriz ere eta eszenara eraman.&#x20;
4. Inspector-ean, ziurtatu Kameran ondoko ezarpenak aktibatuta daudela:&#x20;
   1. OVR Manager -> Quest 3 ✅
   2. Quest Features -> General -> Hand Tracking Support -> **Controllers And Hands**
   3. Quest Features -> General -> Hand Tracking Frequency -> **High**
   4. Quest Features -> General -> Scene Support -> **Required**
   5. Quest Features -> General -> Passthrough Support -> **Supported**

<figure><img src="../.gitbook/assets/Pantaila-argazkia 2025-12-11 1206531.png" alt=""><figcaption></figcaption></figure>

5. Kamera barruan LeftHandAnchor blokean gehitu Proiektutik arrastratuta <kbd>OVRHandPrefab</kbd> bana ezkerreko eskura. RightHandAnchorekin ere berdina egin. Batekin zein bestearekin moldatu ezarpenak esku bakoitzera:

<p align="center"><img src="../.gitbook/assets/irudia (7).png" alt=""></p>

### 2. Gela Kargatu (<kbd>Scene Mesh</kbd>)

All-in-One daukazun arren, gela kargatzeko blokea gehitu behar duzu.

1. Arrastatu Scene Mesh blokea eszenara (Building Blocks leihotik).
2. Play ematean gela ez bada agertzen, ziurtatu All-in-One barruko `OVRManager` ezarpenetan Scene Support aktibatuta dagoela (askotan All-in-One-k automatikoki egiten du, baina egiaztatzea ondo dago).

<div><figure><img src="../.gitbook/assets/Pantaila-argazkia 2025-12-11 113225.png" alt=""><figcaption></figcaption></figure> <figure><img src="../.gitbook/assets/Pantaila-argazkia 2025-12-11 1142011.png" alt=""><figcaption></figcaption></figure> <figure><img src="../.gitbook/assets/Pantaila-argazkia 2025-12-11 1154271.png" alt=""><figcaption></figcaption></figure></div>

### 3. Oklusioa Konfiguratu (Hormen Atzean Ezkutatzea)

Hormak ikusezin bihurtuko ditugu, baina kuboa hormaren atzean badoa, desagertu egingo da (errealismoa).

1. Materiala Sortu: Project leihoan, eskuin klik > `Create > Material`. Deitu "_OcclusionMat_".
2. Shader-a Aldatu: Materiala aukeratuta, Inspector-ean `Shader` jartzen duen goiko menuan, bilatu eta aukeratu hauetako bat:
   * Aukera A (Gomendatua): `Meta > Universal > Occlusion` (URP erabiltzen baduzu).
   * Aukera B (Alternatiba): `Oculus > Unlit > Transparent` (eta jarri kolorearen Alpha/Gardentasuna 0-ra).
   * Aukera C (Azkarra): Bilatu proiektuan "MatteShadow" izeneko materiala existitzen den (Samples badituzu).
3. Prefab-a Editatu:
   * Aukera Hierarchy-an <kbd>Scene Mesh</kbd> objektua.
   * Inspector-ean, bilatu `Mesh Prefab` edo `Volume Prefab` aldagaia.
   * Egin klik aldagaian dagoen prefab-ean proiektuan non dagoen ikusteko.
   * Ireki prefab hori (klik bikoitza).
   * Arrastatu sortu duzun "OcclusionMat" materiala prefabaren gainera (urdina izateari utzi eta beltz/garden bihurtu behar da).
   * Gorde eta atera prefab-etik.

> Argazkia: _OcclusionMat materialaren ezarpenak eta Scene Mesh prefab-ari aplikatzen._

### 4. Kuboa Sortu eta Konfiguratu (Fisika)

Zati hau berdin mantentzen da, funtsezkoa baita zero grabitaterako.

1. Sortu: <kbd>GameObject > 3D Object > Cube</kbd>.
2. Gehitu osagaia: <kbd>Rigidbody</kbd>.
3. Konfigurazio zehatza Inspector-ean:
   * Mass: `0.01` (Arina).
   * Linear Damping: `0` (Frenorik ez).
   * Angular Damping: `0.05`.
   * Use Gravity: ⬜ DESMARKATU.
   * Is Kinematic: ⬜ DESMARKATU.
   * Collision Detection: Continuous Dynamic.
   * Constraints: Ziurtatu `Freeze Position` guztiak hutsik daudela.

<figure><img src="../.gitbook/assets/Pantaila-argazkia 2025-12-11 121259.png" alt=""><figcaption></figcaption></figure>

### 5. Eskuak Aurkitu eta Prestatu (Garrantzitsua)

"OVRCameraRig" prefab bat denez, eskuak barruan "ezkutatuta" daude.&#x20;

1. Hierarchy-an, zabaldu <kbd>OVRCameraRig</kbd> objektuaren gezia.
2.  Jarraitu bide hau:

    TrackingSpace > LeftHandAnchor.
3. LeftHandAnchor aukeratuta, gehitu osagaiak Inspector-ean:
   * Sphere Collider (Radius: `0.05`).
   * Rigidbody:
     * Use Gravity: ⬜ Desmarkatu.
     * Is Kinematic: ✅ MARKATU.
     * Collision Detection: Continuous Dynamic.
4. Errepikatu `RightHandAnchor`-arekin (TrackingSpace barruan dagoena ere).

<figure><img src="../.gitbook/assets/irudia (3) (1).png" alt=""><figcaption></figcaption></figure>

### 6. Material Fisikoa (Irristakorra)

Kuboa gainazaletan ez itsasteko.

1. Sortu Physic Material bat (Project leihoan) `ZeroFriction` izenarekin.
2. Ezarri balioak:
   * Friction (Dynamic/Static): `0`
   * Bounciness: `0.8`
   * Friction Combine: `Minimum`
   * Bounce Combine: `Maximum`
3. Arrastatu material hau Kuboaren <kbd>Box Collider</kbd>-era.

> Argazkia: _Physic Materialaren ezarpenak._

***

{% hint style="info" %}
**Oharra:**

"All-in-One" erabiltzean, gogoratu beti TrackingSpace karpeta bilatu behar dela kameraren eta eskuen osagaiak aurkitzeko. Prefab bat denez, geruzaka antolatuta dago.
{% endhint %}

</details>

<details>

<summary>ADIBIDE PRAKTIKOA </summary>

## EM Kubo irristagarriarena: passthrough, oklusioa eta gela eredua

<kbd>Meta XR All-in-One Building Block</kbd>a erabiliko da v83 eguneraketan eta Unityren 6 bertsioan. Ezertan hasi orduko XR pluginak instalatu: Open XR aktibatu (<kbd>Meta XR feature group</kbd> aukeratu) bai Windows zein Androiderako, eta, azken hau aktibatu Build Profile gisa.&#x20;

***

### 1. PAUSOA: Oinarria (MRUK Manager)

Hau da garuna. Dena kudeatzen duena.

1. Hierarchy: Sortu objektu huts bat eta deitu `MRUK`.
2. Osagaia: Gehitu `MRUK` script-a.
3. Konfigurazioa:
   * Config: Ziurtatu `MRUK` fitxategia baduela (berezkoa).

***

### 2. Materialak Prestatu

#### A. Oklusio Shaderra (Ikusezina)

1. Project leihoan: Eskuin Klik > Create > Shader > Unlit Shader.
2. Deitu: <kbd>OcclusionMat</kbd>.
3. Ireki eta itsatsi hau:

```shader
Shader "Custom/OcclusionMat" {
    SubShader {
        Tags { "RenderType"="Opaque" "Queue"="Geometry-1" }
        ColorMask 0
        ZWrite On
        Pass {}
    }
}
```

4. Sortu materiala shader horretatik: saguaren eskuineko botoiari sakatu eta materiala sortu (zuzenean shaderra erantsiko dio) `OcclusionMat` izendu.

#### B. Fisika Materiala (Irristakorra)

1. Project leihoan: Eskuin Klik > Create > Physic Material.
2. Deitu: `ZeroFriction`.
3. Inspector: Friction (`0`), Bounciness (`0.8`), Combinations (`Minimum`/`Maximum`).

#### 2. PAUSOA: Gela Sortu (Fisika + Oklusioa)

Mailak edo saretak (mesh) sortuko ditugu `EffectMesh` erabiliz:

1. Hierarchy-an, egin klik eskuineko botoiarekin `MRUK` objektuaren gainean -> Create Empty Child. Deitu `Room_Environment`.
2. Gehitu osagaia: `EffectMesh`.
3. Konfiguratu `EffectMesh` honela (oso garrantzitsua):
   * Labels: Aukeratu `WALL_FACE`, `FLOOR`, `CEILING`, <kbd>TABLE</kbd>, <kbd>COACH</kbd>.
   * Create Colliders: ✅ MARKATU. (Hau gabe, ez dago fisikarik).
   * Hide Mesh: ⬜ DESMARKATU (Kendu tick-a). Ikusi egin nahi dugu, baina gure material bereziarekin.
4. Materialak:
   * Mesh Material: Arrastatu zure `OcclusionMat`. (Honek hormak ikusezin bihurtuko ditu, baina atzean dagoena estaliko du).

***

#### 3. PAUSOA: Pareten Fisikak Bermatu

Pareten fisikak ez dute nahikoa indar ematen bueltan kolisioaren ondoren pilotak (edo kuboak) nahikoa indarrarekin errebotatu dezan. <kbd>MRUK</kbd> kudeatzailearen barruko <kbd>EffectMesh</kbd> objektuak paretak automatikoki sortuko dituenez (<kbd>Collider</kbd> aukera) aurretik gela eskaneatu dugulako (betaurrekoetan <kbd>Configuración > Entorno</kbd>) sortzerakoan zehaztuko diogun lehentasunezko materialarekin sortzea eragingo dugu:

1. Joan Edit > Project Settings > Physics.
2. Default Material: Arrastatu hona zure `ZeroFriction` materiala.
   * _(Gogoratu ZeroFriction materiala: Bounciness = 1, Bounce Combine = Maximum)._

{% hint style="info" %}
Zer lortzen da honekin? `EffectMesh`-ek hormak sortzen dituenean, ez die material fisiko espezifikorik jartzen. Unityk ikusten duenean horma horiek "hutsik" daudela, zure Default Material (`ZeroFriction`) aplikatuko die automatikoki.
{% endhint %}

***

#### 4. PAUSOA: Jokalaria (Kamera + Eskuak + Aginteak)

Hau lehen bezala mantentzen da.

1. Hierarchy-an `OVRCameraRig`.
2. `OVRManager` script-ean:
   * Passthrough Capability: Enabled.
   * Insight Passthrough: Enabled.
3. `OVRPassthroughLayer` script-a gehitu `OVRCameraRig`-i:
   * Placement: `Underlay`.
4. Eskuak eta Aginteak:
   * `LeftHandAnchor` barruan: `OVRHandPrefab` (Left) eta `OVRControllerPrefab` (Left).
   * `RightHandAnchor` barruan: `OVRHandPrefab` (Right) eta `OVRControllerPrefab` (Right).

***

#### 5. PAUSOA: Kuboa

Hau da hormen kontra jaurtiko duzun objektua.

1. Hierarchy-an sortu Cube bat.
2. Tamaina (Scale): `0.2, 0.2, 0.2` (Kubo handiegiek arazoak ematen dituzte gela txikietan).
3. Rigidbody:
   * Use Gravity: ✅.
   * Collision Detection: Continuous Dynamic (Derrigorrezkoa hormak ez zeharkatzeko).
4. Box Collider:
   * Ziurtatu baduela.
   * Material: Jarri hemen ere `ZeroFriction` (badaezpada).
5. Mesh Renderer:
   * Material: Kolore bizi bat (Gorria adibidez), ondo ikusteko.
   * Dynamic Occlusion: ⬜ Desmarkatu.

</details>

#### Hurrengo urratsa?

Orain oinarri sendoa daukazuenez (gela erreala + fisika), zer gustatuko litzaizuke gehitzea esperientziari? Hona hemen ideia batzuk:

1. Interakzioa: Kuboa eskuarekin hartu eta bota ahal izatea (Grab Interaction).
2. Spawner: Objektuak (monstruotxoak, mamuak edo loreak) automatikoki sortzea lurrean edo mahai gainean.
3. Joko Mekanikak: Kuboak horma jotzen duenean soinua egitea edo kolorez aldatzea. Pilota batekin ordezkatu eta eskupilota partiduak antolatu bigarren botea ematen baldin badu tantoa zenbatuz.
