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


























