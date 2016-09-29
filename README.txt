set Path="C:\Program Files\Git\bin";%Path%

git status
git add *.md
git rm --cached _toc.md
git commit -m "first release"

GitHub wiki では 
	blockquote
		後に空行１行必要。
		前にも必要な場合あり。意味不明。 -> f2bt.md, db_shell.md, linklet_window.md, bvxxplotl.md, cpil_rel.md
		db_shell.md では更に blockquote の indent もズレる。
	
	**bold** の様にスペースを空けない

	x.md へのリンクは [[x-title|x]] とする

	ali.md でのみ ``` のインデントできない。
