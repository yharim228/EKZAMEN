```using System;
using System.Collections.Generic;
using System.Linq;

public class ATM
{
    private static Dictionary<int, int> balance = new Dictionary<int, int>()
    {
        { 100, 1000 },
        { 50, 1000 },
        { 20, 1000 },
        { 10, 1000 }
    };

    public static void Main(string[] args)
    {
        Console.WriteLine("Добро пожаловать в банкомат!");

        while (true)
        {
            Console.WriteLine("\nВыберите операцию:");
            Console.WriteLine("1. Снятие наличных");
            Console.WriteLine("2. Просмотр баланса");
            Console.WriteLine("3. Выход");

            string choice = Console.ReadLine();

            switch (choice)
            {
                case "1":
                    Withdraw();
                    break;
                case "2":
                    DisplayBalance();
                    break;
                case "3":
                    Console.WriteLine("Спасибо за использование банкомата!");
                    return;
                default:
                    Console.WriteLine("Неверный выбор. Попробуйте снова.");
                    break;
            }
        }
    }

    private static void Withdraw()
    {
        Console.Write("Введите сумму для снятия: ");
        if (!int.TryParse(Console.ReadLine(), out int amount) || amount <= 0)
        {
            Console.WriteLine("Неверный ввод. Введите целое положительное число.");
            return;
        }


        if (!CanWithdraw(amount))
        {
            Console.WriteLine($"К сожалению, недостаточно наличных на эту сумму.");
            return;
        }


        Dictionary<int, int> dispensedNotes = DispenseNotes(amount);

        Console.WriteLine("Выдача:");
        foreach (var note in dispensedNotes)
        {
            Console.WriteLine($"{note.Key} - {note.Value} шт.");
        }
        Console.WriteLine($"Итого: {amount}");

    }



    private static Dictionary<int, int> DispenseNotes(int amount)
    {
        Dictionary<int, int> dispensedNotes = new Dictionary<int, int>();
        List<int> denominations = balance.Keys.ToList();
        denominations.Sort( (a, b) => b - a);

        foreach (int denomination in denominations)
        {
            int count = amount / denomination;
            if (count > 0 && balance[denomination] >= count)
            {
                dispensedNotes[denomination] = count;
                amount -= denomination * count;
            }

        }
        if (amount > 0)
        {
            throw new Exception("Невозможно выдать всю сумму"); //Невозможно выдать всю сумму
        }
        UpdateBalance(dispensedNotes);


        return dispensedNotes;
    }

        // Обновление баланса после снятия
    private static void UpdateBalance(Dictionary<int, int> dispensedNotes)
    {
        foreach (var note in dispensedNotes)
        {
            balance[note.Key] -= note.Value;
        }
    }

    //Проверка возможности снятия
    private static bool CanWithdraw(int amount)
    {
        List<int> denominations = balance.Keys.ToList();
        denominations.Sort( (a, b) => b - a);

        foreach (int denomination in denominations)
        {
            int count = amount / denomination;
            if (count > 0 && balance[denomination] >= count)
            {
                return true;
            }
        }
        return false;
    }



    private static void DisplayBalance()
    {
        Console.WriteLine("Текущий баланс:");
        foreach (var entry in balance)
        {
            Console.WriteLine($"{entry.Key} - {entry.Value} шт.");
        }
    }
}```
