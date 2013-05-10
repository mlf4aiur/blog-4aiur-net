Type: Blog Post (Markdown)
Blog: blog.4aiur.net
Link: http://blog.4aiur.net/2012/03/variable-scope-in-subshell/
Post: 690
Title: Variable scope in subshell
Slug: variable-scope-in-subshell
Postformat: standard
Keywords: subshell
Status: publish
Date: 2012-03-10 23:26:28 +0800
Pings: On
Comments: On
Category: Shell

Variable scope in subshell
=======

### functions

<pre lang="bash">[mlf4aiur@4aiur Shell]$ cat function_return.sh 
subshell () {
    # pipeline will generate subshell
    echo 1 2 3 | while read line
    do
        echo "subshell: inside 1"; var=1; return 1
    done
    echo "subshell: inside 0"; var=0; return 0
}
subshell
echo "subshell: outside return $? var=$var"

foo () {
    for line in $(echo 1 2 3)
    do
        echo "foo: inside 1"; var=1; return 1
    done
    echo "foo: inside 0"; var=0; return 0
}

foo
echo "foo: outside return $? var=$var"

bar () {
    while read line
    do
        echo "bar: inside 1"; var=1; return 1
    # Bash Process Substitution, original shell doesn't work
    done < <(echo 1 2 3)
    echo "bar: inside 0"; var=0; return 0
}

bar
echo "bar: outside return $? var=$var"</pre>

### using shell run it

<pre lang="bash">[mlf4aiur@4aiur Shell]$ sh function_return.sh 
subshell: inside 1
subshell: inside 0
subshell: outside return 0 var=0
foo: inside 1
foo: outside return 1 var=1
function_return.sh: line 28: syntax error near unexpected token `<'
function_return.sh: line 28: `    done < <(echo 1 2 3)'
[mlf4aiur@4aiur Shell]$ </pre>

### using bash run it

<pre lang="bash">[mlf4aiur@4aiur Shell]$ bash function_return.sh 
subshell: inside 1
subshell: inside 0
subshell: outside return 0 var=0
foo: inside 1
foo: outside return 1 var=1
bar: inside 1
bar: outside return 1 var=1
[mlf4aiur@4aiur Shell]$ </pre>
