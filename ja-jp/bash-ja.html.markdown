---
category: tool
tool: bash
contributors:
    - ["Max Yankov", "https://github.com/golergka"]
    - ["Darren Lin", "https://github.com/CogBear"]
    - ["Alexandre Medeiros", "http://alemedeiros.sdf.org"]
translators:
  - ["Luminous Spice", "https://github.com/luminousspice"]
filename: LearnBash.sh
lang: ja-jp
---

<!--
Bash is a name of the unix shell, which was also distributed as the shell for the GNU operating system and as default shell on Linux and Mac OS X.
Nearly all examples below can be a part of a shell script or executed directly in the shell.
-->
Bash とは、Unix のシェルの名前です。GNU オペレーティングシステムのシェルとして、Linux や Mac OS X では既定のシェルとして配布されていています。
これから紹介するほとんど全ての例は、シェルスクリプトの一部として利用したり、シェルから直接実行したりすることができます。

<!--
[Read more here.](http://www.gnu.org/software/bash/manual/bashref.html) 
-->
[詳しくはこちらをお読みください。](http://www.gnu.org/software/bash/manual/bashref.html) ([日本語訳](http://linuxjm.sourceforge.jp/html/GNU_bash/man1/bash.1.html))

```bash
#!/bin/sh
<!--
# First line of the script is shebang which tells the system how to execute
# the script: http://en.wikipedia.org/wiki/Shebang_(Unix)
# As you already figured, comments start with #. Shebang is also a comment.
-->
# このスクリプトの一行目は、システムにスクリプトの実行方法を指定するシバンです。
# http://ja.wikipedia.org/wiki/%E3%82%B7%E3%83%90%E3%83%B3_%28Unix%29
# 既にお気づきかと思いますが、コメントは # から始めます。そして、シバンもコメントです。

<!--
# Simple hello world example:
-->
# 簡単な hello world の例:
echo Hello, world!

<!--
# Each command starts on a new line, or after semicolon:
-->
# それぞれのコマンドは改行で終わるか、セミコロンが続きます:
echo 'This is the first line'; echo 'This is the second line'

<!--
# Declaring a variable looks like this:
-->
# 変数は次のように定義します:
VARIABLE="Some string"

<!--
# But not like this:
-->
# 次の方法は間違っています:
VARIABLE = "Some string"
<!--
# Bash will decide that VARIABLE is a command it must execute and give an error
# because it couldn't be found.
-->
# Bash は VARIABLE を実行すべきコマンドと判断し、それが存在しないためエラーを返します。

<!--
# Using the variable:
-->
# 変数の使用例です:
echo $VARIABLE
echo "$VARIABLE"
echo '$VARIABLE'
<!--
# When you use the variable itself — assign it, export it, or else — you write
# its name without $. If you want to use variable's value, you should use $.
# Note that ' (single quote) won't expand the variables!
-->
# 変数自体を使いたい時 (代入、エクスポートなど) は、$ を付けずにその名前を書きます。
# 変数の値を使いたい場合は、$ を使わなくてはなりません。
# ' (シングルクオート) は変数の値を展開しないことに注意してください。

<!--
# String substitution in variables
-->
# 変数内の文字列置換
echo ${VARIABLE/Some/A}
<!--
# This will substitute the first occurance of "Some" with "A"
-->
# これは最初に一致した "Some" を "A" に置換します。

<!--
# Bultin variables:
# There are some useful builtin variables, like
-->
# 組み込み変数:
# 次のような便利な組み込み変数があります
<!--
echo "Last program return value: $?"
echo "Script's PID: $$"
echo "Number of arguments: $#"
echo "Scripts arguments: $@"
echo "Scripts arguments separeted in different variables: $1 $2..."
-->
echo "最後のプログラムの戻り値: $?"
echo "スクリプトの PID: $$"
echo "引数の数: $#"
echo "スクリプトの引数: $@"
echo "個々の変数に分割したスクリプトの引数: $1 $2..."

<!--
# Reading a value from input:
-->
# 入力からの値の読み込み:
echo "What's your name?"
<!--
read NAME # Note that we didn't need to declare new variable
-->
read NAME # 新たな変数の定義が必要ないことに注意してください
echo Hello, $NAME!

<!--
# We have the usual if structure:
# use 'man test' for more info about conditionals
-->
# 構造が必要なときはふつうに指定できます:
# 条件式について更に詳しい情報は 'man test' をお使いください
if [ $NAME -ne $USER ]
then
    echo "Your name is you username"
else
    echo "Your name isn't you username"
fi

<!--
# There is also conditional execution
-->
# 条件付きの実行も行えます
echo "Always executed" || echo "Only executed if first command fail"
echo "Always executed" && echo "Only executed if first command does NOT fail"

<!--
# Expressions are denoted with the following format:
-->
# 数式は次の書式で指定します:
echo $(( 10 + 5 ))

<!--
# Unlike other programming languages, bash is a shell — so it works in a context
# of current directory. You can list files and directories in the current
# directories with ls command:
-->
# 他のプログラミング言語とは違い bash はシェルなので、カレントディレクトリの状況内で動作します。
# カレントディレクトリのファイルとディレクトリの一覧は ls コマンドで得られます:
ls

<!--
# These commands have options that control their execution:
ls -l # Lists every file and directory on a separate line
-->
# これらのコマンドは実行を制御するオプションを持っています:
ls -l # 全てのファイルとディレクトリを一行ずつ一覧表示します

<!--
# Results of the previous command can be passed to the next command as input.
# grep command filters the input with provided patterns. That's how we can list
# txt files in the current directory:
-->
# 直前のコマンドの結果を、次のコマンドに入力として引き渡すことができます。
# grep コマンドは、指定したパターンで入力をフィルターします。カレントディレクトリのテキスト
# ファイルの一覧を表示する方法です:
ls -l | grep "\.txt"

<!--
# You can also redirect a command output, input and error output.
-->
# コマンドの出力、入力やエラー出力をリダイレクトすることもできます。
python2 hello.py < "input.in"
python2 hello.py > "output.out"
python2 hello.py 2> "error.err"
<!--
# The output error will overwrite the file if it exists, if you want to
# concatenate them, use ">>" instead.
-->
# 出力エラーは、出力先に指定したファイルが既に存在する場合、上書きします。
# 既に存在する内容に追加出力したい場合は、">>" を使ってください。

<!--
# Commands can be substituted within other commands using $( ):
# The following command displays the number of files and directories in the
# current directory.
-->
# $( ) を使うと別のコマンドの中で置換することができます:
# 次のコマンドは、カレントディレクトリのファイルとディレクトリの数を表示します。
echo "There are $(ls | wc -l) items here."

<!--
# Bash uses a case statement that works similarly to switch in Java and C++:
-->
# Bash では、Java や C++ の switch に似た case 文が使えます:
case "$VARIABLE" in 
    #List patterns for the conditions you want to meet
    0) echo "There is a zero.";;
    1) echo "There is a one.";;
    *) echo "It is not null.";;
esac

<!--
# For loops iterate for as many arguments given:
# The contents of var $VARIABLE is printed three times.
# Note that ` ` is equivalent to $( ) and that seq returns a sequence of size 3.
-->
# たくさんの引数を与えた for ループの繰り返し:
# 変数 $VARIABLE　の内容を 3 回表示します。
# ` ` は $( ) と同じ機能で、大きさ 3 の連番を返します。
for VARIABLE in `seq 3`
do
    echo "$VARIABLE"
done

<!--
# while loop:
-->
# while ループ:
while [true]
do
    echo "loop body here..."
    break
done

<!--
# You can also define functions
# Definition:
-->
# 関数を定義することもできます
# 定義:
function foo ()
{
    echo "Arguments work just like script arguments: $@"
    echo "And: $1 $2..."
    echo "This is a function"
    return 0
}

<!--
# or simply
-->
# 単純な定義
bar ()
{
    echo "Another way to declare functions!"
    return 0
}

<!--
# Calling your function
-->
# 関数の呼び出し
foo "My name is" $NAME

<!--
# There are a lot of useful commands you should learn:
-->
# 覚えておくと良い便利なコマンドがたくさんあります:
tail -n 10 file.txt
<!--
# prints last 10 lines of file.txt
-->
# file.txt の最後の 10 行を出力します
head -n 10 file.txt
<!--
# prints first 10 lines of file.txt
-->
# file.txt の最初の 10 行を出力します
sort file.txt
<!--
# sort file.txt's lines
-->
# file.txt の行を並び替えます
uniq -d file.txt
<!--
# report or omit repeated lines, with -d it reports them
-->
# 重複行を無視して出力する。-d を付けると重複行だけを表示する
cut -d ',' -f 1 file.txt
<!--
# prints only the first column before the ',' character
-->
# ',' キャラクタより前の最初の列だけを出力します
```
