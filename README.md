Resumen del Contrato SupplyChainTracker
Propósito: Gestionar una cadena de suministro descentralizada con roles definidos (Productor → Fábrica → Minorista → Consumidor) y transferencia de tokens que representan productos.

🔹 Componentes Clave
Roles y Estados:

Roles: Producer, Factory, Retailer, Consumer (codificados como bytes32).
Estados:
Usuarios: Pending, Approved, Rejected, Canceled.
Transferencias: Pending, Accepted, Rejected.
Estructuras de Datos:

Token: Representa un producto (ID, nombre, cantidad, características, balances por usuario).
Transfer: Registra transferencias entre usuarios (ID, remitente, destinatario, cantidad, estado).
User: Almacena información de usuarios (ID, dirección, rol, estado).
Funcionalidades Principales:

Gestión de Usuarios:
requestUserRole: Solicita un rol (ej: keccak256("Producer")).
changeStatusUser: El admin aprueba/rechaza usuarios (solo él puede hacerlo).
Tokens:
createToken: Crea un token (solo usuarios aprobados).
getToken: Consulta detalles de un token (incluye balance del usuario que consulta).
Transferencias:
transfer: Inicia una transferencia (valida roles y balances).
acceptTransfer/rejectTransfer: El destinatario acepta/rechaza la transferencia.
Consultas:
getUserTokens: Lista tokens asociados a un usuario.
getUserTransfers: Lista transferencias de un usuario.
Seguridad:

ReentrancyGuard: Protege contra ataques de reentrada en transfer y rejectTransfer.
Validaciones:
Roles secuenciales (ej: solo Producer → Factory).
Balances suficientes antes de transferir.
El admin es inmutable (asignado en el constructor).
Eventos:

TokenCreated, TransferRequested, UserRoleRequested, etc. (para seguimiento off-chain).
🔹 Flujo de Trabajo
Registro: Un usuario solicita un rol (ej: Producer).
Aprobación: El admin aprueba el usuario (UserStatus.Approved).
Creación: El usuario aprobado crea un token (ej: "Manzanas", 100 unidades).
Transferencia:
El remitente llama a transfer(to, tokenId, amount).
El destinatario acepta con acceptTransfer(transferId) (o rechaza).
Auditoría: Los eventos permiten rastrear todas las acciones.
🔹 Ejemplo Rápido (Remix IDE)
SOLIDITY
// 1. Solicitar rol como Productor (desde otra cuenta):
requestUserRole(keccak256("Producer"));

// 2. Admin aprueba al usuario:
changeStatusUser(0xUsuario, 1); // 1 = UserStatus.Approved

// 3. Productor crea un token:
createToken("Manzanas", 100, "Orgánicas", 0);

// 4. Transfiere a una Fábrica (previamente registrada):
transfer(0xFabrica, 1, 50);

// 5. Fábrica acepta la transferencia:
acceptTransfer(1); // ID de la transferencia
🔹 Dependencias
OpenZeppelin: ReentrancyGuard (para seguridad).
Solidity: ^0.8.20 (protección nativa contra overflows).

