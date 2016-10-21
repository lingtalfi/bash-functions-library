Bash functions library
=============================
2016-10-20

A library of bash functions.


This page lists the bash functions that I needed while developing bash projects.

bash 4.4 was used


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
- [replace](https://github.com/lingtalfi/bash-functions-library#replace)
- [strlen](https://github.com/lingtalfi/bash-functions-library#strlen)
- [substr](https://github.com/lingtalfi/bash-functions-library#substr)




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




replace
-------------------------

```bash
# replacedString	replace ( what, byWhat, sourceString )
function replace {
	echo "${3/$1/$2}"
}

replace "food" "pizza" "I love food" # I love pizza
replace "food" "pizza" "I love cakes" # I love cakes
```





strlen
-------------
```bash
# number    strlen ( string )
# Returns the length of {string}
function strlen {
	echo ${#1} 
}

strlen "fileName.txt" # 12
strlen "fi" # 2
strlen "" # 0
```




substr
-------------
```bash
# string	substr ( string, offset )
# string	substr ( string, offset, length )
# Returns the substring of {string} starting from index {offset} (first char is at 0).
# If length is not given, the substring will end at the end of the string
# If length is given, the substring will end when {length} characters are consumed.
# {offset} can be negative.
function substr {
	if [ -n "$3" ]; then
		echo ${1: $2: $3}
	else
		echo ${1: $2}
	fi
}


substr "123456" "0" # 123456
substr "123456" "1" # 23456
substr "123456" "1" "3" # 234
substr "123456" "1" "30" # 23456
substr "123456" "10"  # (empty string)
substr "123456" "-3"  # 456
substr "123456" "-3" "1" # 4
```












