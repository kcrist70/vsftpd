make the image from panubo/vsftpd
insert fonts and can be use with bridge network

data volume:

    /srv
config volume:

    /etc/vsftpd.conf
and config drectory:

    /etc/vsftpd

docker-compose.yml:

    version: '2'
    services:
       vsftpd:
         container_name: vsftpd
         image: kcrist/vsftpd:latest
         hostname: vsftp.neunb.rd
         restart: unless-stopped
         network_mode: "host"
         ports:
           - "21:21"
           - "4559-4564:4559-4564"
         environment:
           - LANG=zh_CN.utf8
         volumes:
           - data:/srv
           - conf:/etc/vsftpd
           - vsftpd-conf:/etc/vsftpd.conf
         volumes:
           data:
           conf:
           vsftpd-conf:

after start you must change the /etc/vsftpd.conf file:

    pasv_address=192.168.0.229 # to your external ip
and after that add your username like follow lines to add user and password use this command:

    echo > /etc/vsftpd/virtual-users.txt
    echo "USERNAME" >> /etc/vsftpd/virtual-users.txt
    mkpasswd -s -m sha-512 PASSWORD >> /etc/vsftpd/virtual-users.txt
    db5.3_load -T -t hash -f /etc/vsftpd/virtual-users.txt /etc/vsftpd/virtual-users.db
permit your username to create delete put get file or directory:

    echo "anon_upload_enable=YES
    anon_other_write_enable=YES
    anon_mkdir_write_enable=YES" > /etc/vsftpd/user_conf/YOUR_USERNAME
and now make the home directory:

    mkdir /srv/YOUR_USERNAME
    chown -R ftp.ftp /srv/

after all up step ,you can ftp your server now
