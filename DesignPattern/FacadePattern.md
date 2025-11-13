# 퍼사드 패턴(외관 패턴)

프로젝트 업데이트 과정에서 인스턴스 생성에 필요한 클래스들을 모두 DI(의존성 주입) 방식으로 전환하였는데, 이로 인해 기존에 static으로 관리되던 객체들도 주입받아야 했고
그 결과 생성자 파라미터 개수가 과도하게 많아지고 코드 가독성이 저하되는 문제가 발생하였다.

이 문제를 해결하기 위해 퍼사드(Facade) 패턴을 적용하려 한다.
퍼사드 패턴은 여러 개의 복잡한 의존 객체들을 하나의 단일 인터페이스로 감싸는 구조적 디자인 패턴이다.
이를 통해 외부에서는 퍼사드 클래스 하나만 주입받으면 내부적으로 필요한 의존성들을 한 번에 관리할 수 있다.

ex) 
자동차 시동을 걸 때 엔진, 연료펌프, 스타터모터 각각 호출하는 대신
그냥 car.Start() 한 번만 부르면 내부에서 다 처리되게 하는 것

## 일반 코드
```csharp
public class Engine
{
    public void TurnOn() => Console.WriteLine("엔진 가동");
}

public class FuelPump
{
    public void Pump() => Console.WriteLine("연료 공급");
}

public class Starter
{
    public void Activate() => Console.WriteLine("스타터 작동");
}

var engine = new Engine();
var pump = new FuelPump();
var starter = new Starter();

pump.Pump();
engine.TurnOn();
starter.Activate();
```

## 퍼사드 패턴을 적용한 코드
```csharp
public class Car
{
    private readonly Engine _engine;
    private readonly FuelPump _fuelPump;
    private readonly Starter _starter;

    public Car()
    {
        _engine = new Engine();
        _fuelPump = new FuelPump();
        _starter = new Starter();
    }

    public void Start()
    {
        _fuelPump.Pump();
        _engine.TurnOn();
        _starter.Activate();
    }
}

var car = new Car();
car.Start();
```

# DI 환경에서 적용

## 일반 코드
```csharp
public class WorkOrderService(
    IPrinterManager printerManager,
    IDataSyncService dataSync,
    IJobFileGenerator jobFile,)
{
}
```
## 퍼사드 패턴을 적용한 코드

```csharp
public class PrinterFacade
{
    private readonly IPrinterManager _printer;
    private readonly IDataSyncService _dataSync;
    private readonly IJobFileGenerator _jobFile;

    public PrinterFacade(IPrinterManager printer, IDataSyncService dataSync, IJobFileGenerator jobFile)
    {
        _printer = printer;
        _dataSync = dataSync;
        _jobFile = jobFile;
    }

    public void PrintWorkOrder()
    {
        var data = _dataSync.GetData();
        var job = _jobFile.Create(data);
        _printer.Print(job);
    }
}
```

```csharp
public class PrinterFacade
{
    private readonly IPrinterManager _printer;
    private readonly IDataSyncService _dataSync;
    private readonly IJobFileGenerator _jobFile;

    public PrinterFacade(IPrinterManager printer, IDataSyncService dataSync, IJobFileGenerator jobFile)
    {
        _printer = printer;
        _dataSync = dataSync;
        _jobFile = jobFile;
    }

    public void PrintWorkOrder()
    {
        var data = _dataSync.GetData();
        var job = _jobFile.Create(data);
        _printer.Print(job);
    }
}

// 실제 사용
public class WorkOrderService
{
    private readonly PrinterFacade _printerFacade;

    public WorkOrderService(PrinterFacade printerFacade)
    {
        _printerFacade = printerFacade;
    }

    public void Process()
    {
        _printerFacade.PrintWorkOrder();
    }
}

```

