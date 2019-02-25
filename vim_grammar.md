Vim, Spell and Grammar checker
===============================

Vim is a very powerful tool for many tasks. One I use a lot is to write 
documents using LaTeX, and I make a lot of mistakes :disappointed_relieved:.

A couple of tool I found very useful for it are:
1. Vim spell checker
    This integrated tool for checks words spelling (no grammar at all).
    
    Basic usage (under ``command mode``):
    - Enable spell check on the document
    ```bash
    :set spell spelllang=<spell_lang>
    ```
    where <spell_labg> cold be any of:
        * ``en_us`` for USA
        * ``en_gb`` for Great Britain
        * ``es`` for Spain 
        * Many others, [see the docs](http://vimdoc.sourceforge.net/htmldoc/spell.html)
    - Move to the next misspelled word
    ```bash
    ]s
    ```
    - Display a list of suggestions for the bad word under/after the cursor
    ```bash
    z=
    ```
    - Set highlighting colors (on ``~/.vimrc`` file):
    ```bash
    "--- for errors related to local languaje differences (e.g. color vs colour) "
    highlight SpellLocal ctermfg=White term=Reverse ctermbg=Green
    "--- for errors realted to capitalization (e.g. . example --> . Example) "
    highlight SpellCap ctermfg=White term=Reverse ctermbg=Blue
    ```
2. Vim grammar checker
    This tool integrates [LanguageTool](https://www.languagetool.org/) into Vim.
    For installation instructions [see the docs](https://www.vim.org/scripts/script.php?script_id=3223).

    Basic usage (under ``command mode``):
    - Check grammar in current document
    ```bash
    :LanguageToolCheck
    ```
    - Move through errors
    ```bash
    #--- move to errors window
    Ctrl+W ↓
    #--- use arrw keys to move between errors and press <Enter> to jump to that error.
    ```
    - Clone errors window and remove highlighting of grammar mistakes
    ```bash
    :LanguageToolClear
    ```


