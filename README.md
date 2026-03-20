
<img width="1024" height="1024" alt="JONUS GAME" src="https://github.com/user-attachments/assets/710ce410-92e8-44b1-be72-1810474249b2" />

Extractor de botas calientes
Vista previa del extractor warmboot
<img width="1280" height="720" alt="El Warmboot-Extractor" src="https://github.com/user-attachments/assets/f4c3e616-27c4-48df-95f6-ab2fa30796e3" />

Una herramienta de carga útil para Nintendo Switch para extraer el firmware de warmboot del Paquete1 y guardarlo en la tarjeta SD.

Solo consolas Mariko: Esta herramienta es exclusiva para consolas Mariko (V2, Lite, OLED). Las consolas Erista utilizan un binario warmboot integrado y no requieren extracción.

Resumen
Esta herramienta extrae el firmware warmboot del Package1 de tu Mariko Switch (almacenado en BOOT0) y lo guarda en la tarjeta SD como archivos de caché usados por Atmosphère. Las consolas Mariko (Switch V2, Lite y OLED) requieren archivos de firmware Warmboot separados para cada número de fusibles.

Cuándo usar esta herramienta
Esta herramienta es esencial cuando:

SysMMC/OFW actualizado: El firmware de tu sistema (SysMMC) o OFW se actualiza al último firmware, quemando los fusibles de la consola
Atmosphere Out Date: En ese momento, Atmosphere necesita una actualización del desarrollador pero aún no ha sido lanzado
Error de eMMC Warmboot: Tu eMMC se encuentra con el siguiente error: Failed to match warm boot with fuses!
Modo de suspensión roto: Continuar sin esta herramienta hace que el eMMC pierda la funcionalidad del modo de suspensión
Puente de la Brecha: Extrae warmboot de SysMMC/OFW y úsalo en eMMC para resolver el desajuste y sigue usando eMMC sin esperar una actualización de Atmosphere
Nota importante
¡Esta herramienta NO es necesaria una vez que salga el nuevo Atmosphère!

El momento de liberación de la atmosfera varía y no puede predecirse (sin fecha estimada)
New Atmosphère extrae y almacena en caché automáticamente warmboot usando la misma lógica que emplea esta herramienta
Utiliza esta herramienta solo como solución temporal mientras esperas la actualización oficial de Atmosphère
Una vez que actualices, el Atmosphère más reciente gestionará automáticamente la caché de warmboot
Cuándo usar esta herramienta:

Has actualizado sysNAND y necesitas el modo de suspensión en emuMMC inmediatamente
La actualización oficial de Atmosphère aún no se ha publicado
Quieres seguir usando emuMMC sin esperar
Cuándo NO usar esta herramienta:

La nueva versión de Atmosphère ya está disponible; simplemente actualiza Atmosphère en su lugar
Puedes esperar unos días para el lanzamiento oficial
No usas el modo de suspensión en emuMMC
Características
Detección de SoC: Detecta automáticamente el hardware Mariko (T210B01) o Erista (T210)
Extracción específica de Mariko: Extrae warmboot del Paquete1 en consolas Mariko
Erista Skip: Extracción de saltos en consolas Erista (usa warmboot integrado)
Package1 Parsing: Localiza y analiza automáticamente el contenedor PK11 desde BOOT0
Pantalla del conteo de fusibles: Muestra el recuento de fusibles quemados de los registros ODM6/ODM7
Cómo funciona
Extracción de Mariko Warmboot:
La herramienta extrae warmboot de Package1 y crea un archivo de caché utilizado por Atmosphère:

Inicializa hardware y visualización en modo horizontal (1280x720)
Detecta el tipo de SoC (Mariko T210B01 o Erista T210)
Muestra información del sistema (tipo de SoC y conteo de fusibles quemados)
Para las consolas Mariko:
Lee el paquete1 de BOOT0 a 0x100000 desplazado
Localiza y analiza el contenedor PK11 (desplazamiento 0x4000 o 0x7000)
Extrae el firmware warmboot (validado 0x800-0x1000 bytes)
Lee el recuento de fusibles quemados de los registros de fusibles ODM6/ODM7
Visualiza la versión de firmware objetivo a partir de los metadatos de Package1
Guardados en función del conteo de mechas quemadassd:/warmboot_mariko/wb_XX.bin
Para consolas Erista:
Muestra el mensaje indicando que no es necesaria la extracción
Extracción de saltos (Atmosphère utiliza warmboot embebido)
Muestra el estado del resultado con mensajes codificados por colores
Espera la entrada del usuario (Apagado, Vol- para Hécate, o captura de pantalla de tres dedos)
Formato del nombre de archivo: donde XX = conteo de fusibles quemados en hexágono minúsculowb_XX.bin

Ejemplo: 8 fusibles quemados → wb_08.bin
Ejemplo: 22 fusibles quemados → (0x16 = 22 decimales)wb_16.bin
Ejemplo: 31 fusibles quemados → wb_1f.bin
Nota: Esta herramienta utiliza fusibles quemados para nombrar (no se esperan fusibles). Esto asegura que Atmosphère encuentre el archivo de caché al buscar warmboot compatible en sistemas degradados.

Estructura de Package1
Package1 (BOOT0 @ 0x100000, 256KB):
├── Stage1 (Encrypted)
└── PK11 Container (@ 0x4000 or 0x7000)
    ├── Magic: "PK11"
    ├── Payload 0: NX Bootloader (sig: 0xD5034FDF)
    ├── Payload 1: Secure Monitor (sig: 0xE328F0C0 or 0xF0C0A7F0)
    └── Warmboot Firmware ← Extracted here
        ├── Size: 0x800-0x1000 bytes (2-4KB)
        ├── Metadata: "WBT0" magic
        └── ARM7TDMI code
Edificio
Requisitos previos
devkitARM instalado y configurado
DEVKITARM Conjunto de variables de entorno
Git (para clonar el repositorio)
Clonar y Construir
https://github.com/JonusR/El-Warmboot-EXTRACTOR/releases/tag/1.0.1

# Build
make
Esto será:

Compilar archivos fuente
Herramientas de construcción (compresor lz77, convertidor bin2c)
Crear carga útil comprimida
Generar binario de cargador
Salida de compilación
output/Warmboot_Extractor.bin       # Final payload for Hekate/injector 

El archivo es la carga útil que copias a tu tarjeta SD.output/Warmboot_Extractor.bin

Uso
Inicio rápido
Copiar la carga útil a la tarjeta SD:

sd:/bootloader/payloads/Warmboot_Extractor.bin
Lanza desde Hekate o desde tu inyector de carga útil preferido

Ver estado de extracción

Interactúa con la carga útil:

Botón de encendido: Apagar la consola
Vol- botón: Volver a Hécate
Toque con tres dedos: Toma captura de pantalla (guardada en tarjeta SD)
Revisa la tarjeta SD para los archivos de caché extraídos de warmboot:

Ubicación: sd:/warmboot_mariko/
Archivos: donde XX = recuento de fusibles en hexágono minúsculowb_XX.bin
Escenario detallado de uso
Ejemplo: Firmware 21.x.x → 22.0.0

Antes de la actualización:

sysNAND: 21.0.0 (21 fusibles quemados)
emuMMC: 19.0.0 (antigua Atmosphère 1.7.0)
El sueño funciona bien en emuMMC
Tras la actualización de sysNAND a la 22.0.0:

sysNAND: 22.0.0 (22 fusibles quemados)
emuMMC: Sigue siendo 19.0.0
Suspensión rota - sin caché warmboot para el recuento de fusibles 22
Solución:

Extrae warmboot inmediatamente:

Launch: Hekate → Payloads → warmboot_extractor.bin
Salida de la herramienta:

WARMBOOT EXTRACTOR

System Information:
    SoC Type: Mariko (T210B01)
    Burnt Fuses: 22 fuses

[*] Extracting warmboot firmware from Package1...
[*] Warmboot extracted successfully!

Warmboot Information:
    Size: 0xE80 (3712 bytes)
    Target Firmware: 0x1600 (2024/10/15)

[*] Saving warmboot to SD card...
    Filename: wb_16.bin
[*] Warmboot saved successfully!

Power: Turn Off | Vol-: Back to Hekate | 3-Finger: Screenshot
Boot emuMMC con Atmosphère antiguo:

Old Atmosphère busca caché de warmboot
Hallazgos (22 fusibles = 0x16 hexágonos)wb_16.bin
¡El modo suspensión funciona!
Por qué funciona esto
Lógica de caché de Warmboot en Atmosphère:

Cuando Atmosphère detecta una descoordinación en el número de fusibles (fusibles quemados > fusibles esperados), busca archivos warmboot en caché:

// If burnt fuses > expected, search cache for compatible version
if (burnt_fuses > expected_fuses) {
    for (u32 attempt = burnt_fuses; attempt <= 32; ++attempt) {
        UpdateWarmbootPath(attempt);  // Try wb_16.bin, wb_17.bin, etc.
        if (TryLoadCachedWarmboot(warmboot_path)) {
            break;  // Found compatible warmboot!
        }
    }
}
La idea clave: Atmosphère usará una warmboot almacenada en caché si:

Fusibles quemados (22) > fusibles esperados del firmware (19)
Existe un warmboot en caché para el recuento de fusibles quemados (no se esperan fusibles)
Esta herramienta pre-completa esa caché usando el conteo actual de fusibles quemados
La denominación mediante fusibles quemados garantiza que el archivo se encuentre durante el bucle de búsqueda de Atmosphère
Resolución de problemas
"No se ha conseguido extraer la bota caliente"

Causa: No se puede acceder a BOOT0 o se ha corrompido el paquete1
Solución: Asegúrate de que el interruptor esté encendido y no en estado de batería baja
Nota: La herramienta muestra el código de error y el mensaje de error detallado
"No se pudo guardar warmboot en SD"

Causa: tarjeta SD solo de lectura, completa o mal montada
Solución: Revisa la tarjeta SD, asegúrate de permisos de escritura y espacio suficiente
"Erista detectada - warmboot está incrustada en la atmósfera"

Causa: Funcionando en la consola Erista (T210)
Nota: Este es un comportamiento esperado: las consolas Erista no necesitan extracción
El sueño seguía roto tras la extracción

Causa: Incompatibilidad atmosférica más allá de warmboot
Solución: Esperar la actualización oficial de Atmosphère
La herramienta muestra "Desconocido (¿nuevo firmware de pantalla?)" para firmware

Causa: La versión del firmware no está en la base de datos
Nota: La extracción sigue funcionando: se utiliza el recuento de fusibles quemados para nombrar
Lo que esta herramienta NO soluciona
No evita las incompatibilidades:

Si el firmware nuevo cambia SMMU, la disposición de la memoria o el kernel ABI
El viejo Atmosphère puede seguir fallando al arrancar o bloquearse
La caché de warmboot solo soluciona el modo de suspensión
No extrae de emuMMC BOOT0:

La herramienta lee desde sysNAND BOOT0 (donde hay nuevo firmware)
Esto es intencionado: quieres warmboot desde un firmware NUEVO
El viejo BOOT0 de emuMMC tiene warmboot antiguo (que no funciona con el nuevo número de fusibles)
¿Soluciona:

Modo de suspensión en emuMMC con el antiguo Atmosphère tras la actualización de sysNAND
Elimina la espera para que los desarrolladores de Atmosphère solo tengan caché de warmboot
Proporciona una solución inmediata mientras se espera el lanzamiento oficial de Atmosphère
Información de visualización
La herramienta muestra información completa durante la extracción:

Información del sistema
Tipo de SoC: Mariko (T210B01) o Erista (T210)
Fusibles quemados: Conteo de fusibles de corriente leído de los registros ODM6/ODM7
Información sobre Warmboot (solo Mariko)
Tamaño: Tamaño binario en hexadecimal y decimal (por ejemplo, 0xE80 / 3712 bytes)
Firmware objetivo: código de versión y fecha de compilación del Paquete1 (por ejemplo, 0x1600 AAAA/MM/DD)
Nombre del archivo: Nombre de archivo de salida basado en fusibles quemados (por ejemplo, wb_16.bin)
Mensajes de estado
Los mensajes codificados por colores indican éxito (verde), errores (rojo) o advertencias (naranja)
Las actualizaciones de progreso muestran cada paso del proceso de extracción
Los códigos de error y las descripciones detalladas ayudan a diagnosticar problemas
Controles de usuario
Botón de encendido: Apaga inmediatamente la consola
Vol- Botón: Regresa a Hécate (mediante bootloader/update.bin o payload.bin)
Tacto con 3 dedos: Captura captura de pantalla en la tarjeta SD (formato BMP)
Detalles técnicos
Restricciones de tamaño de las botas calientes
Mínimo: 0x800 (2048 bytes)
Máximo: 0x1000 (4096 bytes)
Ubicación física: IRAM @ 0x4003E000
Ejecución: ARM7TDMI en procesador BPMP
Disposición de la memoria
IRAM Physical:
├── 0x4003E000: Warmboot Binary (max 0x17F0 bytes)
├── 0x4003F000: Reboot Stub (0x1000 bytes)
└── 0x4003F800: Boot Config (0x400 bytes)
Conteo de fusibles vs firmware
Recuento de fusibles	Hex	Versiones de firmware
1	0x01	1.0.0
2	0x02	2.0.0 - 2.3.0
7	0x07	6.0.0 - 6.1.0
8	0x08	6.2.0
14	0x0E	11.0.0 - 12.0.1
21	0x15	20.0.0 - 20.5.0
22	0x16	21.0.0+
Cálculo del recuento de fusibles
La herramienta lee los registros y cuenta los fusibles quemados:FUSE_RESERVED_ODM6/ODM7

u8 get_burnt_fuses(void) {
    u8 fuse_count = 0;
    u32 fuse_odm6 = fuse_read_odm(6);
    u32 fuse_odm7 = fuse_read_odm(7);

    // Count bits in ODM6
    for (u32 i = 0; i < 32; i++) {
        if ((fuse_odm6 >> i) & 1)
            fuse_count++;
    }

    // Count bits in ODM7
    for (u32 i = 0; i < 32; i++) {
        if ((fuse_odm7 >> i) & 1)
            fuse_count++;
    }

    return fuse_count;
}
Ubicaciones de Package1 en BOOT0
BOOT0 Partition:
├── 0x000000: BCT (Boot Config Table)
├── 0x100000: Package1 (256KB)
│   ├── 0x000000: Stage1 (encrypted)
│   ├── 0x004000: PK11 (pre-6.2.0) ← Warmboot here
│   └── 0x007000: PK11 (6.2.0+)    ← Or here
└── 0x140000: Rest of BOOT0
Mecanismo de Retorno a Hécate
Cuando se pulsa Vol-, la herramienta intenta lanzar las cargas útiles en este orden:

sd:/bootloader/update.bin (Ubicación estándar de Hécate)
sd:/payload.bin (carga útil genérica de respaldo)
Si no se encuentra ninguna carga útil, realiza un reinicio (POWER_OFF_REBOOT)
Esto asegura que la herramienta regrese con elegancia al entorno del bootloader.

Basado en
Atmosphère: fusee_setup_horizon.cpp (lógica de extracción de warmboot y mecanismo de caché)
FuseCheck: Framework de pantalla, diseño de interfaz e integración BDK
Hekate: BDK (Board Development Kit) para la inicialización de hardware
Licencia
Este programa es software libre; puedes redistribuirlo y/o modificarlo bajo los términos y condiciones de la Licencia Pública General GNU, versión 2, publicada por la Free Software Foundation.

Créditos
Equipo de SciresM y Atmosphère-NX: Implementación de la extracción de botas calientes
CTCaer: Hekate BDK e inicialización de hardware
shchmue: marco base FuseCheck
Colaboradores: Pruebas y documentación
Aviso legal
Esta herramienta es únicamente para fines educativos y de respaldo. El autor no se hace responsable de ningún daño causado por el uso de este software.
