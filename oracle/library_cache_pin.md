Mostrar el proceso que tiene bloqueado el objeto:
```sql
select
    s2.sid, 
   s.saddr, 
   sw.p1raw
from
     v$session_wait sw,
     v$session s,
     dba_kgllock w ,
     dba_kgllock b,
     v$session s2
where
    sw.sid = s.sid
    and sw.event='library cache pin'
    and w.KGLLKHDL  = b.KGLLKHDL
    and w.KGLLKREQ  > 0 and b.KGLLKMOD > 0
    and w.KGLLKTYPE = b.KGLLKTYPE
    and w.KGLLKUSE = s.saddr
    and w.KGLLKHDL = sw.p1raw
    and s2.saddr = b.KGLLKUSE;
```
 
El resultado es el siguiente:

|SID | SADDR | P1RAW|
-----|-------|---------
|154	| 00007FF9E80D9078	| 00007FF9FF6814D0|

Ahora solo queda matar la sesión `154`
