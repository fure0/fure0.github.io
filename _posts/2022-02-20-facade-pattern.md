---
layout: post
categories: "DesignPattern"
title: "Facade pattern"
author: "TY_K"
---

### [구조 패턴] 퍼사드 패턴(Facade Pattern) 이해

> 퍼사드 패턴(Facade Pattern)은 Flyweight 패턴, Adapter 패턴, Decorator 패턴처럼 구조 패턴 중 하나로, 클라이언트가 쉽게 시스템과 상호작용 할 수 있도록 도와주는 패턴입니다.

### 구조 패턴(Structural Pattern)이란?

구조 패턴이란 작은 클래스들을 상속과 합성을 이용하여 더 큰 클래스를 생성하는 방법을 제공하는 패턴입니다.

이 패턴을 사용하면 서로 독립적으로 개발한 클래스 라이브러리를 마치 하나인 양 사용할 수 있습니다. 또, 여러 인터페이스를 합성(Composite)하여 서로 다른 인터페이스들의 통일된 추상을 제공합니다.

구조 패턴의 중요한 포인트는 인터페이스나 구현을 복합하는 것이 아니라 객체를 합성하는 방법을 제공한다는 것입니다. 이는 **컴파일 단계에서가 아닌 런타임 단계에서 복합 방법이나 대상을 변경할 수 있다는 점에서 유연성을 갖습니다.**

### 퍼사드 패턴 이해 및 예제

디자인 패턴의 교과서인 GoF에서는 퍼사드 패턴에 대해 다음과 같이 정의하고 있습니다.

**퍼사드 패턴은 서브시스템을 더 쉽게 사용할 수 있도록 higher-level 인터페이스를 정의하고, 제공합니다.**

이해를 돕기 위해 예시를 들어보겠습니다.

MySql 과 Oracle 데이터베이스를 사용하는 인터페이스가 있고, 이를 이용해 HTML 리포트 또는 PDF 리포트를 생성해야 한다고 가정해보도록 하겠습니다.

우리는 이 인터페이스들을 서로 조합하여 리포트를 만들어야 합니다. 지금은 database connection 방법 2가지, generate report 방법 2가지의 인터페이스를 가지고서 조합을 해야 하지만 만약 경우의 수가 더 복잡해질 경우 일반적인 방법으로는 관리하기가 어려워질 것입니다.

그래서 우리는 퍼사드 패턴을 적용하여 wrapper 인터페이스를 제공하여 클라이언트가 보다 쉽게 관리할 수 있도록 도와보겠습니다.

#### MySqlHelper.java

```java
import java.sql.Connection;
 
public class MySqlHelper {
	
	public static Connection getMySqlDBConnection(){
		// 실제 커넥션을 리턴해야 하지만, 예제이기에 null 을 리턴하겠습니다.
		return null;
	}
	
	public void generateMySqlPDFReport(String tableName, Connection con){
		// get data from table and generate pdf report
	}
	
	public void generateMySqlHTMLReport(String tableName, Connection con){
		// get data from table and generate pdf report
	}
}
```

#### OracleHelper.java

```java
import java.sql.Connection;
 
public class OracleHelper {
 
	public static Connection getOracleDBConnection(){
		// 실제 커넥션을 리턴해야 하지만, 예제이기에 null 을 리턴하겠습니다.
		return null;
	}
	
	public void generateOraclePDFReport(String tableName, Connection con){
		// get data from table and generate pdf report
	}
	
	public void generateOracleHTMLReport(String tableName, Connection con){
		// get data from table and generate pdf report
	}
	
}
```

이번에는 퍼사드 패턴 인터페이스를 생성해보겠습니다. 타입 안정성을 위해서 디비 타입과 리포트 타입에 대해서는 Enum을 사용하도록 하겠습니다.

#### HelperFacade.java

```java
import java.sql.Connection;
 
public class HelperFacade {
 
	public static void generateReport(DBTypes dbType, ReportTypes reportType, String tableName){
		Connection con = null;
		switch (dbType){
		case MYSQL: 
			con = MySqlHelper.getMySqlDBConnection();
			MySqlHelper mySqlHelper = new MySqlHelper();
			switch(reportType){
			case HTML:
				mySqlHelper.generateMySqlHTMLReport(tableName, con);
				break;
			case PDF:
				mySqlHelper.generateMySqlPDFReport(tableName, con);
				break;
			}
			break;
		case ORACLE: 
			con = OracleHelper.getOracleDBConnection();
			OracleHelper oracleHelper = new OracleHelper();
			switch(reportType){
			case HTML:
				oracleHelper.generateOracleHTMLReport(tableName, con);
				break;
			case PDF:
				oracleHelper.generateOraclePDFReport(tableName, con);
				break;
			}
			break;
		}
		
	}
	
	public static enum DBTypes{
		MYSQL,ORACLE;
	}
	
	public static enum ReportTypes{
		HTML,PDF;
	}
}
```

위 코드가 모두 준비되었다면 이제 메인 함수를 통해 테스트해보겠습니다.

#### FacadePatternTest.java

```java
import java.sql.Connection;
 
import com.journaldev.design.facade.HelperFacade;
import com.journaldev.design.facade.MySqlHelper;
import com.journaldev.design.facade.OracleHelper;
 
public class FacadePatternTest {
 
	public static void main(String[] args) {
		String tableName="Employee";
		
		//generating MySql HTML report and Oracle PDF report using Facade
		HelperFacade.generateReport(HelperFacade.DBTypes.MYSQL, HelperFacade.ReportTypes.HTML, tableName);
		HelperFacade.generateReport(HelperFacade.DBTypes.ORACLE, HelperFacade.ReportTypes.PDF, tableName);
	}
}
```

보시다시피 퍼사드 패턴을 사용하게 되면 클라이언트 사이드에서 무겁게 로직을 작성할 필요 없이 쉽고 깔끔하게 리포트를 생성할 수 있게 됩니다.

#### 퍼사드 패턴의 중요 포인트!

* 퍼사드 패턴은 클라이언트 어플리케이션의 헬퍼 역할을 하는 것이지, 서브시스템 인터페이스를 숨기는 것은 아닙니다.
* 퍼사드 패턴은 특정 기능에 대해 인터페이스의 수가 확장되고(위 예제로 치면 디비 종류나 리포트 종류가 늘어난다는 등), 시스템이 복잡해질 수 있는 상황에서 사용하기 적합합니다.
* 퍼사드 패턴은 비슷한 작업을 해야하는 다양한 인터페이스들 중 하나의 인터페이스를 클라이언트에 제공해야 할 때 적용하는 것이 좋습니다.
* 팩토리 패턴과 종종 함께 사용됩니다.

#### 원문링크

[[구조 패턴] 퍼사드 패턴(Facade Pattern) 이해 및 예제][link1]

[link1]: https://readystory.tistory.com/193?category=822867 "link1"