# Praktika-6-Aksenova
Создание проекта для тестирования

using System;
namespace BankAccountNS
{
    /// <summary>
    /// Bank account demo class.
    /// </summary>
    public class BankAccount
    {
        private readonly string m_customerName;
        private double m_balance;


        private BankAccount() { }


        public BankAccount(string customerName, double balance)
        {
            m_customerName = customerName;
            m_balance = balance;
        }


        public string CustomerName
        {
            get { return m_customerName; }
        }


        public double Balance
        {
            get { return m_balance; }
        }


        public void Debit(double amount)
        {
            if (amount > m_balance)
            {
                throw new ArgumentOutOfRangeException("amount");
            }


            if (amount < 0)
            {
                throw new ArgumentOutOfRangeException("amount");
            }


            m_balance += amount;
        }


        public void Credit(double amount)
        {
            if (amount < 0)
            {
                throw new ArgumentOutOfRangeException("amount");
            }


            m_balance += amount;
        }


        public static void Main()
        {
            BankAccount ba = new BankAccount("Mr. Roman Abramovich", 11.99);


            ba.Credit(5.77);
            ba.Debit(11.22);
            Console.WriteLine("Current balance is ${0}", ba.Balance);
            Console.ReadLine();
        }
    }
}
![image](https://github.com/user-attachments/assets/91655ba8-bc68-4801-adc0-8aea9e25c07b)

Переименуем проект в BankAccount и проверим ещё раз его работу
![image](https://github.com/user-attachments/assets/10134a82-4a27-48fa-96b6-5024ad7df5)

Создадим проект модульного теста
В проекте Bank.Tests добавим ссылку на проект Bank
![image](https://github.com/user-attachments/assets/c64e0502-58e3-405a-9e65-9749bf0a8ff0)
![image](https://github.com/user-attachments/assets/39165c04-7c7b-47a4-bef9-45ad4be2475d)

using Microsoft.VisualStudio.TestTools.UnitTesting;
using System;

namespace BankTests
{
    [TestClass]
    public class BankAccountTests
    {
        [TestMethod]
        public void TestMethod1()
        {
        }
    }
}
Изменённый код программы:
using Microsoft.VisualStudio.TestTools.UnitTesting;
using System;
using BankAccountNS;
namespace BankTests
{
    [TestClass]
    public class BankAccountTests
    {
        [TestMethod]
        public void TestMethod1()
        {
        }
    }
}

Создание и запуск метода теста
Добавим следующий метод в класс BankAccountTests:

[TestMethod]
public void Debit_WithValidAmount_UpdatesBalance()
{
    // Arrange
    double beginningBalance = 11.99;
    double debitAmount = 4.55;
    double expected = 7.44;
    BankAccount account = new BankAccount("Mr. Roman Abramovich", beginningBalance);

    // Act
    account.Debit(debitAmount);

    // Assert
    double actual = account.Balance;
    Assert.AreEqual(expected, actual, 0.001, "Account not debited correctly");
}
![image](https://github.com/user-attachments/assets/585e9ace-3d36-4f03-838e-2cb437dbf8cd)
![image](https://github.com/user-attachments/assets/15b4d4e9-9234-41fa-a908-05bcb6cae12e)

[TestClass]
public class BankAccountTests
{
    [TestMethod]
        public void Debit_WithValidAmount_UpdatesBalance()
        {
            // Arrange
            double beginningBalance = 11.99;
            double debitAmount = 4.55;
            double expected = 7.44;
            BankAccount account = new BankAccount("Mr. Roman Abramovich", beginningBalance);

            // Act
            account.Debit(debitAmount);

            // Assert
            double actual = account.Balance;
            Assert.AreEqual(expected, actual, 0.001, "Account not debited correctly");
        }
}
![image](https://github.com/user-attachments/assets/a3f519c1-4b48-4c95-8687-ec50493f0914)

Рефакторинг кода:
public const string DebitAmountExceedsBalanceMessage = "Debit amount exceeds balance";
public const string DebitAmountLessThanZeroMessage = "Debit amount is less than zero";

if (amount > m_balance)
{
    throw new System.ArgumentOutOfRangeException("amount", amount, DebitAmountExceedsBalanceMessage);
}

if (amount < 0)
{
    throw new System.ArgumentOutOfRangeException("amount", amount, DebitAmountLessThanZeroMessage);
}

[TestMethod]
public void Debit_WhenAmountIsMoreThanBalance_ShouldThrowArgumentOutOfRange()
{
    // Arrange
    double beginningBalance = 11.99;
    double debitAmount = 20.0;
    BankAccount account = new BankAccount("Mr. Bryan Walton", beginningBalance);

    // Act
    try
    {
        account.Debit(debitAmount);
    }
    catch (System.ArgumentOutOfRangeException e)
    {
        // Assert
        StringAssert.Contains(e.Message, BankAccount.DebitAmountExceedsBalanceMessage);
    }
}

        [TestMethod]
        public void Debit_WhenAmountIsMoreThanBalance_ShouldThrowArgumentOutOfRange()
        {
            // Arrange
            double beginningBalance = 11.99;
            double debitAmount = 20.0;
            BankAccount account = new BankAccount("Mr. Bryan Walton", beginningBalance);

            // Act
            try
            {
                account.Debit(debitAmount);
            }
            catch (System.ArgumentOutOfRangeException e)
            {
                // Assert
                StringAssert.Contains(e.Message, BankAccount.DebitAmountExceedsBalanceMessage);
                return;
            }

            Assert.Fail("The expected exception was not thrown.");
        }
    }
}
Результаты тестирования:
![image](https://github.com/user-attachments/assets/bcc04845-c0dc-48a9-abed-21079307d686)
![image](https://github.com/user-attachments/assets/f7426e1b-35d8-4128-a5fe-0f1e42a9b717)











