# BEAST Memory Limit Fix
BEAST scripts (e.g., treeannotator) have hard-coded RAM limits that cause large files to hang indefinitely. To fix this, the executables can be edited to change the -Xmx argument of the java call:

## The original file (4GB limit)

"-Xmx4096m" in the final line.

```bash
#!/bin/sh

if [ -z "$BEAST" ]; then
	## resolve links - $0 may be a link to application
	PRG="$0"

	# need this for relative symlinks
	while [ -h "$PRG" ] ; do
	    ls=`ls -ld "$PRG"`
	    link=`expr "$ls" : '.*-> \(.*\)$'`
	    if expr "$link" : '/.*' > /dev/null; then
		PRG="$link"
	    else
		PRG="`dirname "$PRG"`/$link"
	    fi
	done

	# make it fully qualified
	saveddir=`pwd`
	BEAST0=`dirname "$PRG"`/..
	BEAST=`cd "$BEAST0" && pwd`
	cd "$saveddir"
fi

BEAST_LIB="$BEAST/lib"
java -Xms64m -Xmx4096m -Djava.library.path="$BEAST_LIB" -cp "$BEAST_LIB/beast.jar" dr.app.tools.treeannotator.TreeAnnotator $*
```

## Updated file (32GB limit)

Final line changed to use "-Xmx32G".

```bash
#!/bin/sh

if [ -z "$BEAST" ]; then
	## resolve links - $0 may be a link to application
	PRG="$0"

	# need this for relative symlinks
	while [ -h "$PRG" ] ; do
	    ls=`ls -ld "$PRG"`
	    link=`expr "$ls" : '.*-> \(.*\)$'`
	    if expr "$link" : '/.*' > /dev/null; then
		PRG="$link"
	    else
		PRG="`dirname "$PRG"`/$link"
	    fi
	done

	# make it fully qualified
	saveddir=`pwd`
	BEAST0=`dirname "$PRG"`/..
	BEAST=`cd "$BEAST0" && pwd`
	cd "$saveddir"
fi

BEAST_LIB="$BEAST/lib"
java -Xms64m -Xmx32G -Djava.library.path="$BEAST_LIB" -cp "$BEAST_LIB/beast.jar" dr.app.tools.treeannotator.TreeAnnotator $*
```
