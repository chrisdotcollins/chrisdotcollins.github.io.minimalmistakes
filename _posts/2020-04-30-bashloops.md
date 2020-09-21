---
title: Bash loops
subtitle: Safer internet sessions for kids
date: 2020-04-30
categories: [bash]
---

### Bash for loops and while loops

```
 for i in $(cat filename ); do 
   echo "$i" ; 
 done
``` 

However, if file 'filename' contains lines with spaces then need to use while read

```
 while read i; do 
   echo $i ;
done < RPMrequiresA
````
