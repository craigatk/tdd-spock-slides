## Spock

Behavior style test framework in Groovy with support for easy data-driven testing.

---
## Behavior-Style Testing

Test cases separated into three main sections

* given (setup)
* when (execute method)
* then (verify results)

---
## Data-driven tests

Run **same test body** with **multiple sets** of test **inputs** and expected **outputs**

---
## Writing a Spock test

---

## Test Class Name
Test class name ends in ```Spec``` or ```Specification``` and class extends ```spock.lang.Specification```

```
class BankAccountSpec extends Specification {

}
```

---
## Test Case Name
Test case method names can be descriptive sentences

```
def "after depositing 10 dollars into account then balance should be 10 dollars"() {

}
```

---

## Test case body

---

```
class BankAccountSpec extends Specification {
  def "after depositing 10 dollars into account then balance should be 10 dollars"() {
    given:
    BankAccount bankAccount = new BankAccount()
      
    when:
    bankAccount.deposit(10)
      
    then:
    assert bankAccount.balance == 10
  }
}
```
---

## Data-Driven Testing

---
## Where block

```
where:
input1 | input2 || output
4      | 6      || 5
12     | 18     || 15
20     | 14     || 17
```

---
## @Unroll

Include inputs and outputs from ```where:``` block in tests results

---

```
@Unroll
def 'depositing #amount should increase balance to #expectedBalance'() {
  given:
  BankAccount bankAccount = new BankAccount()
  
  when:
  bankAccount.deposit(amount)
  
  then:
  assert bankAccount.balance == expectedBalance
  
  where:
  amount || expectedBalance
  10     || 10
  20     || 20
}
```

---
## Groovy Power Assert

```
assert result == expectedValue
```

---

```
def 'x plus y equals z'() {
    when:
    int x = 4
    int y = 5
    int z = 10

    then:
    assert x + y == z
}
```

---

```
Condition not satisfied:

x + y == z
| | | |  |
4 9 5 |  10
      false
```
---

```
@ToString
class Dimensions {
    int x
    int y

    int getArea() {
        return x * y + 1
    }
}
```

---

```
Condition not satisfied:

dimensions.findArea() == 15
|          |          |
|          16         false
bank.Dimensions(3, 5)
```

---

## Test Driven Development

Use tests to help guide development

---
## TDD Benefits

* Verify when we're done coding a feature
 * Avoid writing unnecessary code
* Easily write testable code
* Remove any need for calculating code coverage

---
## TDD Side Effects

Extensive **safety net** for code changes

* Last-minute requirement changes
* Performance improvements
* Code cleanup

---
## Executable Documentation

Thoroughly document expected behavior in tests

---
## TDD Workshop
```BankAccount``` class

* balance
* deposit
* withdraw
* surprise feature

---
## Fetch Project

```
git clone git@github.com:craigatk/tdd-spock.git

cd tdd-spock

git fetch --all
```

---
## Project Structure

```
src/main/groovy/bank

src/test/groovy/bank
```

---
## Gradle for Running Tests

```
gradlew test --info
```

---
## Create First Test

```BankAccountSpec``` with one test that creates a new ```BankAccount``` and verifies ```bankAccount.balance``` is 0

---
    
```
class BankAccountSpec extends Specification {
  def "bank account starting balance should be 0"() {
    given:
    BankAccount bankAccount = new BankAccount()
    
    when:
    BigDecimal startingBalance = bankAccount.balance
    
    then:
    assert startingBalance == 0
  }
}
```

---
## Run test, verify failure

Hint: Should be a test compilation failure because BankAccount class does not exist yet

---    
## Make Test Pass

Write minimal code to make test pass

* hint: 'balance' should be BigDecimal type

---

```
class BankAccount {
  BigDecimal balance = 0      
}
```

---
## Run test, verify it passes

---
## Write Test for "deposit" Method

Takes one parameter, a BigDecimal 'amount' and updates the balance

---

```
def 'deposit should increase balance'() {
  given:
  BankAccount bankAccount = new BankAccount()
  
  when:
  bankAccount.deposit(10)
  
  then:
  assert bankAccount.balance == 10
}
```

---
## Run test, verify failure

---

## Write minimal code to make test pass

---

```
void deposit(BigDecimal amount) {
  balance = 10
}
```

---
## Additional Test Case

Using ```where``` block, write an additional test case for the ```deposit()``` method that deposits a different amount.

---

```
@Unroll
def 'depositing #amount should increase balance to #expectedBalance'() {
  given:
  BankAccount bankAccount = new BankAccount()
  
  when:
  bankAccount.deposit(amount)
  
  then:
  assert bankAccount.balance == expectedBalance
  
  where:
  amount || expectedBalance
  10     || 10
  20     || 20
}
```

---

## Run tests, verify failure

---

## Write full deposit method

---

```
void deposit(BigDecimal amount) {
  balance += amount
}
```

---

## Withdraw method

Write a test case for ```withdraw(BigDemicmal amount)``` method

---

```
def "withdraw should reduce balance"() {
  given:
  BankAccount bankAccount = new BankAccount()
  
  bankAccount.deposit(20)
  
  when:
  bankAccount.withdraw(15)
  
  then:
  assert bankAccount.balance == 5
}
```

---

## Run test, verify failure

---

## Make test pass

Write minimal code to make test pass

---

```
void withdraw(BigDecimal amount) {
  balance = 5
}
```

---

## Run test, verify pass

---

## Additional test cases

Similar to ```deposit()``` method, use ```where:``` block to add two additional test cases that withdraw different amounts

---

```
@Unroll
def "withdrawing #amount should reduce balance to #expectedBalance"() {
  given:
  BankAccount bankAccount = new BankAccount()
  
  bankAccount.deposit(20)
  
  when:
  bankAccount.withdraw(amount)
  
  then:
  assert bankAccount.balance == expectedBalance
  
  where:
  amount || expectedBalance
  5      || 15
  10     || 10
  15     || 5
}
```

---

## Run tests, verify pass

---

## Verify Deposits and Withdrawals

Write additional test case that deposits 50 dollars, then withdraws 20, deposits 10, then verifies balance is 40.

---

```
def "withdrawing and depositing should update balance and create transaction list"() {
    given:
    BankAccount bankAccount = new BankAccount()

    when:
    bankAccount.deposit(50)
    bankAccount.withdraw(20)
    bankAccount.deposit(10)

    then:
    assert bankAccount.balance == 40
}
```

---

## Surprise Feature

---

## Transaction History

* Keep a list of all transactions
* Record transaction type and amount
    * ```List<Transaction> transactions```
    * ```'d'``` for deposit, ```'w'``` for withdrawal
---

## Start with tests

Update our last test to also verify three transactions in transaction list

---

## Run test, verify failure

---

## Transaction class

* Create ```Transaction``` class with ```amount``` and ```type``` fields
* Add ```@groovy.transform.EqualsAndHashCode``` annotation to ```Transaction``` class to help in testing equality

---

## Assertion failure message

```
bankAccount.transactions.contains(new Transaction(amount: 50, type: 'd'))
|           |            |        |
|           []           false    bank.Transaction@298d5
bank.BankAccount@42652110
```

---

## @ToString

Add ```@groovy.transform.ToString``` annotation to the ```Transaction``` class a more readable failure message

---

```
bankAccount.transactions.contains(new Transaction(amount: 50, type: 'd'))
|           |            |        |
|           []           false    bank.Transaction(50, d)
bank.BankAccount@121321f5
```

---

## Write code to make test pass

Hint: create a ```Transaction``` class with ```BigDecimal amount``` and ```String type``` fields

---

```
List<Transaction> transactions = []

void deposit(BigDecimal amount) {
    transactions << new Transaction(type: 'd', amount: amount)
}

void withdraw(BigDecimal amount) {
    transactions << new Transaction(type: 'w', amount: amount)
}
```

---

## Balance calculated from transactions

Create a ```.getBalance()``` method that calculates balance from transaction list.

---

```
BigDecimal getBalance() {
    if (transactions) {
        return transactions.sum { transaction ->
            if (transaction.type == 'd') {
                transaction.amount
            } else {
                -transaction.amount
            }
        }
    } else {
        return 0
    }
}
```

---

## Run all tests, verify they pass

---

## Recap

* BDD-style testing with Spock
* Groovy power assert
* Test-drive Groovy bank account

---

## Q & A