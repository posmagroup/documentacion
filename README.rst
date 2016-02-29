Proyecto de Documentación de Posma Group
=========================================

.. image:: https://readthedocs.org/projects/posmagroup/badge/?version=latest
   :target: http://posmagroup.readthedocs.org/es/latest/?badge=latest
   :alt: Documentation Status

Instalación
-----------

El proyecto usa la herramienta `Sphinx`_ para la generación de documentación.
Esta documentación se escribe con el formato de texto `reStructuredText`_. 

Sphinx es una herramienta basada en Python, para la instalación del proyecto y
todos sus requerimientos se incluye un archivo de requerimientos:

.. code:: bash

     $ pip install -f requirements.pip

Para instalar `Sphinx`_ se recomienda seguir las instrucciones en la
`documentación oficial`_.


Agregar contenido
-----------------

Para agregar o modificar contenido, deben editarse los documentos `*.rst` que se
encuentran dentro de la estructura de directorios disponible en **source**.

Generar la documentación
------------------------

Para leer la documentación, es necesario compilar los documentos, esto se hace
automáticamente cada vez que se actualiza la rama `master` de este repositorio,
y la documentación estará disponible en `Read The Docs`_.

Para generar formatos adicionales, o tener una copia local de la documentación,
se utiliza el siguiente comando.

.. code:: bash
    
    $ make html

**NOTA**: `html` puede ser cambiado por `pdf`, `laTex`, o cualquiera de los
formatos disponibles. Para conocer la lista de parámetros, basta con hacer la
llamada a **make** sin parámetros.


.. _`Sphinx`: sphinx-doc.org
.. _`documentación oficial`: http://www.sphinx-doc.org/en/stable/tutorial.html#install-sphinx
.. _`reStructuredText`: http://docutils.sourceforge.net/docs/ref/rst/restructuredtext.html
