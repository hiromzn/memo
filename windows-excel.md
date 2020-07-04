# function

- =IF(COUNTIF(範囲,条件),真の場合,偽の場合)
  - ex
    - =IF(COUNTIF(A1,"*Excel*"),"○","")


- INDEX(範囲,縦位置[,横位置])

  - 範囲の縦位置で示されるカラムの値を取得する関数。
  - 範囲の縦位置と横位置で示されるカラムの値を取得する関数。
  
- MATCH(検索値,範囲,FLAG)

  - 範囲のカラムに一致する、もしくは近似する値のカラムの位置を求める関数
  - FLAG
    - 0 : 一致
    - 1 or -1 : 近似値

- example

  - INDEX( value_list, MATCH( A7, select_list, 0 ) )
    - select_list = ( apple, orange, banana )
    - value_list  = ( 100yen, 150yen, 50yen )

# TIPS

- 行の最後まで選択
  - Shift + Ctrl + 下矢印（上矢印）

- Excel:ヘッダーフッターの印刷
  - 「ページレイアウト」「ページ設定」「印刷タイトル」で印刷タイトル行を指定する。
　
- Excel format
  - You can use the Open XML Format for excel Practically your Excel is a zip file containing few other XML files.
  - http://msdn.microsoft.com/en-us/library/aa338205%28office.12%29.aspx#office2007aboutnewfileformat_introduction
