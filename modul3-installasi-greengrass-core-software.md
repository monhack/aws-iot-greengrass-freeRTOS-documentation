## Install IoT Greengrass Core Software pada Raspberry Pi

```Note : Harus lakuin step pada modul2 dulu```

Instalasi dalam CLI
1. Pertama masuk ke ssh raspi dengan perintah:

	```
	$ ssh pi@{nomor IP raspi} /
	```
2. Download IoT Greengrass Core Software lalu ekstrak dengan perintah:
	```
	$ wget https://d1onfpft10uf5o.cloudfront.net/greengrass-core/downloads/1.10.2/greengrass-linux-armv7l-1.10.2.tar.gz
	$ tar -xzvf greengrass-linux-armv7l-1.10.2.tar.gz
	```

2. Download keyring untuk update GPG key dan ekstrak dengan perintah:

	```
	$ wget -O aws-iot-greengrass-keyring.deb https://d1onfpft10uf5o.cloudfront.net/greengrass-apt/downloads/aws-iot-greengrass-keyring.deb
	$ sudo dpkg -i aws-iot-greengrass-keyring.deb
	$ echo "deb https://dnw9lb6lzp2d8.cloudfront.net stable main" | sudo tee /etc/apt/sources.list.d/greengrass.list
	```

3. Buat folder dan ekstrak file (cert, private dan public key) yang didownload saat konfigurasi greengrass di iot core dengan perintah:

	```
	$ sudo mkdir -p /greengrass
	$ sudo tar -xzvf <b>hash</b>-setup.tar.gz -C /greengrass 
	```

4. Download sertifikat root CA dan ekstrak dengan perintah:

	```
	$ cd /greengrass/certs/
	$ sudo wget -O root.ca.pem https://www.amazontrust.com/repository/AmazonRootCA1.pem 
	```

5. Re-boot raspi dengan perintah:

	```
	$ sudo reboot 
	```