# sysadminks
# Este repositorio está diseñado para almacenar y organizar diversas instrucciones y guías utilizadas en el área de administración de sistemas (Sysadmin). Aquí se recopilarán los procedimientos, scripts y comandos esenciales para gestionar y mantener la infraestructura de TI de manera eficiente y efectiva.

# Manual Despliegue 
1)Descargar el proyecto .tar.gz de gitlab y guardarlo en cualquier ruta.

2) Posicionandonos en la ruta donde esta guardado y enviar el archivo al contenedor.
scp archivo.tar.gz usuario@ip_contenedor:.

3) Entrar al contenedor.
ssh usuario@ip_contenedor

4) Descomprimir el proyecto.
sudo tar -xzvf archivo.tar.gz

5) Renorbrar el proyecto.
mv archivo_descomprimido nombre_nuevo

6) Mover el proyecto a la carpeta /var/www o /home/proyecto
sudo mv nombre_nuevo /var/www
sudo mv nombre_nuevo /home/proyecto

7) Entrar al proyecto anterior
cd /var/www/proyecto-anterior o cd /home/proeycto/proyecto-anterior

8) Copiar los archivos necesarios. 

	Escolares:
	sudo cp Gemfile Gemfile.lock config.ru ../nes-escolares-test-version/
	cd public
	sudo cp -r estancias_docs servicio_social_docs ../../nes-escolares-test-version/public/
	cd ../config
	sudo cp database.yml ../../nes-escolares-test-version/config/
	cd initializers/
	sudo cp secret_token.rb ../../../nes-escolares-test-version/config/initializers/
	
	Los demás proyectos:
	sudo cp Gemfile Gemfile.lock ../nes-alumnos-version/
	cd config
	sudo cp database.yml secrets.yml ../../nes-alumnos-version/config/
	cd initializers/
	sudo cp assets.rb ../../../nes-alumnos-version/config/initializers/
	cd ../environments
	sudo cp production.rb ../../../nes-alumnos-version/config/environments/


9) Regresar a donde estan todos los proyectos
cd /var/www o /home/proyecto

10) Darle los permisos correspondientes a todos los archivos del proyecto.
sudo chmod -R 755 nes-sistema-test-version
sudo chown -R escolares:escolares nes-sistema-test-version

11) Eliminar enlace simbólico.
sudo unlink nes-escolares-test

12) Crear el nuevo enlace simbólico con la nueva versión a desplegar.
sudo ln -s nes-sistema-test-version nes-sistema-test

13)Ejecutamos bundle install

14)
cd nes-alumnos-suneo-4-3.0.23-alpha.1
* En caso de despliegue de ESCOLARES:
sudo ./identidad-institucional-nes.sh utm
* En caso de despliegue de FINANCIEROS:
sudo ./identidad-institucional-financieros.sh utm
* En caso de despliegue de ALUMNOS:
sudo ./identidad-institucional-alumnos.sh unsis
* En caso de despliegue de ADEUDOS:
sudo ./identidad-institucional-adeudos.sh uncos

16) Para Alumnos, Inscripciones, Financieros, Adeudos o Idiomas sólo en caso que no aparezcan los estilos
rake assets:clean RAILS_ENV=production
rake assets:clobber RAILS_ENV=production
rake assets:precompile RAILS_ENV=production 

17) Reiniciar Nginx.
sudo systemctl restart nginx

18) verificar los database.yml que apunten a la base de datos correcta.

