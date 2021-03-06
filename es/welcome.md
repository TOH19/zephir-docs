# ¡Bienvenido!

Bienvenido a Zephir, un lenguaje de código abierto de alto nivel/dominio específico, diseñado para facilitar la creación y mantenimiento de extensiones para PHP con un enfoque de tipo y cuidado de memoria.

<a name='some-features'></a>

## Algunas características

Las principales características de Zephir son:

| Características       | Descripción                                             |
| --------------------- | ------------------------------------------------------- |
| Tipo de sistema       | dinámica/estática                                       |
| Ahorro de memoria     | punteros o gestión de memoria directa no está permitido |
| Modelo de compilación | adelantado                                              |
| Modelo de memoria     | recolección local de basura                             |

<a name='a-small-taste'></a>

## Una pequeña prueba

El siguiente código registra una clase con un método que filtra variables, regresando sus caracteres alfabéticos:

    namespace MyLibrary;
    
    /**
     * Filtro
     */
    class Filter
    {
        /**
         * Filtra una cadena de texto, retornando solo caracteres alfabéticos
         *
         * @param string str
         */
        public function alpha(string str)
        {
            char ch; string filtered = "";
    
            for ch in str {
               if (ch >= 'a' && ch <= 'z') || (ch >= 'A' && ch <= 'Z') {
                  let filtered .= ch;
               }
            }
    
            return filtered;
        }
    }
    

La clase puede ser utiliza desde PHP de la siguiente manera:

    <?php
    
    $filter = new MyLibrary\Filter();
    echo $filter->alpha("01ho#.la?/1"); // imprime hola