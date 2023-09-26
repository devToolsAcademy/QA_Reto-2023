# Workshop Cypress

Bienvenido al Workshop de Cypress!!! Durante el taller exploraremos los conocimientos necesarios para construir pruebas automáticas de la interfaz gráfica (GUI) usando Cypress. Durante el taller exploraremos la configuración de un proyecto desde cero, prepararlo para un proceso de integración continua por medio de Travis CI, interactuar con diferentes componentes web y mucho más.

Para el desarrollo del taller usaremos [GitHub](https://github.com/) y [GitHub Flow](https://guides.github.com/introduction/flow/) para realizar la entrega de cada ejercicio práctico.

Ten en cuenta tener estudiados ciertos conceptos importantes (te dejamos unos enlaces :sunglasses:):

-   [Git](https://www.freecodecamp.org/news/learn-the-basics-of-git-in-under-10-minutes-da548267cc91/)
-   [JavaScript](https://javascript.info/)

**Tips de GitHub Flow:**

1. Para cada ejercicio crear una rama (Investiga: _gitflow naming conventions_ )
2. Crea un Pull Request por cada punto (**Recuerda las interacciones como comentarios en ingles**)
3. Después de que se recibe aprobación de cada punto se debe hacer merge de la rama, utilizando squash.
4. Antes de empezar un nuevo punto se debe hacer pull de main para asegurarnos que tenemos los últimos cambios del anterior punto.

## Tabla de contenidos

1. [Creación y configuración del repositorio](#1-creación-y-configuración-del-repositorio)
1. [Configuración inicial del proyecto](#2-configuración-inicial-del-proyecto)
1. [Instalación de Cypress](#3-instalación-de-cypress)
1. [Creando la primera prueba](#4-creando-la-primera-prueba)
1. [Configurando las pruebas con TypeScript](#5-configurando-las-pruebas-con-typescript)
1. [Análisis de código estático](#6-análisis-de-código-estático)
1. [Configurar Integración Continua (CI)](#7-configurar-integración-continua-ci)
1. [Flujos del reto](#8-flujos-del-reto)
1. [Page Object Model (POM)](#9-page-object-model-pom)
1. [Mejorando los reportes - Mochawesome](#10-mejorando-los-reportes---mochawesome)
1. [Filling form](#11-filling-form)
1. [Subiendo un archivo](#12-subiendo-un-archivo)
1. [Descargando un archivo](#13-descargando-un-archivo)
1. [Interactuando con IFrames](#14-interactuando-con-iframes)

## 1. Creación y configuración del repositorio

1. Crear un repositorio en GitHub con el nombre de **cypress-training** (previo requisito disponer de una cuenta en GitHub, no seleccione ninguna opción de inicialización de repositorio).

2. Crear localmente una carpeta con el nombre de **cypress-training** y luego sitúese dentro de la carpeta.

3. Crear el archivo **.gitignore** en la raíz del proyecto, luego ingrese a la página <https://www.toptal.com/developers/gitignore> y en el campo de texto digite su sistema operativo (ej: windows, osx, macos) y selecciónelo de la lista de autocompletar. Repita este paso para su entorno de desarrollo (ej:vscode, sublime, intellij, jetbrains), también agregue la palabra `node` y por ultimo `CypressIO`. Presione el botón "Create" para crear el archivo que contendrá una lista de carpetas y archivos de exclusión y copie su contenido dentro del archivo **.gitignore**.

4. A continuación realice el primer commit y suba los cambios a nuestro repositorio remoto de GitHub, digitando los siguientes comandos en tu consola favorita, cada línea es un comando distinto:

    ```bash
    echo "# cypress-training" >> README.md
    git init -b main
    git add README.md
    git add .gitignore
    git commit -m "first commit"
    git remote add origin https://github.com/<usuario>/cypress-training.git
    git push -u origin main
    ```

5. Proteger la rama `main` para que los pull request requieran revisión de otros desarrolladores y se compruebe el estado de nuestros test ("ok" :heavy_check_mark: o "fallaron" :x:) antes de hacer un merge a la rama.

    > Ir a Settings > Branches adicionamos una regla dando click en **add rule**. Escribimos `main` en el campo de **branch name pattern**. Una vez hecho eso, damos click en las siguientes opciones:
    > ![branch rules](https://github.com/devToolsAcademy/QA_Reto-2023/assets/100431019/30ee9104-aa26-41f0-ad78-0565a8a2e9f0)

6. Añadir como colaboradores a:
    - [abdulflorez](https://github.com/abdulflorez)
    - [DanielCalleCO](https://github.com/DanielCalleCO)

## 2. Configuración inicial del proyecto

1. Instalar la versión ultima versión LTS de Node.js.

2. Crear una nueva rama local ejecutando por consola `git checkout -b setup`.

3. Crear una carpeta en la raíz del proyecto llamada `.github` con un archivo llamado `CODEOWNERS` (sin extensión) con lo siguiente:

    ```bash
    * @abdulflorez @DanielCalleCO
    ```

4. Ejecutar en consola `npm init` y colocar la siguiente información:
   | Parámetro | Valor |
   | ------------------ | --------------------------------------------- |
   | **Name** | cypress-training |
   | **Version** | _[Por Defecto]_ |
   | **Description** | This is a Workshop about Cypress |
   | **Entry Point** | _[Por Defecto]_ |
   | **Test Command** | `cypress open` |
   | **Git Repository** | _[Por Defecto]_ |
   | **Keywords** | ui-testing, practice, cypress |
   | **Author** | _[Su nombre]_ <_[Su correo]_> (_[su GitHub]_) |
   | **License** | MIT |

5. Realizar un commit donde incluya los archivos creados con el mensaje “setup project configuration” y subir los cambios al repositorio:

    ```bash
    git add .
    git commit -m "setup project configuration"
    git push origin setup
    ```

6. Crear un pull request (PR), asignarle los revisores y esperar la aprobación o comentarios de mejora (en este caso deberá hacer los ajustes requeridos, subir los cambios y esperar nuevamente la aprobación) de los revisores. Si no sabe cómo realizarlo, le recomendamos leer el siguiente artículo [instrucciones](https://help.github.com/articles/creating-a-pull-request/).

7. Una vez hemos obtenido la aprobación de los revisores, realizar el merge a main seleccionando la opción “squash and merge” (squash te permite unir todos los commits en un solo, es más por un concepto de organización). Posteriormente, en su rama local "main" realice el pull para traer los cambios mergeados en el PR.

    ```bash
    git checkout main
    git pull
    ```

## 3. Instalación de Cypress

1. Ejecutar el siguiente comando:

    ```bash
    npm install cypress --save-dev
    ```

2. Esto instalará cypress dentro del **node_modules**. Para verificar la correcta instalación e iniciar la configuración para ver el demo de cypress, ejecutamos el siguiente comando:

    ```bash
    npm test
    ```

    - Si te aparece un mensaje de Windows Defender similar al de la imagen, selecciona todas las opciones y presiona el botón Allow Access.

        ![allow access](https://github.com/DanielCalleCO/cypress-training-doc/raw/main/media/allow-access.png)

    - Después, se abrirá una ventana dándote la bienvenida Cypress. Te dará a escoger entre dos opciones (E2E Testing y Component Testing). Selecciona E2E Testing.

        ![cypress install step 1](https://github.com/DanielCalleCO/cypress-training-doc/raw/main/media/cypress-install-step-1.png)

    - Luego, Cypress mostrará el contenido de 4 archivos que agregará a nuestro proyecto: `cypress.config.js`, `cypress\support\e2e.js`, `cypress\support\commands.js`, `cypress\fixtures\example.json`. Presiona el botón de continuar.

        ![cypress install step 2](https://github.com/DanielCalleCO/cypress-training-doc/raw/main/media/cypress-install-step-2.png)

    - A continuación, Cypress te pedira que selecciones uno de los navegadores soportados. Seleccionaras `Chrome`, y luego presionaras el botón "Start E2E Testing in Chrome".

        ![cypress install step 3](https://github.com/DanielCalleCO/cypress-training-doc/raw/main/media/cypress-install-step-3.png)

    - Después, Cypress abrirá una ventana de Chrome y te pedira que elijas entre dos opciones. Selecciona "Scaffold example specs". Esto generará varios archivos de prueba, con ejemplos acerca de cómo utilizar cypress.

        ![cypress install step 4](https://github.com/DanielCalleCO/cypress-training-doc/raw/main/media/cypress-install-step-4.png)

    - Por último, Cypress te mostrara todos los archivos que ha generado. Presiona el botón "Okay, I got it!"

3. Selecciona alguno de los archivos que cypress ha generado, para ejecutar las pruebas de ejemplo. Es aquí donde vemos cómo funciona la magia de cypress. Una vez finalice, cerramos la ventana de cypress.

4. Agregue a la sección de cypress de su archivo **.gitignore** las siguientes líneas para no subir las pruebas que hacen parte del demo:

    ```bash
    cypress/e2e/1-getting-started/*
    cypress/e2e/2-advanced-examples/*
    ```

5. Observar que se crea una carpeta llamada **cypress** con [la siguiente estructura](https://docs.cypress.io/guides/core-concepts/writing-and-organizing-tests.html#Folder-Structure)

6. Modificar el archivo `package.json` la propiedad `test` ubicada dentro de la sección de `scripts` para que quede de la siguiente manera:

    ```json
    "scripts": {
      "test": "cypress open",
      "test:open": "cypress open"
    },
    ```

7. Esto hará que el comando `test:open` ejecute la instrucción `cypress open`. Ejecuta el comando `npm run test:open` para verificar que el demo de cypress ahora se inicia con este comando.

8. Crear una rama y realizar un commit donde incluya los archivos creados y/o modificados en esta sección, con el mensaje “setup cypress configuration” y subir los cambios al repositorio

9. Crear un pull request (PR), asignarle los revisores y esperar la aprobación o comentarios de mejora

10. Una vez hemos obtenido la aprobación de los revisores, realizar el merge a main seleccionando la opción “squash and merge” (squash te permite unir todos los commits en un solo, es más por un concepto de organización). Posteriormente, en su rama local "main" realice el pull para traer los cambios mergeados en el PR.

## 4. Creando la primera prueba

Una vez hemos ejecutado las pruebas de ejemplo, eliminamos las carpetas que contienen ejemplos: `cypress/e2e/1-getting-started` y `cypress/e2e/2-advanced-examples`.

1. Creamos un archivo llamado `google.cy.js` dentro de la carpeta `/cypress/e2e/` con el siguiente contenido:

    ```javascript
    describe("This is my first cypress test", () => {
        it("should have a title", () => {
            cy.visit("https://www.google.com/");
            cy.title().should("eq", "Google");
        });
    });
    ```

2. Ejecutar el comando `npm run test:open` para correr la prueba (deberas seleccionar la opción "E2E Testing" y despues seleccionar nuevamente el navegador chrome y presionar el botón "Start E2E Testing in Chrome"). Una vez finalice y si todo está bien veremos que la prueba paso satisfactoriamente:

    ![google spec result browser](https://github.com/DanielCalleCO/cypress-training-doc/raw/main/media/google-spec-result.png)

3. Cree una nueva rama y envíe el Pull Request con estos cambios (incluya una captura de pantalla donde se evidencie que las pruebas están pasando). No olvide actualizar su rama `main` una vez el PR ha sido aprobado y se haya hecho el proceso de Squash and Merge.

## 5. Configurando las pruebas con TypeScript

1. Instalar las dependencias necesarias para la transpilación de nuestras pruebas escritas en TypeScript a JavaScript por medio de la instalación de la dependencia de TypeScript.

    ```bash
    npm install --save-dev typescript
    ```

2. Crear el archivo `tsconfig.json` en la raíz del proyecto y copiar dentro de este la siguiente configuración:

    ```json
    {
        "compilerOptions": {
            "target": "es5",
            "skipLibCheck": true,
            "strict": true,
            "lib": ["es5", "dom"],
            "types": ["cypress", "node"]
        },
        "include": ["cypress/**/*.ts", "cypress.config.ts"],
        "exclude": ["node_modules/"]
    }
    ```

3. Cambiar la extensión de nuestra prueba `cypress.config.js` por `cypress.config.ts` y reemplace el código existente por este:

    ```js
    import { defineConfig } from "cypress";

    export default defineConfig({
        // setupNodeEvents can be defined in either
        // the e2e or component configuration
        e2e: {
            setupNodeEvents(on, config) {
                // modify config values examples
                // config.defaultCommandTimeout = 10000

                // IMPORTANT return the updated config object
                return config;
            },
        },
    });
    ```

4. Cambiar la extensión de nuestra prueba los siguientes archivos

    - `commands.js` -> `commands.ts`
    - `e2e.js` -> `e2e.ts`

5. Cambiar la extensión de nuestra prueba `google.cy.js` por `google.cy.ts` y ejecutar el comando de pruebas para comprobar que la transpilación se ejecuta correctamente al correr las pruebas

    ```bash
    npm run test:open
    ```

6. Cree una nueva rama y envíe el Pull Request con estos cambios (incluya una captura de pantalla donde se evidencie que las pruebas están pasando). No olvide actualizar su rama `main` una vez el PR ha sido aprobado y se haya hecho el proceso de Squash and Merge.

## 6. Análisis de código estático

1. Para realizar el análisis de código estático usaremos la herramienta ESLint para validar un conjunto de reglas sobre el código de pruebas y mantener un estilo consistente. Para esto se debe instalar ESLint como dependencia de desarrollo, luego iniciar la configuración del linter y seguimos los pasos que aparecen en consola (ver respuestas sugeridas):

    ```bash
    npm install eslint --save-dev
    npx eslint --init
    ```
    <details>
    <summary><b><u>Mostrar configuración detallada eslint</u></b></summary>

    ```bash
    ? How would you like to use ESLint?
    Opción 3:   To check syntax, find problems, and enforce code style

    ? What type of modules does your project use? ...
    Opción 1: > JavaScript modules (import/export)

    ? Which framework does your project use? ...
    Opción 3: > None of these

    Does your project use TypeScript?
    / Yes

    ? Where does your code run?
    Opción 1: > Browser

    ? How would you like to define a style for your project? ...
    Opción 1: > Use a popular style guide

    ? Which style guide do you want to follow? ...
    Opción 1: > Standard-with-typescript

    ? What format do you want your config file to be in? ...
    Opción 1: > JavaScript

    ? Would you like to install them now?
    Yes

    ? Which package manager do you want to use?
    npm
    ```

    </details>

2. Instalar una extensión del linter para cypress que contiene reglas de estilo siguiendo las buenas prácticas que sugiere cypress:

    ```bash
    npm install eslint-plugin-cypress --save-dev
    ```

3. Luego agregar el plugin de cypress y las reglas en el archivo eslintrc.js

    ```javascript
    ...
        "plugins": [
          "@typescript-eslint",
          "cypress"
        ],
        "rules": {
          "quotes": ["error", "double"],
          "cypress/no-assigning-return-values": "error",
          "cypress/no-unnecessary-waiting": "error",
          "cypress/assertion-before-screenshot": "warn",
          "cypress/no-force": "warn",
          "no-unused-vars": "warn",
          "require-jsdoc": "warn",
          "max-len": [ "error", { "code": 120 } ]
        },
    ...
    ```

4. Posteriormente modificamos el script test:open en el "package.json" para ejecutar la verificación de código estático antes de correr las pruebas:

    ```json
    "scripts": {
        "test:open": "npm run lint && cypress open",
        "lint": "eslint ./cypress/e2e/**/*.ts",
        "lint:fix": "npm run lint -- --fix"
    },
    ```

5. Ejecutamos las pruebas por corriendo el comando test:open

    ```bash
    npm run test:open
    ```

    > **Nota:** En caso de tener errores, algunos de ellos son posible arreglarlos automáticamente añadiendo el argumento --fix, es decir, usamos `npm run lint -- --fix`.

6. Cree una nueva rama y envíe el Pull Request con estos cambios (incluya una captura de pantalla donde se evidencie que las pruebas están pasando). No olvide actualizar su rama `main` una vez el PR ha sido aprobado y se haya hecho el proceso de Squash and Merge.

## 7. Configurar Integración Continua (CI)

En esta sección se configura la integración continua por medio de GitHub Actions, lo cual nos permitirá correr nuestras pruebas en un servidor remoto y validar continuamente que los cambios que vamos a ingresar a nuestra aplicación no han afectado su funcionamiento correcto.

1. Inicialmente modificar el siguiente script en el package.json para ejecutar todas las pruebas de `cypress/e2e/` sin tener que levantar la interfaz gráfica del explorador. A esto le llamamos "headless mode":

    ```json
    "scripts": {
        ... scripts existentes, no borrar
        "test": "cypress run"
        ... scripts existentes, no borrar
      },
    ```

2. Para crear la configuración del workflow de GitHub actions, vamos a crear un archivo `main.yml` en el directorio `.github/workflows` que realice los siguientes `steps` cuando creamos o actualizamos un Pull Request:

    1. Obtener nuestro repositorio (A esto se le conoce como checkout)
    2. Preparar el workflow para usar Node.js v16
    3. Instalar dependencias
    4. Ejecutar el análisis de código estático
    5. Ejecutar las pruebas E2E que hemos construido

    _Nota_: Intenta construir tu workflow, pero si te bloqueas, abajo tienes una sección con una posible solución

    <details>
    <summary><b><u>Mostrar una solución</u></b></summary>

    ```yml
    name: Continuous integration

    on: [pull_request]

    jobs:
        cypress-run:
            runs-on: ubuntu-latest
            steps:
                - name: Checkout
                  uses: actions/checkout@v2

                - name: Setup
                  uses: actions/setup-node@v2
                  with:
                      node-version: 16
                      cache: "npm"

                - name: Install
                  run: npm ci

                - name: Code style
                  run: npm run lint

                - name: UI E2E Tests
                  uses: cypress-io/github-action@v4
                  with:
                      browser: chrome
                      headed: true
    ```

    _Nota_: El action de cypress ejecuta por default el comando `npm test`

      </details>

3. Crea el archivo `.nvmrc` y especifica la versión de Node.js que seas usar para la ejecución.

4. Debido a que cypress por default graba videos de la ejecución de las pruebas es útil desactivar esta funcionalidad para disminuir el tiempo de ejecución y el uso de recursos en el servidor del CI. Adicionalmente, desactivaremos temporalmente las capturas de pantalla debido a un [error](https://github.com/cypress-io/cypress/issues/5016) que aún no ha sido solucionado en las versiones recientes de cypress. Para esto se debe ingresar la siguiente configuración en el archivo `cypress.config.ts`

    ```js
    // Código existente no modificar
    e2e: {
      // solo necesitas agregar estas dos propiedades, no incluir este comentario
      video: false,
      screenshotOnRunFailure: false,
      setupNodeEvents(on, config) {
        // Código existente no modificar
      }
      // Código existente no modificar
    ```

5. Finalmente subir los cambios al repositorio y crear un Pull Request. Se ejecutarán las pruebas en el servidor que provee GitHub Actions y se mostrara los resultados de la ejecución en el PR.

## 8. Flujos del reto

En esta sección se realiza un flujo para cubrir 3 aspectos de calidad en la página: <https://www.demoblaze.com/index.html>, vamos a usar los CSS selector para interactuar con cada elemento del DOM.

:scroll:

1. Hacer Login en la página mediante "Sign in"

-   Puntos a validar:
    -   El Login es exitoso.
    -   El nombre de usuario es el correspondiente.

2. Añade al carrito al menos 1 teléfono, 1 laptop y 1 monitor y completa la compra.

-   Puntos a validar:
    -   Cuando se hace click en cada ítem de la tienda se muestra el ítem que corresponde con su título.
    -   En el carrito se muestra el total correcto de los precios.
    -   La compra es exitosa.

3. Enviar un mensaje a través de "Contact"

-   Puntos a validar:
    -   El envío fue exitoso.

Puedes ir un paso a la vez, primero crea el archivo `shopping-flow.cy.ts` y agrega el los selectores y las acciones necesarias con el "describe" de toda la vida y los pasos que sean necesarios, también podrías crear un archivo por cada Test.

Un ejemplo:

```js
describe("Logg in feature", () => {
    beforeEach(() => {
        cy.visit("https://www.demoblaze.com/index.html/");
        cy.get("selectorSignin").click();
        // pasos para loguearme
    });

    it("Validating successful login", () => {
        //Pasos para test 1
    });
});
```

En algunos pasos la red u otros factores externos a la prueba pueden afectar los tiempos de espera, así que en el archivo de configuración de cypress `cypress.config.ts` modifica la función `setupNodeEvents` de la siguiente forma:

```js
setupNodeEvents(on, config) {
  config.defaultCommandTimeout = 20000
  config.responseTimeout = 20000

  // IMPORTANT return the updated config object
  return config
}
```

Para finalizar sube tus cambios al repositorio y crea un PR.

## 9. Page Object Model (POM)

Page Object Model es un patrón para mejorar la mantenibilidad de las pruebas ya que podemos establecer una capa intermedia entre las pruebas y UI de la aplicación, ya que los cambios que requieran las pruebas debido a cambios en la aplicación se pueden realizar rápidamente en el POM. Te recomendamos investigar el patrón y otros patrones útiles que puedan ser usados para el código de pruebas.

1. Crea el folder `cypress/page/` donde almacenaras las vistas que requieras de tus casos para realizar tu estructura de POM `vistas.ts`, aquí un ejemplo:

```javascript
  class CategoriesPage {
      private phoneCategorie: string;
      private menuContentPageURL: string

      constructor() {
          this.phoneCategorie = "#block_top_menu > ul > li:nth-child(3) > a";
          this.menuContentPageURL = "https://www.demoblaze.com/index.html/"
      }

      public visitMenuContentPage(): void {
          cy.visit(this.menuContentPageURL)
      }

      public goToPhoneCategorie(): void {
          cy.get(this.phoneCategorie).click()
      }
  }

  export { CategoriesPage }
```

2. Posteriormente crear el archivo `cypress/page/index.ts` para usar como archivo de salida de todos los page object:

    ```javascript
    export { CategoriesPage } from ".categories.page";
    ```

3. Luego modificar el archivo `shopping-flow.cy.ts` para utilizar el POM que acabamos de crear en la prueba:

    ```javascript
    import { CategoriesPage, Home } from "../page/index";

    const homePage = new HomePage();
    const categoriesPage = new CategoriesPage();

    describe("Buy a t-shirt", () => {
        beforeEach(() => {
            homePage.visitCategoriesPage();
            categoriesPage.login("username", "password");
            // pasos para logearme
        });

        it("then should be bought a t-shirt", () => {
            categoriesPage.goToPhoneCategories();
            // El resto del flujo de la prueba....
        });
    });
    ```

4. Ejecute las pruebas y verifica que pasen. Si alguna falla modifícala usando los CSS locators y el tiempo de espera configurado hasta que pasen.

5. Cree un PR y solicite revisión del punto anterior.

## 10. Mejorando los reportes - Mochawesome

Algunas veces es bueno mejorar el reporte visual de la ejecución de nuestras pruebas, para eso agregaremos `mochawesome` y lo integraremos con cypress. Siga los siguientes pasos:

1. Instalaremos las siguientes dependencias:

    ```bash
    npm install mocha mochawesome cypress-mochawesome-reporter --save-dev

    # Para mantener el reporte actual (en la terminal) y agregar mochawesome
    npm install cypress-multi-reporters --save-dev

    # Para procesar los reportes generados al terminar la ejecución
    npm install mochawesome-merge --save-dev
    npm install mochawesome-report-generator --save-dev
    ```

2. Agregamos la siguiente configuración a la sección `e2e` del archivo `cypress.config.ts`:

    ```javascript
    reporter: "cypress-multi-reporters",
    reporterOptions: {
      reporterEnabled: "mochawesome",
      mochawesomeReporterOptions: {
        reportDir: "cypress/reports/mocha",
        quite: true,
        overwrite: false,
        html: false,
        json: true,
      },
    },
    ```

3. Agrega script en el `package.json` para limpiar la carpeta `cypress/reports`

    **tip:** Ten en cuenta que el servidor de CI corre en linux.

4. Agrega estos sripts para procesar el reporte generado al ejecutar las pruebas:

    ```json
    "combine:reports": "mochawesome-merge cypress/reports/mocha/*.json > cypress/reports/report.json",
    "generate:reports": "marge cypress/reports/report.json -f report -o cypress/reports",
    ```

5. Invetiga los hooks **pre** y **post** de npm para ejecutar scripts antes y después de las pruebas:

    - **pre:** Limpiar la carpeta de reportes
    - **post:** ejecutar los scripts para procesar el reporte generado por la ejecución de pruebas.

6. Sube el cambio con una foto del reporte generado por `mochawesome`, crea un PR y solicita la revisión.

## 11. Filling form

Usualmente en las aplicaciones nos encontramos formularios que los usuarios deben llenar para guardar información. En esta sección interactuaremos con algunos de los componentes más comunes que nos podemos encontrar. La prueba consiste en:

1. Visitar la página: [Formulario de pruebas automatización](https://demoqa.com/automation-practice-form)
2. Construir un método que llene el formulario y de click en el botón de **submit**:

    ```javascript
    const personalInformation = {
        name: "Holmes",
        lastName: "Salazar",
        email: "test@email.com",
        gender: "Male",
        dateOfBirth: "27 Jul 2016",
        mobileNumber: "3656589156",
        hobbies: ["Music", "Reading"],
        currentAddress: "Av siempreViva # 123",
    };
    personalFormPage.fillForm(personalInformation);
    ```

    **tip:** Recuerda crear un page object

    <details>
    <summary><b><u>Nota:</u></b> Si tienes problemas con la ejecución de las pruebas en esta página, te sale un mensaje de error de tipo "uncaught exception", click aquí para ver una solución.</summary>

    Agrega las siguientes líneas al final del archivo: `cypress/support/e2e.ts`

    ```javascript
    // Ignoring uncaught exceptions since errors from external apps should not stop de workshop
    Cypress.on("uncaught:exception", (err, runnable) => {
        // returning false here prevents Cypress from
        // failing the test
        return false;
    });
    ```

    </details>

3. Verifique el título del pop-up que aparece una vez se termina de diligenciar el formulario.

    **Nota:** La verificación anterior es sencilla de realizar, a continuación, opcionalmente, proponemos dos verificaciones adicionales:

    - **mini-challenge:** Agregue la interacción con el campo de State y City.
    - **Challenge:** En el modal, verifique que se muestra correctamente, la información que ingresó al enviar el formulario.

4. Verifique que las pruebas pasen, cree un PR y solicite la revisión.

## 12. Subiendo un archivo

Usualmente nos podemos encontrar con la necesidad de subir archivos por medio de nuestra aplicación web. Realizaremos los siguiente:

1. Instalar el plugin de cypress para subir archivos: [cypress-file-upload](https://www.npmjs.com/package/cypress-file-upload). Sigue las instrucciones de configuración.

2. Crea el archivo `upload.page.ts` que contenga tres métodos:

    - Visitar la página de pruebas de subida de archivos: [upload-demo-site](https://the-internet.herokuapp.com/upload)
    - Subir un archivo. Recibe como parámetro el nombre del archivo almacenado en la carpeta: `cypress/fixtures`
    - Obtener el elemento del título que contiene el nombre después de subir

3. Crea el archivo de pruebas `upload-download.cy.ts` y agrega una prueba para subir un archivo usando el page object creado anteriormente.

4. Verifica que las pruebas pasen, crea un PR y solicita revisión.

## 13. Descargando un archivo

Para esta sección descargaremos un archivo y verificaremos el contenido, realizaremos la siguiente prueba:

1. Construye la siguiente prueba en el archivo `upload-download.cy.ts`:

    - Visita la página: [download-demo-site](https://demoqa.com/upload-download)
    - Descarga la imagen a través del boton "Download".
    - Verifica que el archivo exista en la carpeta de descargas de Cypress.

2. Crea el archivo `download.page.ts` con los método necesarios para construir la prueba automatica.

3. Verifica que todas las pruebas pasen, además incluye los archivos que no se deben subir al `.gitignore`.

4. Crea un PR y solicita revisión.

## 14. Interactuando con IFrames

Los iframes son elementos HTML que nos podemos encontrar comunmente en aplicaciones web antiguas, pero es bueno saber cómo interactuar con ellos. En esta sección interactuaremos, navegaremos y verificaremos data dentro de un iframe.

1. Primero instalaremos el siguiente plugin de cypress: [Cypress Iframe](https://www.npmjs.com/package/cypress-iframe). Sigue las instrucciones del link.

2. Crea un page object `iframe.page.ts` que contenga los siguiente métodos:

    ```javascript
    visit(){
      // visit the test page: https://www.w3schools.com/html/html_iframe.asp
    }

    getFrameTitle(){
      // get the title of the page in the iframe
    }

    goToCssPageInFrame(){
      // navigate to the css page in the iframe
    }
    ```

3. Crea un archivo de pruebas llamado `iframe.cy.ts` y construye las siguientes pruebas:

    - Cuando un usuario navega a la página: [página iframe](https://www.w3schools.com/html/html_iframe.asp) se muestra un Iframe que tiene como título `HTML Tutorial`

    - **Optional:** Cuando un usuario navega a la página: [página iframe](https://www.w3schools.com/html/html_iframe.asp) se muestra un i-frame y cuando el usuario navega a la página de CSS al darle click en la barra de navegación, se carga la página de CSS dentro del IFrame con el título `CSS Tutorial`

    **Challenge:** algunas pruebas pueden ser inestables por diferentes factores como latencias. Implementa una estrategia de retry si encuentras alguna inestabilidad!

4. Verifica que las pruebas pasen, crea un PR y solicita la revisión.

    <details>
    <summary><b><u>Nota:</u></b> Si tienes problemas con la ejecución de las pruebas en esta página, te sale un mensaje de error de tipo "SecurityError: Blocked a frame with origin...", click aquí para ver una solución.</summary>

    Agrega las siguientes líneas en el método "setupNodeEvents" al final del archivo: `cypress.config.ts`

    ```javascript
    config.chromeWebSecurity = false;
    ```

    </details>

## Conclusión

Muchas gracias por haber participado, Esperamos que tengas nuevos conocimientos que impulsen tu carrera profesional.
Te invitamos a seguir aprendiendo, y te dejamos unos temas para que investigues y sigas estudiando:

-   Component testing UI
-   End-to-End Testing API
-   Component Testing API
-   Mutation Tests
-   Contract Tests

Sigue aprendiendo! :smile: :book:

## Challenges

Hay muchos temas que puedes seguir aprendiendo y herramientas que puedes incluir, estos son algunos de los Retos que te dejamos :smiley:

### Given - When - Then

Otro estilo de escritura de pruebas es el Given-When-Then, aqui te dejamos un blog para que inicies tu estudio de este estilo y un enlace a una librería para que lo lleves a la práctica:

-   [Blog - Martin Fowler](https://martinfowler.com/bliki/GivenWhenThen.html)
-   [Librería - Cucumber for cypress](https://www.npmjs.com/package/cypress-cucumber-preprocessor)

devTools 2023 - QA Team
