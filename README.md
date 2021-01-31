# credder

Tool to parse the output from Impacke's secretsdump.py output of NTDS.dit files.

E.g. a workflow could be

```
$ secretsdump.py -ntds NTDS.dit -system SYSTEM LOCAL > dump.txt
$ ./credder demo/dump.txt --ntlm-only --uniq > ntlm.txt
$ hashcat -a 0 -m 1000 ntlm.txt wordlist.txt 
$ hashcat -m 1000 ntlm.txt --show > cracked.txt
$ # OPTIONAL: <collect a list of enabled users> > enabled-users.txt
$ # MATCH (n:User) WHERE n.enabled = TRUE RETURN n
```
and then:

```
$ ./credder dump.txt -e enabled-users.txt -c cracked.txt
mj c533b83e4d7f0821522f9c4eded6accd skrivebord!0
kr b3b02ade319e3f273334b7a65510fda8 rocknroll0
normaluser 53bc35258864c4be92521f1cffabb33f lakrids 89
testmj 7841157eb085b14b305b7461bba79d30
benny 31d6cfe0d16ae931b73c59d7e0c089c0
```


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
