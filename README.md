```using System;
using System.Collections.Generic;

public class Bankomat
{
    private Dictionary<int, int> notes; 
    private int balance;

    public Bankomat(int initialBalance)
    {
        notes = new Dictionary<int, int>();
        notes.Add(50, 0);
        notes.Add(20, 0);
        notes.Add(10, 0);
        notes.Add(5, 0);
        notes.Add(1, 0);
        balance = initialBalance;
        LoadNotes();
    }

    private void LoadNotes()
    {
        notes[50] = 10;
        notes[20] = 20;
        notes[10] = 30;
        notes[5] = 40;
        notes[1] = 50;
    }


    public void Deposit(int amount)
    {
        if (amount <= 0)
        {
            Console.WriteLine("Внесение некорректного значения.");
            return;
        }
        balance += amount;
        Console.WriteLine($"Внесено {amount}. Текущий баланс: {balance}.");
        UpdateNotes();
    }



    public void Withdraw(int amount)
    {
        if (amount <= 0 || amount > balance)
        {
            Console.WriteLine("Некорректная сумма для снятия.");
            return;
        }

        Dictionary<int, int> dispensedNotes = new Dictionary<int, int>();
        
        foreach (var note in notes.Keys.OrderByDescending(k => k))
        {
            int count = Math.Min(notes[note], amount / note);
            if (count > 0)
            {
                dispensedNotes.Add(note, count);
                amount -= note * count;
            }
        }

        if (amount > 0)
        {
            Console.WriteLine("Не удалось выдать всю сумму.");
            return;
        }

        Console.WriteLine($"Выдано:");
        foreach (var note in dispensedNotes)
        {
            Console.WriteLine($"{note.Key} * {note.Value}");
        }


    }


    private void UpdateNotes()
    {
        foreach (var note in notes)
        {
           
        }
    }
        

    public void CheckBalance()
    {
        Console.WriteLine($"Текущий баланс: {balance}");
    }


    public static void Main(string[] args)
    {
        Bankomat bankomat = new Bankomat(0);

        while (true)
        {
            Console.WriteLine("\nВыберите операцию:");
            Console.WriteLine("1. Внести деньги");
            Console.WriteLine("2. Снять деньги");
            Console.WriteLine("3. Проверить баланс");
            Console.WriteLine("4. Выход");

            string choice = Console.ReadLine();

            switch (choice)
            {
                case "1":
                    Console.Write("Введите сумму для внесения: ");
                    if (int.TryParse(Console.ReadLine(), out int depositAmount))
                    {
                        bankomat.Deposit(depositAmount);
                    }
                    else
                    {
                        Console.WriteLine("Некорректный ввод.");
                    }
                    break;
                case "2":
                    Console.Write("Введите сумму для снятия: ");
                    if (int.TryParse(Console.ReadLine(), out int withdrawalAmount))
                    {
                        bankomat.Withdraw(withdrawalAmount);
                    }
                    else
                    {
                        Console.WriteLine("Некорректный ввод.");
                    }
                    break;
                case "3":
                    bankomat.CheckBalance();
                    break;
                case "4":
                    return;
                default:
                    Console.WriteLine("Некорректный выбор.");
                    break;
            }
        }
    }
}```
