# Understanding /etc/hosts file

What is the /etc/hosts file?

- A Local file on your computer
- Maps domain names to IP addresses
- Takes precedence over DNS for specific entries

### Modifying the host file

- Open the file with a text editor (vim, nano etc)
- Add an entry in the format `129.124.1.5 google.com`
This maps google.com to the IP address we provided and is what our machine will use to go to google.com