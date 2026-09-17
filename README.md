# TGO (Transfer & Go) - Gestión de Transfers y Tours 🚍🏔️

## 📌 Descripción del Proyecto
**TGO (Transfer & Go)** es una solución móvil Android nativa desarrollada para conductores y guías turísticos en Chile[cite: 1, 2, 3]. La plataforma elimina la comunicación informal por llamadas y chats, centralizando la recepción de asignaciones, confirmaciones, registro de ejecuciones (*Check-in/out*) y la gestión de disponibilidad en terreno en un único sistema ordenado[cite: 2, 3].

---

## 🎨 Identidad Visual

* **Logotipo:** `docs/diseno/logo.png`
* **Paleta de Colores (Material Design 3):**
  * **Principal:** `#0B5B3B` (Verde Esmeralda)
  * **Secundario:** `#C0C0C0` (Plateado Metálico)
  * **Fondo:** `#F4F7F5` (Neutro Claro)
  * **Texto:** `#121E17` (Verde Oscuro On-Surface)
  * **Éxito:** `#1E8E5A` (Confirmado / Check-in)
  * **Alerta:** `#C62828` (Rechazado / Taxis no autorizados)

---

## 🔄 Flujo de Usuario (Diagrama de Actividad UML)

```mermaid
stateDiagram-v2
    [*] --> PantallaLogin: Abrir App

    state PantallaLogin {
        [*] --> IngresarCredenciales
        IngresarCredenciales --> ValidarUsuario: Presionar Iniciar Sesión
    }

    ValidarUsuario --> ErrorLogin: Credenciales inválidas
    ErrorLogin --> IngresarCredenciales: Reintentar
    ValidarUsuario --> HomeScreen: Credenciales válidas

    state HomeScreen {
        [*] --> VerListaServicios
        VerListaServicios --> FiltrarServicios: Aplicar filtro
        VerListaServicios --> SeleccionarServicio: Tap en tarjeta
    }

    SeleccionarServicio --> DetalleServicio

    state DetalleServicio {
        [*] --> VerInformacion
        VerInformacion --> EvaluarAsignacion
    }

    state "Aceptar / Rechazar" as DecisionAsignacion <<choice>>
    EvaluarAsignacion --> DecisionAsignacion

    DecisionAsignacion --> RechazarServicio: Rechazar
    RechazarServicio --> HomeScreen: Notifica a Coordinación

    DecisionAsignacion --> ConfirmarServicio: Confirmar
    ConfirmarServicio --> RegistroEjecucion: En fecha del servicio

    state RegistroEjecucion {
        [*] --> IniciarViaje
        IniciarViaje --> CheckIn: Marcar Check-in
        CheckIn --> EnTrayecto: Registra Hora/Punto
        EnTrayecto --> CheckOut: Marcar Check-out
        CheckOut --> ServicioFinalizado: Guarda Observaciones
    }

    ServicioFinalizado --> HomeScreen: Servicio completado

    HomeScreen --> ModuloDisponibilidad: Navegar a Calendario

    state ModuloDisponibilidad {
        [*] --> VerCalendario
        VerCalendario --> SeleccionarFecha
    }

    state "Validar 1 semana de anticipación" as DecisionAnticipacion <<choice>>
    SeleccionarFecha --> DecisionAnticipacion

    DecisionAnticipacion --> BloqueoRechazado: Menos de 7 días
    BloqueoRechazado --> VerCalendario: Muestra alerta

    DecisionAnticipacion --> BloqueoExitoso: 7 o más días
    BloqueoExitoso --> VerCalendario: Marca día inhabilitado

    HomeScreen --> [*]: Cerrar Sesión