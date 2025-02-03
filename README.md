Brainfuck is a satirical coding language written with just 8 different symbols. Everything is based on a large integer vector and a pointer on one of its cells. The symbols are:
| brainfuck  | C             |
|------------|---------------|
| +          | (*ptr)++;     |
| -          | (*ptr)--;     |
| >          | ptr++;        |
| <          | ptr--;        |
| [          | while(*p){    |
| ]          | }             |
| .          | putchar(*p);  |
| ,          | *p=getchar(); |

Such minimalistic language requires a high level of attention to detail and memory management. Even the most simple mathematical operations like multiplication require a certain number of temporary cells.

As a form of challenge, I made a simple calculator in brainfuck. In order to do so, I had to write and carefully describe each function and then put these pieces together.

To run it correctly, use interpreter with 2-side/wrapping vector and larger cells.
