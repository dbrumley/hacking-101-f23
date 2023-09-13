```C
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

int main(int argc, char *argv[]) {
  void *ptr;
  sscanf(argv[1], "%p", &ptr);
  strcpy(ptr, argv[2]);
}
```

```C
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

void write_what_where(char *a, char *b) { 
  void *ptr;
  sscanf(a, "%p", &ptr);
  strcpy(a, b);
}

int main(int argc, char *argv[]) {
  write_what_where(argv[1], argv[2]);
  printf("/bin/sh");
  return 0;
}
```

```C
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

void write_what_where(char *a, char *b) { 
  void *ptr;
  sscanf(a, "%p", &ptr);
  strcpy(a, b);
}

int main(int argc, char *argv[]) {
  write_what_where(argv[1], argv[2]);  
  printf("argv[3] length: %d\n", strlen(argv[3]));
  return 0;
}
```