---
layout: post
categories: "DesignPattern"
title: "Prototype pattern"
author: "TY_K"
---

### [생성 패턴] 프로토타입 패턴(Prototype Pattern) 이해

프로토타입은 주로 실제 제품을 만들기에 앞서 대략적인 샘플 정도의 의미로 사용되는 단어입니다.

프로토타입 패턴은 객체를 생성하는 데 비용(시간과 자원)이 많이 들고, 비슷한 객체가 이미 있는 경우에 사용되는 생성 패턴 중 하나입니다.

**프로토타입 패턴은 Original 객체를 새로운 객체에 복사하여 우리의 필요에 따라 수정하는 메커니즘을 제공합니다.**

이 패턴은 복사를 위하여 Java에서 제공하는 clone()을 사용합니다.

### 프로토타입 패턴은 생성 패턴(Creational Pattern) 중 하나이다.

생성 패턴은 인스턴스를 만드는 절차를 추상화하는 패턴입니다.

생성 패턴에 속하는 패턴들은 객체를 생성, 합성하는 방법이나 객체의 표현 방법을 시스템과 분리해줍니다.

생성 패턴은 시스템이 상속(inheritance) 보다 복합(composite) 방법을 사용하는 방향으로 진화되어 가면서 더 중요해지고 있습니다.

생성 패턴에서는 중요한 이슈가 두 가지 있습니다.

1. 생성 패턴은 시스템이 어떤 Concrete Class를 사용하는지에 대한 정보를 캡슐화합니다.
2. 생성 패턴은 이들 클래스의 인스턴스들이 어떻게 만들고 어떻게 결합하는지에 대한 부분을 완전히 가려줍니다.

쉬운 말로 정리하자면, 생성 패턴을 이용하면 무엇이 생성되고, 누가 이것을 생성하며, 이것이 어떻게 생성되는지, 언제 생성할 것인지 결정하는 데 유연성을 확보할 수 있게 됩니다.

### 프로토타입 패턴(Prototype Pattern)의 이해 및 예제

앞서 말씀드린 것처럼 프로토타입 패턴은 객체를 생성하는 데 시간과 노력이 많이 들고, 이미 유사한 객체가 존재하는 경우에 사용됩니다. 그리고 java의 clone()을 이용하기 때문에 생성하고자 하는 객체에 clone에 대한 Override를 요구합니다. 이때 주의할 점은 반드시 생성하고자 하는 객체의 클래스에서 clone()이 정의되어야 한다는 것입니다.

예를 들어 DB로부터 데이터를 가져오는 객체가 존재한다고 가정해보겠습니다.

만약 DB로부터 가져온 데이터를 우리의 프로그램에서 수차례 수정을 해야하는 요구사항이 있는 경우, 매번 new 라는 키워드를 통해 객체를 생성하여 **DB로부터 항상 모든 데이터를 가져오는 것은 좋은 아이디어가 아닙니다.**

왜냐하면 DB로 접근해서 데이터를 가져오는 행위는 비용이 크기 때문입니다.

따라서 한 번 DB에 접근하여 데이터를 가져온 객체를 필요에 따라 새로운 객체에 복사하여 데이터 수정 작업을 하는 것이 더 좋은 방법입니다.

이때 객체의 복사를 얕은 복사(shallow copy)로 할 지, 깊은 복사(deep copy)로 할 지에 대해서는 선택적으로 행하시면 됩니다.

샘플 코드를 통해 이해를 돕도록 하겠습니다.

실제 DB와 연동되는 샘플 코드를 작성하는 것은 다소 복잡할 수 있으니 쉽게 직원의 명단을 갖고 있는 Employees 클래스를 통해 살펴보겠습니다.

```java
public class Employees implements Cloneable{
 
    private List<String> empList;
	
    public Employees(){
        empList = new ArrayList<String>();
    }
	
    public Employees(List<String> list){
        this.empList=list;
    }
    
    public void loadData(){
        //read all employees from database and put into the list
        empList.add("Pankaj");
        empList.add("Raj");
        empList.add("David");
        empList.add("Lisa");
    }
	
    public List<String> getEmpList() {
        return empList;
    }
 
    @Override
    public Object clone() throws CloneNotSupportedException{
        List<String> temp = new ArrayList<String>();
        for(String s : this.empList){
            temp.add(s);
        }
        return new Employees(temp);
    }
	
}
```

위 코드를 보시면 **clone() 메소드를 재정의하기 위해 Cloneable 인터페이스를 구현**한 것을 확인할 수 있습니다. 여기서 사용되는 **clone()은 empList에 대하여 깊은 복사(deep copy)를 실시**합니다.

이번에는 위에서 작성한 코드를 메인에서 테스트해보도록 하겠습니다.

```java
public class PrototypePatternTest {
 
    public static void main(String[] args) throws CloneNotSupportedException {
        Employees emps = new Employees();
        emps.loadData();
		
        //Use the clone method to get the Employee object
        Employees empsNew = (Employees) emps.clone();
        Employees empsNew1 = (Employees) emps.clone();
        List<String> list = empsNew.getEmpList();
        list.add("John");
        List<String> list1 = empsNew1.getEmpList();
        list1.remove("Pankaj");
		
        System.out.println("emps List: "+emps.getEmpList());
        System.out.println("empsNew List: "+list);
        System.out.println("empsNew1 List: "+list1);
    }
 
}
```

```java
emps List: [Pankaj, Raj, David, Lisa]
empsNew List: [Pankaj, Raj, David, Lisa, John]
empsNew1 List: [Raj, David, Lisa]
```

만약 Employees 클래스에서 clone()을 제공하지 않았다면, DB로부터 매번 employee 리스트를 직접 가져와야 했을 것이고, 그로 인해 상당히 큰 비용이 발생했을 것입니다.

하지만 프로토타입을 사용한다면 1회의 DB 접근을 통해 가져온 데이터를 복사하여 사용한다면 이를 해결할 수 있습니다. **(객체를 복사하는 것이 네트워크 접근이나 DB 접근보다 훨씬 비용이 적습니다.)**

#### 원문링크

[[생성 패턴] 프로토타입 패턴(Prototype Pattern) 이해 및 예제][link1]

[link1]: https://readystory.tistory.com/122?category=822867 "link1"