Linux IPC-Message Queues

# AIM:
To write a C program that receives a message from message queue and display them

# DESIGN STEPS:

### Step 1:

Navigate to any Linux environment installed on the system or installed inside a virtual environment like virtual box/vmware or online linux JSLinux (https://bellard.org/jslinux/vm.html?url=alpine-x86.cfg&mem=192) or docker.

### Step 2:

Write the C Program using Linux message queues API 

### Step 3:

Execute the C Program for the desired output. 

# PROGRAM:

## C program that receives a message from message queue and display them

```

#include <stdio.h>
#include <stdlib.h>
#include <sys/ipc.h>
#include <sys/msg.h>
#include <string.h>

struct message {
    long msg_type;
    char msg_text[100];
};

int main()
{
    key_t key;
    int msgid;
    struct message msg;

    key = ftok("progfile", 65);

    msgid = msgget(key, 0666 | IPC_CREAT);

    if (msgid == -1)
    {
        perror("msgget");
        exit(1);
    }

    msg.msg_type = 1;

    strcpy(msg.msg_text, "Hello from Message Queue");

    msgsnd(msgid, &msg, sizeof(msg.msg_text), 0);

    printf("Message sent successfully.\n");

    return 0;
}



```

## sender Side code:
```
#include <stdio.h>
#include <stdlib.h>
#include <sys/ipc.h>
#include <sys/msg.h>
#include <string.h>

struct message {
    long msg_type;
    char msg_text[100];
};

int main()
{
    key_t key;
    int msgid;
    struct message msg;

    // Generate a unique key
    key = ftok("progfile", 65);

    // Create / access message queue
    msgid = msgget(key, 0666 | IPC_CREAT);

    if (msgid == -1)
    {
        perror("msgget");
        exit(1);
    }

    // Receive message
    if (msgrcv(msgid, &msg, sizeof(msg.msg_text), 1, 0) == -1)
    {
        perror("msgrcv");
        exit(1);
    }

    // Display received message
    printf("Message received from queue: %s\n", msg.msg_text);

    // Remove message queue
    msgctl(msgid, IPC_RMID, NULL);

    return 0;
}



```

## OUTPUT

<img width="648" height="567" alt="image" src="https://github.com/user-attachments/assets/5c4d17eb-4798-44fd-b111-96f3fb1499c2" />



# RESULT:
The programs are executed successfully.
