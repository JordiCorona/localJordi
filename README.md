s5530253@EXT-PW0E5KYJ-CC MINGW64 /c/ProyectosDigital/tools/merge master/ib-advisor-monitor-service (feature/IBPLATF-28863-despliegue-ist)
$ git status
On branch feature/IBPLATF-28863-despliegue-ist
Your branch is up to date with 'origin/feature/IBPLATF-28863-despliegue-ist'.

Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
        new file:   src/test/java/com/scotiabank/ib/advisor/monitor/service/AdvisorMonitorServiceApplicationTest.java
        new file:   src/test/java/com/scotiabank/ib/advisor/monitor/service/services/AdvisorServiceTest.java
        new file:   src/test/java/resources/application-test.yml
        new file:   src/test/java/resources/sdui/mx/collaboration-tools/personal-banking/affluent-config.json

Untracked files:
  (use "git add <file>..." to include in what will be committed)
        src/test/java/com/scotiabank/ib/advisor/monitor/service/controller/


s5530253@EXT-PW0E5KYJ-CC MINGW64 /c/ProyectosDigital/tools/merge master/ib-advisor-monitor-service (feature/IBPLATF-28863-despliegue-ist)
$ git add .

s5530253@EXT-PW0E5KYJ-CC MINGW64 /c/ProyectosDigital/tools/merge master/ib-advisor-monitor-service (feature/IBPLATF-28863-despliegue-ist)
$ git commit --amend -m "feat(IBPLATF-28863): test unit"
[feature/IBPLATF-28863-despliegue-ist e320d31] feat(IBPLATF-28863): test unit
 Date: Mon Sep 8 17:03:59 2025 -0600

s5530253@EXT-PW0E5KYJ-CC MINGW64 /c/ProyectosDigital/tools/merge master/ib-advisor-monitor-service (feature/IBPLATF-28863-despliegue-ist)
$ git push origin feature/IBPLATF-28863-despliegue-ist
To https://bitbucket.agile.bns/scm/ibsrvc/ib-advisor-monitor-service.git
 ! [rejected]        feature/IBPLATF-28863-despliegue-ist -> feature/IBPLATF-28863-despliegue-ist (non-fast-forward)
error: failed to push some refs to 'https://bitbucket.agile.bns/scm/ibsrvc/ib-advisor-monitor-service.git'
hint: Updates were rejected because the tip of your current branch is behind
hint: its remote counterpart. If you want to integrate the remote changes,
hint: use 'git pull' before pushing again.
hint: See the 'Note about fast-forwards' in 'git push --help' for details.

s5530253@EXT-PW0E5KYJ-CC MINGW64 /c/ProyectosDigital/tools/merge master/ib-advisor-monitor-service (feature/IBPLATF-28863-despliegue-ist)
$ git merge master
Already up to date.

s5530253@EXT-PW0E5KYJ-CC MINGW64 /c/ProyectosDigital/tools/merge master/ib-advisor-monitor-service (feature/IBPLATF-28863-despliegue-ist)
$ git push origin feature/IBPLATF-28863-despliegue-ist
To https://bitbucket.agile.bns/scm/ibsrvc/ib-advisor-monitor-service.git
 ! [rejected]        feature/IBPLATF-28863-despliegue-ist -> feature/IBPLATF-28863-despliegue-ist (non-fast-forward)
error: failed to push some refs to 'https://bitbucket.agile.bns/scm/ibsrvc/ib-advisor-monitor-service.git'
hint: Updates were rejected because the tip of your current branch is behind
hint: its remote counterpart. If you want to integrate the remote changes,
hint: use 'git pull' before pushing again.
hint: See the 'Note about fast-forwards' in 'git push --help' for details.
t
s5530253@EXT-PW0E5KYJ-CC MINGW64 /c/ProyectosDigital/tools/merge master/ib-advisor-monitor-service (feature/IBPLATF-28863-despliegue-ist)
$ git pull
CONFLICT (modify/delete): src/test/java/com/scotiabank/ib/advisor/monitor/service/controller/AdvisorMonitorServiceControllerTest.java deleted in 75ba61f0807d2b54882de827c6aca307025d90dd and modified in HEAD.  Version HEAD of src/test/java/com/scotiabank/ib/advisor/monitor/service/controller/AdvisorMonitorServiceControllerTest.java left in tree.
Automatic merge failed; fix conflicts and then commit the result.

s5530253@EXT-PW0E5KYJ-CC MINGW64 /c/ProyectosDigital/tools/merge master/ib-advisor-monitor-service (feature/IBPLATF-28863-despliegue-ist|MERGING)
$ git pull
error: Pulling is not possible because you have unmerged files.
hint: Fix them up in the work tree, and then use 'git add/rm <file>'
hint: as appropriate to mark resolution and make a commit.
fatal: Exiting because of an unresolved conflict.

s5530253@EXT-PW0E5KYJ-CC MINGW64 /c/ProyectosDigital/tools/merge master/ib-advisor-monitor-service (feature/IBPLATF-28863-despliegue-ist|MERGING)
$ git push origin feature/IBPLATF-28863-despliegue-ist
To https://bitbucket.agile.bns/scm/ibsrvc/ib-advisor-monitor-service.git
 ! [rejected]        feature/IBPLATF-28863-despliegue-ist -> feature/IBPLATF-28863-despliegue-ist (non-fast-forward)
error: failed to push some refs to 'https://bitbucket.agile.bns/scm/ibsrvc/ib-advisor-monitor-service.git'
hint: Updates were rejected because the tip of your current branch is behind
hint: its remote counterpart. If you want to integrate the remote changes,
hint: use 'git pull' before pushing again.
hint: See the 'Note about fast-forwards' in 'git push --help' for details.

s5530253@EXT-PW0E5KYJ-CC MINGW64 /c/ProyectosDigital/tools/merge master/ib-advisor-monitor-service (feature/IBPLATF-28863-despliegue-ist|MERGING)
$ git merge master
error: Merging is not possible because you have unmerged files.
hint: Fix them up in the work tree, and then use 'git add/rm <file>'
hint: as appropriate to mark resolution and make a commit.
fatal: Exiting because of an unresolved conflict.

s5530253@EXT-PW0E5KYJ-CC MINGW64 /c/ProyectosDigital/tools/merge master/ib-advisor-monitor-service (feature/IBPLATF-28863-despliegue-ist|MERGING)
$ git commit
U       src/test/java/com/scotiabank/ib/advisor/monitor/service/controller/AdvisorMonitorServiceControllerTest.java
error: Committing is not possible because you have unmerged files.
hint: Fix them up in the work tree, and then use 'git add/rm <file>'
hint: as appropriate to mark resolution and make a commit.
fatal: Exiting because of an unresolved conflict.

s5530253@EXT-PW0E5KYJ-CC MINGW64 /c/ProyectosDigital/tools/merge master/ib-advisor-monitor-service (feature/IBPLATF-28863-despliegue-ist|MERGING)
