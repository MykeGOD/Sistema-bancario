# Sistema Bancario en C#

Este proyecto es una aplicación en C# diseñada para gestionar un sistema bancario. Permite a los usuarios crear y gestionar cuentas, realizar transacciones, y consultar el estado de sus cuentas.

## Características

- **Gestión de cuentas**: Crear, eliminar y modificar cuentas bancarias.
- **Transacciones**: Realizar depósitos y retiros.
- **Consultas**: Ver el saldo y el historial de transacciones.
  ![Representacion del sistema](https://img.freepik.com/vector-gratis/conjunto-iconos-sistema-bancario_1284-14615.jpg?t=st=1724263597~exp=1724267197~hmac=172a6fd5a91da5ee6a4a367227d31446d35e680d586fafbd02bcf83b8dca1075&w=826)
```
using System;
using System.Collections.Generic;

namespace SistemaBancario
{
    class Program
    {
        static void Main(string[] args)
        {
            Banco banco = new Banco();
            bool continuar = true;

            while (continuar)
            {
                Console.Clear();
                Console.WriteLine("Sistema Bancario");
                Console.WriteLine("1. Crear Cuenta");
                Console.WriteLine("2. Realizar Depósito");
                Console.WriteLine("3. Realizar Retiro");
                Console.WriteLine("4. Consultar Saldo");
                Console.WriteLine("5. Salir");
                Console.Write("Selecciona una opción: ");
                
                string opcion = Console.ReadLine();

                switch (opcion)
                {
                    case "1":
                        banco.CrearCuenta();
                        break;
                    case "2":
                        banco.RealizarDeposito();
                        break;
                    case "3":
                        banco.RealizarRetiro();
                        break;
                    case "4":
                        banco.ConsultarSaldo();
                        break;
                    case "5":
                        continuar = false;
                        break;
                    default:
                        Console.WriteLine("Opción inválida. Presiona cualquier tecla para continuar...");
                        Console.ReadKey();
                        break;
                }
            }
        }
    }

    class Banco
    {
        private Dictionary<int, Cuenta> cuentas = new Dictionary<int, Cuenta>();
        private int idSiguiente = 1;

        public void CrearCuenta()
        {
            Console.Write("Ingresa el nombre del titular: ");
            string nombre = Console.ReadLine();
            Cuenta nuevaCuenta = new Cuenta(idSiguiente, nombre);
            cuentas.Add(idSiguiente, nuevaCuenta);
            Console.WriteLine($"Cuenta creada con ID: {idSiguiente}");
            idSiguiente++;
            Console.WriteLine("Presiona cualquier tecla para continuar...");
            Console.ReadKey();
        }

        public void RealizarDeposito()
        {
            Console.Write("Ingresa el ID de la cuenta: ");
            int id = int.Parse(Console.ReadLine());
            if (cuentas.TryGetValue(id, out Cuenta cuenta))
            {
                Console.Write("Ingresa el monto a depositar: ");
                decimal monto = decimal.Parse(Console.ReadLine());
                cuenta.Depositar(monto);
                Console.WriteLine("Depósito realizado con éxito.");
            }
            else
            {
                Console.WriteLine("Cuenta no encontrada.");
            }
            Console.WriteLine("Presiona cualquier tecla para continuar...");
            Console.ReadKey();
        }

        public void RealizarRetiro()
        {
            Console.Write("Ingresa el ID de la cuenta: ");
            int id = int.Parse(Console.ReadLine());
            if (cuentas.TryGetValue(id, out Cuenta cuenta))
            {
                Console.Write("Ingresa el monto a retirar: ");
                decimal monto = decimal.Parse(Console.ReadLine());
                if (cuenta.Retirar(monto))
                {
                    Console.WriteLine("Retiro realizado con éxito.");
                }
                else
                {
                    Console.WriteLine("Fondos insuficientes.");
                }
            }
            else
            {
                Console.WriteLine("Cuenta no encontrada.");
            }
            Console.WriteLine("Presiona cualquier tecla para continuar...");
            Console.ReadKey();
        }

        public void ConsultarSaldo()
        {
            Console.Write("Ingresa el ID de la cuenta: ");
            int id = int.Parse(Console.ReadLine());
            if (cuentas.TryGetValue(id, out Cuenta cuenta))
            {
                Console.WriteLine($"El saldo de la cuenta {id} es: {cuenta.Saldo:C}");
            }
            else
            {
                Console.WriteLine("Cuenta no encontrada.");
            }
            Console.WriteLine("Presiona cualquier tecla para continuar...");
            Console.ReadKey();
        }
    }

    class Cuenta
    {
        public int Id { get; }
        public string Nombre { get; }
        public decimal Saldo { get; private set; }

        public Cuenta(int id, string nombre)
        {
            Id = id;
            Nombre = nombre;
            Saldo = 0m;
        }

        public void Depositar(decimal monto)
        {
            if (monto > 0)
            {
                Saldo += monto;
            }
        }

        public bool Retirar(decimal monto)
        {
            if (monto > 0 && monto <= Saldo)
            {
                Saldo -= monto;
                return true;
            }
            return false;
        }
    }
}
```
