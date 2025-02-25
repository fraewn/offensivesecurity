## Exercise 2 COUNTER

## Intro 
* vulnerability: nonce is used to create **both** shown encryptions 
* die AES ist im counter mode (AES.MODE_CTR): turns block cipher into a stream cipher. 
Each byte of plaintext is XOR-ed with a byte taken from a keystream: the result is the 
ciphertext. The keystream is generated by encrypting a sequence of counter blocks with ECB.
https://pycryptodome.readthedocs.io/en/latest/src/cipher/classic.html#ctr-mode 
* ein Block aus nonce+counter ist genau 16 bytes lang 
* der counter ist big endian encoded 

## Attacke
Da die gleiche nonce verwendet wurde, kann man einfach die beiden cipher texte xoren: 
c1 xor c2 = verschl(a,key) xor versch(b,k) = a xor b (weil die keys rausgestrichen werden weil die genau gleich sind wegen gleicher nonce). 
D.h. wir haben a xor b = Ergebnis von xor(c1,c2)
D.h. jetzt können wir einfach, wenn wir a oder b haben, diesen mit dem Ergebnis von xor(c1,c2) xoren um das andere zu bekommen. 
Da wir unsere Eingabe haben, können wir a (a=unsere eingabe) xor ergebnis(xor(c1,c2)) = b (b=flag)
Vorher müssen wir die Ausgaben der Anwendung aber noch zu byte arrays decoden, da die in hex vorliegen im Moment. 

## Ablauf Counter Mode Encryption von hinten
(1. hex() zu dem Format davor)
2. xor mit plaintext
3. mit key entschlüsseln 
4. dann hat mit die nonce+counter 

## Lösung 
### Schritt 1: Strings von Hex- in die Original-Formatierung umwandeln 
Flag in hex: 
```
b'5cd9430d66c0245e6691568fef403d798711d6da2d63298678978711e5ec78a5c2'
```

Flag in Orginal-Formatierung nach Encryption: 
```
b'\\\xd9C\rf\xc0$^f\x91V\x8f\xef@=y\x87\x11\xd6\xda-c)\x86x\x97\x87\x11\xe5\xecx\xa5\xc2'
```

Bekannter String in hex: 
```
b'7df0764a32ae5c6451a426cca40443'
```

Bekannter String in Original-Formatierung nach Encryption: 
```
b'}\xf0vJ2\xae\\dQ\xa4&\xcc\xa4\x04C'
```

### Schritt 2: 
Die beiden byte arrays in Original-Formatierung werden jetzt gexored: 
```
b'\x07\r\x00\x06\x1a/Q\x0f\x02\x04>\x13\x04\x14\x12\x04>\x08\x12>\x10\x14\x08\x15\x04>\x03\x08\x15\x15\x04\x13\x1c'
```

### Schritt 3: 
Das Ergebnis von Schritt 2 und der plain text von einem der beiden cipher texte werden jetzt gexored: 
plaintext den ich eingegeben hab: 
```
aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa
```

Ergebnis xor(plaintext, ergebnis(Schritt2)): flag
```
flag{N0nce_reuse_is_quite_bitter}
```


