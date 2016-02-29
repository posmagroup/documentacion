git
===

El sistema de de `control de versiones` utilizado en *Posma Group* es `git`_.

Por agilidad, se recomienda generar y agregar tu `clave pública ssh` tanto en tu
equipo, como en tu configuración de usuario del repositorio remoto.


Comando básicos
---------------

En esta sección se hará una pequeña reseña sobre lo mínimo que se debe saber
para empezar a trabajar en el equipo de *Posma Group*, puedes ver la sección
de :ref:`referencias-git` para conocer más a fondo estos comandos.

git init
++++++++
Inicializará el directorio actual como un nuevo repositorio `git`.

.. code:: bash

    $ git init

git clone
+++++++++
Comando que se usará para descargar una copia completa del repositorio del
proyecto.

.. code:: bash

    $ git clone <dirección del repositorio>.git

git status
++++++++++
Mostrará el estado actual del repositorio local, mostrando archivos nuevos,
cambiados, o eliminados.

.. code:: bash

    $ git status

git add
++++++++
Agregará el archivo modificado al siguiente commit.

.. code:: bash

    $ git add <nombre del archivo modificado>

git commit
++++++++++
Creará un nuevo `commit` en el repositorio, persistiendo los cambios
realizados hasta el momento.

Se recomienda realizar un `commit` por cada cambio que se esté desarrollando
de este modo será fácil deshacer nada más los cambios que introduzcan problemas.

.. code:: bash

    $ git commit

git push
++++++++
Enviará los commits realizados al repositorio remoto y rama indicados en el comando.

.. code:: bash

    $ git push origin <branch>

git pull
++++++++
Descargará los últimos cambios de la rama indicada en el comando a la rama local actual.

.. code:: bash

    $ git pull origin <branch>

git checkout
++++++++++++
Este comando permite al usuario cambiar de una rama a otra, o de una versión a otra. 

.. code:: bash

    $ git checkout <branch>

git revert
++++++++++
Dado uno o más commits existentes, este comando deshace los cambios agregados por
esos commits, generando un nuevo commit para guardarlo.

.. code:: bash

    $ git revert <commit hash>

git merge
+++++++++
Se usará para mezclar cambios de distintas. Introduce los cambios de `<branch>`
a la rama actual.

.. code:: bash

    $ git merge <branch>


Para mayor información de estos comandos se recomienda revisar la
`documentación oficial de git`_ así como también los tutoriales disponibles en
la sección de :ref:`referencias-git`.


Manejo de ramas
---------------

El flujo de trabajo dentro de *Posma Group* define el siguiente manejo de ramas
basado en la estrategia de ramas publicada por `Vincent Driessen` en 
`A successful Git branching model`_ (en inglés). 

En este flujo de trabajo, se definen dos ramas principales, cuyo tiempo de vida
abarca la duración del proyecto.

    - master
    - develop

El código disponible en **origin/master** siempre será considerado listo para
producción.

El código disponible en **origin/develop** siempre tendrá los últimos cambios
entregados para el próximo `release`. Cualquier proceso de `Integración Continua`
tomará los cambios de esta rama. Es el equivalente a una rama de `integracion`.

En el momento en que el código `develop` sea considerado estable, este será
integrado en la rama `master` y ahí se etiquetará como listo para un nuevo release.

Ramas adicionales
+++++++++++++++++

Adicionalmente a las ramas `master` y `develop` este modelo plantea el uso de una
serie de ramas de soporte para permitir el desarrollo colaborativo entre varios
miembros del equipo.

Ramas de `feature`
..................

Son las ramas de características de producto. Debe crearse una rama `feature` por
cada característica de producto a desarrollar.

- Los cambios introducidos por esta rama solamente deben ser integrados a **develop**.
- Todos los cambios en **develop** deben ser mezclados en esta rama.
- La convención para el nombramiento es `feature-*`, donde `*` es el nombre del feature que está siendo desarrollado.

Una vez mezclada una rama de `feature` en `develop`, esta se puede eliminar.


Ramas de `release`
..................

Son las ramas de estabilización del próximo entregable. Debe crearse una rama
`release` por cada nueva versión del producto. Esta rama será la próxima versión
estable del código.

- Los cambios en **develop** deben ser introducidos en esta rama.
- Los cambios introducidos por esta rama deben ser integrados a **develop** y **master**.
- La convención para el nombramiento es `release-*`, donde `*` es el código del release (puede ser la fecha o la versión).

Una vez mezclada una rama de `release` en `develop` y `master`, esta se puede eliminar.


Ramas de `hotfix`
.................

Cuando se consigue un `bug` o problema en una versión estable, debe generarse una
rama para corregirlo.

- Los cambios en **develop** deben ser introducidos en esta rama.
- Los cambios introducidos por esta rama deben ser integrados a **develop** y **master**.
- La convención para el nombramiento es `fix-*`, donde `*` es el código del issue que se esstá corrigiendo.

Una vez mezclada una rama de `hotfix` en `develop` y `master`, esta se puede eliminar.


Ramas `locales`
...............

Adicionalmente a las ramas implementadas en este modelo, es recomendado que el
desarrollador cree la cantidad de ramas locales que considere necesaria para el
cumplimiento de sus objetivos, sin embargo, al finalizar el día, sus cambios deberán
verse reflejados en alguna rama de `feature`, `fix`, `release`.

El responsable de integrar los nuevos features desarrollados en `develop` y `master`
será el líder técnico/integrador de tu equipo. Para que tus cambios sean aprobados en
alguna de estas ramas, tu código debe haber pasado todas las pruebas.


.. image:: ../images/git-model.png

Para facilitar la implementación y uso local de este modelo de ramas, se recomienda
instalar la biblioteca de git `git-flow`_ (en inglés).

Existen otros modelos de desarrollo con git, sin embargo estos se dejan como ejercicio
al lector. Puedes leer acerca de ellos en el documento `Comparing Workflows`_ (en inglés).


.. _referencias-git:

Referencias
-----------

- `git`_: Referencia oficial del Sistema de Control de Versiones Distribuido git (en inglés).
- `A successful Git branching model`_: Propuesta original del modelo de manejo de ramas (en inglés).
- `git - La guía sencilla`_: Una guía sencilla para comenzar con git. sin complicaciones (en español)

  
.. _`git - La guía sencilla`: http://rogerdudler.github.io/git-guide/index.es.html
.. _`git`: http://git-scm.com/
.. _`documentación oficial de git`: http://git-scm.com/doc
.. _`git-flow`: http://jeffkreeftmeijer.com/2010/why-arent-you-using-git-flow/
.. _`A successful Git branching model`: http://nvie.com/posts/a-successful-git-branching-model/
.. _`Comparing Workflows`: https://www.atlassian.com/git/tutorials/comparing-workflows/
