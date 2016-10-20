Bash functions library
=============================
2016-10-20

A library of bash functions.


This page lists the bash functions that I needed while developing bash projects.



How does it work?
--------------------
Most functions echo something, which is meant as the function's "return".
In bash, you can 'capture' what the function outputs.

For instance, to capture the extension of a file, you can use the getFileExtension function like this:

```bash
fileExt=$(getFileExtension "fileName.txt")  # store the result of the getFileExtension function in the fileExt variable
echo "$fileExt" # txt
```

In the function comments, the word 'return' is (mis)used and actually means echo.


Functions index
------------------

- [endsWith](https://github.com/lingtalfi/bash-functions-library#endswith)
- [getFileExtension](https://github.com/lingtalfi/bash-functions-library#getfileextension)




endsWith
-------------------------


```bash
# true|false    endsWith ( fileName, expression )
# Returns whether or not the {fileName} ends with the {expression} 
function endsWith {
	if grep -q "$2""$" <<<"$1"; then
		echo 'true'
	else
		echo 'false'
	fi
}

endsWith "fileName.txt" "t" # true
endsWith "fileName.txt" "p" # false
endsWith "fileName.txt" "txt" # true
endsWith "fileName.txt" ".txt" # true
endsWith "fileName.txt" "+txt" # false
endsWith "fileName.txt" "fileName.txt" # true
endsWith "fileName.txt" "ofileName.txt" # false
endsWith "fileName.txt" "TXT" # false
```





getFileExtension
-------------------------

```bash
# emptyString|extension    getFileExtension ( fileName )
# return the extension of the {fileName} string, or an empty string if there is none
function getFileExtension {
	if grep -q '\.' <<<"$1"; then
		echo "${1##*.}"
	else
		echo ""
	fi
}

getFileExtension "fileName.txt" # txt 
getFileExtension "fileName" # (empty string)
getFileExtension "fileName.doo.vm" # vm

```
















