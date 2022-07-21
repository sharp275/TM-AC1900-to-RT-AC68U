<h1 align="center">
Asus TM-AC1900 to RT-AC68U
</h1>

<h3>
The T-Mobile Asus TM-AC1900 Dual Band Wireless Router is an abandoned WiFi 5 router that has the same hardware as the ASUS RT-68U, a router that features Asus's AiMesh ability.

The RT-AC68U goes for between $80 refurbished and $110 to $130 new.  The TM-AC1900 however goes for $30 to $35 on Ebay in new condition.  A firmware conversion will change a TM-AC1900 to an RT-AC68U.

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

<h2>

Process adapted from this [medium post](https://moreless.medium.com/flash-tm-ac-1900-to-rt-ac68u-to-use-aimesh-80b84387358a). I followed this successfully on 7/20/2022.

</h2>

---

<h2>Download firmware and tools.</h2>

<br/>

Download TM-AC1900toRT-AC68U.7z from [here](https://mega.nz/file/jAEi0S5S#JvWZ0s9G4pwMLpaIW8jQ_q-Zd8MBdihch3ZSSAs8Vs0).  This contains 3 files.
<code>TM-AC1900_3.0.0.4_376_1703-g0ffdbba.trx</code> is for downgrading the current firmware to allow for ssh.
<code>mtd-write</code> is tool for changing the bootloader and firmware.
<code>FW_RT_AC68U_30043763626.trx</code> is the firmware that makes the TM-AC1900 a RT-AC68U.

Extract the files to somewhere you can easily use secure copy (scp) from.

<h2>Enable SSH</h2>

The TM-AC1900 most likely comes with a firmware version that has disabled SSH.
The router will not let you simply downgrade the firmware.  It needs to be put into rescue mode.

```
1. Hold the reset button for 10 seconds.
2. Power off the TM-AC1900 while still holding the reset button.
3. Wait 10 seconds while continuing to hold the reset button.
4. Turn the TM-AC1900 on and continue to hold the reset button for another 10 seconds
5. Power LED at the front panel flashes slowly, which indicates that the wireless router is in the rescue mode
```

Connect to the router in recovery mode.
```
1. Connect your computer by ethernet to the router
2. Setup your computer to use a static IPv4 address of 192.168.29.5 subnet mask 255.255.255.0.
3. Open a web broswer.
4. Navigate to 192.168.29.1.
```

The page should look like below.

<p  align="center">
<img src="cfe.png"/>
</p>

Choose <code>TM-AC1900_3.0.0.4_376_1703-g0ffdbba.trx</code> as the file and click upload.

After the flash finishes and the router reboots, refresh the web page at 192.168.29.1.
I had to go through the initial setup to set an admin password (set it to something simple) as the default wasn't working for me.  I left it as a router.  You can set it up as an access point but the IP address and subnet mask need to be manually set.

In the web GUI, got Administration > System.
Click to enable SSH and apply.

<h2>SSH into the router</h2>

To SSH into the router with your client of choice.  I used a chromebook with Linux setup.
I opened a terminal and type <code>ssh ```admin@192.168.29.1```</code>
This will most likely give you an error, <code>Unable to negotiate with 192.168.29.1 port 22: no matching key exchange method found. Their offer: diffie-hellman-group1-sha1.</code>  
This means diffie-hellman-group1-sha1 key exchange needs to be added to <code>/etc/ssh/ssh_config</code> or where ever appropriate for your SSH client.
For me, I installed the nano text editor by typing <code>sudo apt install nano</code>.
Then editted the <code>ssh_config</code> file by typing <code>sudo nano /etc/ssh/ssh_config</code>.
