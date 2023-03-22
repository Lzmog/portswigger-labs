## **Simple OS command injection**

![Untitled](https://github.com/Lzmog/portswigger-labs/blob/main/images/command-injection/simple-os-command-injection.png)

| Purpose of command | Linux | Windows |
| --- | --- | --- |
| Name of current user | whoami | whoami |
| Operating system | uname -a | ver |
| Network configuration | ifconfig | ipconfig /all |
| Network connections | netstat -an | netstat -an |
| Running processes | ps -ef | tasklist |

## **Blind OS command injection with time delays**

![Untitled](https://github.com/Lzmog/portswigger-labs/blob/main/images/command-injection/blind-os-sql-injection-time-delay.png)

## **Blind OS command injection with output redirection**

![Untitled](https://github.com/Lzmog/portswigger-labs/blob/main/images/command-injection/blind-os-sql-injection-output-redirect.png)

when request is sent, go to product and intercept traffic, forward when you see filename and change it into whoami.tx

## **Exploiting blind OS command injection using out-of-band ([OAST](https://portswigger.net/burp/application-security-testing/oast)) techniques**

(required burp collabprator)

![Untitled](https://github.com/Lzmog/portswigger-labs/blob/main/images/command-injection/blind-os-command-injection-out-of-band-0.png)

## **Blind OS command injection with out-of-band data exfiltration**

![Untitled](https://github.com/Lzmog/portswigger-labs/blob/main/images/command-injection/blind-os-command-injection-out-of-band-1.png)

![Untitled](https://github.com/Lzmog/portswigger-labs/blob/main/images/command-injection/blind-os-command-injection-out-of-band-2.png)