# Contributing to Rheinwerk OSP


## 1. Fork von Rheinwerk erzeugen — Public

### Fork erzeugen und lokal klonen
`-p` für "use git / ssh protocol"

`[lukas@muffin]  ~/Documents/Work/CD/src  ➜  hub clone -p centerdevice/osp`

## 2. Mirror Repo für Cluster B Konfiguration erstellen

- hub create -p für private Repo
- Username und Password für OAuth Token, um GitHub API benutzen zu können
- Macht Pushes unmöglich: set-url upstream DISABLE

```
[lukas@muffin]  ~/Documents/Work/CD/src/infrastructure.next_gen  ➜  git init infra-osp-cluster-b
Initialized empty Git repository in /Users/lukas/Documents/Work/CD/src/infrastructure.next_gen/infra-osp-cluster-b/.git/
[lukas@muffin]  ~/Documents/Work/CD/src/infrastructure.next_gen/infra-osp-cluster-b git:(master) ➜  hub create -p centerdevice/infra-osp-cluster-b
github.com username: lukaspustina
github.com password for lukaspustina (never stored):
Updating origin
created repository: CenterDevice/infra-osp-cluster-b

[lukas@muffin]  ~/Documents/Work/CD/src/infrastructure.next_gen/infra-osp-cluster-b git:(master) ➜  git remote add cd ../../osp
[lukas@muffin]  ~/Documents/Work/CD/src/infrastructure.next_gen/infra-osp-cluster-b git:(master) ➜  git remote set-url --push cd DISABLE
```

## 3. Sync mit Upstream/CD — also CenterDevice/ops

```
[lukas@muffin]  ~/Documents/Work/CD/src/infrastructure.next_gen/infra-osp-cluster-b git:(master) ➜  git pull cd master
```

## 4. Workflow

### Start
```
cd/osp: git pull --rebase
cd/cluster-b: git pull --rebase
rheinwerk/osp: git pull --rebase
```

### Arbeiten
```
cd/osp: git checkout -b my_feature
-> work, work, work
cd/osp: commit, commit, commit

cd/cluster-b: git checkout -b my_feature
cd/cluster-b: git pull --rebase cd my_feature
-> work, work, work
cd/cluster-b: commit, commit, commit
```

### Wiederhole 'Arbeiten'
...

### Testen, Deployen
...

### Push
```
cd/osp: git rebase -i #compact commits
cd/osp: git checkout master
cd/osp: git merge my_feature # Should ff

cd/cluster-b: git checkout master
cd/cluster-b: git pull --rebase cd master
cd/cluster-b: git checkout my_feature
cd/cluster-b: git rebase master
cd/cluster-b: git rebase -i #compact commits
cd/cluster-b: git checkout master
cd/cluster-b: git merge my_feature # Should ff

cd/osp: git pull --rebase origin master
cd/osp: git push origin master
cd/cluster-b: git pull --rebase origin master
cd/cluster-b: git push origin master
```

### PR für Upstream
```
cd/osp: hub pull-reqeust -b rheinwerk/osp:master
```
