# vimdiff

```
vimdiff a.c b.c


[ c                 前の差分
] c                 次の差分

do                  他の窓の今の差分を、自分のほうへ取り込む(obtain)
dp                  自分の窓の今の差分を、他の窓へ押し込む(put)

ctrl+w w            窓を移動
ctrl+w ctrl+w       〃

ctrl+w h            左の窓のファイルへ移動
ctrl+w j            下の窓のファイルへ移動
ctrl+w k            上の窓のファイルへ移動
ctrl+w l            右の窓のファイルへ移動

ctrl+w H            ファイルの位置を左窓へ＋横分割に変更
ctrl+w J            ファイルの位置を下窓へ＋縦分割に変更
ctrl+w K            ファイルの位置を上窓へ＋縦分割に変更
ctrl+w L            ファイルの位置を右窓へ＋横分割に変更

ctrl+w [num] <      窓の仕切りを num 個左へ
ctrl+w [num] >      窓の仕切りを num 個右へ
ctrl+w [num] +      窓の仕切りを num 個上へ
ctrl+w [num] -      窓の仕切りを num 個下へ
ctrl+w =            窓の仕切りを戻す

:wqa                変更を全部ファイルに書き込んで修了
:qa!                変更を全部破棄して修了
```

# encode / fineformat

- change encode

  ```
  :setl fenc=<code>
  ```
  - fenc : fileencoding

- change return code

  ```
  :setl ff=<type>
  ```
  - ff : fileformat

- re-open with <code>

  ```
  :e ++enc=<code>
  ```

- re-open with return code

  ```
  :e ++ff=<type>
  ```

# sample of .vimrc of windows

```
syntax off
if &t_Co > 1
	syntax enable
endif
set t_Co=256
set ts=4

" set background=dark
" colorscheme molokai
" colorscheme moria
set hlsearch

set list
set listchars=tab:>-

set ic
set smartcase

set encoding=utf-8
set fileencodings=utf-8,cp932,eucjp,latin
set fileformats=unix,dos,mac
```
