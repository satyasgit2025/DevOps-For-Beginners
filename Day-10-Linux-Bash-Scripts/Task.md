a.Create a zip archive named xfusioncorp_ecommerce.zip of /var/www/html/ecommerce directory.

b. Save the archive in /backup/ on App Server 3. 

This is a temporary storage, as backups from this location will be clean on weekly basis. 

Therefore, we also need to save this backup archive on Nautilus Backup Server. 

c.Copy the created archive to Nautilus Backup Server server in /backup/ location. 

d.Please make sure script won't ask for password while copying the archive file. 

Additionally, the respective server user (for example, tony in case of App Server 1) must be able to run it. 

e.Do not use sudo inside the script. 

Note: The zip package must be installed on given App Server befoire executing the script. 

This package is essential for creating the zip archive of the website files. Install it manually outside the script.
