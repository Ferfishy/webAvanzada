# webAvanzada
##Pregunta 1 (2 pts). ¿Por qué no se recomienda desarrollar directamente sobre main en este laboratorio?

No se recomienda ya que podemos trabajar sobre una rama "separada" a través de pull request, para no hacer cambios directamente en el main.

##Pregunta 2 (2 pts). ¿Qué problema se evita al utilizar --skip-git al crear el proyecto Angular?

Se evita hacer un segundo repositorio dentro de la carpeta /frontend, dado que el repositorio principal ya fue inicializado.

##Pregunta 3 (2 pts): ¿Qué verifica npm run build en esta etapa del laboratorio?

Verifica que el proyecto de Angular se compile correctamente sin errores de forma local.

##Pregunta 4 (2 pts): ¿Qué utilidad tiene revisar git status o git diff --cached antes de realizar un commit?

Sirve para comprobar exactamente qué información, archivos modificados y cambios preparados serán versionados en el próximo commit. Esto previene subir por accidente archivos innecesarios, tokens, contraseñas o archivos que deberían estar ignorados.

##Pregunta 5 (2 pts): ¿Qué evento activa el workflow ci.yml?

Lo activa el evento pull_request cuando se intenta integrar código hacia la rama main.

##Pregunta 6 (2 pts): En runs-on: ubuntu-latest, ¿qué representa ubuntu-latest?

Representa el entorno de ejecución (el sistema operativo del "runner" o servidor proporcionado por GitHub) donde se ejecutarán todas las tareas del trabajo (job).

##Pregunta 7 (2 pts): Ordene las etapas de validación que ejecuta el job frontend y explique por qué npm ci se ejecuta antes que las pruebas.

El orden es: 1. Obtener código, 2. Configurar Node.js, 3. Instalar dependencias (npm ci), 4. Ejecutar pruebas, 5. Construir Angular. npm ci se ejecuta antes porque las pruebas dependen de las librerías y herramientas (como Jasmine/Karma o Jest) que se descargan e instalan en la carpeta node_modules durante este paso.  

##Pregunta 8 (3 pts): Después del push, indique qué etapa del pipeline falla y qué ocurre con las etapas siguientes.

Falla la etapa "Ejecutar pruebas" (npm test). Como consecuencia, el pipeline se detiene y las etapas siguientes (como "Construir Angular") no se ejecutan (se cancelan u omiten).  

##Pregunta 9 (3 pts): ¿Debería integrarse este Pull Request a main mientras el pipeline está fallando? Justifique.

No debería integrarse, porque el fallo indica que hay un error en el código (las pruebas no pasan). Integrarlo rompería la rama main, lo cual va en contra del principio de la Integración Continua, que busca mantener la rama principal siempre estable y funcional.

##Pregunta 10 (4 pts): Clasifique cada elemento como "versionable", "variable/configuración" o "secreto/no versionable": package.json, API_URL pública, AWS_REGION, DB_PASSWORD, API_TOKEN, terraform.tfstate.  

package.json: versionableAPI_URL pública: variable/configuraciónAWS_REGION: variable/configuraciónDB_PASSWORD: secreto/no versionableAPI_TOKEN: secreto/no versionableterraform.tfstate: secreto/no versionable

##Pregunta 11 (2 pts): ¿Por qué una contraseña o token no debe escribirse directamente dentro de ci.yml, cd.yml o un archivo TypeScript del frontend? 

Porque esos archivos quedan guardados en el historial de Git y son visibles para cualquier persona con acceso al repositorio. Si el código fuente se expone, las credenciales también lo harán.

##Pregunta 12 (2 pts): Si un secreto real fue incluido en un commit y luego se agrega su archivo a .gitignore, ¿queda solucionado el problema? Explique qué acción adicional debe realizarse.

No queda solucionado, porque Git conserva el historial completo y el secreto seguirá visible en los commits anteriores. La acción indispensable es revocar (invalidar) el secreto inmediatamente en el servicio proveedor (AWS, base de datos, etc.) y generar uno nuevo.

##Pregunta 13 (2 pts): ¿Qué diferencia existe entre terraform validate, terraform plan y terraform apply?

terraform validate verifica que la sintaxis y estructura del código sean correctas sin conectarse a la nube. terraform plan muestra un borrador de los cambios exactos que Terraform va a realizar (qué recursos creará, modificará o eliminará), pero sin hacerlos efectivos. terraform apply es el comando que ejecuta y aplica esos cambios reales en la infraestructura. 

##Pregunta 14 (2 pts): ¿Por qué ci.yml se activa con pull_request y cd.yml se activa con push sobre main?

ci.yml usa pull_request porque su objetivo es probar el código antes de que se integre a la rama principal, previniendo errores. cd.yml usa push sobre main porque el Despliegue Continuo (CD) solo debe ejecutarse cuando el código ya fue validado, aprobado e integrado definitivamente en la rama principal.

##Pregunta 15 (2 pts): ¿Qué función cumple Terraform dentro de este flujo de CD? 

Cumple la función de automatizar el aprovisionamiento y preparación del entorno de "staging". En este caso, ejecuta las instrucciones para crear la carpeta de destino y copiar los archivos compilados de Angular hacia ella, gestionando la infraestructura como código.

##Pregunta 16 (2 pts): ¿Por qué el workflow usa ${{ secrets.DEMO_TOKEN }} en lugar de escribir el valor directamente?

Porque permite inyectar el secreto de forma segura y dinámica durante la ejecución, sin tener que escribirlo en texto plano en el código fuente. Esto evita exponer credenciales en el repositorio de Git. 
