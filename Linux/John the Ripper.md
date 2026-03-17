```




# Extraer Hash de archivo
gpg2john UNICARIBE.txt.gpg > gpg_hash.txt

# Usar John paracomparar hashes
john --wordlist=rockyou.txt gpg_hash.txt


```