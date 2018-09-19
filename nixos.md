%title: NixOS - Eine Einführung
%author: noqqe
%date: 2018-08-20

Warning: There might be dragons.

-----

-> # Am Anfang <-

Am 8. Tage schuf Gott ein UNIX
Und er sah, dass es Crap war.

-----

-> # Convincing Operating Systems <-

Die 90er.
Hart.
Rau.
Dein OS wird *manuell* konfiguriert.

```
ssh user@host
vim /etc/postfix/main.cf
/etc/init.d/postfix restart
```

Du loggst dich ein, änderst Config.
Restartest deinen Dienst.

\*Würg\*

-----

-> # The Rise of Configuration Management <-

2010.

Der Durchschnittsadmin peitscht immer mehr Server in Clustern vor sich her.
Repitative Arbeit.

Login, Change, Repeat

Viele schreiben Shell Skripte in Not und Verzweiflung um nicht der Tippgicht
zu verfallen.

-----

-> # The Rise of Configuration Management <-

Neue Tools. Aus Shell Scripts haben sich schlaue Köpfe eigene
DSLs ausgedacht um Systeme beschreiben zu können.

* Puppet
* Chef
* Salt
* CFEngine
* Ansible

Jedes dieser Tools verfolgt ein anderes Ziel aber im Grunde alles
Konfigurations Management.

-----

-> # The Rise of Configuration Management <-

Sie definieren wie das System
aussehen soll und "Versprechen" die Umsetzung.

Use Cases:

* File erstellen
* User anlegen
* Dienst muss gestartet sein
* Exec eines bestimmten Kommands

-----

-> # CfgMgmt - Beispiel 1 <-

Zum Beispiel in Puppet einen neuen User anlegen:

```
user { 'bernd':
  ensure     => present,
  uid        => 1001,
  home       => /home/bernd,
}
```

-----

-> # CfgMgmt - Beispiel 2 <-

oder in Ansible ein File erstellen:

```
- file:
    path: /etc/foo.conf
    owner: foo
    group: foo
    mode: 0644
```

-----

-> # The Rise of Configuration Management <-

Step 1: Katalog aus Config-File(s) zusammen bauen
Step 2: Auf Zielhost Katalog (State) aufbauen
Step 3: Unterschiede identifizieren (Diff beider Kataloge)
Step 4: Korrekturen durchführen auf Zielhost


Paper zu [Promise Theory](http://markburgess.org/PromiseMethod.pdf)

-----

-> # NixOS Zitat aus dem IRC <-

> Setting up NixOS to me is like setting up Arch but you're guaranteed to only need to do it once
>
> jD91mZM2

-----

-> # NixOS <-

Was NixOS im Kern ausmacht ist, dass es Configmanagement und das OS
verheiratet.

Man muss es sich so vorstellen als wäre das Config Management das einzige
Interface wie man mit dem OS kommuniziert, anstatt ein Tool zu anzuweisen die
Arbeit für einen zu erledigen.


-----

-> # NixOS Configuration Management! <-

Gehen wir von einem Plain NixOS (frisch installiert) aus.

In /etc/nixos/configuration.nix ist der Einstiegspunkt

~~~ {.numberLines}
{ config, pkgs, ... }:

{
  services.openssh.enable = true;
  services.openssh.permitRootLogin = "no";

  # Use the GRUB 2 boot loader.
  boot.loader.grub {
    enable = true;
    version = 2;
    device = "/dev/sda";
  }
}
~~~

Und das würde theoretisch ausreichen um ein NixOS zu betreiben.

-----

-> # NixOS Configuration Management! <-

Um das System zu verändern

~~~
vim /etc/nixos/configuration.nix
nixos-rebuild switch
~~~

Demo Time!

-----

-> # User-local Installs! <-

-----

-> # Rollbacks! <-

-----

-> # Wie funktioniert das technisch alles?! <-

Permissions, Symlinks, cgroups

-----

-> # nix Package Manager <-


-----

-> # Nix Eco System <-

nix - Package Manager
nixos - Operating System
nixops - Service Orchestration Tool


-----

-> # Meta Slide <-

Danke!

Für diesen Talk habe ich [mpd]() benutzt und würde gerne erfahren wie ihr
diese Art der Darstellung fandet.

Außerdem lenkt das wunderbar von Inhaltlichen Fragen zum Vortrag ab.
