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





getValueFromStore
-------------------------

```bash

# string 	getValueFromStore ( key, storePath )
# the store is a file which contains key/value pairs,
# one per line, using the following format:
# key=value
# this function first tries to access the store.
# if the store is not found, the returned value is ERR_STORE_NOT_FOUND
# then, if the key is empty or not found, the returned value is empty string
# if multiple keys are identical, the first one found only will be considered
# The rare case where a line consist only of a matching key (no value, wrong syntax) is not handled, for performance reasons
function getValueFromStore {
	local content
	if [ -f "$2" ]; then
		if [ -n "$1" ]; then
			found="false"
			# http://www.unix.com/shell-programming-and-scripting/161645-read-file-using-while-loop-not-reading-last-line.html
			DONE="false"
			until $DONE; do
			    read line || DONE=true
			    if [ "false" = "$found" ]; then
				    key="$(echo "$line" | cut -d "=" -f 1)"
				    if [ "$1" = "$key" ]; then
				    	echo "$line" | cut -d "=" -f 2
				    	found="true"
				    fi
			    fi
			done < "$2"
		fi
	else
		echo "ERR_STORE_NOT_FOUND"
	fi
}

```

Examples shown below use a file named store.txt, which contains the following:

```txt
key1=value1
key2=This is value 2
key-3="This is value 3"
password1=xaB45
```

```bash
getValueFromStore "ali" "./store.txt" # (empty string)
getValueFromStore "key1" "./store.txt" # value1
getValueFromStore "key1" "./storeddd.txt" # ERR_STORE_NOT_FOUND
getValueFromStore "key2" "./store.txt" # This is value 2
getValueFromStore "key-3" "./store.txt" # "This is value 3"
getValueFromStore "password1" "./store.txt" # xaB45
```



Note:
the getValueFromStore function was originally created to store passwords in a 
secured centralized place, so that scripts can access it.







replace
-------------------------

```bash
# replacedString	replace ( what, byWhat, sourceString )
# note: % symbol are not replaced, unless prefixed with a slash (\%)
function replace {
	echo "${3/$1/$2}"
}

replace "food" "pizza" "I love food" # I love pizza
replace "food" "pizza" "I love cakes" # I love cakes
replace "\%s" "burgers" "I love %s" # I love burgers
replace "%s" "burgers" "I love %s" # I love %burgers
replace "_s_" "burgers" "I love _s_" # I love burgers
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












