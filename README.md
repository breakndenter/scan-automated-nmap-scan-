# scan-#automated-nmap-scan-using-aliases-bat-and-nmapbar
Automate and combine full port scan and perform default scripts and versions on found ports. ***potentially dangerous modifications

Steps performed:

Full ports scan (can be adjusted via modification of nmapbar.rb ) with lovely progress bar thanks to nmapbar\
Targeted port scan (-sC -sV) on found ports\
Print result of targeted scan with colors thanks to bat\
Save both allports and targeted scans inside newly created nmap folder (using -o instead of -oA... if needed adjust as you need)


Prerequisites:#Need to be installed first 

bat - for colorfull output

source:https://github.com/sharkdp/bat

=============================================

nmapbar - for progress bar on full port scan

source:https://github.com/Mr-P4p3r/nmapbar

(allports alias looks for it in /opt so git clone there or edit your bashrc)

============================================= 

*Important step:
Since we are tasking nmapbar with the full port scan for its lovely progress bar we need to adjust it to our needs

```
sed -i 's/nmap  \-sS \-A \-p\- \-n \-T4 \-oN completescan\#{ARGV\[1\]}.txt \#{ARGV\[1\]}"/nmap \-p\- \-T4 \-\-max\-retries 0 \-o allports.log \#{ARGV\[1\]}"/g' /opt/nmapbar/nmapbar.rb
```


So what's left is to add some aliases to your bashrc and we are done :)

the things we need addded to bashrc:

------------------------------------------------------------------------------------------

```
alias ports='echo Y2F0IGFsbHBvcnRzLmxvZyB8IGF3ayAnL29wZW4veyBzID0gJDE7IGZvciAoaSA9IDU7IGkgPD0gTkYtNDsgaSsrKSBzID0gc3Vic3RyKCRpLDEsbGVuZ3RoKCRpKS00KSAiXG4iOyBzcGxpdChzLCBhLCAiLyIpOyBwcmludCAkNCAiIiBhWzFdfSd8dHIgJ1xuJyAnLCc7ZWNobw== | base64 -d | bash'
alias fullscan='nmap -p`ports` -sC -sV -o fullscan.log '
alias allports='mkcd nmap && ruby /opt/nmapbar/nmapbar.rb -c '
mkcd ()
{
                  mkdir -p -- "$1" && cd -P -- "$1"
}
scan ()
{
                  allports "$1" 1>/dev/null && echo "$(tput setaf 1)Full Port Scan Finished! $(tput setab 3)Now starting targeted Nmap scan$(tput sgr 0)" && fullscan "$1" 1>/dev/null && bat fullscan.log -l qml && cd ..
}
```
------------------------------------------------------------------------------------------

eighther use vi or add using echo and base64

```
echo YWxpYXMgcG9ydHM9J2VjaG8gWTJGMElHRnNiSEJ2Y25SekxteHZaeUI4SUdGM2F5QW5MMjl3Wlc0dmV5QnpJRDBnSkRFN0lHWnZjaUFvYVNBOUlEVTdJR2tnUEQwZ1RrWXRORHNnYVNzcktTQnpJRDBnYzNWaWMzUnlLQ1JwTERFc2JHVnVaM1JvS0NScEtTMDBLU0FpWEc0aU95QnpjR3hwZENoekxDQmhMQ0FpTHlJcE95QndjbWx1ZENBa05DQWlJaUJoV3pGZGZTZDhkSElnSjF4dUp5QW5MQ2M3WldOb2J3PT0gfCBiYXNlNjQgLWQgfCBiYXNoJwphbGlhcyBmdWxsc2Nhbj0nbm1hcCAtcGBwb3J0c2AgLXNDIC1zViAtbyBmdWxsc2Nhbi5sb2cgJwphbGlhcyBhbGxwb3J0cz0nbWtjZCBubWFwICYmIHJ1YnkgL29wdC9ubWFwYmFyL25tYXBiYXIucmIgLWMgJwoKbWtjZCAoKQp7CgkgICAgICAgICAgbWtkaXIgLXAgLS0gIiQxIiAmJiBjZCAtUCAtLSAiJDEiCn0KCnNjYW4gKCkKewoJICAgICAgICAgIGFsbHBvcnRzICIkMSIgMT4vZGV2L251bGwgJiYgZWNobyAiJCh0cHV0IHNldGFmIDEpRnVsbCBQb3J0IFNjYW4gRmluaXNoZWQhICQodHB1dCBzZXRhYiAzKU5vdyBzdGFydGluZyB0YXJnZXRlZCBObWFwIHNjYW4kKHRwdXQgc2dyIDApIiAmJiBmdWxsc2NhbiAiJDEiIDE+L2Rldi9udWxsICYmIGJhdCBmdWxsc2Nhbi5sb2cgLWwgcW1sICYmIGNkIC4uCn0K |base64 -d >> ~/.bashrc
```
and thats all
open fresh bash terminal and initiate 'scan {insert target here:)}'

<p align="center">
  <img src="https://www.offsec.dev/page/1/nmap.gif" />
</p>

