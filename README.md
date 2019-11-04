# ICTFF-Intermediate-2019-Writeup

## bin100
This is a windows binary.
Check the file type using linux.
```
file bin100.exe 
bin100.exe: PE32 executable (console) Intel 80386 Mono/.Net assembly, for MS Windows
```

Check the source code with dnSpy
```
using System;
using System.Linq;
using System.Text;

namespace bin100
{
	// Token: 0x02000002 RID: 2
	internal class Program
	{
		// Token: 0x06000001 RID: 1 RVA: 0x00002050 File Offset: 0x00000250
		private static void Main(string[] args)
		{
			if (args.Length == 0)
			{
				Console.Write("Enter serial : ");
				string text = Console.ReadLine();
				if (text[0] == 'u' && text[1] == 'i' && text[2] == 't' && text[3] == 'm')
				{
					Console.WriteLine("Flag : " + text + " ? ");
					return;
				}
				Console.WriteLine("Nope !");
				return;
			}
			else
			{
				byte[] array = Convert.FromBase64String(args[0]);
				array = array.Reverse<byte>().ToArray<byte>();
				byte[] array2 = new byte[array.Length];
				array.CopyTo(array2, 0);
				for (int i = 0; i < array2.Length; i++)
				{
					byte[] array3 = array2;
					int num = i;
					array3[num] = (byte)(array3[num] >> 1);
				}
				if (array2[0] == 117 && array2[1] == 105 && array2[2] == 116 && array2[3] == 109 && Convert.ToBase64String(array).Equals("6tLo2vauZpjGYJpmvuievo7c0qTKZpxijtyK+g=="))
				{
					Console.WriteLine("Flag : " + Encoding.UTF8.GetString(array2, 0, array2.Length) + " ! ");
					return;
				}
				Console.WriteLine("Nope !");
				return;
			}
		}
	}
}

```
This code will decode the input to base64.

From the code we can see that a line that will print the flag
```
Console.WriteLine("Flag : " + Encoding.UTF8.GetString(array2, 0, array2.Length) + " ! ");
					return;
```
And there is a base64 encrypted string
```
6tLo2vauZpjGYJpmvuievo7c0qTKZpxijtyK+g==
```
Copy the line and replace it on this line
```
Console.WriteLine("Nope !")
```
Compile and run the program with the encrypted strings as the argument.
```
.\bin1003.exe 6tLo2vauZpjGYJpmvuievo7c0qTKZpxijtyK+g==
Flag : }EnG1N3eRinG_Ot_3M0cL3W{mtiu !
```
The flag is uitm{W3LcoM3_tO_GniRe3N1GnE}

## bin200
This challenge provide 2 files
decryptor and flag
When we run the decryptor file it will ask for a key to print the flag

```
decryptor: python 3.6 byte-compiled
```
A python compiled file =D
Decompiled it using ``` uncompyle6 ```
```
key = input('key:')
ci = bytearray()
with open('flag', 'rb') as (f):
    te = f.read()
    for i in range(len(te)):
        ci.append(te[i] ^ ord(key[(i % len(key))]))

print(ci)
```
From the code,

encrypted bytes ^ unknown key = known plaintext
We can change the position
encrypted bytes ^ known plaintext = unknown key

we have encrypted bytes and known plaintext
the known plaintext attact is the first 5 chars of the flag's format which is
``` "uitm{" ```
Run it and enter ``` uitm{ ```
```
key:uitm{
bytearray(b'yFk*}T@M\x18Cm\\F\x18E~Vo\x136Sfe\x18Cm\\f:')
```
So the first 5 byte from the output is the key ``` yFk*} ```
Run again and enter it with the known key and we get the flag
```
key:yFk*} 
bytearray(b'uitm{XoR_EasY_CrypT0_Iz_Easy}')
```
Flag: uitm{XoR_EasY_CrypT0_Iz_Easy}



