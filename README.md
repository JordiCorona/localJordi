$ git push origin feature/base-project
Enumerating objects: 35, done.
Counting objects: 100% (35/35), done.
Delta compression using up to 14 threads
Compressing objects: 100% (13/13), done.
Writing objects: 100% (21/21), 2.99 KiB | 339.00 KiB/s, done.
Total 21 (delta 5), reused 0 (delta 0), pack-reused 0 (from 0)
remote: Rejected by YACC
remote:
remote: ---------------------------------------------------------------------------
remote: |                                                                         |
remote: |  ERROR: Commit rejected. Please check YACC requirements and try again.  |
remote: |  (this error message can be customized in settings)                     |
remote: |                                                                         |
remote: ---------------------------------------------------------------------------
remote:
remote:
remote: Push rejected.
remote:
remote: refs/heads/feature/base-project: 838dc2a7ea8: commit message doesn't match regex: (?s)^(?:feat|fix|chore|docs|styles|test|refactor)\(([A-Z][A-Z_0-9]+-[0-9]+)\): .*$
remote:         config project
remote:
remote:
To https://bitbucket.agile.bns/scm/ibsrvc/ib-advisor-monitor-service.git
 ! [remote rejected] feature/base-project -> feature/base-project (pre-receive hook declined)
error: failed to push some refs to 'https://bitbucket.agile.bns/scm/ibsrvc/ib-advisor-monitor-service.git'

