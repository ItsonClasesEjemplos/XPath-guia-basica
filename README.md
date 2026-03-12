# XPath-guia-basica
Guia basica de XPath para automatizar con Selenium

# XPath para Automatización con Selenium

## ¿Qué es XPath?

**XPath (XML Path Language)** es un lenguaje que permite **localizar elementos dentro de documentos XML o HTML**.

En automatización con Selenium se usa para **encontrar elementos en el DOM (Document Object Model)** cuando no existen identificadores simples como `id` o `name`.

Ejemplo de uso en Selenium:

```python
driver.find_element(By.XPATH, "//input[@name='username']")
```

---

# ¿Qué es el DOM?

El **DOM (Document Object Model)** es la representación estructurada de una página web.  
El navegador convierte el HTML en un **árbol de nodos** que puede ser recorrido por herramientas como Selenium.

Ejemplo de HTML:

```html
<html>
  <body>
    <div>
      <input id="username">
      <button>Login</button>
    </div>
  </body>
</html>
```

Este HTML se interpreta como un árbol:

```
html
 └── body
      └── div
           ├── input
           └── button
```

XPath permite **navegar este árbol para localizar elementos**.

---

# Tipos de XPath

## XPath Absoluto

El XPath absoluto define la **ruta completa desde la raíz del documento**.

Ejemplo:

```xpath
/html/body/div/input
```

Problema:

- Es muy **frágil**
- Si cambia la estructura del HTML, el selector deja de funcionar

Por esta razón **no se recomienda en automatización**.

---

## XPath Relativo (Recomendado)

El XPath relativo comienza desde **cualquier punto del documento**.

Ejemplo:

```xpath
//input[@id='username']
```

Significado:

- `//` buscar en cualquier parte del documento
- `input` tipo de elemento
- `@id='username'` atributo del elemento

Este es el tipo de XPath que se usa normalmente en Selenium.

---

# Estructura básica de XPath

La estructura general es:

```
 //tag[@atributo='valor']
```

Ejemplo:

```xpath
//input[@name='username']
```

Uso en Selenium:

```python
driver.find_element(By.XPATH, "//input[@name='username']")
```

---

# Seleccionar elementos por atributo

HTML:

```html
<input type="text" name="username">
```

XPath:

```xpath
//input[@name='username']
```

Selenium:

```python
driver.find_element(By.XPATH, "//input[@name='username']")
```

---

# Seleccionar elementos por texto

HTML:

```html
<button>Login</button>
```

XPath:

```xpath
//button[text()='Login']
```

Selenium:

```python
driver.find_element(By.XPATH, "//button[text()='Login']")
```

---

# Seleccionar por texto parcial

Cuando el texto puede variar se usa `contains()`.

XPath:

```xpath
//button[contains(text(),'Log')]
```

Esto significa:

```
buscar botón cuyo texto contenga "Log"
```

Ejemplo en Selenium:

```python
driver.find_element(By.XPATH, "//button[contains(text(),'Log')]")
```

---

# Seleccionar por clase

HTML:

```html
<button class="btn btn-primary">
```

XPath:

```xpath
//button[contains(@class,'btn-primary')]
```

Selenium:

```python
driver.find_element(By.XPATH, "//button[contains(@class,'btn-primary')]")
```

---

# Funciones útiles de XPath

## contains()

Busca coincidencias parciales en atributos o texto.

```xpath
//a[contains(@href,'login')]
```

Significado:

```
buscar enlace cuyo atributo href contenga "login"
```

---

## starts-with()

Busca atributos que comiencen con cierto valor.

```xpath
//input[starts-with(@id,'user')]
```

---

## normalize-space()

Elimina espacios innecesarios en el texto.

```xpath
//button[normalize-space()='Add']
```

---

# Navegación en el DOM con XPath

XPath permite navegar entre elementos del DOM.

## Seleccionar hijo

```xpath
//div/span
```

Selecciona un `span` dentro de un `div`.

---

## Seleccionar padre

```xpath
//span/..
```

Obtiene el elemento padre del `span`.

---

## Seleccionar ancestro

```xpath
//span/ancestor::div
```

Selecciona un `div` ancestro del `span`.

---

# Uso de índices

Cuando existen múltiples elementos iguales.

HTML:

```html
<ul>
  <li>Item1</li>
  <li>Item2</li>
</ul>
```

XPath:

```xpath
//li[1]
```

Selecciona el primer elemento.

```xpath
//li[2]
```

Selecciona el segundo elemento.

---

# Operadores lógicos

XPath permite combinar condiciones.

## AND

```xpath
//input[@type='text' and @name='username']
```

Significado:

```
input que tenga type="text" y name="username"
```

---

## OR

```xpath
//input[@name='username' or @name='email']
```

---

# Buscar cualquier tipo de elemento

El símbolo `*` representa cualquier etiqueta.

```xpath
//*[@id='username']
```

Significa:

```
cualquier elemento que tenga id="username"
```

---

# Ejemplos completos en Selenium

## Escribir en un input

```python
driver.find_element(
    By.XPATH,
    "//input[@name='username']"
).send_keys("Admin")
```

---

## Hacer clic en un botón

```python
driver.find_element(
    By.XPATH,
    "//button[contains(text(),'Login')]"
).click()
```

---

## Esperar un elemento antes de interactuar

En automatización es recomendable esperar a que el elemento esté disponible.

```python
from selenium.webdriver.support.ui import WebDriverWait
from selenium.webdriver.support import expected_conditions as EC

button = WebDriverWait(driver,10).until(
    EC.element_to_be_clickable(
        (By.XPATH,"//button[contains(text(),'Add')]")
    )
)

button.click()
```

---

# Buenas prácticas al usar XPath

Orden recomendado para seleccionar elementos en Selenium:

```
1. ID
2. NAME
3. CSS Selector
4. XPath
```

Ejemplo ideal:

```python
driver.find_element(By.ID,"username")
```

XPath se usa cuando **no existen selectores más simples o estables**.

---

# Errores comunes

## Usar XPath absoluto

Incorrecto:

```xpath
/html/body/div[2]/form/input
```

Correcto:

```xpath
//input[@name='username']
```

---

## Usar clases dinámicas

Muchos frameworks generan clases dinámicas.

Incorrecto:

```xpath
//div[@class='abc123']
```

Correcto:

```xpath
//div[contains(@class,'menu')]
```

---

# Ejemplo completo de login automatizado

```python
driver.get("https://example.com/login")

driver.find_element(
    By.XPATH,
    "//input[@name='username']"
).send_keys("Admin")

driver.find_element(
    By.XPATH,
    "//input[@name='password']"
).send_keys("admin123")

driver.find_element(
    By.XPATH,
    "//button[contains(text(),'Login')]"
).click()
```

---

# Conclusión

XPath es una herramienta poderosa para localizar elementos en el DOM cuando:

- No existe `id`
- Los selectores CSS no son suficientes
- Se necesita buscar por texto
- Se requiere navegar por la estructura del documento

Usado correctamente, XPath permite automatizar interfaces web complejas de forma flexible y confiable.
