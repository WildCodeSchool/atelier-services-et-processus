# Gestion des services et des processus

## La gestion en PowerShell

#### Introduction

Sur les OS Windows, on utilise couramment le mode graphique. Néanmoins, le passage en mode console est un moyen d'optimiser son temps.

#### 1. Prérequis

* Une VM ou un hôte Windows (à partir de Windows 7 ou à partir de Windows Server 2012). Cet atelier a été fait sous Windows 11.
* La console shell de PowerShell ou bien l’éditeur PowerShell ISE ou bien encore un éditeur comme VSCode.
* ***Attention*** : Si vous ne maitrisez pas votre OS, les modifications de services peuvent amener des dysfonctionnements sur votre ordinateur. Pour plus de sécurité, vous pouvez utiliser une VM.

#### 2. Gestion des services

Les services sont des programmes qui, en général, s'exécutent au démarrage du système d'exploitation, et qui tournent en arrière-plan. C'est-à-dire qu'ils fonctionnent sans que l'on ait nécessairement une fenêtre ou une notification à l'écran.

Pour la suite, tu vas devoir ouvrir une console PowerShell en mode administrateur.
Recherche **powershell** dans le champ de recherche (Cortana) et clic sur **Exécuter en tant qu'administrateur"

**Affichage des services**
La commande ci-dessous affiche tous les services.

```powershell
PS C:\Users\wilder> Get-Service
```

Ici, nous voyons donc trois colonnes qui représentent 3 attributs :

* Status
* Name
* DisplayName

Cela correspond à ce que nous voyons dans la fenêtre **Services** en mode graphique.
Pour le voir, chercher **services** dans la recherche Windows (Cortana).

Pour n’afficher que les colonnes **Nom** et **Status**, par exemple :

```powershell
PS C:\Users\wilder> Get-Service | Select-Object -Property Status,Name
```

Pour trier par l'attribut **status** :

```powershell
PS C:\Users\wilder> Get-Service | Sort-Object -Property Status
```

Pour lister tous les services en cours d’exécution :

```powershell
PS C:\Users\wilder> Get-Service | Where-Object {$_.Status -eq « Running »}
```

Pour lister un service en particulier, par exemple **WalletService** :

```powershell
PS C:\Users\wilder> Get-Service | Where-Object {$_.DisplayName -like "*WalletService*"}
```

**Démarrage des services**
Démarrage d’un service, par exemple **WalletService** :

```powershell
PS C:\Users\wilder> Start-Service -Name WalletService
```

Ou

```powershell
PS C:\Users\wilder> Get-Service | Where-Object {$_.Name -eq "WalletService"} | Start-Service
```

Comprends-tu ces 2 lignes de codes différentes ?

* Dans la première, on va démarrer directement le service concerné, ici **WalletService**
* Dans la seconde, on va chercher tous les services, avec `Get-Service`, on va envoyer le résultat dans un pipe `|` et avec `Where-Object` on va rechercher le service, donc **WalletService**. Ensuite on va le démarrer.

**Arrêt des services**

Arrêt d’un service, par exemple **WalletService** :

```powershell
PS C:\Users\wilder> Stop-Service -Name WalletService
```

Ou

```powershell
PS C:\Users\wilder> Get-Service | Where-Object {$_.Name -eq "WalletService"} | Stop-Service
```

Ici tu reconnais 2 lignes presque similaires à celles du démarrage d'un service.

**Changement du mode de démarrage (en console administrateur)**
Un service a 3 démarrages possible :

* **Automatique** : le service est lancé à chaque démarrage de Windows
* **Manuel** : le service n'est lancé qu'en cas de besoin
* **Désactivé** : le service ne sera jamais utilisé

Pour changer le mode de démarrage du service **WalletService** à **automatique** :

```powershell
PS C:\Users\wilder> Set-Service -Name WalletService -StartupType Automatic
```

**Changement d’état d’un service**
Un service a 3 états possible :

* **Running** : en cours d'exécution
* **Stopped** : en arrêt
* **Paused** : en attente

Pour changer l’état du service **WalletService** à **stopped** par exemple :

```powershell
PS C:\Users\wilder> Set-Service -Name WalletService -Status Stopped
```

#### 3. Gestion des processus

Les processus sont des programmes en cours d'exécution.
**Démarrer un processus**
Exécuter le code ci-dessous :

```powershell
PS C:\Users\wilder> Start-Process -FilePath calc.exe
```

En faisant cela, on démarre le processus **calc.exe** qui est associé à l’application calculatrice.

De la même façon qu'il existe 2 manières de lancer un service, il y a 2 manières de démarrer un processus. Essaye les commandes `Get-Process` suivi d'un pipe `|` , puis de `Where-Object`. Enfin termine la ligne de code par un `Start-Process`.
Essaye avec le processus **calc.exe**

**Affichage d'un processus**
Pour afficher le processus correspondant :

```powershell
PS C:\Users\wilder> Get-Process -Name CalculatorApp
```

> **CalculatorApp** peut aussi apparaitre sous le terme **Calculator** sur certaines version de windows (Windows 10 par ex)

En ajoutant le paramètre **FileVersionInfo** et en formattant la sortie standard en affichage **Format-List**, on obtient plus d’information :

```powershell
PS C:\Users\wilder> Get-Process -Name CalculatorApp -FileVersionInfo | Format-List
```

**Arrêt d'un processus**
Pour arrêter un processus :

```powershell
PS C:\Users\wilder> Stop-Process -Name CalculatorApp
```

Comme pour le lancement d'un processus, tu peux exécuter la même série d'instructions, mais avec `Stop-Process` derrière un pipe `|`.