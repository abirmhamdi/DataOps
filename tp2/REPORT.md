## TP2

## Step 1

**Q1.** Pendant que vous travailliez sur la branche, main contenait-elle la règle 6 ? À quelle commande est-elle apparue sur main ? Pourquoi est-ce utile si le rapport tourne chaque nuit depuis main ?
- No, `main` did not contain Rule 6 while working on the feature branch. It only appeared on `main` after running the merge command (`git merge --no-ff feature/country-rule`). This is useful because it keeps `main` stable and free of experimental or unfinished code while nightly reports run, ensuring unvalidated changes don't break production workflows.

**Q2.** Dans une vraie équipe, la fusion passerait par une pull request sur GitHub. Qu’apporterait une pull request qu’un git merge local n’apporte pas ?
- A pull request enables code review by team members, automated CI/CD checks (like running pytest automatically), comment threads for discussions, and traceability before code enters the main production branch.

## Step 2

**Q3.** Recopiez la ligne md5 de data/raw.dvc. Qu’identifie-t-elle, et que lui arriverait-il si vous changiez un seul caractère dans un des CSV ?
- Example md5 line: `md5: ab410f736844f0c850d241c141fc4a6e.dir`. It identifies the exact content and state of the raw data directory. If you change a single character in any CSV file, the MD5 hash changes entirely to reflect the new data version.

**Q4.** Les CSV sont toujours sur votre disque, mais Git ne les voit plus. Quel fichier fait que Git les ignore ?
- The `.gitignore` file (specifically inside the `data/` folder and root) instructs Git to ignore the actual raw data files, leaving them solely managed by DVC.

## Step 3

**Q5.** Les fichiers de dvc-storage ne s’appellent pas orders_day1.csv. Comment sont-ils nommés, et pourquoi est-is une bonne idée quand il existe plusieurs versions du même fichier ?
- They are named using their cryptographic hash/checksum values (organized into subfolders based on the first characters of the hash). This is a good idea because it prevents name collisions, enables content-addressable storage, and allows multiple distinct versions of files to coexist without overwriting each other.

## Step 4

**Q6.** git diff data/raw.dvc n’a montré que deux lignes modifiées, alors que des données ont été ajoutées. Pourquoi est-ce suffisant pour Git ? Pourquoi faut-il quand même commiter ?
- It is sufficient because Git only needs to track changes to the pointer file (`data/raw.dvc`), which stores the hash pointing to the dataset. Committing it to Git ensures that the code version and the exact expected data version pointer are locked together in version control history.

**Q7.** Entre data-v1 et data-v2, le code a-t-il changé ? Écrivez les deux totaux.
- No, the code did not change between data-v1 and data-v2. 
  - `data-v1` total: **7655.90**[cite: 1]
  - `data-v2` total: **8835.40**[cite: 1]

## Step 5

**Q8.** Avec vos mots : qu’a changé git checkout data-v1 -- data/raw.dvc, et qu’a changé dvc checkout ?
- `git checkout data-v1 -- data/raw.dvc` updated the DVC pointer file to point to version 1. `dvc checkout` read that pointer file and restored the physical CSV files corresponding to that version on your hard drive.

**Q9.** Que se passe-t-il si vous oubliez dvc checkout ? Pourquoi est-ce dangereux pour un rapport ?
- If you forget `dvc checkout`, your DVC pointer file changes, but your physical files on disk remain untouched (showing the newer data). This is dangerous because your pipeline would process the wrong data version while reporting a false historical total, destroying reproducibility.

## Step 6

**Q10.** Qu’est-ce que git clone a donné au collègue, et qu’est-ce que dvc pull lui a donné ?
- `git clone` gave the teammate the project source code, test suite, and the lightweight DVC pointer files (`.dvc`). `dvc pull` connected to the remote storage and downloaded the exact raw dataset matching those pointers.

**Q11.** Imaginez que vous ayez oublié dvc push à l’étape 4. Qu’obtiendrait le collègue avec dvc pull, et quel total ?
- If `dvc push` was forgotten, `dvc pull` would fail to find the new data version on the remote storage (or would pull the older version), and running the pipeline would fail or output the older total (`7655.90`) instead of the updated one.

## Git Graph & Tags

```text
*   f0dbe84 (HEAD -> master, tag: data-v2) Merge the country rule
|\  
| * d962ac5 (feature/country-rule) Reject orders from countries the shop does not sell to
|/  
* 6fd43cb (tag: data-v1) Track raw data with DVC
* aac08f1 Add the DVC remote
* 420d970 Add dvc to requirements.txt
* 9ccbe51 (origin/master) Add pipeline, data checks and unit tests
* cb40e53 Empty project with requirements