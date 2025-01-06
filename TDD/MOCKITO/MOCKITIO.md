# Mockito Testing Basics

![alt text](image.png)

## Registering Mockito for a Test Class

### Properties

- `@ExtendWith(MockitoExtension.class)`: Modern approach to extend Mockito.

### Code

```java
@ExtendWith(MockitoExtension.class)
class MyTest {
    @Mock
    private Test t;

    @Test
    void testSomething() {
        // Test logic...
    }
}
```

## Instantiating a Mock Object

### Properties

- **Mock Object**: A simulated instance of a class.
- **Mocking**: Technique to control behavior, isolate tests, and simulate specific scenarios without using the original class.

- **@Mock:** Gebruikt om een mock-object te maken van een specifieke klasse of interface.
- **@InjectMocks:** Gebruikt om een object van de klasse te maken en automatisch de gemockte objecten in de constructor of setter te injecteren.
- **.when():** Gebruikt om het gedrag van de gemockte methoden in te stellen.
- **.verify():** Verifieert of een specifieke actie is uitgevoerd op een gemockt object.
- **.lenient():** Wordt gebruikt om een meer flexibele manier van het mocken van methoden mogelijk te maken die standaard een exception zouden gooien als ze niet expliciet gemockt zijn.

## Voorbeeld

Stel, we hebben een eenvoudige service CalculatorService en een afhankelijkheid MathService die we willen mocken voor de test.

```java
// CalculatorService.java
public class CalculatorService {
    private final MathService mathService;

    public CalculatorService(MathService mathService) {
        this.mathService = mathService;
    }

    public int add(int a, int b) {
        return mathService.add(a, b);
    }

    public int subtract(int a, int b) {
        return mathService.subtract(a, b);
    }
}
```

```java
// MathService.java
public interface MathService {
    int add(int a, int b);
    int subtract(int a, int b);
}
```

### Test

```java
import static org.mockito.Mockito.*;
import org.junit.jupiter.api.Test;
import org.mockito.InjectMocks;
import org.mockito.Mock;
import org.mockito.junit.jupiter.MockitoExtension;
import static org.junit.jupiter.api.Assertions.assertEquals;

@ExtendWith(MockitoExtension.class)
public class CalculatorServiceTest {

    // Het mocken van MathService
    @Mock
    private MathService mathService;

    // Het injecteren van de gemockte MathService in de CalculatorService
    @InjectMocks
    private CalculatorService calculatorService;

    @Test
    public void testAdd() {
        // Gedrag instellen voor de gemockte MathService
        when(mathService.add(2, 3)).thenReturn(5);  // Wanneer mathService.add(2, 3) wordt aangeroepen, retourneer 5

        // De test uitvoeren
        int result = calculatorService.add(2, 3);

        // Verifiëren dat de juiste waarde wordt geretourneerd
        assertEquals(5, result);

        // Verifiëren of de add-methode van MathService precies één keer werd aangeroepen
        verify(mathService).add(2, 3);
    }

    @Test
    public void testSubtract() {
        // Het gedrag van subtract instellen
        when(mathService.subtract(5, 3)).thenReturn(2);

        // De test uitvoeren
        int result = calculatorService.subtract(5, 3);

        // Verifiëren dat de juiste waarde wordt geretourneerd
        assertEquals(2, result);

        // Verifiëren of subtract is aangeroepen
        verify(mathService).subtract(5, 3);
    }

    @Test
    public void testLenientMock() {
        // lenient mocken, zodat er geen exception wordt gegooid als de methode niet wordt ingesteld
        lenient().when(mathService.add(2, 3)).thenReturn(5);

        // Test uitvoeren
        int result = calculatorService.add(2, 3);

        // Verifiëren dat de gemockte waarde wordt geretourneerd
        assertEquals(5, result);
    }
}
```

## VOORBEELD 2

```java
// BankService.java
public class BankService {
    private final BankRepository bankRepository;

    public BankService(BankRepository bankRepository) {
        this.bankRepository = bankRepository;
    }

    public boolean withdraw(String accountNumber, double amount) {
        Account account = bankRepository.findAccountByNumber(accountNumber);
        if (account.getBalance() >= amount) {
            account.setBalance(account.getBalance() - amount);
            bankRepository.saveAccount(account);
            return true;
        } else {
            return false;
        }
    }
}
```

```java
// BankRepository.java
public interface BankRepository {
    Account findAccountByNumber(String accountNumber);
    void saveAccount(Account account);
}
```

```java
// Account.java
public class Account {
    private String accountNumber;
    private double balance;

    // Constructor
    public Account(String accountNumber, double balance) {
        this.accountNumber = accountNumber;
        this.balance = balance;
    }

    // Getters en setters
    public String getAccountNumber() {
        return accountNumber;
    }

    public double getBalance() {
        return balance;
    }

    public void setBalance(double balance) {
        this.balance = balance;
    }
}
```

### Test

```java
import static org.mockito.Mockito.*;
import static org.junit.jupiter.api.Assertions.*;
import org.junit.jupiter.api.Test;
import org.mockito.InjectMocks;
import org.mockito.Mock;
import org.mockito.junit.jupiter.MockitoExtension;
import org.junit.jupiter.api.extension.ExtendWith;

@ExtendWith(MockitoExtension.class)
public class BankServiceTest {

    @Mock
    private BankRepository bankRepository;

    @InjectMocks
    private BankService bankService;

    @Test
    public void testWithdraw_Successful() {
        // Arrange: Stel een mockaccount in met voldoende saldo
        String accountNumber = "12345";
        Account mockAccount = new Account(accountNumber, 100.0);
        when(bankRepository.findAccountByNumber(accountNumber)).thenReturn(mockAccount);

        // Act: Voer de opname uit
        boolean result = bankService.withdraw(accountNumber, 50.0);

        // Assert: Controleer het resultaat
        assertTrue(result);
        assertEquals(50.0, mockAccount.getBalance()); // Controleer of het saldo is aangepast
        verify(bankRepository).saveAccount(mockAccount); // Controleer dat saveAccount is aangeroepen
    }

    @Test
    public void testWithdraw_Failed_InsufficientFunds() {
        // Arrange: Stel een mockaccount in met onvoldoende saldo
        String accountNumber = "12345";
        Account mockAccount = new Account(accountNumber, 40.0);
        when(bankRepository.findAccountByNumber(accountNumber)).thenReturn(mockAccount);

        // Act: Voer de opname uit
        boolean result = bankService.withdraw(accountNumber, 50.0);

        // Assert: Controleer het resultaat
        assertFalse(result);
        assertEquals(40.0, mockAccount.getBalance()); // Controleer dat het saldo niet is aangepast
        verify(bankRepository, never()).saveAccount(mockAccount); // Controleer dat saveAccount niet is aangeroepen
    }

    @Test
    public void testWithdraw_Failed_AccountNotFound() {
        // Arrange: Simuleer dat het account niet bestaat
        String accountNumber = "99999";
        when(bankRepository.findAccountByNumber(accountNumber)).thenReturn(null);

        // Act: Voer de opname uit
        boolean result = bankService.withdraw(accountNumber, 50.0);

        // Assert: Controleer het resultaat
        assertFalse(result); // Opname moet falen
        verify(bankRepository, never()).saveAccount(any(Account.class)); // Controleer dat saveAccount niet is aangeroepen
    }
}
```
