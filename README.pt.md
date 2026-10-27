# Escrever LaTeX localmente com Git: VS Code ou Antigravity IDE

**Português** · [English](README.md)

Esta é a configuração que uso para escrever LaTeX: um editor (**VS Code** ou **Antigravity IDE**), a extensão LaTeX Workshop e o **Git**. Assim posso abrir o meu trabalho em qualquer sítio e voltar a qualquer versão anterior. Este guia mostra-te como fazer o mesmo.

- Escreves LaTeX e vês o PDF ao lado do código.
- O teu trabalho fica no Git: qualquer computador o consegue obter e todas as versões ficam guardadas.
- Um [`template/`](template/) pronto a usar dá-te uma estrutura de projeto limpa: capa e resumo, capítulos, figuras, tabelas e bibliografia.

## O que precisas

- Um computador com Windows ou Ubuntu.
- Internet e cerca de 5 GB livres. A distribuição LaTeX é a parte mais pesada.
- Uma conta [GitHub](https://github.com) gratuita.

## Visão geral

Quatro peças trabalham em conjunto. Se souberes o que cada uma faz, os problemas ficam muito mais fáceis de resolver.

| Peça | O que faz |
| --- | --- |
| Editor (VS Code ou Antigravity) | Onde escreves. Não sabe LaTeX sozinho. |
| Distribuição LaTeX (TeX Live ou MiKTeX) | Os programas que transformam `.tex` em PDF. Instala-se à parte. |
| LaTeX Workshop (extensão) | Liga o editor à distribuição: compila ao guardar, mostra o PDF e salta entre código e PDF. |
| Git + GitHub | Guarda todas as versões e deixa-te aceder aos ficheiros em qualquer lado. |

## Passo 1: Escolher e instalar o editor

Escolhe um. O resto do guia é igual para os dois.

**Opção A: VS Code.** Descarrega-o em [code.visualstudio.com](https://code.visualstudio.com) e corre o instalador.

**Opção B: Antigravity IDE.** Descarrega-o no site oficial do Antigravity e corre o instalador. Funciona como o VS Code, por isso as extensões, os atalhos e o painel Source Control são iguais.

No Windows, mantém as opções por defeito. No Ubuntu, usa o pacote `.deb` se houver, ou segue os passos da página de download.

## Passo 2: Instalar uma distribuição LaTeX e as dependências

O projeto LaTeX Workshop recomenda o **TeX Live**, que funciona em Windows e Linux. O **MiKTeX** é uma opção mais leve no Windows, mas tens de instalar o Perl à parte (vê a nota abaixo).

### Windows

1. Vai a [tug.org/texlive/windows.html](https://tug.org/texlive/windows.html) e descarrega `install-tl-windows.exe`.
2. Corre-o e mantém as opções por defeito. Descarrega vários GB, por isso demora. Deixa acabar.
3. Fecha e volta a abrir o editor, para ele ver os programas novos.

> **Preferes MiKTeX?** Instala o MiKTeX e depois instala também o Perl (por exemplo [Strawberry Perl](https://strawberryperl.com)). Sem Perl, o `latexmk` falha e o LaTeX Workshop não consegue compilar.

### Ubuntu

Abre um terminal e corre:

```bash
sudo apt install texlive-full latexmk
```

### Verificar que funciona

Abre um terminal **novo**, para apanhar o `PATH` atualizado, e corre:

```bash
pdflatex --version
latexmk --version
```

Os dois devem mostrar um número de versão. Se aparecer "command not found", fecha o terminal e o editor, abre-os de novo e repete. No Windows, se continuar a falhar, reinicia o computador.

## Passo 3: Instalar a extensão LaTeX Workshop

1. Abre o painel de extensões (`Ctrl+Shift+X`).
2. Procura **LaTeX Workshop** (publicador: James Yu) e clica em **Install**.

![LaTeX Workshop no painel de extensões](images/03-latex-workshop.png.png)

## Passo 4: Compilar o primeiro PDF

1. Obtém o template (vê o Passo 6 para a forma com Git) ou, para já, cria uma pasta com um `main.tex` e abre-a com *File → Open Folder*.
2. Abre o `main.tex` e guarda (`Ctrl+S`). Aparece um ícone **TeX** na barra lateral esquerda.
3. Clica no ícone TeX e depois em **Build LaTeX project**.
4. Clica em **View LaTeX PDF → View in tab** para abrir o PDF ao lado do código.

O LaTeX Workshop volta a compilar o PDF sempre que guardas. Usa `Ctrl+Clique` no PDF para saltar para a linha correspondente no código.

Se a compilação falhar, abre o painel **Problems** e o output do **LaTeX Workshop** (View → Output) para leres o erro.

## Passo 5: Instalar e configurar o Git

**Instalar o Git**

- Windows: descarrega-o em [git-scm.com](https://git-scm.com) e mantém as opções por defeito.
- Ubuntu: `sudo apt install git`

**Diz ao Git quem és** (uma vez por computador):

```bash
git config --global user.name "O Teu Nome"
git config --global user.email "tu@exemplo.com"
```

Usa o mesmo email da tua conta GitHub.

## Passo 6: Começar o projeto a partir do template

1. No GitHub, cria um repositório novo. Dá-lhe um nome e escolhe **Private**, que recomendo para trabalhos de unidades curriculares e teses.
2. Copia o endereço dele (botão verde **Code**, separador HTTPS).
3. No editor, abre a Command Palette (`Ctrl+Shift+P`), corre **Git: Clone**, cola o endereço e escolhe uma pasta.
4. Descarrega este repositório (botão verde **Code**, depois **Download ZIP**) e descomprime-o, ou usa outro template que tenhas.
5. Copia **tudo o que está dentro da pasta `template/`** para a pasta que clonaste, de forma a que o `main.tex` fique na raiz do teu projeto.
6. Abre o `main.tex` e compila (Passo 4).

Ou clona num terminal:

```bash
git clone https://github.com/O-TEU-UTILIZADOR/O-TEU-REPOSITORIO.git
cd O-TEU-REPOSITORIO
```

### A estrutura do template

```
main.tex            o único ficheiro que compilas
config/             pacotes e definições
frontmatter/        capa, resumo
body/               um ficheiro por capítulo
backmatter/         referências e bibliografia
figures/            imagens
tables/             código das tabelas
examples/           excertos para reutilizar
.gitignore          deixa de fora do Git os ficheiros auxiliares do LaTeX
```

Muda o título e o autor em `config/settings.tex` e depois escreve os capítulos em `body/`.

## Passo 7: Guardar o trabalho com o Git

No editor, abre o **Source Control** (`Ctrl+Shift+G`):

1. Revê os ficheiros alterados.
2. Escreve uma mensagem curta a dizer o que fizeste, por exemplo "Add methods chapter".
3. Clica em **Commit** e depois em **Sync Changes** (ou **Push**).

Ou num terminal:

```bash
git add .
git commit -m "Add methods chapter"
git push
```

À primeira vez, o GitHub pede-te que inicies sessão. Usa o início de sessão pelo browser que o editor ou o Git Credential Manager oferece. Se um terminal pedir palavra-passe, o que precisa é de um [Personal Access Token](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/managing-your-personal-access-tokens), não da palavra-passe da conta.

### Abrir o projeto noutro computador

Repete os passos 1, 2, 3 e 5 (editor, LaTeX, extensão, Git) e depois faz `git clone` do teu repositório. Corre `git pull` para trazer as alterações mais recentes antes de começares.

## Passo 8: Voltar a uma versão anterior

Cada commit é uma versão. Para as ver:

```bash
git log --oneline
```

Cada linha começa por um código curto (o id do commit). Comandos úteis:

```bash
git diff                         # o que mudou desde o último commit
git diff <id> main.tex           # o que mudou num ficheiro desde uma versão antiga
git restore --source=<id> body/02-methods.tex   # recupera esse ficheiro como estava
```

O `git restore` altera o teu ficheiro de trabalho; faz commit se o quiseres guardar. Também podes ver todas as versões no GitHub, em **Commits**.

## Passo 9: Exportar o PDF final

O PDF que vês no editor já é o resultado. Antes de entregares, garante que é uma compilação limpa e completa.

1. **Faz uma compilação limpa.** Na barra lateral TeX, corre **Clean up auxiliary files** e depois **Build LaTeX project**. Ou num terminal, na pasta do `main.tex`:

   ```bash
   latexmk -C
   latexmk -pdf main.tex
   ```

   O `latexmk` repete a compilação as vezes que forem precisas, por isso as referências, o índice e as citações ficam todos resolvidos.

2. **Encontra o ficheiro.** Por defeito o PDF é o `main.pdf`, na mesma pasta do `main.tex`. No editor, clica com o botão direito em `main.pdf` na lista de ficheiros e escolhe **Reveal in File Explorer** (Windows) ou **Open Containing Folder** (Linux).

3. **Confirma antes de enviar.** Abre o PDF e procura `??` (uma referência ou citação partida), páginas vazias, figuras em falta, e se o título e o autor estão certos. Lê o output do LaTeX Workshop à procura de linhas `Warning` sobre referências indefinidas.

4. **Copia e muda o nome** para o que vais entregar, por exemplo `Nome_Relatorio_v1.pdf`.

### Guardar uma cópia da versão final no Git

O `.gitignore` do template deixa o `main.pdf` de fora do Git, porque o PDF pode sempre ser reconstruído a partir do código. Se quiseres guardar o PDF exato que entregaste, põe a cópia com o novo nome numa pasta à parte e faz commit dela:

```bash
mkdir -p submitted
cp main.pdf submitted/Nome_Relatorio_v1.pdf
git add submitted/Nome_Relatorio_v1.pdf
git commit -m "Add submitted PDF"
git push
```

Um bom hábito é marcar também esse momento com uma tag, para poderes voltar ao código exato do que entregaste:

```bash
git tag v1-submitted
git push --tags
```

## Problemas comuns

| Sintoma | Causa provável | Solução |
| --- | --- | --- |
| `latexmk` ou `pdflatex` não encontrado | Não instalado, ou `PATH` não atualizado | Revê o Passo 2. Reinicia o editor e, no Windows, o computador. |
| Erro de Perl no Windows | MiKTeX sem Perl | Instala o Strawberry Perl, ou muda para o TeX Live. |
| `File 'xxx.sty' not found` | Falta um pacote LaTeX | Ubuntu: instala `texlive-latex-extra` ou `texlive-full`. MiKTeX: permite instalações automáticas. |
| O PDF não atualiza | A compilação falhou | Lê o painel Problems e o output do LaTeX Workshop. |
| Citações aparecem como `[?]` | A bibliografia ainda não foi compilada | Compila duas vezes, ou deixa o `latexmk` tratar disso (faz-o por defeito). |
| A extensão não instala no Antigravity | Limite de pedidos do Open VSX | Espera e tenta de novo, ou instala o `.vsix` à mão (Passo 3). |
| O `git push` rejeita a palavra-passe | O GitHub não aceita palavras-passe por HTTPS | Usa o início de sessão pelo browser ou um Personal Access Token. |
| Conflito depois de `git pull` | Editaste o mesmo ficheiro em dois sítios | Abre o ficheiro, mantém as partes que queres, remove as marcas `<<<<<<<` e faz `git add` e `git commit`. |

## Contribuir

Encontraste um erro, ou um passo que não funcionou no teu sistema? Abre um **Issue** ou um **Pull request**. Correções e notas para outros sistemas são bem-vindas.

## Licença

O texto do guia está sob a licença [CC BY 4.0](LICENSE). Os ficheiros em `template/` estão sob a [Licença MIT](LICENSE-CODE).
