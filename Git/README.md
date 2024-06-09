# .gitignoreのテンプレート
github上に.gitignoreのテンプレートがある
[https://github.com/github/gitignore](https://github.com/github/gitignore)

# ローカルブランチの削除
```
git branch -d (branch name)
```

# ブランチのマージ
```
git merge origin/main
```
```
git merge --allow-unrelated-histories origin/main
```

# Github上からディレクトリを削除
```
git rm -r --cached MissilesPerfectMaster/
```
# Github上からファイルを削除
```
git rm -r --cached /.DS_Store  
```
# addとかcommitを取り消したいとき
混乱してきたらこの記事を確認  
[Qiita:困ったときの git reset コマンド集](https://qiita.com/ChaaaBooo/items/459d5417ff4cf815abce)

 - [githubのmainブランチに初pushするときに出たエラー解決方法](https://qiita.com/yuta_mimi/items/d34a1440914cac128239) 

# checkoutをし忘れて間違ったbranchでpushした時
1. まずは正しい方にcheckoutしたpush
2. 間違った方のbranchに戻ってコミットを取り消す
```
git revert HEAD
```
3. 訂正したというコミットを反映させる
```
git push origin HEAD
```
[https://qiita.com/S42100254h/items/db435c98c2fc9d4a68c2](https://qiita.com/S42100254h/items/db435c98c2fc9d4a68c2)

# commitログを見るとき
```
git log --first-parent --graph --abbrev-commit --decorate <ブランチ名>
```
[https://qiita.com/fuubit/items/2fe38f0e20bd44df2e4c](https://qiita.com/fuubit/items/2fe38f0e20bd44df2e4c)

# Commit Message Template
```
git config --local commit.template ./.gitmessage.txt
```

# Git運用
## ルール
### ブランチ命名規則
| ブランチ | 役割 | 派生元 | マージ先 |  
| - | - | - | - |  
| main | 完成品を置くブランチ| - | - |  
| develop |	開発中のものを置くブランチ | master | master |  
| feature-* | 新機能開発中に使うブランチ | develop | develop |  
| hotfix-* | バグ修正用ブランチ | master | develop,　master |  

### コミットメッセージ
`Conventional Commits` を採用  
https://www.conventionalcommits.org/ja/v1.0.0/
#### コミットメッセージのテンプレート
```
<型>: <タイトル> #<Issue番号>

[任意 本文]

[任意 フッター]
```
##### 補足
- 型
```
feat: 新しい機能
fix: バグの修正
docs: ドキュメントのみの変更
style: 空白、フォーマット、セミコロン追加など
refactor: 仕様に影響がないコード改善(リファクタ)
perf: パフォーマンス向上関連
test: テスト関連
chore: ビルド、補助ツール、ライブラリ関連
```
- タイトル...必須
- Issue番号...任意
  
## コミット方法
ベーシックなやり方。  
`git commit -m` は改行がしづらいかもしれないので、  
`git commit` から `vi` でコミットメッセージを入力することをオススメ。
```
$ git commit
## コミットメッセージを記載
```

