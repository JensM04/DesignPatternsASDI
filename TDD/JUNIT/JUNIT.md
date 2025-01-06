# **JUnit Annotations**

- `@NullAndEmptySource` voor testen lege waarden

- `@ParameterizedTest`: Een JUnit 5 testannotatie die herhaaldelijk dezelfde test uitvoert met verschillende parameters.
- `@EnumSource`: Geef alle waarden van een enum als invoer voor de testmethode.
  ```java
  @ParameterizedTest
  @EnumSource(Maand.class)
  void geefDagVanDeWeek(Maand maand) {
   Assertions.assertEquals(Maand.JANUARI.getDagVanDeWeek(), geefDagVanDeWeek(maand));
  }
  ```
- `@ValueSource`: Een annotatie om een array van waarden als invoer voor de testmethode te specificeren.
  ```java
  @ParameterizedTest
  @ValueSource(ints = {2, 4, 8})
  void isEven(int n){
      Assertions.assertEquals(0,n%2);
  }
  ```
- `@CsvSource`: Geef een CSV-bestand met waarden als invoer voor de testmethode. Kunnen ook grote paren zijn: "1,2,3","4,5,6", ...
  ```java
  @ParameterizedTest
  @CsvSource({"1, 2", "3, 4", "5, 6"})
  void som(int getal1, int getal2) {
    Assertions.assertEquals(getal1 + getal2, som(getal1, getal2));
  }
  ```
- `@MethodSource`: Geef de uitvoer van een andere methode als invoer voor de testmethode.

  ```java
  @ParameterizedTest
  @MethodSource("geefGetallen")
  void som(int getal1, int getal2) {
      Assertions.assertEquals(5, getal1 + getal2);
  }
  static Stream<Arguments> geefGetallen() {
      return Stream.of(
        Arguments.of(3, 2),
        Arguments.of(1, 4),
        Arguments.of(5, 0)
        );
  }
  ```

- nog een voorbeeld voor `@MethodSource`:

```java
private static Stream<Arguments> addFixture(){
    return Stream.of(
        Arguments.of(new int[]{5}, 5),
        Arguments.of(new int[]{2, 3}, 5),
        Arguments.of(new int[]{5, 6, 9}, 5)
        );
    }

    @ParameterizedTest@MethodSource("addFixture")
    public void voorbeeldTest(int[] getallen, int resultaat) {
        assertEquals(resultaat, eenKlasse.verwerking(getallen));
    }
```

- `Arguments`: De `Arguments`-class is een container voor een set van argumenten en kunnen vervolgens worden gebruikt als argumenten voor een geparametriseerde test.
- `@Test`: Geeft aan dat de gemarkeerde methode een testmethode is.
- `@Before`: Geeft aan dat de gemarkeerde methode vóór elke testmethode moet worden uitgevoerd.
- `@After`: Geeft aan dat de gemarkeerde methode na elke testmethode moet worden uitgevoerd.
- `@BeforeClass`: Geeft aan dat de gemarkeerde methode vóór alle testmethoden in de testklasse moet worden uitgevoerd.
- `@AfterClass`: Geeft aan dat de gemarkeerde methode na alle testmethoden in de testklasse moet worden uitgevoerd.
- `@Ignore` of `@Disabled`: Geeft aan dat de gemarkeerde test moet worden genegeerd tijdens het uitvoeren van de test.
- `@RunWith`: Geeft aan welke testrunner moet worden gebruikt. In JUnit 5 is dit vaak niet meer nodig vanwege de nieuwe uitvoeringsengine.

## ASSERTIONS

- **`assertEquals(expected, actual)`**: Checks if `expected` equals `actual`.
- **`assertNotEquals(unexpected, actual)`**: Checks if `unexpected` does not equal `actual`.
- **`assertTrue(condition)`**: Checks if `condition` is `true`.
- **`assertFalse(condition)`**: Checks if `condition` is `false`.
- **`assertNull(object)`**: Checks if `object` is `null`.
- **`assertNotNull(object)`**: Checks if `object` is not `null`.
- **`assertSame(expected, actual)`**: Checks if `expected` and `actual` refer to the same object.
- **`assertNotSame(unexpected, actual)`**: Checks if `unexpected` and `actual` do not refer to the same object.
- **`assertThrows(expectedType, executable)`**: Ensures `expectedType` exception is thrown.
- **`assertEquals(expected, actual, delta)`**: Checks if `expected` and `actual` are equal within a `delta`.
