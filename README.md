# Maestro Test — ECommerce

Proyecto de pruebas end-to-end (E2E) para una aplicación Android de comercio
electrónico, con **[Maestro](https://maestro.mobile.dev/)** (framework de
automatización de UI para apps móviles).

## Estructura

```
maestro-test/
├── apps/
│   └── ECommerce.apk              # APK de la app bajo prueba
├── components/
│   └── 01_login.yaml              # Flujo reutilizable de inicio de sesión (POM)
├── test/
│   └── 02_flujo_de_pago.yaml      # Prueba E2E del flujo de compra
├── .github/workflows/
│   └── e2e.yml                    # Pipeline de CI (GitHub Actions)
└── .maestro/screenshots/          # Capturas generadas al ejecutar las pruebas
```

## Patrón utilizado

El proyecto aplica una variante de **Page Object Model (POM)** usando el
mecanismo de `runFlow` de Maestro:

- **`components/01_login.yaml`** — componente reutilizable que agrupa los pasos
  de autenticación (lanzar app, ingresar usuario/contraseña, validar catálogo).
  Recibe parámetros por variables de entorno (`APP_ID`, `USERNAME`, `PASSWORD`).
- **`test/02_flujo_de_pago.yaml`** — prueba de extremo a extremo que reutiliza el
  login mediante `runFlow` y luego ejecuta el recorrido de búsqueda, agregado al
  carrito y validación del pago.

De esta forma el login se escribe una sola vez y se comparte entre todas las
pruebas que requieran sesión.

## Prerrequisitos

- [Maestro CLI](https://maestro.mobile.dev/getting-started/installing-maestro)
- Android SDK y `adb`
- Un emulador o dispositivo físico con la app instalada

## Ejecución local

```bash
# Instalar la app
adb install apps/ECommerce.apk

# Correr todas las pruebas
maestro test test/

# Correr una prueba específica
maestro test test/02_flujo_de_pago.yaml
```

Para el flujo de login parametrizado:

```bash
APP_ID=com.ecommerce 
USERNAME=testuser 
PASSWORD=password123 
```

## Integración continua

`.github/workflows/e2e.yml` ejecuta las pruebas automáticamente en
`macos-latest` con un emulador de Android (API 33) en cada push/PR a `main`,
instalando Maestro vía su script oficial.


