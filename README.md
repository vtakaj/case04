# 間違えてコミットしてしまった、コミットする前時点に戻したい。
*間違ったコミットを公開しているシナリオ*

シナリオ3では、``git reset``を実行するまで、コミットをリモートに``git push``していません。  
もし、先に``git push``を実行していれば、``git reset``してからの再度``git push``はエラーで怒られる。  
このエラーは既に公開されたコミットが改ざんされるというエラーになります。  
問題がなければ、そのまま``git push -f``で無理やりリモートのコミット履歴を書き換えるのもできますが  
シナリオ4では、公開したコミットのもう一つの「打ち消す」方法を紹介します。


>ブラウザから操作
## case03と同じ操作でcase04のリポジトリをフォークする
https://github.com/git-study-session-demo/case04
(略)

## フォークしたリポジトリをローカルにクーロンして、その中に入る

```bash
git clone <your repository url> case04
cd case04
```
### 開発ブランチ01を作成する

```
git switch -c feature/case0401
```
## サンプルファイルを追加して、そのファイルをコミットし、プッシュする

```
touch sample.txt
echo "hello, world" > sample.txt
git add .
git commit -m 'add sample.txt'
```

## sample02.txtとconfig.txt　のサンプルファイルを追加して、そのファイルをコミットする

```
touch sample02.txt
echo "hello, world" > sample02.txt
touch config.txt
echo "個人設定" > config.txt
git add .
git commit -m 'add sample02 file'
```

## config.txt を間違ってコミットしたことをすぐに気付き、そのコミットを「打ち消す」
git logコマンドで打ち消したいコミットを確認する。

```
git log
git revert <打ち消したいコミットのコミットID>
```
そして、再度 sample02.txtのみをコミットする

```
touch sample02.txt
echo "hello, world" > sample02.txt
touch config.txt
echo "個人設定" > config.txt
git add sample02.txt
git commit -m 'add sample02 file'
```

## 開発ブランチをプッシュする

```
git push origin feature/case0401
```
## case01と同様操作で、PRを作成し・マージを行う
(略)  
*mainブランチに sample.txtとsample02.txt ファイルは取り込んでいて、そして config.txt は取り込んでいないことを確認*

## ローカルの開発ブランチを削除

```
git branch -d feature/case0401
```

## 考える
``git revert``コマンドは、コミット履歴を改ざんしない形でソース変更を打ち消すことができる。  
この場合、一連に変更した内容は、履歴の中に維持したままになるので、場合により大きな問題となる。  
どういう場合で問題になるかを考えてみてください。