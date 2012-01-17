Init script for minecraft/bukkit servers
=======================================
A init script that apart from starting and stopping the server correctly also has some extra features for running a mincraft/craftbukkit server.

This is a modification of Ahtenus's scripts for a different set of requirements. To function properly it expects ramdisk to be used for all worlds that are to be backed up. It also expects ramdisk to be large enough to hold the worlds twice while it runs the backup. The issue that forced this fork is that on our server our admins watch the minecraft console, and interact through it. Thus having text stuffed into the console buffer was not acceptable. It is likely that these changes will not be in the best for the most configurable server, and for that I would point to the [original repository](https://github.com/Ahtenus/minecraft-init).

Features
--------

 * Utilization of ramdisk for world data, decreases lag when getting world chunks
 * Cleaning of server.log, a big logfile slows down the server
 * Backup for worlds
 * Server updating and complete backup
 * Does not requre stuffing commands into screen console in any of the backup commands.

Requirements
------------
screen,rsync,7z (p7zip-full package in ubuntu)

a ramdisk twice as large as the worlds run from ramdisk

Access server console
=====================

	screen -r minecraft

Exit the console
	
	Ctrl+A D

Setup
=====

1. Symlink the minecraft file to `/etc/init.d/minecraft`, set the required permissions and update rc.d.

		chmod 755 /etc/init.d/minecraft
		update-rc.d minecraft defaults

2. Edit the variables in `config.example` to your needs and rename it to `config` (leaving it in the same folder as the original minecraft script)

3. Move your worlds to the folder specified by `WORLDSTORAGE`

4. Edit crontab

		sudo crontab -e

	and add these lines

		#m 	h 	dom	mon	dow	command
		02 	05 	*	*	*	/etc/init.d/minecraft backup
		55 	04 	*	*	*	/etc/init.d/minecraft log-roll
		*/30 	* 	*	*	*	/etc/init.d/minecraft to-disk


5. To load a world from ramdisk run:

		/etc/init.d/minecraft ramdisk WORLDNAME
	
	to disable ramdisk, run the same command again.


For more help with the script, run

	/etc/init.d/minecraft help


Good stuff
==========
[Backup rotation script](https://github.com/adamfeuer/rotate-backups) good if you want some kind or rolling of the world backups.

[original minecraft-init repository](https://github.com/Ahtenus/minecraft-init)
