# Tablet Bar Mount

Své repo vytvoříte na https://classroom.github.com/a/my7o3Upb

Deadline je **7.1.2018** 23:59 CET. Vypracovaný úkol **nahrajte do repozitáře vytvořeného na odkaze výše**. Nikam jinam jej neposílejte, jako odevzdání se počítá to, co bude ve vašem repozitáři (ve výchozí větvi (většinou `master`)) v momentu deadlinu.

**Pokud máte jakékoliv dotazy, či naleznete chyby, napište je prosím do Issues k tomuto repozitáři.**

## Motivace

Nic podobného neexistuje, i přes poptávku motorkářů, čili po splnění úkolu ho můžete opensourcovat a publikovat například na Thingiverse nebo podobných webech a býti slavní... :)

## Zadání

Vaším úkolem je namodelovat držák na tablet na řídítka motocyklu, za účelem použití tabletu k navigaci.
Držák tabletu je potřeba namontovat na představce řídítek, protože to je nejbezpečnější a zároveň nejpevnější místo, které je jezdci snadno na očích.

## Model

Vámi namodelovaný model se bude skládat ze dvou objektů.

  - Horní část modelu s výřezem pro display tabletu
  - Spodní část modelu, který se dá přídělat na přestavce řídítek
  - Je nezbytné aby vámi naprogramovaný model byl správně umístěn do souřadnic
    - Spodní část objímek musí ležet na `z = 0`
    - Střed rozteče objímek musí ležet na `x = 0` a `y = 0`
    - Řídítka motocyklu jsou rovnoběžné s osou x
    - Jezdec se na tablet dívá ze záporného směru po ose y (tedy dívá se kladným směrem)
    - Samostatně použitý `top_part` leží na podložce (`z = 0`), vystředěn podle os x a y
        - displayem vzhůru, dírami na kabely po směru osy x
    - **Pokud toto nebude Váš model splňovat, tak neprojdete ani jedním testem**

## Nefunkční požadavky

  - S hodnotou `$fn` můžete pracovat pouze v případě vytvoření šestihranných děr pro matky (viz zadání)
    - Manipulace s jinými `$f*` hodnotami je zakázána
  - Je zakázáno použít konstrukci `minkowski()` (ve 3D i ve 2D prosotoru)
  - Není doporučováno používat rekurzi, ani to k vyřešení úkolu není zapotřebí
  - Využití externích knihoven (včetně knihovny MCAD) je zakázáno
  - Pokud je něco **zakázáno**, vede použití k tomu, že **neprojdou testy** a dostáváte **0 bodů**
  - Váš kód musí splňovat určitou kvalitu (**tato část tvoří 5 bodů z celkových 30 možných**)
    - Opakování v kódu je špatně, vždy použijte moduly a cykly
    - Bulharské konstanty musí být doplněny o vysvětlující komentář
    - Dodržte logickou úroveň odsazení

## Interface modelu

```
module tablet_bar_mount(holder_width=180,
                        holder_height=100,
                        holder_thickness=5,
                        holder_overlay_thickness=3,
                        holder_position_x=0,
                        holder_position_y=0,
                        holder_position_angle=0,
                        tablet_width=160,
                        tablet_height=80,
                        tablet_depth=8,
                        tablet_screen_width=145,
                        tablet_screen_height=65,
                        rounded_corner_radius=5, 
                        connecting_screw_diameter=3,
                        cable_cutout_height=60,
                        cable_cutout_depth=5,
                        nut_diameter=5,
                        nut_offset=6.5,
                        nut_depth=1.5,
                        screw_diameter=4,
                        screw_head_diameter=6,
                        screw_head_depth=3,
                        screw_spacing=15,
                        raisers_spacing=100,
                        raiser_width=20,
                        raiser_height=40,
                        raiser_depth=20,
                        raiser_inlet_wall_thickness=6,
                        raiser_inlet_top_thickness=5,
                        bar_diameter=15,
                        bar_location=17,
                        parts_offset=1.5) {}
```

Tento interface se dělí na na interface dalších dvou modulů `top_part(...)` a `bottom_part(...)`

V případě zavolání `tablet_bar_mount` se vykreslí obě části napozicované dle dalších pravidel a obrázků.
Mezera mezi nimi je `parts_offset` (měřeno kolmo ke styčným plochám).

## Horní část

### Interface

```
module top_part(holder_width,
                holder_height,
                holder_overlay_thickness,
                tablet_width,
                tablet_height,
                tablet_depth,
                tablet_screen_width,
                tablet_screen_height,
                rounded_corner_radius, 
                connecting_screw_diameter,
                connecting_screw_offset,
                cable_cutout_height,
                cable_cutout_depth) {}
```

### Argumenty
  - `holder_width, holder_height` jsou rozměry držáku, tloušťka horní části se odvíjí od `tablet_depth` a `holder_overlay_thickness`
  - `holder_overlay_thickness` je tloušťka modelu mezi displayem a horní částí držáku, čili ta část, která překrývá okraje tabletu, kde není display
  - `tablet_width, tablet_height, tablet_depth` jsou rozměry tabletu
  - `tablet_screen_width, tablet_screen_height` jsou rozměry displaye tabletu, čili otvor, který bude vyříznut do horní části držáku.
  - `rounded_corner_radius` poloměr vnějšího zakřivení rohů, pokud je argument nulový tak rohy nejsou zaoblené
  - `connecting_screw_diameter` průměr šroubu, které spojují horní a spodní část držáku
  - `connecting_screw_offset` pozice šroubu výše, definována nákresem, u modulu `tablet_bar_mount` přejímá hodnotu `nut_offset`
  - `cable_cutout_height` výška výřezu pro kabely
  - `cable_cutout_depth` tloušťka výřezu pro kabely

## Spodní část

### Interface

```
 module bottom_part(holder_width,
                    holder_height,
                    holder_thickness,
                    rounded_corner_radius,
                    connecting_screw_diameter,
                    nut_diameter,
                    nut_offset,
                    nut_depth,
                    screw_diameter,
                    screw_head_diameter,
                    screw_head_depth,
                    screw_spacing,
                    raiser_width,
                    raiser_height,
                    raiser_depth,
                    raiser_inlet_wall_thickness,
                    raiser_inlet_top_thickness,
                    bar_diameter,
                    bar_location,
                    raisers_spacing,
                    holder_position_x,
                    holder_position_y,
                    holder_position_angle) {}
```

### Argumenty

  - `holder_width, holder_height` jsou rozměry držáku
  - `holder_thickness` je tloušťka spodní části držáku na kterém bude ležet tablet
  - `rounded_corner_radius` poloměr vnějšího zakřivení rohů, pokud je argument nulový tak rohy nejsou zaoblené
  - `connecting_screw_diameter` průměr šroubu, které spojují horní a spodní část držáku
  - `nut_diameter` průměr šestihranné matky (průměr kružnice opsané)
  - `nut_offset` pozice matky (více info na obrázku)
  - `nut_depth` tloušťka matky, která bude zasazena do spodní části držáku
  - `screw_diameter` průměr šroubu, který se pasuje do představce řídítek
  - `screw_head_diameter` průměr hlavičky šroubu, který pasuje do představce řídítek
  - `screw_head_depth` výška hlavičky šroubu, který bude zapuštěn do spodní části modelu
  - `screw_spacing` je rozteč po ose Y (mezi středy), po směru X jsou vždy uprostřed objímky
  - `raiser_width` je šířka reálného představce řídítek
  - `raiser_height` je délka reálného představce řídítek 
  - `raiser_depth` je výška reálného představce řídítek
  - `raiser_inlet_wall_thickness` je tloušťka objímky představce řídítek
  - `raiser_inlet_top_thickness` je tloušťka horní části objímky představce
  - `bar_diameter` průměr řídítek
  - `bar_location` je délka změřena od spodní části představce k vrchní bodu řídítek
  - `raisers_spacing` je rozteč středu šroubu představců, nezáleží zda horních či spodních, vzdálenosti horních a spodních šroubů jsou vždy stejné
  - `holder_position_x` pozice držáku po ose x, pro kladné hodnoty bude držák posunut po kladném směru (doprava v defaultním OpenSCAD renderu), pro záporné opačným směrem
  - `holder_position_y` pozice držáku po ose y, pro kladné hodnoty bude držák posunut po kladném směru (doprava v defaultním OpenSCAD renderu), pro záporné opačným směrem
  - `holder_position_angle` je úhel držáku ve stupních na objímkách představců, mezní testované hodnoty jsou `-45` a `45`, pro kladný úhel bude držák natočen směrem k jezdci na motocyklu, tedy zápornému směru osy y (defaultní OpenSCAD render vypadá tak, že pokud tento argument bude kladný, tak se držák bude rotovat tak, aby bylo na display lépe vidět).

Výška objímky představce je měřena ve středu představce a je kombinací argumentů `raiser_depth` a `raiser_inlet_top_thickness`

Můžete předpokládat, že `raiser_width < raiser_height` a `screw_head_depth < holder_thickness`.

Nejprve aplikujte `holder_position_{x,y}` a potom `holder_position_angle` podle červeného bodu v obrázku níže.

## Představce

![představce (http://www.lewisportusa.com/Images/vmar/vmar_barmounts_a.jpg)](./assets/barmounts.jpg)
![řídítka (https://spieglerusa.com/media/catalog/product/cache/1/image/736x460/17f82f742ffe127f42dca9de82fb58b1/i/m/img_1937sm_2_4.jpg)](./assets/handlebars.jpg)

## Horní část držáku

![top_part](./assets/top_part_no_rouded_corners.png)
![top_part_bottom_view](./assets/top_part_no_rouded_corners_bottom_view.png)
![top_view_side_view](./assets/top_part_side_view.png)
![top_rouded](./assets/top_rouded.png)

## Spodní část držáku tabletu

![tablet_bottom](./assets/tablet_bottom_2.png)

Zespodu:

![tablet_bottom](./assets/tablet_bottom.png)

## Kombinace horního a spodního držáku tabletu

![tablet_holder](./assets/tablet_holder.png)

## Objímka představce řídítek

![riser_holder](./assets/riser1.png)
![riser_holder](./assets/riser2.png)
![riser_holder](./assets/riser3.png)

## Kombinace objímek řídítek

![riser_holder](./assets/2risers_top_view.png)

## Kombinace objímky a spodního držáku tabletu

![riser_bottom](./assets/riser_bottom.png)
![riser_bottom](./assets/riser_bottom2.png)
![riser_bottom_side_view](./assets/riser_bottom_side_view.png)

## Celý držák

![](./assets/holder_complete.png)
![](./assets/holder_complete2.png)

## Pozice matic / spojovacích šroubů

Je dána hodnotou `connecting_screw_offset` respektive `nut_offset`, dle červené čáry na obrázku:

![](./assets/nut_distance.png)

Jde o vzdálenost osy šroubu od (někdy pomyslného) rohu obdélníku tvořícího podstavu držáku.

Otočení díry na matici je irelevantní (nezajímá nás).

## Možnost nastavení pozorovacího úhlu

![](./assets/holder_complete_rotated.png)
![](./assets/holder_complete_rotated2.png)
![](./assets/holder_cut_rotated.png)

Všimněte si, že hlavičky šroubů jsou zanořené podle neotočeného modelu (znázorněného zde pomocí OpenSCAD modifikátoru `%`). Je na uživateli modulu, aby v případě natočení zvolil vhodnou hloubku zanoření tak, aby šrouby nepřekážely. Nesnažte se šrouby zanořovat kvůli naklopení více (či snad méně). Také si všimněte, že `raiser_inlet_top_thickness` platí pouze na středu modelu podle osy y. Horní stěna objímky představce nerotuje společně s držákem. Pouze se vyplní vniklá mezera nahoře (a vyřízne vzniklá překážka dole).

## Pozice držáku vůči objímkám představců

### Posun po ose Y
![](./assets/holder_position_y_positive.png)

### Posun po ose X
![](./assets/holder_position_x_positive.png)


