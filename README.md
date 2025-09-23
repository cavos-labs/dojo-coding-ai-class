# Dojo Coding Cairo AI Class
Ejemplo de como hacer una app completa en Starknet, con contratos y cliente.

## 📋 Tabla de Contenidos
1. [Descripción del Proyecto](#descripción-del-proyecto)
2. [Estructura del Proyecto](#estructura-del-proyecto)
3. [Configuración del Entorno](#configuración-del-entorno)
4. [Deploy de Contratos con Sncast](#deploy-de-contratos-con-sncast)
5. [Pasos](#pasos)

## 📝 Descripción del Proyecto

Este proyecto es un ejemplo completo de una aplicación descentralizada (dApp) en Starknet que incluye:

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
├── Starknet-ex-app/             # Frontend Next.js
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

1. **Instalación de herramientas Cairo/Starknet:**
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

## Contratos
### Pasos

1.

   ```bash
    mkdir contracts
    cd contracts
    scarb init --name contracts
   ```

2.
    IMPORTANTE: cada una de estas funciones debe ser desarrollada y enviada al Cairo coder una por una para seguir las mejores prácticas. Cairo coder no funciona como un LLM tradicional; si le das demasiadas tareas a la vez, fallará.

    ```
    Cairo coder Prompt: "Create a comprehensive Cairo lottery contract called StarknetLotto using Cairo 2.12.2, remove everything from lib.cairo

    - Owner can set winning number (only owner access), constructor should receive the owner address
    - Users can buy tickets with specific numbers (no mapping), emit an event when a number is bought
    - Track total ticket count and user tickets using Map, use this code to implement, each user can only buy one ticket:
    ```
    ```
    #[Starknet::contract]
    mod UserValues {
        use Starknet::storage::{
            Map, StoragePathEntry, StoragePointerReadAccess, StoragePointerWriteAccess,
        };
        use Starknet::{ContractAddress, get_caller_address};

        #[storage]
        struct Storage {
            user_values: Map<ContractAddress, u64>,
        }

        #[abi(embed_v0)]
        impl UserValuesImpl of super::IUserValues<ContractState> {
            fn set(ref self: ContractState, amount: u64) {
                let caller = get_caller_address();
                self.user_values.entry(caller).write(amount);
            }

            fn get(self: @ContractState, address: ContractAddress) -> u64 {
                self.user_values.entry(address).read()
            }
        }
    }
    ```
    ```
    - Determine winner by matching ticket numbers with winning number use a loop to determine the winner

    Requirements:
    - Use latest Cairo syntax (2.12.2)
    - Implement proper storage patterns
    - Include comprehensive interface definition
    - Add proper access controls for owner functions
    - Follow Starknet development best practices"
    ```

4.

    ```bash
    # Build the contract
    scarb build

    # Run comprehensive test suite
    snforge test
    ```

5.
   ```bash
   sncast account create \
     --name my_account \
     --network sepolia
   ```

   ```bash
   sncast account deploy \
     --network sepolia \
     --name my_account
   ```

   ```bash
   sncast \
     --account my_account \
     declare \
     --contract-name StarknetLotto \
     --network sepolia
   ```

   ```bash
   sncast \
     --account my_account \
     deploy \
     --class-hash 0x68aff410ee15f04470122e3d6f90043beeaf481604323fe79c859e77421ed5c \
     --arguments 0x06f58A6581F348180A4A8EF0f6a2168cef1a53874A3ec1980D35FFB9b4227948 \
     --network sepolia
   ```

## Cliente
### Pasos

1.

    ```bash
    npx create-next-app@latest starknet-ex-app --typescript --tailwind --eslint --app
    cd starknet-ex-app
    npm install @cavos/aegis
    ```

2. Agrega 'use client' en 'layout.tsx', elimina la metadata de la pagina, esto para poder usar el provider de in app wallets.

3.

    ```
    Claude Prompt: "Implement this context provider on my layout:
    import { AegisProvider } from '@cavos/aegis';

    export default function App() {
      return (
        <AegisProvider
          config={{
            network: 'SN_SEPOLIA',
            appName: 'MyApp',
            appId: 'your-unique-app-id',
            paymasterApiKey: 'your-avnu-api-key',
            enableLogging: true
          }}
        >
          {/* Your app */}
        </AegisProvider>
      );
    }

    Implement this button on my page.tsx, delete everything else:
    import { useAegis } from '@cavos/aegis';

    function WalletButton() {
      const { isConnected, currentAddress, deployWallet, disconnect, aegisAccount } = useAegis();

      if (isConnected) {
        return (
          <View>
            <Text>Connected: {currentAddress}</Text>
            <Button onPress={disconnect} title="Disconnect" />
          </View>
        );
      }

      return <Button onPress={deployWallet} title="Create Wallet" />;
    }
    "
    ```

4.
    ```
    Claude Prompt: "Create a lottery interface in page.tsx with buttons for these functions:
    - set_winning_number (with input field)
    - get_winning_number
    - get_owner
    - buy_ticket (with input field)
    - get_user_ticket (with address input)
    - get_ticket_count
    - determine_winner
    ```

5. Usa las funciones de 'aegisAccount' para llamar hacer read/write en el contrato.

https://www.npmjs.com/package/@cavos/aegis