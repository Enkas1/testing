# Variables
 
## 1. `toolchain/BUILD`
 
Denna fil beskriver hur Bazel ska bygga C++-program. Här är de viktigaste delarna:
 
- `package(default_visibility = ["//visibility:public"])`: Gör så att verktygskedjan kan användas av andra delar av projektet.
 
- `filegroup(name = "empty")`: Skapar en "tom" grupp av filer. Här används den för att inte behöva specificera riktiga filer just nu.
 
- `cc_toolchain_config(name = "linux_x86_64_toolchain_config")`: Definierar inställningar för C++-verktygskedjan. Denna del säger hur verktyg som kompilatorer och länkar ska fungera.
 
- `cc_toolchain(...)`: Denna del beskriver själva verktygskedjan:

  - **`toolchain_identifier`**: Ger verktygskedjan ett unikt namn så att den kan kännas igen.

  - **`toolchain_config`**: Länkar till inställningarna som definierades ovan.

  - **`all_files`, `compiler_files`, osv.**: Dessa fält handlar om filer som behövs för att bygga programmet; i det här fallet används de tomma filgrupperna.
 
- `toolchain(...)`: Registrerar verktygskedjan så att den kan användas i byggprocessen. Den specificerar vilka system (CPU och operativsystem) den fungerar med.
 
## 2. `cc_toolchain_config.bzl`
 
Denna fil ger detaljer om hur verktyg och inställningar används i verktygskedjan:
 
- **Importera funktioner**: Här laddas verktyg som hjälper till att definiera hur bygget ska göras.
 
- **`all_link_actions`**: Lista över saker som görs när man länkar program (t.ex. skapa körbara filer).
 
- **`def _impl(ctx)`**: Denna funktion definierar hur verktygskedjan ska fungera.
 
- **`tool_paths`**: Här anger vi var verktygen finns, som `gcc` (kompilator), `afl-gcc` (en speciell kompilator för fuzzing), och `ld` (länkare). Vissa verktyg sätts till `/bin/false` för att de inte används.
 
- **`features`**: Här kan vi definiera inställningar som används vid länkning. Till exempel, `-lstdc++` används för att länka mot C++-biblioteket.
 
- **`cc_toolchain_config = rule(...)`**: Denna rad skapar en regel för verktygskedjan med alla inställningar vi just definierade.
 
## 3. `MODULE.bazel`
 
Denna fil kopplar samman projektet med andra beroenden:
 
- **`bazel_dep(name = "platforms", version = "0.0.10")`**: Anger att projektet behöver ett annat paket som heter `platforms`.
 
- **`register_toolchains(...)`**: Registrerar verktygskedjan så att Bazel kan använda den för byggprocessen.
 
---
 
## Sammanfattning
 
- **`toolchain/BUILD`**: Beskriver hur verktygskedjan ser ut och vilka verktyg den använder.

- **`cc_toolchain_config.bzl`**: Definierar specifika inställningar och vägar till verktygen.

- **`MODULE.bazel`**: Registrerar beroenden och gör verktygskedjan tillgänglig för hela projektet.

 