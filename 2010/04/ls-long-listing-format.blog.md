Type: Blog Post (Markdown)
Blog: blog.4aiur.net
Link: http://blog.4aiur.net/2010/04/ls-long-listing-format/
Post: 94
Title: ls long listing format各个字段的含义
Slug: ls-long-listing-format
Postformat: standard
Keywords: Linux
Status: publish
Date: 2010-04-06 10:57:36 +0800
Pings: On
Comments: On
Category: Linux

**ls long listing format各个字段的含义**

取自info ls

<pre lang="bash">'-l'
'--format=long'
'--format=verbose'
     In addition to the name of each file, print the file type,
     permissions, number of hard links, owner name, group name, size,
     and timestamp (*note Formatting file timestamps::), normally the
     modification time.

     Normally the size is printed as a byte count without punctuation,
     but this can be overridden (*note Block size::).  For example, '-h'
     prints an abbreviated, human-readable count, and
     '--block-size="1"' prints a byte count with the thousands
     separator of the current locale.

     For each directory that is listed, preface the files with a line
     'total BLOCKS', where BLOCKS is the total disk allocation for all
     files in that directory.  The block size currently defaults to 1024
     bytes, but this can be overridden (*note Block size::).  The
     BLOCKS computed counts each hard link separately; this is arguably
     a deficiency.

     The permissions listed are similar to symbolic mode specifications
     (*note Symbolic Modes::).	But 'ls' combines multiple bits into the
     third character of each set of permissions as follows:
    's'
	  If the setuid or setgid bit and the corresponding executable
	  bit are both set.

    'S'
	  If the setuid or setgid bit is set but the corresponding
	  executable bit is not set.

    't'
	  If the sticky bit and the other-executable bit are both set.

    'T'
	  If the sticky bit is set but the other-executable bit is not
	  set.

    'x'
	  If the executable bit is set and none of the above apply.

    '-'
	  Otherwise.

     Following the permission bits is a single character that specifies
     whether an alternate access method applies to the file.  When that
     character is a space, there is no alternate access method.	 When it
     is a printing character (e.g., '+'), then there is such a method.</pre>
