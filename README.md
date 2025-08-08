# tech-coach-internal-test og tech-coach-public-test

Disse to repoene viser hvordan man automatisk kan publisere kode-releaser fra et internt repo
til et public repo.
Hver gang det opprettes en *tag* i det interne repoet, vil den koden som taggen referer til
bli kopiert til et tilhørende public repo, og det blir laget en *pull request* der.
Dette blir gjort av GitHub Action'en [push-to-public-repo.yml].


Etter at *pull requesten* blir *merget* sørger [autotag-public-repo.yml] for at det public repoet
automatisk blir tagget med den samme taggen som ble brukt i det interne repoet.

---

Se filen [SYNC_WORKFLOW_SETUP.md](SYNC_WORKFLOW_SETUP.md) for flere detaljer og instruksjoner.

[autotag-public-repo.yml]: https://github.com/statisticsnorway/tech-coach-internal-test/blob/main/.github/workflows/autotag-public-repo.yml
[push-to-public-repo.yml]: https://github.com/statisticsnorway/tech-coach-internal-test/blob/main/.github/workflows/push-to-public-repo.yml
