Type: Blog Post (Markdown)
Blog: blog.4aiur.net
Link: http://blog.4aiur.net/2010/03/bash-sum-ten-way/
Post: 50
Title: 使用bash用10种不同的方法计数到11
Slug: bash-sum-ten-way
Postformat: standard
Keywords: Bash
Status: publish
Date: 2010-03-31 21:07:45 +0800
Pings: On
Comments: On
Category: Shell

**使用bash用10种不同的方法计数到11**

<pre lang="bash">
#!/bin/bash
# Counting to 11 in 10 different ways.
n=1; echo -n "$n "
let "n = $n + 1"   # let "n = n + 1"  also works.
echo -n "$n "

: $((n = $n + 1))
#  ":" necessary because otherwise Bash attempts
#+ to interpret "$((n = $n + 1))" as a command.
echo -n "$n "

(( n = n + 1 ))
#  A simpler alternative to the method above.
#  Thanks, David Lombard, for pointing this out.
echo -n "$n "

n=$(($n + 1))
echo -n "$n "

: $[ n = $n + 1 ]
#  ":" necessary because otherwise Bash attempts
#+ to interpret "$[ n = $n + 1 ]" as a command.
#  Works even if "n" was initialized as a string.
echo -n "$n "

n=$[ $n + 1 ]
#  Works even if "n" was initialized as a string.
#* Avoid this type of construct, since it is obsolete and nonportable.
#  Thanks, Stephane Chazelas.
echo -n "$n "

# Now for C-style increment operators.
# Thanks, Frank Wang, for pointing this out.

let "n++"          # let "++n"  also works.
echo -n "$n "

(( n++ ))          # (( ++n )  also works.
echo -n "$n "

: $(( n++ ))       # : $(( ++n )) also works.
echo -n "$n "

: $[ n++ ]         # : $[ ++n ]] also works
echo -n "$n "

echo
exit 0</pre>
