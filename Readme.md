<h1 align="center">
Asus TM-AC1900 to RT-AC68U
</h1>

<h3>
The T-Mobile Asus TM-AC1900 Dual Band Wireless Router is an abandoned WiFi 5 router that has the same hardware as the ASUS RT-68U, a router that features Asus's AiMesh ability.

The RT-AC68U goes for between $80 refurbished and $110 to $130 new.  The TM-AC1900 however goes for $30 to $35 on Ebay in new condition.  A firmware conversion will change a TM-AC1900 to an RT-AC68U

<table align="center">
<tbody>
<tr>
<td><figure align="center">
<img width="500" src="s-l1600.jpg"/>
<figcaption align = "center">Asus TM-AC1900</figcaption>
</figure>
</td>
<td><figure align="center">
<img width="500" src="AC68U.jpg"/>
<figcaption align = "center">Asus RT-AC68U</figcaption>
</figure>
</td>
</tr>
</tbody>
</table>

---

<h3>

Process adapted from this [medium post](https://moreless.medium.com/flash-tm-ac-1900-to-rt-ac68u-to-use-aimesh-80b84387358a).

</h3>

---

<h3>Download firmware and tools.</h3>




Expand **Advanced details** and in **User data** enter the following:

    #!/bin/bash
    sudo su
    yum update -y
    yum install httpd -y
    mkdir /var/www/html/admin
    systemctl start httpd
    systemctl enable httpd

This updates the instance, installs and enables Apache on the instance, and makes a directory to host the webpage.

