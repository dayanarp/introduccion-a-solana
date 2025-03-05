# Principios básicos de Solana

Guía básica para navegar los conceptos fundamentales de Solana usando su CLI (Interfaz de Línea de Comandos). Esta guía cubrirá la configuración inicial, la creación y gestión de cuentas, y cómo trabajar con tokens SPL (Solana Program Library).


## Instalación y Configuración del CLI de Solana

Antes de empezar, necesitas instalar el CLI de Solana y configurar tu entorno.

### Instalar el CLI
- Abre tu terminal y ejecuta el siguiente comando para instalar el CLI en sistemas basados en Unix (Linux/MacOS), para Windows, puedes usar WSL (Windows Subsystem for Linux):

    `sh -c "$(curl -sSfL https://release.solana.com/stable/install)"`

- Verifica la instalación:

    `solana --version`

### Configurar el entorno
- Configura el CLI para usar una red específica (mainnet, testnet o devnet). Para desarrollo, usaremos devnet:

    `solana config set --url https://api.devnet.solana.com`

- Verifica la configuración:

    `solana config get`

### Crear una billetera
- Genera un par de claves (pública y privada) para tu cuenta:

    `solana-keygen new`

    Esto creará un archivo JSON en *~/.config/solana/id.json* por defecto, con la clave privada y pública. **Guárdalo en un lugar seguro.**

- Si ya tienes un archivo JSON con una clave privada, puedes importarla:

    `solana config set --keypair /ruta/del/archivo.json`

- Verifica tu dirección pública:

    `solana address`

### Obtener SOL de prueba

- Para probar transacciones sin gastar dinero real, puedes solicitar faucet SOL en Devnet:

    `solana airdrop 2`

- Consulta el saldo de tu cuenta:

    `solana balance`

- Para ver el saldo de otra cuenta:

    `solana balance <DIRECCIÓN_PÚBLICA>`

## Gestión de Cuentas

En Solana, las "cuentas" son fundamentales: almacenan datos, tokens o programas.

### Ver información de tu cuenta
- Usa tu dirección pública para inspeccionar tu cuenta:

    `solana account <TU_DIRECCIÓN_PÚBLICA>`

- Puedes inspeccionar la información de cualquier otra cuenta en Solana si conoces su dirección pública:

    `solana account <DIRECCIÓN_PÚBLICA>`

### Enviar SOL a otra cuenta

- Para enviar SOL a otra dirección, usa:

    `solana transfer <DIRECCIÓN_DESTINO> <MONTO_SOL>`

-  Si la cuenta de destino es nueva, usa *--allow-unfunded-recipient* para crearla automáticamente:

     `solana transfer <DIRECCIÓN_DESTINO> <MONTO_SOL> --allow-unfunded-recipient`


 ### Inspeccionar Transacciones

 - Consultar el historial de transacciones de una cuenta:

   ` solana transaction-history <DIRECCIÓN_PÚBLICA>`

- Ver detalles de una transacción específica:

    `solana confirm <ID_DE_TRANSACCIÓN>`

- Explorar el estado de la red:

    `solana cluster-version`


## Trabajando con Tokens SPL

Los tokens SPL son el estándar de tokens en Solana, similares a ERC-20 en Ethereum.

### Crear un token SPL

- Para crear un token en Solana, ejecuta:

    `spl-token create-token`

    Esto generará la dirección del token


### Crear una Cuenta de Token para almacenarlo

- Cada usuario que desee recibir un SPL Token necesita una cuenta de almacenamiento asociada al token. Para crearla:

    `spl-token create-account <DIRECCIÓN_TOKEN>`

- Para ver todas las cuentas de token asociadas con tu wallet:

    `spl-token accounts`

### Imprimir Tokens (Minting)

- Para generar (mint) tokens en tu cuenta:

    `spl-token mint <DIRECCIÓN_TOKEN> <CANTIDAD>`

- Para generar tokens (mint) y enviarlos a un cuenta específica:

     `spl-token mint <DIRECCIÓN_TOKEN> <CANTIDAD> <CUENTA_TOKEN>`

- Para verificar la cantidad de tokens alamacenados en una cuenta:

    `spl-token balance <CUENTA_TOKEN>`

- Para verificar cuantas unidades de un token existen en la red:

    `spl-token supply <DIRECCIÓN_TOKEN>`


### Transferir Tokens a otra cuenta

- Para enviar tokens a otro usuario:

    `spl-token transfer <DIRECCIÓN_TOKEN> <CANTIDAD> <DIRECCIÓN_DESTINO>`

- Si la cuenta de destino no tiene una cuenta de token asociada, agrégale* --fund-recipient*:

    `spl-token transfer  --fund-recipient <TOKEN_ADDRESS> <CANTIDAD> <DIRECCIÓN_DESTINO>`

### Crear un Token no Fungible (NFT)

- Crea un nuevo token especificando los decimales en 0:

    `spl-token create-token --decimals 0`

- Imprime (mintea) una única unidad del token:

    `spl-token mint <DIRECCIÓN_TOKEN> 1 <CUENTA_TOKEN>`

- Desabilita la emisión de más tokens en el futuro:

    `spl-token authorize <DIRECCIÓN_TOKEN> mint --disable`

### Congelando cuentas Token

- Si deseas poder congelar cuentas en caso de uso indebido, al crear el token, agrega *--enable-freeze*:

    `spl-token create-token --enable-freeze`

- Congelar una cuenta específica:

    `spl-token freeze <DIRECCIÓN_TOKEN> <CUENTA_DEL_USUARIO>`

- Descongelar una cuenta:

    `spl-token thaw <DIRECCIÓN_TOKEN> <CUENTA_DEL_USUARIO>`

### Quemar Tokens

- Si deseas eliminar tokens de circulación:

    `spl-token burn <CUENTA_DEL_TOKEN> <CANTIDAD>`

