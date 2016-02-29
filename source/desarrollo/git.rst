git
===

El sistema de de `control de versiones` utilizado en *Posma Group* es `git`_.


Comandos básicos
-----------------



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
tomará los cambios de esta rama.


El responsable de integrar los nuevos features desarrollados en `develop` y `master`
será el líder técnico/integrador de tu equipo. Para que tus cambios sean aprobados en
alguna de estas ramas, tu código debe haber pasado todas las pruebas.




.. image:: ../images/git-model.png

Para facilitar la implementación y uso local de este modelo de ramas, se recomienda
instalar la biblioteca de git `git-flow`_ (en inglés).

Existen otros modelos de desarrollo con git, sin embargo estos se dejan como ejercicio
al lector. Puedes leer acerca de ellos en el documento `Comparing Workflows`_ (en inglés).



Referencias
-----------

- `git`_
- `A successful Git branching model`_


.. _`git`: http://git-scm.com/
.. _`git-flow`: http://jeffkreeftmeijer.com/2010/why-arent-you-using-git-flow/
.. _`A successful Git branching model`: http://nvie.com/posts/a-successful-git-branching-model/
.. _`Comparing Workflows`: https://www.atlassian.com/git/tutorials/comparing-workflows/
