
plugins {
    id 'net.fabricmc.fabric-loom' version '1.15.2'
}

version = project.mod_version
group = project.maven_group

base {
    archivesName = project.archives_base_name
}

repositories {
}

dependencies {
    // 26.1+ jest bez obfuskacji: brak mappings, brak modImplementation
    minecraft "com.mojang:minecraft:${project.minecraft_version}"
    implementation "net.fabricmc:fabric-loader:${project.loader_version}"
    implementation "net.fabricmc.fabric-api:fabric-api:${project.fabric_version}"
}

processResources {
    inputs.property "version", project.version
    filesMatching("fabric.mod.json") {
        expand "version": project.version
    }
}

tasks.withType(JavaCompile).configureEach {
    it.options.release = 25
}

java {
    sourceCompatibility = JavaVersion.VERSION_25
    targetCompatibility = JavaVersion.VERSION_25
}

org.gradle.jvmargs=-Xmx2G
org.gradle.parallel=true

minecraft_version=26.1.2
loader_version=0.19.2
fabric_version=0.145.4+26.1.2

mod_version=1.0.0
maven_group=pl.areadig
archives_base_name=areadig

# Area Dig (Fabric 26.1.2)

Zaznacz teren drewniana motyka, a mod wykopie go pietro po pietrze.

## Sterowanie
- PPM drewniana motyka na blok - rog 1
- PPM drewniana motyka na drugi blok - rog 2 (zielona ramka z czasteczek)
- Klawisz `-` - start kopania (od gory do dolu, pietro po pietrze); zmienisz w Ustawienia > Sterowanie
- Kucniecie + PPM motyka - anuluj zaznaczenie / przerwij kopanie

W ekwipunku musi byc kilof. Dla kazdego bloku mod wybiera najlepsze narzedzie
(kilof/lopata/siekiera) z ekwipunku, zuzywa jego wytrzymalosc, a dropy wkladaja sie do ekwipunku
(nadmiar wypada na ziemie).

## Ustawienia (stale na gorze AreaDigMod.java)
- BLOCKS_PER_TICK - tempo kopania
- LAYER_PAUSE_TICKS - przerwa miedzy pietrami

## Budowanie
Wymaga JDK 25 i Gradle 9.4+ (np. 9.5.1) (albo skopiuj `src/` do oficjalnego szablonu z https://fabricmc.net/develop/template/):

    gradle wrapper --gradle-version 9.5.1   # jednorazowo, generuje gradlew
    ./gradlew build

Gotowy mod: `build/libs/areadig-1.0.0.jar` -> do folderu `mods` razem z Fabric API.

## Uwagi do 26.1.2
- 26.1+ jest bez obfuskacji, wiec uzywamy nazw Mojang (Yarn nie istnieje dla tej wersji).
- Plugin Loom to `net.fabricmc.fabric-loom` (bez remapowania), zaleznosci przez `implementation`.
- Wymagana Java 25 (zarowno do budowania, jak i do uruchomienia).
- Jar dziala na 26.1, 26.1.1 i 26.1.2.

pluginManagement {
    repositories {
        maven { url = 'https://maven.fabricmc.net/' }
        mavenCentral()
        gradlePluginPortal()
    }
}
