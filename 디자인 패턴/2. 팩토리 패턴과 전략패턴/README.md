## 팩토리 패턴
객체를 사용하는 코드에서 객체 생성 부분을 떼어내 추상화한 패턴이자, 상속 관계에 있는 두 클래스에서 `상위 클래스`가 `중요한 뼈대`, `하위 클래스`에서 객체 생성에 대한 `구체적인 내용`을 결정하는 패턴이다.

- 상위와 하위 클래스가 분리되므로 `느슨한 결합`을 가지며, 상위 클래스에서는 인스턴스 생성방식에 대해 전혀 알 필요 없기 때문에 `더 많은 유연성`
- 객체 생성 로직이 따로 떼어져 있기에 코드를 리팩토링하더라도 한 곳만 고칠 수 있게 되니 `유지 보수성이 증가`된다.


```js
class Latte {
    constructor() {
        this.name = "latte"
    }
}
class Espresso {
    constructor() {
        this.name = "Espresso"
    }
} 

class LatteFactory {
    static createCoffee() {
        return new Latte()
    }
}
class EspressoFactory {
    static createCoffee() {
        return new Espresso()
    }
}
const factoryList = { LatteFactory, EspressoFactory } 
 
class CoffeeFactory {
    static createCoffee(type) {
        const factory = factoryList[type]
        return factory.createCoffee()
    }
}   
const main = () => {
    // 라떼 커피를 주문한다.  
    const coffee = CoffeeFactory.createCoffee("LatteFactory")  
    // 커피 이름을 부른다.  
    console.log(coffee.name) // latte
}
main()
```
CoffeeFactory라는 상위 클래스가 `중요한 뼈대`를 결정하고 하위 클래스인 LatteFactory
가 `구체적인 내용`을 결정하고 있다. 

```
참고로 이는 의존성 주입이라고도 볼 수 있다. 
CoffeeFactory에서 LatteFactory의 인스턴스를 생성하는 것이 아닌 
LatteFactory에서 생성한 인스턴스를 CoffeeFactory에 주입하고 있기 때문이다.
```
또한, CoffeeFactory를 보면 static으로 createCoffee() 정적 메서드를 정의한 것을 알
수 있는데, 정적 메서드를 쓰면 클래스의 인스턴스 없이 호출이 가능하여 메모리를 절약
할 수 있고 개별 인스턴스에 묶이지 않으며 클래스 내의 함수를 정의할 수 있는 장점이 있
다. 

## 전략 패턴
정책 패턴이라고도 하며, 객체들이 할 수 있는 행위 각각에 대해 **전략 클래스를 생성하고**, **유사한 행위들을 캡슐화 하는 인터페이스를 정의**하여, 객체의 행위를 동적으로 바꾸고 싶은 경우 직접 **행위를 수정하지 않고 전략을 바꿔주기만 함으로써** 행위를 유연하게 확장하는 방법을 말합니다.

간단히 말해서 객체가 할 수 있는 행위들 각각을 전략으로 만들어 놓고, 동적으로 행위의 수정이 필요한 경우 전략을 바꾸는 것만으로 행위의 수정이 가능하도록 만든 패턴입니다.

ex) 검색 시 모드 바꿔주는 것, 결제 시 다양한 방법 사용하는 것

```java
import java.text.DecimalFormat;
import java.util.ArrayList;
import java.util.List;
interface PaymentStrategy { 
    public void pay(int amount);
} 

class KAKAOCardStrategy implements PaymentStrategy {
    private String name;
    private String cardNumber;
    private String cvv;
    private String dateOfExpiry;
    
    public KAKAOCardStrategy(String nm, String ccNum, String cvv, String expiryDate){
        this.name=nm;
        this.cardNumber=ccNum;
        this.cvv=cvv;
        this.dateOfExpiry=expiryDate;
    }

    @Override
    public void pay(int amount) {
        System.out.println(amount +" paid using KAKAOCard.");
    }
} 

class LUNACardStrategy implements PaymentStrategy {
    private String emailId;
    private String password;
    
    public LUNACardStrategy(String email, String pwd){
        this.emailId=email;
        this.password=pwd;
    }
    
    @Override
    public void pay(int amount) {
        System.out.println(amount + " paid using LUNACard.");
    }
} 

class Item { 
    private String name;
    private int price; 
    public Item(String name, int cost){
        this.name=name;
        this.price=cost;
    }

    public String getName() {
        return name;
    }

    public int getPrice() {
        return price;
    }
} 

class ShoppingCart { 
    List<Item> items;
    
    public ShoppingCart(){
        this.items=new ArrayList<Item>();
    }
    
    public void addItem(Item item){
        this.items.add(item);
    }
    
    public void removeItem(Item item){
        this.items.remove(item);
    }
    
    public int calculateTotal(){
        int sum = 0;
        for(Item item : items){
            sum += item.getPrice();
        }
        return sum;
    }
    
    public void pay(PaymentStrategy paymentMethod){
        int amount = calculateTotal();
        paymentMethod.pay(amount);
    }
}  

public class HelloWorld{
    public static void main(String []args){
        ShoppingCart cart = new ShoppingCart();
        
        Item A = new Item("kundolA",100);
        Item B = new Item("kundolB",300);
        
        cart.addItem(A);
        cart.addItem(B);
        
        // pay by LUNACard
        cart.pay(new LUNACardStrategy("kundol@example.com", "pukubababo"));
        // pay by KAKAOBank
        cart.pay(new KAKAOCardStrategy("Ju hongchul", "123456789", "123", "12/01"));
    }
}
/*
400 paid using LUNACard.
400 paid using KAKAOCard.
*/
```

앞의 코드는 쇼핑 카트에 아이템을 담아, LUNAcard, KAKAOCard라는 두 개의 전략으로 결제하는 코드이다.

passport에서도 전략패턴을 사용하며, LocalStoragegy전략과 OAuth 전략을 지원해서 전략만 바꿔서 인증한다.

```js
var passport = require('passport')
    , LocalStrategy = require('passport-local').Strategy;

passport.use(new LocalStrategy(
    function(username, password, done) {
        User.findOne({ username: username }, function (err, user) {
          if (err) { return done(err); }
            if (!user) {
                return done(null, false, { message: 'Incorrect username.' });
            }
            if (!user.validPassword(password)) {
                return done(null, false, { message: 'Incorrect password.' });
            }
            return done(null, user);
        });
    }
));
```