# Dojo Coding Cairo AI Class
Ejemplo de como hacer una app completa en Starknet, con contratos y cliente.

## 📋 Tabla de Contenidos
1. [Descripción del Proyecto](#descripción-del-proyecto)
2. [Estructura del Proyecto](#estructura-del-proyecto)
3. [Configuración del Entorno](#configuración-del-entorno)
4. [Deploy de Contratos con Sncast](#deploy-de-contratos-con-sncast)

## 📝 Descripción del Proyecto

Este proyecto es un ejemplo completo de una aplicación descentralizada (dApp) en StarkNet que incluye:

- **Smart Contract**: Contrato de lotería implementado en Cairo
- **Frontend**: Aplicación web Next.js con integración de wallet
- **Testing**: Suite completa de tests para el contrato

### ¿Qué hace la aplicación?

La aplicación implementa un sistema de lotería descentralizada donde:
- El propietario puede establecer un número ganador
- Los usuarios pueden comprar boletos con números específicos
- El sistema puede determinar automáticamente el ganador
- Cada usuario solo puede comprar un boleto
- La aplicación web permite interactuar con todas las funciones del contrato

## 🏗️ Estructura del Proyecto

```
dojo-coding-ai-class/
├── contracts/                    # Smart contracts en Cairo
│   ├── src/
│   │   └── lib.cairo             # Contrato principal StarknetLotto
│   ├── tests/
│   │   └── test_contract.cairo   # Tests del contrato
│   ├── Scarb.toml               # Configuración de Scarb
│   └── target/                  # Archivos compilados
├── starknet-ex-app/             # Frontend Next.js
│   ├── app/
│   │   ├── components/          # Componentes React
│   │   │   └── WalletButton.tsx # Componente de wallet
│   │   ├── hooks/               # Hooks personalizados
│   │   │   └── useAegisSDK.ts   # Hook para manejo de cuentas
│   │   ├── layout.tsx           # Layout principal
│   │   └── page.tsx             # Página principal
│   └── package.json             # Dependencias del frontend
└── README.md                    # Este archivo
```

## ⚙️ Configuración del Entorno

### Prerequisites

1. **Instalación de herramientas Cairo/StarkNet:**
   - Scarb
   - Snfoundry
   - Sncast

2. **Instalación de Node.js y npm:**
   ```bash
   # Recomendado usar nvm para gestionar versiones de Node.js
   nvm install 24
   nvm use 24
   ```

3. **Verificar instalaciones:**
   ```bash
   scarb --version
   sncast --version
   snforge --version
   node --version
   npm --version
   ```

## Deploy de Contratos con Sncast

### Paso 1: Crear y configurar cuenta

1. **Declarar una cuenta**
   ```bash
   sncast account create \
     --name my_account \
     --network sepolia
   ```

2. **Copiar la dirección de la cuenta** (se muestra después del comando anterior)

3. **Fondear la cuenta con STRKs**:
   - Visita: https://starknet-faucet.vercel.app/
   - Pega tu dirección y solicita tokens

4. **Deployar la cuenta**
   ```bash
   sncast account deploy \
     --network sepolia \
     --name my_account
   ```

### Paso 2: Compilar y deployar el contrato

1. **Navegar al directorio de contratos**
   ```bash
   cd contracts
   ```

2. **Compilar el contrato**
   ```bash
   scarb build
   ```

3. **Declarar el contrato**
   ```bash
   sncast \
     --account my_account \
     declare \
     --contract-name StarknetLotto \
     --network sepolia
   ```

4. **Copiar el class hash** que devuelve el comando anterior

5. **Deployar el contrato**
   ```bash
   sncast \
     --account my_account \
     deploy \
     --class-hash 0x[TU_CLASS_HASH] \
     --arguments 0x[TU_DIRECCION_COMO_OWNER] \
     --network sepolia
   ```

### Paso 3: Verificar deployment

```bash
# Verificar el estado del contrato
sncast call \
  --contract-address 0x[CONTRACT_ADDRESS] \
  --function get_owner \
  --network sepolia
```

## 🚀 Desarrollo del Frontend

### Instalación y configuración

1. **Navegar al directorio del frontend**
   ```bash
   cd starknet-ex-app
   ```

2. **Instalar dependencias**
   ```bash
   npm install
   ```

3. **Configurar variables de entorno**
   - Reemplaza `'your-avnu-api-key'` con tu API key real de AVNU

4. **Ejecutar en modo desarrollo**
   ```bash
   npm run dev
   ```

5. **Abrir en el navegador**
   ```
   http://localhost:3000
   ```