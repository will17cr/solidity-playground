#  Asignación final Modulo 3
## ETH-KIPU Latinoamerica
## ETH Developer Pack 

# Estudiantes

1. Randall Brenes
2. Wilmer Ramírez
3. ¿Jose...?

# Detalles

## Requisitos

1. Contrato de Staking de token utilizando solidity y estándares de Open-Zepellin
2. Explicaciones de decisiones tomadas

## Salidas 

1. Repositorio `GitHub` con contratos y README con explicación de decisiones
2. Contrato inteligente de _Staking_, enlace de etherscan, sin verificación por complicaciones de librerías en Remix IDE
3. Contrato inteligente de _token_, enlace de etherscan, sin verificación por complicaciones de librerías en Remix IDE
4. Billetera de emisor

# Resultados de implementación

## Sobre implementación

1. Se requirió implementar un _token_ personalizado para poder probar el contrato de _Staking_ en modo virtualizado o simulado dado que se pide verificación de uso de un _token_ con cumplimiento de ERC20 o IERC20.

2. Se probaron con éxitos las implementaciones virtualizadas o simuladas del contrato de _Staking_ y el contrato del _Token_

## Decisiones tomadas en contrato de _Staking_

### Librerias
1. Uso  de librería `IERC20` de _Open-Zepellin_, para heredar requisitos, objetos y métodos del contrato estándar  `IERC20`, _tokens_ inmutables como criptomonedas, y lograr conformidad con estándar. 
2. Uso  de librería `Ownable` de _Open-Zepellin_, para heredar requisitos, objetos y métodos del contrato estándar  `Ownable`, contratos que permiten propiedad por otros,  y lograr acceso a funciones y métodos de conformidad con estándar. 
3. Uso  de librería `ReentrancyGuard` de _Open-Zepellin_, para heredar requisitos, objetos y métodos del contrato estándar  `ReentrancyGuard`, mecanismos de protección contra re-entrada de registros, y lograr acceso a funciones y métodos de conformidad con estándar.
4. Uso infructuoso de librería `safeERC20` dado que está limitado a versión de compilador 0.8.0, se decidió no usar para aplicar compilador más reciente. 

### Decisiones en código
1. Implementación de modificación `nonReentrant` para proteger los métodos de transferencia de _tokens_ de ataques de reentradas.
2. Implementación de método `_updateRewards` como `private` para evitar llamadas por cualquier agente externo y limitarlas a que se hagan dentro del contrato.
3. Implementación de un estado `bool` llamado `success` y un `require` de ese booleano para hacer las transferencias más seguras. Si no se logra éxito en la transferencia no se actualizan los registros del contrato de _Staking_.
4. Intento sin éxito de operaciones `safeTransfer` y `safeTransferFrom` de librería `safeERC20` por antigüedad y no compatibilidad con el compilador.
5. Implementación en código de `_updateRewards` del objeto `block.number` en lugar de `block.timestamp` para aumentar la seguridad en el cálculo de rendimiento o recompensa por realizar _staking_. 
6. Modificación de `_updateRewards` a una división por 1000 para hacer realista el cálculo de rendimientos o recompensa. Los métodos usuales de mínimo `rewardRate` de 1, 1 _token_ por segundo por cada _token_ cedido, hacen que  el saldo de rendimiento rápidamente alcance y supere por varios órdenes de magnitud el monto de _tokens_ cedido al contrato de _Staking_.
7. Implementaciones separadas de montos para mayor claridad, transparencia y control; el monto cedido se registra como `amount` y los rendimientos ganados se registran como `rewardEarned` 
8. Implementaciones separadas de retiros de montos para mayor claridad, transparencia y control; el monto cedido se retira con método `withdraw` y los rendimientos ganados se retiran con `claimRewards` 