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

6. Membuat database baru

       CREATE DATABASE dumbflix;

   ![image](https://user-images.githubusercontent.com/40049149/188913105-d165828f-da69-4e3f-ab03-5d4d19ba9aa3.png)

7. Jangan lupa untuk menggunakan nya

       use dumbflix;
      
   ![image](https://user-images.githubusercontent.com/40049149/188913500-18c5c759-584e-4081-9f2d-48b19ac96a22.png)

8. Ubah configurasi MySQL nya agar bisa di akses dari luar jaringan local menjadi 0.0.0.0

       sudo nano /etc/mysql/mysql.conf.d/mysqld.cnf
       
       sudo systemctl restart mysql.service
       
   ![image](https://user-images.githubusercontent.com/40049149/188915711-9b91b7b5-8559-47a5-8db1-8ab50db3ebf2.png)

9. Install NVM

       wget -qO- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.1/install.sh | bash
       
       exec bash
       
       nvm i 10
       
   ![image](https://user-images.githubusercontent.com/40049149/188919101-765d4a53-1992-4c5f-aad4-d886720cc616.png)
   
10. Import backend dumbflik dengan git clone

        git clone https://github.com/dumbwaysdev/dumbflix-backend.git

    ![image](https://user-images.githubusercontent.com/40049149/188927922-b98a5fd6-6f8f-47ac-b1b0-a648ea041a89.png)

11. Masuk ke directory dumbflix-backend dan install sequelize-cli untuk migrasi database
    
        cd dumbflix-backend/

        npm install --save-dev sequelize-cli

    ![image](https://user-images.githubusercontent.com/40049149/188928736-582d7dc9-c5e7-4490-9f36-36faa668a092.png)

11. Seting konfigurasi sequelize-cli

        nano config/config.json

    ![image](https://user-images.githubusercontent.com/40049149/188929276-f55678f7-612c-4a0c-b72b-cd1a7e68e7de.png)

12. Jalankan perintah berikut untuk migrasi data ke database

        npx sequelize-cli db:migrate

    ![image](https://user-images.githubusercontent.com/40049149/188929690-45fc54de-ebf0-468e-96aa-4f39b0b4e984.png)

13. Kita login ke server aplikasi kemudian edit konfigurasi api

        nano dumbflix-frontend/src/config/api.js

    ![image](https://user-images.githubusercontent.com/40049149/188949995-ec4d92c3-6864-4b17-9ee0-c57a550b5c86.png)
    
14. Restart PM2 dan instal ulang npm

        pm2 delete all
        
        npm install
        
    ![image](https://user-images.githubusercontent.com/40049149/188944518-d49e1bf4-9496-4409-8390-0cfae26d234f.png)


        











