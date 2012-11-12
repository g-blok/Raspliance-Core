<html><head></head><body>
<h1>"Raspliance Core": Patching Raspbian to a Headless Server Appliance</h1>
<p>Raspliance Core is a remix of Raspian/Raspbian that intends to make the most of Raspberry Pi's potentials as a server platform.

<p>It's as much a remix of Raspian/Raspbian as it is of TurnKey Linux Core 12.0, which it's modeled after.

<p>The devs at Turnkey Linux have made an art of the dedicated appliance platform; I've tried to follow their lead while relying heavily on their code and wise decisions.
<h2>Features</h2>
<ul><li>Webmin and modules: listens on port 12321
<li>Shellinabox (ajax web shell): listens on port 12320
<li>Confconsole (TKL services list)
<li>inithooks: configure root password on first boot</li>
</ul>
<h2>Assumptions</h2>
<ul><li>You have created a password for root: if you're logged in to another account, do sudo passwd root.
<li>You have logged off any users that were logged in.
<li>You have ssh enabled (raspi-config is one method).
<li>Raspi is not configured to load directly into windowed session (use raspi config)
<li>Maximize system memory, minimize display RAM with sudo raspi-config
<li>Patch is downloaded to /tmp; most likely using git clone. If zip package is downloaded, it should be unzipped -- preferably to /tmp.
</ul>
<h2>To Use</h2>
<ul><li>Download from net; you'll need to be able to unzip or use git clone.
<li>Run compile_tklpatch.sh in order to compile the tool to apply this patch to Raspian. or Raspbian.
<li>Use this command to apply the patch (assuming your in the directory with the git clone):</li>
<pre>tklpatch-apply / ./core/</pre>
<li>This applies the root directory to your running instance of raspian. Don't quit reading here.
<li>After the patch completes, you'll need to reboot. Prepare to configure a root password.
</ul>
<h2>Contents</h2>
<h3>compile_tklpatch.sh</h3>
compiles tklpatch, the SDK developed by TurnKey Linux and which has really created a community of appliance builders and contributors.
1. Install dependencies for compile
2. Use git to clone source code to /tmp/tklpatch
3. change directory to /tmp/tklpatch
4. go to the users home directory

<h3>/core/</h3>
1. Includes an overlay that injects webmin - and relevant modules - and a theme - into the file system. These components are lifted directly out of a running instance of Turnkey Linux Core 12.0 (squeeze).
2. Includes an overlay that inject .bashrc etc files into /root and /etc/skel.
3. Inject the modified services.txt file for confconsole into /etc/confconsole.
2. Purges Raspian/Raspbian packages that could obviously be purged.
3. Uses apt-get to install packages. The list of packages is intended to match those in the Turnkey Linux Core 12.0 manifest (with the exception of webmin and modules).
4. Purges unnecessary, inessential packages.
Reviewing assumptions
3. You have logged in as root.
4. You have installed git by doing apt-get install git.
5. You're starting from a fresh .img of Raspian.

<h2>Known Problems</h2>
1. Can't log in to shellinabox as root. Don't know why. Looking at http://code.google.com/p/shellinabox/issues/detail?id=113
2. 3 or 4 python modules that belong with confconsole are in /usr/bin. There's an appropriate place for them, I just couldn't get confconsole to work with them anywhere else but /usr/bin.
</body></html>