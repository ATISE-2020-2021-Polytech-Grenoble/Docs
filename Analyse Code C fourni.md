# Analyse Code C fourni

## hpyx_spi

Interface avec le périphérique HPYX, pas pertinant dans notre cas.

## atise_vdma

Permet d'initier la connection entre une caméra et la RAM et d'y associer des callbacks sur la lecture ou l'échec de la lecture. Même verdict que pour **hpyx_spi**.

## atise_img

Gère l'écriture de l'image sur la carte SD et la création de l'entête BMP. Il utilise **ff.h**  pour gérer le volume FAT, **FIL** est la représentation d'un fichier sur le volume, son utilisation est similaire à l'utilisation d'un **FILE** tel qu'utilisé avec **stdio.h**.

La structure **bmp_header** permet de gérer le format de sauvegarde bmp. Cette structure poura ou non être utile dépendant dépendant des données envoyées par UART, doit on envoyer un BMP ou les données telles que récupérées par VDMA?

C'est dans ce fichier que nous aurons le plus de modifications à faire:

- remplacer **save_image_grayscale(const char* filename, uint32_t width, uint32_t height, uint8_t* img)** par **send_image_grayscale(uint32_t width, uint32_t height, uint8_t* img)** pour remplacer l'écriture sur la carte par un envoie par UART.
- remplacer **save_image_rgb(const char* filename, uint32_t width, uint32_t height, uint8_t* img)** par **send_image_rgb(uint32_t width, uint32_t height, uint8_t* img)** pour remplacer l'écriture sur la carte par un envoie par UART.
- Intégrer le format CSP



## main

### Initialisations

Initialise la connexion à la carte SD, le driver GPIO, HPYX et VDMA.

Initialise les callbacks pour VDMA lignes 332,  348 et 364 pour les troiscaméras:

```C
//initialisation du callback
err = setup_vdma_callbacks(&vdma1, vdma_done1, vdma_err1);
//déclaration des callbacks mentionés
	//ligne 595
static void vdma_done0(void* callback_ref, u32 it_mask) {
	write_done0 = 1;
}
	//ligne 614
static void vdma_err0(void* callback_ref, u32 it_mask) {
	write_err0 = 1;
}
```

La partie inialisation ne sera sans doute pas à modifier si des pins de la carte sont bel et bien dédiés à UART.

Il est bon de noter que les références à **TPG **que l'on peut trouver dans le code font référence à **Xilinx Video Test Pattern Generator** qui semble être le moyen utilisé pour générer des fausses données en provenance des caméra.

### Boucle principale

Itérée 200 fois la boucle enregistre une photo pour chacune des trois caméras en RAM. Cette sauvegarde se fait entre les lignes 442 et 478:

```{c}
/////////////////////////////////////////////////////////////////////////////////////////////////////////////////
// SAVE #1 //////////////////////////////////////////////////////////////////////////////////////////////////////
/////////////////////////////////////////////////////////////////////////////////////////////////////////////////
snprintf(name, 20, "img%i-1.pgm", i);
err = save_image_grayscale(name, IMG_WIDTH, IMG_HEIGHT, (uint8_t*)(&img_buffer1[buffer_offset]));

if (err != XST_SUCCESS) {
	xil_printf("[ERROR] Image save failed : %i\r\n", err);
} else {
	xil_printf("[OK] Image saved successfully\r\n");
}
```

La fonction **save_image_grayscale** est déclarée dans atise_img.c demême que **save_image_rgb**. Ces deux fonctions seront à remplacer par notre fonction d'envoie via UART.

