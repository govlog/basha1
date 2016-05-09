# SHA1_in_pure_bash

Yet another totally uselessness stuff. Don't ask me why ... 

The goal was to implement SHA1 in pure bash (only bash builtins).


How to use ?
============

- It only works on 64 bytes input

Simply put this in a file :

    H=printf;((A=a=1732584193,B=d=271733878,D=1<<31,C=D-1,b=4023233417,c=2562383102,e=3285377520));while IFS=;read -d '' -n1 y;do((m++));Z+=(`$H '%d' \'$y`);done;for((i=m+1;i<63;i++,Z[m]=128))do Z[i]=0;done;for((i=0;i<63;i+=4,Z[63]=m*8))do W+=(0x`$H '%.2x' ${Z[@]:i:4}`);done;for i in {16..79};do((z=W[i-3]^W[i-8]^W[i-14]^W[i-16],z=z*2|z>>31,W[i]=z&C^z&D));done;for i in {0..79};do((X=a<<5|a>>27,T=0,f=J=b^c^d,k=3395469782,i<60))&&((f=b&c|b&d|c&d,k=2400959708,i<40))&&((f=J,k=1859775393,i<20))&&((f=b&c|~b&d,k=1518500249));for j in $X $f $e $k W[i];do((T=(T&C)+(j&C)^T&D^j&D));done;((e=d,d=c,c=b<<30|b/4,b=a,a=T));done;$H %.8x $((A+(a&C)^a&D)) $((1875749769+(b&C)^D^b&D)) $((414899454+(c&C)^D^c&D)) $((B+(d&C)^d&D)) $((1137893872+(e&C)^D^e&D));echo
    
Then:

    $ echo -en test| bash sha1.sh 
    a94a8fe5ccb19ba61c4c0873d391e987982fbbd3
    $ echo -en test | sha1sum 
    a94a8fe5ccb19ba61c4c0873d391e987982fbbd3  -
    $ echo -en test | openssl sha1
    (stdin)= a94a8fe5ccb19ba61c4c0873d391e987982fbbd3
    
Will post a more detailed/idented version later.
