# Lecture 2 Code


```bash
$ file /bin/ls
/bin/ls: ELF 64-bit LSB pie executable, x86-64, version 1 (SYSV), dynamically linked,
interpreter /lib64/ld-linux-x86-64.so.2, BuildID[sha1]=6461a544c35b9dc1d172d1a1c09043e487326966,
for GNU/Linux 3.2.0, stripped
```

```C

void seghandler(int signum)
{
  puts("SEGMENTATION FAULT");
  printf("Your key is %s\n", key);
  fflush(stdout);
  sleep(1);
  exit(0);
}

int main ()
{
  buf_t buf;
  buf.foo = &buf.buf;
  unsigned int len=0;
  int tmp;
  int keyfd = open("./key",O_RDONLY);
  struct sigaction sa;
  sa.sa_handler = seghandler;
  sigaction(SIGSEGV,&sa,NULL);

  len = read(keyfd,key,KEYSIZE);
}

```


```C
static char *uncompress(int16_t field_count, char *start, char *end,
                        char *ptr, char *uncompressed, int uncomp_len, 
                        char **uncompressed_ptr) 
{
  char *uptr = *uncompressed_ptr;
  while (field_count-- > 0 && ptr < end) {
    /* ... */
    ulen = strlen(name);
    strncpy(uptr, name, uncomp_len - (uptr - uncompressed));    
    uptr += ulen;
    *uptr++ = '\0';
    ptr += pos;
    memcpy(uptr, ptr, NS_RRFIXEDSZ);
  }
}

```


# Reading from network input

This is the first way.  It's not particularly clean, but works.

```python
#!/usr/bin/env python3
import socket
import string
from sys import argv

s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
s.connect((argv[1], int(argv[2])))

# Make the network connection behave like a normal file.
f = s.makefile(mode="rw")

# example read welcome
welcome1 = f.readline().strip()

# Example sending information
payload = 'a'*100
print(payload, file=f, flush=True)

# Read an entire response rather than just a line
while True:
  try:
    response = s.readline().strip()
    print(response)
  except:
    print("[Done reading]")
    sys.exit(0)
```


Or you can do it 1 byte at a time.
```python
import socket
import string
import sys

from sys import argv


s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
s.connect((argv[1], int(argv[2])))

def readInput():
  full_msg = ''

  while True:
    msg = s.recv(1)
    if len(msg) <= 0:
      break
    full_msg += msg.decode("utf-8")

  return full_msg

print(readInput())  # read in the first input.

payload = 'a'*100
s.send(payload + '\n')
print(readInput())
```