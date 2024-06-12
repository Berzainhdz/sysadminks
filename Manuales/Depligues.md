# Manual Despliegue
    1. Descargar el proyecto archivo.tar.gz del repositorio gitlab y lo guardamos en la ruta deseada en nuestra máquina local.

    2. Nos posicionamos en la ruta donde está guardado el archivo archivo.tar.gz y lo enviamos al contenedor:   
        * scp archivo.tar.gz usuario@ip_contenedor:.

    3. Entramos al contenedor.
        * ssh usuario@ip_contenedor 

    4. Descomprimimos el archivo del proyecto (podemos hacerlo como superusuario si es necesario):
        * sudo tar -xzvf archivo.tar.gz

    5. Renonbramos el proyecto. 
        *mv archivo_descomprimido nombre-nuevo

    6. Mover el proyecto a la carpeta /var/www o /home/proyecto 
        * sudo mv nombre-nuevo /var/www 
        * sudo mv nombre-nuevo /home/proyecto

    7. Entramos al proyecto anterior para pasar los archivos necesarios que se muestran en el paso siguiente. 
        * cd /var/www/proyecto-anterior 
        * cd /home/proyecto/proyecto-anterior

    8. Copiar los archivos necesarios.
        Escolares: 
            * sudo cp Gemfile Gemfile.lock .ruby_version ../nombre-nuevo-test-version/ 
            * cd public 
                * sudo cp -r estancias_docs servicio_social_docs ../../nombre-nuevo-test-version/public/ 
            * cd ../config 
                * sudo cp database.yml ../../nombre-nuevo-test-version/config/ 
            * cd initializers/ 
                * sudo cp secret_token.rb ../../../nombre-nuevo-test-version/config/initializers/

        Los demás proyectos: 
            * sudo cp Gemfile Gemfile.lock ../nombre-nuevo-test-version/ 
            * cd config 
                * sudo cp database.yml secrets.yml ../../nombre-nuevo-test-version/config/ 
            * cd initializers/ 
                * sudo cp assets.rb ../../../nombre-nuevo-test-version/config/initializers/ 
            * cd ../environments 
                * sudo cp production.rb ../../../nombre-nuevo-test-version/config/environments/

    9. Regresar a donde estan todos los proyectos.
        * cd /var/www
        * cd /home/proyecto

    10. Darle los permisos correspondientes a todos los archivos del proyecto. 
        * sudo chmod -R 755 nombre-nuevo-test-version 
        * sudo chown -R usuario:grupo nombre-nuevo-test-version

    11. Eliminar enlace simbólico. 
        * sudo unlink nombre-proyecto-test

    11. Crear el nuevo enlace simbólico con la nueva versión a desplegar. 
        * sudo ln -s nombre-nuevo-test-version nombre-proyecto-test

    12. Entramos al enlace simbolico creado.
    
    13. Ejecutamos (bundle install)

    14. Existen tareas paraa restablecer las contraseñas de admin en escolares e alumnos.
        * Escolares:
            * bundle exec rake nes:reset_admin RAILS_ENV=production
        * Alumnos:
            * bundle exec rake users:reset_password_admin RAILS_ENV=production
    
    15. Ejecutamos las identidades instucionales:
        * Escolares: sudo ./identidad-institucional-nes.sh Universidad
        * Inscripciones: sudo ./identidad-institucional-inscripciones.sh Universidad
        * Financieros: sudo ./identidad-institucional-financieros.sh Universidad
        * Alumnos: sudo ./identidad-institucional-alumnos.sh Universidad
        * Adeudos: sudo ./identidad-institucional-adeudos.sh Universidad

     16. Ojo para Alumnos, Inscripciones, Financieros, Adeudos o Idiomas sólo en caso que no aparezcan los estilos solo ejecutar
        * rake assets:clean RAILS_ENV=production o rake assets:clean RAILS_ENV=development
        * rake assets:clobber RAILS_ENV=production o rake assets:clobber RAILS_ENV=development
        * rake assets:precompile RAILS_ENV=production o rake assets:precompile RAILS_ENV=development

    17. Reiniciar Nginx. 
        * sudo systemctl restart nginx
