awk '抽出条件'
```sh
❯ seq 5 | awk '$1%2==0'
2
4
```


awkでパターン（条件）とアクション（処理）を記載して出力
```
❯ seq 5 | awk '$1%2==0{print $1, "偶数"}$1%2{print $1, "奇数"}'
1 奇数
2 偶数
3 奇数
4 偶数
5 奇数
```


合計値を足し込む
```
❯ seq 5 | awk '$1%2==0{print $1, "偶数"}$1%2{print $1, "奇数"}{a=a+$1}END{print "合計", a}'
1 奇数
2 偶数
3 奇数
4 偶数
5 奇数
合計 15
```

集計結果を降順に出力
```
❯ seq 5 | awk '{print $1%2==0?"偶数":"奇数"}' | sort | uniq -c | awk '{print $2, $1}' | sort -k2,2nr
奇数 3
偶数 2
```

nの有無で昇順降順が異なる
```
# nがないと辞書順
❯ seq 19 | awk '{print $1%2==0?"偶数":"奇数"}' | sort | uniq -c | awk '{print $2, $1}' | sort -k2,2
奇数 10
偶数 9
# nがあると数値順
❯ seq 19 | awk '{print $1%2==0?"偶数":"奇数"}' | sort | uniq -c | awk '{print $2, $1}' | sort -k2,2n
偶数 9
奇数 10
```


awkだけで集計
```
❯ seq 5 | awk '$1%2==0{print "偶数"}$1%2{print "奇数"}' | awk '{a[$1]++}END{for (k in a)print k, a[k]}'
奇数 3
偶数 2
```

bashをコマンドとして使い、ディレクトリを作成する
```
❯ seq 4 | awk '{print "mkdir " ($1%2 ? "odd_":"even_") $1}' | bash
❯ ll
Permissions Size User  Date Modified    Date Created     Git Name
drwxr-xr-x     - ykami 2021-12-08 00:50 2021-12-08 00:50  --  even_2
drwxr-xr-x     - ykami 2021-12-08 00:50 2021-12-08 00:50  --  even_4
drwxr-xr-x     - ykami 2021-12-08 00:50 2021-12-08 00:50  --  odd_1
drwxr-xr-x     - ykami 2021-12-08 00:50 2021-12-08 00:50  --  odd_3
.rw-r--r--     2 ykami 2021-11-28 19:21 2021-11-28 19:21  -N  a
.rw-r--r--  1.1k ykami 2021-11-28 23:36 2021-11-28 23:36  --  memo.md
.rw-r--r--    16 ykami 2021-11-28 19:10 2021-11-28 19:10  --  README.md
```
