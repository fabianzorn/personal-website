---
weight: 4
title: "Git"
date: 2023-07-24T21:55:17+02:00
draft: false
description: "Git"
images: []
resources:
  - name: "featured-image"
    src: "featured-image.png"

tags: ["Git"]
categories: ["Git"]

lightgallery: true
---
Dies ist eine Übersicht von einigen nützlichen Git Befehlen.
<!--more-->

## Git einrichten

<strong>Benutzername</strong>
```
git config --global user.name username
```
<br>

<strong>Email</strong>
```
git config --global user.email email
```
<br>

<strong>Vim Editor</strong>
```
git config --global core.editor vim
```
<br>

<strong>Notepad Editor</strong>
```
git config --global core.editor notepad
```
<br>

<strong>Atom Editor</strong>
```
git config --global core.editor "atom --wait"
```
<br>

<strong>Aktivierung der Erkennung von Umbenennungen</strong>
```
git config diff.renames true
```
<br>

## Repository
<strong>Erstellung eines Repositories</strong>
```
git init
```
<br>

<strong>Schreiben in die Object Database</strong>
```
git hash-object -w hello.txt
```
<br>

<strong>Auslesen von Objektinhalt</strong>
```
git cat-file -p 56d90gfdz
```
<br>

<strong>Alle verschobenen Dateien</strong>
```
git log --summary -M90% | grep -e "^ rename"
```
<br>

<strong>Alle kopierten Dateien</strong>
```
git log --summary -C90% | grep -e "^ copy"
```
<br>

<strong>Geschichte von Codezeilen</strong>
```
git blame -M -C -C -C foo.txt
```
<br>

## Commit

<strong>Wähle für einen Commit spezifische Dateien aus</strong>
```
git add foo.txt bar.txt
```
<br>

<strong>Wähle für einen Commit ein Verzeichnis und alles darunter aus</strong>
```
git add directory/
```
<br>

<strong>Wähle für einen Commit das aktuelle Verzeichnis und alles darunter aus</strong>
```
git add .
```
<br>

<strong>Commit</strong>
```
git commit --message "Commit Message"
```
<br>

<strong>Commite alle veränderten Dateien</strong>
```
git commit --all
```
<br>

## Status

```
git status
```
<br>

## Diff

<strong>Workspace vs. Stage</strong>
```
git diff
```
<br>

<strong>Unterschied für Datei</strong>
```
git diff foo.txt
```
<br>

<strong>Stage vs. Repository</strong>
```
git diff --staged
```
<br>

<strong>Unterschied zwischen zwei Dateien</strong>
```
git diff 1c96fgt main
```
<br>

<strong>Unterschied zum Vorgänger</strong>
```
git diff p03sr4f^!
```
<br>

<strong>Anzahl an Änderungen</strong>
```
git diff --stat 1c96fgt 72ser18d
```
<br>

<strong>Änderungen an Fließtext</strong>
```
--word-diff
```
<br>

## Reset

<strong>Alles zurücksetzen</strong>
```
git reset HEAD .
```
<br>

<strong>Zurücksetzen von spezifischen Dateien/Verzeichnissen</strong>
```
git reset HEAD foo.txt src/test/
```
<br>

## Stashing
<strong>Stash geänderte Dateien</strong>
```
git stash --include-untracked
```
<br>

<strong>Erhalte die gestashten Änderungen</strong>
```
git stash pop
```
<br>

<strong>Erhalte ältere, gestashte Änderungen</strong>
```
git stash pop stash@{1}
```
<br>

<strong>Stash Liste</strong>
```
git stash list
```
<br>

## Show

<strong>Übersicht</strong>
```
git show 32rftsgz
```
<br>

<strong>Dateiinhalt</strong>
```
git show 32rftsgz:src/main.kt
```
<br>

<strong>Root Verzeichnis</strong>
```
git show 32rftsgz:
```
<br>

<strong>Verzeichnis "src"</strong>
```
git show 32rftsgz:src
```
<br>

<strong>Verzeichnis "src" mit Unterordnern</strong>
```
git ls-tree -r 32rftsgz -- src
```
<br>

## History

<strong>Logausgabe</strong>
```
git log
```
<br>

<strong>Letzte 3 commits</strong>
```
git log -n 3
```
<br>

<strong>Eine Zeile pro commit</strong>
```
git log --oneline
```
<br>

<strong>Statistiken</strong>
```
git log --stat
```
<br>

<strong>Kurzstatistik</strong>
```
git log --shortstat --oneline
```
<br>

<strong>Graf</strong>
```
git log --graph --oneline
```
<br>

## Branches

<strong>Liste an Zweigen</strong>
```
git branch
```
<br>

<strong>Erstelle Zweig von aktuellen commit</strong>
```
git branch new-branch
```
<br>

<strong>Erstelle Zweig von irgend einem commit</strong>
```
git branch new-branch 672dgzuj
```
<br>

<strong>Erstellen Zweig von existierenden Zweig</strong>
```
git branch new-branch existing-branch
```
<br>

<strong>Wechsel zu Zweig</strong>
```
git checkout main
```
<br>

<strong>Erstelle und wechsel zu Zweig</strong>
```
git checkout -b new-branch
```
<br>

<strong>Zurücksetzen zu älteren commit</strong>
```
git reset --hard rs29bnz1
```
<br>

<strong>Löschen von nicht aktiven Zweig</strong>
```
git branch -d branch-to-delete
```
<br>

<strong>Wiederherstellen von Zweig, wenn der commit hash bekannt ist</strong>
```
git branch deleted-branch ek183lp8 
```
<br>

<strong>Erhalte commit hashes</strong>
```
git reflog
```
<br>

## Merge

<strong>Merge feature Zweig in aktuellen Zweig</strong>
```
git merge feature
```
<br>

<strong>Starten von mergetool</strong>
```
git mergetool
```
<br>

<strong>Merge abbrechen</strong>
```
git merge --abort
```
<br>

## Arbeiten mit Repositories

<strong>Liste der Remote-Tracking-Zweige</strong>
```
git branch --list --remote --verbose
```
<br>

<strong>Fetch: Erhalte Zweige von anderen Repository</strong>
```
git fetch origin alpha master
```
<br>

<strong>Prüfe Integrität des Repositories</strong>
```
git fsck
