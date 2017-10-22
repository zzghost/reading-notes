# The Java Programming Environment
(Basic programming concepts)
1. Eight primitive data types.  
integer types: int ,short, long, byte.  
float types: double, float.  
character type: char.  
boolean type: boolean.  
2. Use **final** to denote a **constant** :the value of this variable can be set only once and for all.  
Use **static final** to denote a **class constant**.
3. string1.`equals`(string2): whether string1 and string2 are equal.  
string1 `==` string2: whether they are stored in the same location.  
4. java.math has `BigInteger` and `BigDecimal`:
BigInteger's API:  
BigInteger add(BigInteger other)  
BigInteger subtract(BigInteger other)  
BigInteger multiply(BigInteger other)  
BigInteger divide(BigInteger other)  
BigInteger mod(BigInteger other)  
int compareTo(BigInteger other): return 0 if equal, negtive if less than other, positive otherwise.  
static BigInteger valueOf(long x)  
BigDecimal's API:  
Same as BigInteger, but has another API: static BigDecimal valueOf(long x, int scale): return a big decimal whose value equals x or x/(10^scale)  
5. Use `Arrays.copyOf()` to increase the size of an array like this:  
`luckyNumbers = Arrays.copyOf(luckyNumbers, 2 * luckyNumbers.length)`  
