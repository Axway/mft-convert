---

    title: create
    linkTitle: create
    weight: 560

---
<span id="create"></span>

### create

#### COPYFILE

\[CREATE = {<u>‘ ’</u> | YES | NO},\]

Options when creating an output file:

- ‘ ’ to create the
    file if it does not exist or substitute the file data if the file already
    exists
- YES to create the
    file; if the file already exists, execution is aborted
- NO to work on an
    existing file; the data received is substituted for the existing data.
    If the file does not exist, execution is aborted
