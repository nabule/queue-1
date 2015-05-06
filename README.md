# queue

A type-agnostic header-only macro-based struct queue implementation in C.

## Usage

To create a queueable struct, include the `queue_handle_t qh` member in your struct.

Before using QUEUE_PUSH, ensure that the queue struct is set to NULL, this is the only initialisation that needs to be done.

This queue implementation is not thread safe.

Freeing the queue does not free every element in the queue if they have been dynamically allocated. This has to be done by popping all the elements in the queue and freeing them manually.

```c
#include <stdio.h>
#include "queue.h"

struct msg {
	char *content;
	queue_handle_t qh;
}

int main(void) {
	struct msgs *queue;
	struct msg m1, m2;

	queue = NULL;

	m1.content = "abc";
	QUEUE_PUSH(queue, &m1);

	printf("queue size: %d\n", QUEUE_SIZE(queue)); // queue size: 1

	QUEUE_POP(queue, &m2);
	printf("m2 content: %s\n", m2.content); // m2 content: abc

	QUEUE_FREE(queue);

	return 0;
}
```