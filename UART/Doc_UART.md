# Documentation UART

## A propos

Ce document est une documentation succinte visant à rappeler les fonctionnement de l'UART dans notre projet ainsi que ses primitives.

## Commandes

En fonction du driver utilisé, le nom des fonctions peut être susceptible de changer.

A priori, Vivado utilise UARTLite, nous utiliserons donc les noms fournis par la documentation de ce dernier.
```
int XUartLite_Initialize(XUartLite *InstancePtr, u16 DeviceId);

unsigned int XUartLite_Send(XUartLite *InstancePtr, u8 *DataBufferPtr,
				unsigned int NumBytes);

unsigned int XUartLite_Recv(XUartLite *InstancePtr, u8 *DataBufferPtr,
				unsigned int NumBytes);
```
Exemple d'initialisation :

## Exemple

Un exemple détaillé est fourni dans ce répertoire d'envoi de buffer par l'uart.
