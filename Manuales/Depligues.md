#Manual Despliegue
1)Descargar el proyecto .tar.gz de gitlab y guardarlo en cualquier ruta.

Posicionandonos en la ruta donde esta guardado y enviar el archivo al contenedor. scp archivo.tar.gz usuario@ip_contenedor:.

Entrar al contenedor. ssh usuario@ip_contenedor

Descomprimir el proyecto. sudo tar -xzvf archivo.tar.gz

Renorbrar el proyecto. mv archivo_descomprimido nombre_nuevo

Mover el proyecto a la carpeta /var/www o /home/proyecto sudo mv nombre_nuevo /var/www sudo mv nombre_nuevo /home/proyecto

Entrar al proyecto anterior cd /var/www/proyecto-anterior o cd /home/proeycto/proyecto-anterior

Copiar los archivos necesarios.

Escolares: sudo cp Gemfile Gemfile.lock config.ru ../nes-escolares-test-version/ cd public sudo cp -r estancias_docs servicio_social_docs ../../nes-escolares-test-version/public/ cd ../config sudo cp database.yml ../../nes-escolares-test-version/config/ cd initializers/ sudo cp secret_token.rb ../../../nes-escolares-test-version/config/initializers/

Los demás proyectos: sudo cp Gemfile Gemfile.lock ../nes-alumnos-version/ cd config sudo cp database.yml secrets.yml ../../nes-alumnos-version/config/ cd initializers/ sudo cp assets.rb ../../../nes-alumnos-version/config/initializers/ cd ../environments sudo cp production.rb ../../../nes-alumnos-version/config/environments/

Regresar a donde estan todos los proyectos cd /var/www o /home/proyecto

Darle los permisos correspondientes a todos los archivos del proyecto. sudo chmod -R 755 nes-sistema-test-version sudo chown -R escolares:escolares nes-sistema-test-version

Eliminar enlace simbólico. sudo unlink nes-escolares-test

Crear el nuevo enlace simbólico con la nueva versión a desplegar. sudo ln -s nes-sistema-test-version nes-sistema-test

13)Ejecutamos bundle install

cd nes-alumnos-suneo-4-3.0.23-alpha.1

En caso de despliegue de ESCOLARES: sudo ./identidad-institucional-nes.sh utm
En caso de despliegue de FINANCIEROS: sudo ./identidad-institucional-financieros.sh utm
En caso de despliegue de ALUMNOS: sudo ./identidad-institucional-alumnos.sh unsis
En caso de despliegue de ADEUDOS: sudo ./identidad-institucional-adeudos.sh uncos
Para Alumnos, Inscripciones, Financieros, Adeudos o Idiomas sólo en caso que no aparezcan los estilos rake assets:clean RAILS_ENV=production rake assets:clobber RAILS_ENV=production rake assets:precompile RAILS_ENV=production

Reiniciar Nginx. sudo systemctl restart nginx

verificar los database.yml que apunten a la base de datos correcta.