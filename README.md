# Write LaTeX locally with Git: VS Code or Antigravity IDE

[Português](README.pt.md) · **English**

This is the setup I use to write LaTeX: an editor (**VS Code** or **Antigravity IDE**), the LaTeX Workshop extension, and **Git**. That way I can open my work from anywhere and go back to any earlier version. This guide shows you how to do the same.

- You write LaTeX and see the PDF next to your code.
- Your work lives in Git, so any computer can get it and every version is saved.
- A ready-to-use [`template/`](template/) gives you a clean project structure: front matter, chapters, figures, tables and bibliography.

## What you need

- A computer with Windows or Ubuntu. Other Linux distributions work too, only the package names change.
- Internet and about 5 GB of free space. The LaTeX distribution is the big one.
- A free [GitHub](https://github.com) account.

## The big picture

Four pieces work together. If you know which one does what, problems are much easier to fix.

| Piece | What it does |
| --- | --- |
| Editor (VS Code or Antigravity) | Where you type. It does not know LaTeX by itself. |
| LaTeX distribution (TeX Live or MiKTeX) | The programs that turn `.tex` into PDF. Installed separately. |
| LaTeX Workshop (extension) | Connects the editor to the distribution: builds on save, shows the PDF, jumps between code and PDF. |
| Git + GitHub | Saves every version and lets you reach your files anywhere. |

## Step 1: Choose and install your editor

Pick one. The rest of the guide is the same for both.

**Option A: VS Code.** Download it from [code.visualstudio.com](https://code.visualstudio.com) and run the installer.

**Option B: Antigravity IDE.** Download it from the official Antigravity website and run the installer. It works like VS Code, so extensions, shortcuts and the Source Control panel are the same.

On Windows, keep the default options. On Ubuntu, use the `.deb` package if there is one, or follow the steps on the download page.

![Editor installed and open](images/01-editor-installed.png)

## Step 2: Install a LaTeX distribution and its dependencies

The LaTeX Workshop project recommends **TeX Live**, and it works on Windows and Linux. **MiKTeX** is a lighter option on Windows, but you have to install Perl yourself (see the note below).

### Windows

1. Go to [tug.org/texlive/windows.html](https://tug.org/texlive/windows.html) and download `install-tl-windows.exe`.
2. Run it and keep the default options. It downloads several GB, so it takes a while. Let it finish.
3. Close and reopen your editor so it sees the new programs.

> **Using MiKTeX instead?** Install MiKTeX, then also install Perl (for example [Strawberry Perl](https://strawberryperl.com)). Without Perl, `latexmk` fails and LaTeX Workshop cannot build.

![TeX Live installer on Windows](images/02-texlive-windows.png)

### Ubuntu

Open a terminal and run:

```bash
sudo apt update
sudo apt install texlive-latex-extra texlive-fonts-recommended texlive-lang-european latexmk
```

This is enough for most reports and theses. If you don't want to worry about missing packages, install everything instead (several GB):

```bash
sudo apt install texlive-full latexmk
```

![Terminal on Ubuntu installing texlive](images/02-texlive-ubuntu.png)

### Check that it works

Open a **new** terminal, so it picks up the updated `PATH`, and run:

```bash
pdflatex --version
latexmk --version
```

Both should print a version number. If you see "command not found", close the terminal and the editor, open them again and retry. On Windows, if it still fails, restart the computer.

## Step 3: Install the LaTeX Workshop extension

1. Open the Extensions panel (`Ctrl+Shift+X`).
2. Search for **LaTeX Workshop** (publisher: James Yu) and click **Install**.

![LaTeX Workshop in the Extensions panel](images/03-latex-workshop.png)

**VS Code:** it installs straight from the Visual Studio Marketplace. Nothing else to do.

**Antigravity:** users report that it gets extensions from the [Open VSX](https://open-vsx.org) registry by default. If the install fails with a rate-limit or "unable to download" error, wait a minute and try again. Or download the `.vsix` file from the LaTeX Workshop page on Open VSX and use *Extensions → ⋯ → Install from VSIX*.

## Step 4: Build your first PDF

1. Get the template (see Step 6 for the Git way), or for now create a folder with a `main.tex` and open it with *File → Open Folder*.
2. Open `main.tex` and save it (`Ctrl+S`). A **TeX** icon appears in the left sidebar.
3. Click the TeX icon, then **Build LaTeX project**.
4. Click **View LaTeX PDF → View in tab** to open the PDF beside your code.

LaTeX Workshop rebuilds the PDF every time you save. Use `Ctrl+Click` on the PDF to jump to the matching line in the code.

![PDF preview next to the source](images/04-pdf-preview.png)

If the build fails, open the **Problems** panel and the **LaTeX Workshop** output (View → Output) to read the error. See [Troubleshooting](#troubleshooting).

## Step 5: Install and configure Git

**Install Git**

- Windows: download it from [git-scm.com](https://git-scm.com) and keep the default options.
- Ubuntu: `sudo apt install git`

**Tell Git who you are** (once per computer):

```bash
git config --global user.name "Your Name"
git config --global user.email "you@example.com"
```

Use the same email as your GitHub account.

![Git configured in the terminal](images/05-git-config.png)

## Step 6: Start your project from the template

1. On GitHub, create a new repository. Give it a name and choose **Private**, which I recommend for coursework and theses.
2. Copy its address (green **Code** button, HTTPS tab).
3. In your editor, open the Command Palette (`Ctrl+Shift+P`), run **Git: Clone**, paste the address and choose a folder.
4. Download this repository (green **Code** button, then **Download ZIP**) and unzip it.
5. Copy **everything inside the `template/` folder** into your cloned folder, so that `main.tex` sits at the root of your project.
6. Open `main.tex` and build it (Step 4).

Or clone in a terminal:

```bash
git clone https://github.com/YOUR-USER/YOUR-REPO.git
cd YOUR-REPO
```

![Cloning a repository in the editor](images/06-clone-repo.png)

### The template structure

```
main.tex            the only file you compile
config/             packages and settings
frontmatter/        title page, abstract
body/               one file per chapter
backmatter/         references and bibliography
figures/            images
tables/             table sources
examples/           snippets to reuse
.gitignore          keeps LaTeX auxiliary files and the PDF out of Git
```

Change the title and author in `config/settings.tex`, then write your chapters in `body/`.

## Step 7: Save your work with Git

In your editor, open **Source Control** (`Ctrl+Shift+G`):

1. Review the changed files.
2. Type a short message that says what you did, for example "Add methods chapter".
3. Click **Commit**, then **Sync Changes** (or **Push**).

Or in a terminal:

```bash
git add .
git commit -m "Add methods chapter"
git push
```

The first time, GitHub asks you to sign in. Use the browser sign-in that the editor or Git Credential Manager offers. If a terminal asks for a password, it needs a [Personal Access Token](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/managing-your-personal-access-tokens), not your account password.

![Source Control panel with a commit](images/07-source-control.png)

### Open the project on another computer

Repeat steps 1, 2, 3 and 5 (editor, LaTeX, extension, Git), then `git clone` your repository. Run `git pull` to get the latest changes before you start.

## Step 8: Go back to an earlier version

Every commit is a version. To see them:

```bash
git log --oneline
```

Each line starts with a short code (the commit id). Useful commands:

```bash
git diff                         # what changed since the last commit
git diff <id> main.tex           # what changed in a file since an old version
git restore --source=<id> body/02-methods.tex   # bring back that file as it was
```

`git restore` changes your working file; commit it if you want to keep it. You can also browse every version on GitHub under **Commits**.

![Commit history on GitHub](images/08-history.png)

## Step 9: Export the final PDF

The PDF you see in the editor is already the output. Before you hand it in, make sure it is a clean, complete build.

1. **Do a clean build.** In the TeX sidebar, run **Clean up auxiliary files**, then **Build LaTeX project**. Or in a terminal, from the folder with `main.tex`:

   ```bash
   latexmk -C
   latexmk -pdf main.tex
   ```

   `latexmk` repeats the build as many times as needed, so references, the table of contents and citations are all resolved.

2. **Find the file.** By default the PDF is `main.pdf`, in the same folder as `main.tex`. In the editor, right-click `main.pdf` in the file list and choose **Reveal in File Explorer** (Windows) or **Open Containing Folder** (Linux).

3. **Check it before you send it.** Open the PDF and look for `??` (a broken reference or citation), empty pages, missing figures, and the correct title and author. Read the LaTeX Workshop output for any `Warning` lines about undefined references.

4. **Copy it and rename it** to what you are submitting, for example `Name_Report_v1.pdf`.

![Reveal main.pdf in the file explorer](images/09-export-pdf.png)

### Keep a copy of the final version in Git

The template's `.gitignore` leaves `main.pdf` out of Git, because the PDF can always be rebuilt from the source. If you want to keep the exact PDF you submitted, put the renamed copy in a separate folder and commit that:

```bash
mkdir -p submitted
cp main.pdf submitted/Name_Report_v1.pdf
git add submitted/Name_Report_v1.pdf
git commit -m "Add submitted PDF"
git push
```

A good habit is to also mark that moment with a tag, so you can always return to the exact source of what you handed in:

```bash
git tag v1-submitted
git push --tags
```

## Troubleshooting

| Symptom | Likely cause | Fix |
| --- | --- | --- |
| `latexmk` or `pdflatex` not found | Not installed, or `PATH` not updated | Check Step 2. Restart the editor, then the computer on Windows. |
| Perl error on Windows | MiKTeX without Perl | Install Strawberry Perl, or switch to TeX Live. |
| `File 'xxx.sty' not found` | Missing LaTeX package | Ubuntu: install `texlive-latex-extra` or `texlive-full`. MiKTeX: allow on-the-fly installs. |
| The PDF does not update | Build failed | Read the Problems panel and the LaTeX Workshop output. |
| Citations show `[?]` | Bibliography not built yet | Build twice, or let `latexmk` do it (it does by default). |
| Extension will not install in Antigravity | Open VSX rate limit | Wait and retry, or install the `.vsix` manually (Step 3). |
| `git push` rejects your password | GitHub does not accept passwords over HTTPS | Use browser sign-in or a Personal Access Token. |
| Conflict after `git pull` | You edited the same file in two places | Open the file, keep the parts you want, remove the `<<<<<<<` markers, then `git add` and `git commit`. |

## Contributing

Found a mistake, or a step that didn't work on your system? Open an **Issue** or a **Pull request**. Corrections and notes for other systems are welcome.

## License

The guide text is licensed under [CC BY 4.0](LICENSE). The files in `template/` are licensed under the [MIT License](LICENSE-CODE).
