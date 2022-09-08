# Managed-Database-Setup-Backend

## Setup ssh key antar server

1. Generate ssh key terlebih dahulu
  
       ssh-keygen
      
   ![image](https://user-images.githubusercontent.com/40049149/188897431-4bd93e25-0b7e-4233-9df5-a5866c877142.png)

2. Masuk directory .ssh untuk melihat hasil generate

       cd .ssh/
       
   ![image](https://user-images.githubusercontent.com/40049149/188898081-a9992367-300f-4b0f-9725-d0722d69a916.png)

3. Kirim file id_rsa.pub ke server

       scp id_rsa.pub  menther@web-server:~/.ssh

   ![image](https://user-images.githubusercontent.com/40049149/188899024-4b001867-1a29-423e-bc36-5a8a99f734bf.png)

4. Masuk ke server yang kita kirim barusan dengan password

       ssh menther@web-server
       
   ![image](https://user-images.githubusercontent.com/40049149/188899379-0299061f-ec25-4f98-a876-d267ca468b9e.png)

5. Masuk directory .ssh untuk memasukan key public ke file authorized_keys

       cd .ssh
       
   ![image](https://user-images.githubusercontent.com/40049149/188899884-978c6642-c5d7-46df-b6f2-bf8f5bd96cb0.png)

6. Masukan isi file id_rsa.pub ke authorized_keys

       cat id_rsa.pub >> authorized_keys

   ![image](https://user-images.githubusercontent.com/40049149/188900413-e74ec1fa-2b48-4c3a-b98f-cfb1705372ab.png)

7. Selesai tinggal coba masuk ke server dengan key ssh

       ssh -i id_rsa menther@web-server
       
   ![image](https://user-images.githubusercontent.com/40049149/188901526-e3a4e15f-a2b9-437f-abed-87002ed0ad9c.png)


## Installation Database mysql

1. Update sistem terlebih dulu

       sudo apt update
       
   ![image](https://user-images.githubusercontent.com/40049149/188903148-893d587d-6071-4366-91a2-187432cc6d88.png)

2. Jalankan comand berikut untuk install MySQL

       sudo apt install mysql-server

   ![image](https://user-images.githubusercontent.com/40049149/188903345-b72e7f04-1fcb-4d40-baa3-26ccfb74d006.png)

3. Konfigurasi MySQL

       sudo mysql
       
   ![image](https://user-images.githubusercontent.com/40049149/188906577-6caa4911-2fa8-47f7-96fc-04919ab474c5.png)
       
       ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password by 'mynewpassword';
       
   ![image](https://user-images.githubusercontent.com/40049149/188907055-3e193a66-fd27-4807-bef2-0aa67b55f4b3.png)
  
       
4. Jalankan skrip keamanan (yes semua untuk skip)
       
       sudo mysql_secure_installation
       
   ![image](https://user-images.githubusercontent.com/40049149/188907475-280894af-a096-4173-8645-ef134ed10fc3.png)

5. Masuk ke MySQL

       sudo mysql -u root -p

   ![image](https://user-images.githubusercontent.com/40049149/188909235-b681dc56-7518-49a9-9167-f701deb261e2.png)

6. Membuat user baru

       CREATE USER 'menther'@'%' IDENTIFIED BY 'Bootcamp13!@';
       
   ![image](https://user-images.githubusercontent.com/40049149/188963727-92dfa662-c7a6-4dc2-921d-c8a46eeac542.png)

7. Membuat database baru

       CREATE DATABASE dumbflix;

   ![image](https://user-images.githubusercontent.com/40049149/188913105-d165828f-da69-4e3f-ab03-5d4d19ba9aa3.png)

8. Memberikan hak akses ke database dumbflix ke user menther

       GRANT ALL ON dumbflix.* TO 'menther'@'%';
       GRANT ALL PRIVILEGES ON dumbflix.* TO 'menther'@'%';

   ![image](https://user-images.githubusercontent.com/40049149/188964272-a8ffeb61-6f32-4f43-8358-2ff1affd2adc.png)

9. Jangan lupa untuk menggunakan nya

       use dumbflix;
      
   ![image](https://user-images.githubusercontent.com/40049149/188913500-18c5c759-584e-4081-9f2d-48b19ac96a22.png)

10. Ubah configurasi MySQL nya agar bisa di akses dari luar jaringan local menjadi 0.0.0.0

        sudo nano /etc/mysql/mysql.conf.d/mysqld.cnf
       
        sudo systemctl restart mysql.service
       
   ![image](https://user-images.githubusercontent.com/40049149/188915711-9b91b7b5-8559-47a5-8db1-8ab50db3ebf2.png)

11. Ke server aplikasi Import backend dumbflik dengan git clone

        git clone https://github.com/dumbwaysdev/dumbflix-backend.git

    ![image](https://user-images.githubusercontent.com/40049149/188961219-c64cc781-fe51-47c6-a027-ab5d6f134f09.png)

12. Masuk ke directory dumbflix-backend dan install sequelize-cli untuk migrasi database
    
        cd dumbflix-backend/

        npm install --save-dev sequelize-cli

    ![image](https://user-images.githubusercontent.com/40049149/188961353-319162f2-906b-4692-aa38-f40285c7bf1a.png)

13. Seting konfigurasi sequelize-cli

        nano config/config.json

    ![image](https://user-images.githubusercontent.com/40049149/188965192-2972d846-4e57-4b6f-b35c-07f3af72ece0.png)

14. Jalankan perintah berikut untuk migrasi data ke database

        npx sequelize-cli db:migrate

    ![image](https://user-images.githubusercontent.com/40049149/188968502-0aba9495-7908-44b9-a8fc-7402e71110a0.png)

15. Kita login ke server aplikasi kemudian edit konfigurasi api

        nano dumbflix-frontend/src/config/api.js

    ![image](https://user-images.githubusercontent.com/40049149/188949995-ec4d92c3-6864-4b17-9ee0-c57a550b5c86.png)
    
16. Restart PM2 dan instal ulang npm

        pm2 delete all
        
        npm install
        
    ![image](https://user-images.githubusercontent.com/40049149/188944518-d49e1bf4-9496-4409-8390-0cfae26d234f.png)

17. Jalankan aplikasi frontend dan backend

        cd dumbflix-frontend/
        pm2 start ecosystem.config.js
        cd ../dumbflix-backend/
        pm2 start ecosystem.config.js

    ![image](https://user-images.githubusercontent.com/40049149/189166254-db67f0ab-3e12-4441-a1cb-dfdf8068802d.png)

18. Buka dibrowser https://frenky.studentdumbways.my.id/

    ![image](https://user-images.githubusercontent.com/40049149/189166820-7729dfe1-c94f-473b-8415-d0416d3aaeda.png)


## Setup SSL dengan CertBot

Jalankan perintah berikut

    sudo apt-get install snapd
    
![Screenshot from 2022-09-08 11-09-43](https://user-images.githubusercontent.com/40049149/189168233-95c77b97-b610-4abd-aa00-b528ab1d6827.png)
    
    sudo snap install core
    sudo snap refresh core
    
![Screenshot from 2022-09-08 11-11-20](https://user-images.githubusercontent.com/40049149/189168421-e31c5a96-61c0-4944-b0b5-69ae66124fbd.png)

    sudo apt-get install certbot python3-certbot-nginx

![Screenshot from 2022-09-08 11-12-42](https://user-images.githubusercontent.com/40049149/189168460-2badc6a2-cc5e-4e25-95fd-85fcfd56a7a7.png)

    sudo certbot --nginx
    
![Screenshot from 2022-09-08 11-13-58](https://user-images.githubusercontent.com/40049149/189168487-41fa0379-754a-40ed-a723-6abd42b5ae65.png)

Ikuti instruksi yang di tampilkan sampai selesai

















