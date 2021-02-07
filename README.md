# credder

Tool to parse the output from Impacket's secretsdump.py output of NTDS.dit files.

E.g. a workflow could be

```
$ secretsdump.py -ntds NTDS.dit -system SYSTEM LOCAL > dump.txt
$ ./credder demo/dump.txt --ntlm-only --uniq > ntlm.txt
$ hashcat -a 0 -m 1000 ntlm.txt wordlist.txt 
$ hashcat -m 1000 ntlm.txt --show > cracked.txt
$ ./credder dump.txt -c cracked.txt
administrator 78f3ebd5f8c524b7242fbed473445a97 kagemand!0
guest 31d6cfe0d16ae931b73c59d7e0c089c0
krbtgt 754f5cb6ced821e2b586e7079593ca42
mj c533b83e4d7f0821522f9c4eded6accd skrivebord!0
kr b3b02ade319e3f273334b7a65510fda8 Rocknroll0
aad_f347c6e96717 1fc9acb5963938ee013df4a13eb50fb4
msol_f347c6e96717 618643ce06a1f6ea734c7c2ac8691935
normaluser 53bc35258864c4be92521f1cffabb33f Lakrids 89
testbj b9de5185588700c3e8156fe2fbe70588
testbc b9de5185588700c3e8156fe2fbe70588
testmj 7841157eb085b14b305b7461bba79d30
t1_bj ec61245260de10e0c9d2b17eb67dee8f
test 46c484036d846139666237eea133165e fiskmed!0
ninepw 6ad730214af485fcbb209baa22b9f423
tenpw f0e746b59cbf9f6be2d62c6be32445fa
benny 31d6cfe0d16ae931b73c59d7e0c089c0
ws-admin 80de3093de8a1714b2077c2f6abd8354
domain-admin c1150fa2899752531eb2cb088dde9e0d
lwp 31d6cfe0d16ae931b73c59d7e0c089c0
bwp fbc0ebd7bdc6cf4f58764cec3758930c
da 839e9d3dc9b0602a6fa2ad8b5ba18939
jump-admin 3f5a1b96956dfd4e799c26412ce1b456
```

(Optional: collect a list of enabled users, e.g. BloodHound could be of help: `MATCH (n:User) WHERE n.enabled = TRUE RETURN n`)


Full usage:

```
usage: credder [-h] [-e <list of enabled users>.txt] [-c <output from hashcat>.txt] [--username-only] [--ntlm-only] [--cleartext-only] [--search-ntlm <NTLM>] [--search-cleartext <password>]
               [--cracked-only] [--uncracked-only] [-im] [-csv] [--sort] [--uniq]
               <output from secertsdump.py>.txt

positional arguments:
  <output from secertsdump.py>.txt
                        Output from secretsdump.py -ntds ... -system ... LOCAL > dump.txt

optional arguments:
  -h, --help            show this help message and exit
  -e <list of enabled users>.txt, --enabled-users <list of enabled users>.txt
                        Used to only show enabled accounts. Maybe get this from bloodhound? MATCH (n:User) WHERE n.enabled = TRUE RETURN n
  -c <output from hashcat>.txt, --cracked-hashes <output from hashcat>.txt
                        take a hashcat --show
  --username-only       only show usernames
  --ntlm-only           only show NTLM hashes
  --cleartext-only      only show cleartext
  --search-ntlm <NTLM>  show all accounts with specified NTLM hash
  --search-cleartext <password>
                        show all accounts with specified cleartext password
  --cracked-only        only show cracked accounts
  --uncracked-only      only show uncracked accounts
  -im, --include-machines
                        include machine accounts as well
  -csv                  print comma separated
  --sort                sort output
  --uniq                omit repeated output lines
```
