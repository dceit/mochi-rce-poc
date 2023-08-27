# Mochi RCE Proof-of-Concept

This vulnerability stems from inadequate markdown parsing in the handling of card faces, along with misconfiguration of the Electron application. Maliciously crafted markdown content within these cards can trigger JavaScript execution. Exploiting this XSS flaw, attackers can then escalate the vulnerability to spawn child processes from within the application, achieving RCE by executing arbitrary code via the Electron app. It's important to emphasize that the exploration of this vulnerability and its technical details are intended solely for educational purposes, aimed at fostering a deeper understanding of security challenges within web applications. Any attempt to exploit these vulnerabilities without proper authorization is strongly discouraged and may be illegal.

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

*tl;dr:
Drink all the booze, hack all the things.*