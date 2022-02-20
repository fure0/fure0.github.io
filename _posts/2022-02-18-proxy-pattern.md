---
layout: post
categories: "DesignPattern"
title: "Proxy pattern"
author: "TY_K"
---

### [구조 패턴] 프록시 패턴(Proxy Pattern) 이해

프록시 패턴은 디자인 패턴 중 이해하기 쉬운 패턴에 속합니다.

> 프록시 패턴은 구조 패턴(Structural Pattern) 중 하나로, 어떤 다른 객체로 접근하는 것을 통제하기 위해서 그 객체의 대리자(surrogate)나 자리표시자(placeholder)의 역할을 하는 객체를 제공하는 패턴입니다.

### 구조 패턴(Structural Pattern)이란?

구조 패턴이란 작은 클래스들을 상속과 합성을 이용하여 더 큰 클래스를 생성하는 방법을 제공하는 패턴입니다.

이 패턴을 사용하면 서로 독립적으로 개발한 클래스 라이브러리를 마치 하나인 양 사용할 수 있습니다. 또, 여러 인터페이스를 합성(Composite)하여 서로 다른 인터페이스들의 통일된 추상을 제공합니다.

구조 패턴의 중요한 포인트는 인터페이스나 구현을 복합하는 것이 아니라 객체를 합성하는 방법을 제공한다는 것입니다. 이는 **컴파일 단계에서가 아닌 런타임 단계에서 복합 방법이나 대상을 변경할 수 있다는 점에서 유연성을 갖습니다.**

### 프록시 패턴(Proxy Pattern) 이해 및 예제

> 프록시 패턴은 어떤 다른 객체로 접근하는 것을 통제하기 위해서 그 객체의 대리자(surrogate)나 자리표시자(placeholder)의 역할을 하는 객체를 제공하는 패턴입니다.

예를 들어, 우리가 시스템 명령어를 실행하는 객체를 갖고 있을 때 우리가 그 객체를 사용하는 것이라면 괜찮지만 만약 그 객체를 클라이언트에게 제공하려고 한다면 **클라이언트 프로그램이 우리가 원치 않는 파일을 삭제하거나 설정을 변경하는 등의 명령을 내릴 수 있기 때문에 심각한 문제를 초래할 수 있습니다.**

프록시 패턴은 클라이언트에게 접근에 대한 컨트롤을 제공하여 위와 같은 문제를 해결합니다.

예제를 살펴보겠습니다.

먼저 CommandExecutor 라는 명령을 실행하는 메소드를 제공하는 인터페이스를 정의하겠습니다.

#### CommandExecutor.java

```java
public interface CommandExecutor {
 
    public void runCommand(String cmd) throws Exception;
}
```

자, 이번에는 만약 프록시 패턴을 적용하지 않고 앞서 말씀드린 것처럼 그냥 클라이언트에게 명령에 대한 권한을 전부 넘겨주는 객체를 생성해보겠습니다.

#### CommandExecutorImpl.java

```java
public class CommandExecutorImpl implements CommandExecutor {
 
    @Override
    public void runCommand(String cmd) throws IOException {
        //some heavy implementation
        Runtime.getRuntime().exec(cmd);
        System.out.println("'" + cmd + "' command executed.");
    }
}
```

보시다시피 CommandExecutorImpl 클래스에서는 runCommand()의 파라미터로 받은 cmd 명령어를 그대로 수행하고 있습니다. 이렇게 구현할 경우에는 앞서 말씀드린 바와 같이 원치 않는 파일 삭제나 설정 변경 등에 문제가 발생할 가능성이 높아집니다.

**그렇다면 이를 해결하기 위해 프록시 객체를 두어 관리자(Admin) 계정이 아닐 경우에는 rm 이라는 명령어에 대해 수행하지 못하도록 구현해보도록 하겠습니다.**

CommandExecutorProxy.java

```java
public class CommandExecutorProxy implements CommandExecutor {
    private boolean isAdmin;
    private CommandExecutor executor;
	
    public CommandExecutorProxy(String user, String pwd){
        if("ReadyKim".equals(user) && "correct_pwd".equals(pwd))
            isAdmin = true;
        executor = new CommandExecutorImpl();
    }
	
    @Override
    public void runCommand(String cmd) throws Exception {
        if(isAdmin){
            executor.runCommand(cmd);
        }else{
            if(cmd.trim().startsWith("rm")){
                throw new Exception("rm command is not allowed for non-admin users.");
            }else{
                executor.runCommand(cmd);
            }
        }
    }
}
```

자 이번에는 위에서 작성한 코드들을 테스트 해보도록 하겠습니다.

#### ProxyPatternTest.java

```java
public class ProxyPatternTest {
 
    public static void main(String[] args){
        CommandExecutor executor = new CommandExecutorProxy("ReadyKim", "wrong_pwd");
        try {
            executor.runCommand("ls -ltr");
            executor.runCommand("rm -rf abc.pdf");
        } catch (Exception e) {
            System.out.println("Exception Message::"+e.getMessage());
        }	
    }
}
```

```java
'ls -ltr' command executed.
Exception Message::rm command is not allowed for non-admin users.
```

결과에서 보다시피 Admin 계정의 ID와 Password가 틀렸기 때문에 프록시 객체가 rm 명령어에 대한 수행을 거부하였고 그 결과로 Exception을 던지게 됐습니다.

**프록시 패턴은 이렇듯 어떤 객체에 대하여 접근할 때에 Wrapper Class를 두어 접근에 대한 통제(Control access)를 위해 사용합니다.**

#### 원문링크

[[구조 패턴] 프록시 패턴(Proxy Pattern) 이해 및 예제][link1]

[link1]: https://readystory.tistory.com/132?category=822867 "link1"

