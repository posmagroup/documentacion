Producción
==========

Nuestro Sistema Operativo de preferencia es Debian en la versión *stable*. Adicionalmente, el software que instalaremos
en nuestros servidores dependerá del stack que se utilizó para el proyecto, pero siempre estará dentro de los siguientes:


Software de servidor
--------------------

**Servidor Web:** `nginx`_, en la versión disponible en la distribución.

Se debe usar una versión con el módulo uwsgi integrado. A partir de la versión
0.8.40 este módulo viene integrado por defecto. Los parámetros de configuración
de este módulo están disponibles en la `documentación oficial de nginx`_.

**Servidor de aplicaciones:** `uWSGI`_, la versión disponible en la distribución,
actualmente 1.0.3.

**Bases de Datos**: En Posma Group, preferimos `PostgreSQL`_. Si el proyecto requiere utilizar 
el stack de Microsoft, usaremos `MS SQL Server`_, en cualquier otro caso, PostgreSQL es la base de datos por defecto.





.. _`nginx`: http://wiki.nginx.org/Main
.. _`uWsgi`: http://projects.unbit.it/uwsgi/wiki
.. _`documentación oficial de nginx`: http://wiki.nginx.org/HttpUwsgiModule
.. _`PostgreSQL`: http://www.postgresql.org/
.. _`MS SQL Server`: http://www.microsoft.com/sqlserver/
