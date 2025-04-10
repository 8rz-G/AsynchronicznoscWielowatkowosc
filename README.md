# WIELOWĄTKOWOŚĆ

## Co to jest wielowątkowość i jak działa w C#?
W C# używa się klasy *Thread* do tworzenia i zarządzania wątkami. Na przykład, można stworzyć wątek, który wykonuje obliczenia w tle, podczas gdy główny wątek obsługuje interfejs użytkownika. Przykładem jest symulacja długotrwałej operacji, jak liczenie w pętli, w osobnym wątku, co zapobiega blokowaniu aplikacji do czasu zakończenia danego zadania. 
### Jak operować na wątkach w C#?
Operowanie na wątkach obejmuje ich tworzenie, nadawanie priorytetów, ustawianie jako wątki tła i synchronizację. Można na przykład nazwać wątek, ustawić jego priorytet (np. najwyższy) i użyć metod takich jak *Start()*, *Join()* czy *Sleep()*. Przykład:  

```csharp
Thread child = new Thread(CallToChildThread);
child.Name = "MójWątek";
child.Start();
child.Join(); //patrz przypis*
```
*.Join() sprawia, że jeden wątek(w tym wypadku główny) czeka, aż inny wątek skończy swoją pracę.

## Własności i metody dla Thread

| Właściwość/Metoda              | Opis                                                                                          |
|--------------------------------|-----------------------------------------------------------------------------------------------|
| CurrentContext                 | Pobiera bieżący kontekst, w którym jest wykonywany wątek                                      |
| CurrentCulture                 | Pobiera lub ustawia ustawienia regionalne dla obecnego wątku                                  |
| CurrentPrinciple               | Pobiera lub ustawia zabezpieczenia wątku (dla bezpieczeństwa opartego na rolach)              |
| CurrentThread                  | Pobiera obecnie uruchomiony wątek                                                             |
| CurrentUICulture               | Pobiera lub ustawia ustawienia regionalne używane przez Resource Manager                      |
| ExecutionContext               | Pobiera obiekt z informacjami o kontekstach bieżącego wątku                                   |
| IsAlive                        | Pobiera wartość wskazującą, czy bieżący wątek jest aktywny                                    |
| IsBackground                   | Pobiera lub ustawia, czy wątek jest wątkiem tła                                               |
| IsThreadPoolThread             | Pobiera wartość wskazującą, czy wątek należy do puli wątków (ThreadPool)                      |
| ManagedThreadId                | Pobiera unikatowy identyfikator bieżącego zarządzanego wątku                                  |
| Name                           | Pobiera lub ustawia nazwę wątku                                                               |
| Priority                       | Pobiera lub ustawia priorytet bieżącego wątku                                                 |
| ThreadState                    | Pobiera informacje o stanie bieżącego wątku                                                   |
| Abort()                        | Przerywa wątek, wywołując wyjątek ThreadAbortException                                        |
| AllocateDataSlot()             | Przydziela anonimowe gniazdo danych dla wszystkich wątków (lepiej używać ThreadStatic)        |
| AllocateNamedDataSlot(string)  | Przydziela nazwane gniazdo danych dla wszystkich wątków (lepiej używać ThreadStatic)          |
| BeginCriticalRegion()          | Powiadamia, że kod wchodzi w obszar krytyczny, gdzie przerwanie może zaszkodzić aplikacji     |
| BeginThreadAffinity()          | Powiadamia, że kod zależy od tożsamości fizycznego wątku systemu operacyjnego                 |
| EndCriticalRegion()            | Powiadamia, że kod opuszcza obszar krytyczny                                                  |
| EndThreadAffinity()            | Powiadamia, że kod przestał zależeć od tożsamości fizycznego wątku                            |
| FreeNamedDataSlot(string)      | Usuwa powiązanie nazwy z gniazdem danych (lepiej używać ThreadStatic)                         |
| GetData(LocalDataStoreSlot)    | Pobiera wartość z gniazda danych bieżącego wątku                                              |
| GetDomain()                    | Zwraca bieżącą domenę, w której działa wątek                                                  |
| GetNamedDataSlot(string)       | Wyszukuje nazwane gniazdo danych (lepiej używać ThreadStatic)                                 |
| Interrupt()                    | Przerywa wątek w stanie WaitSleepJoin                                                         |
| Join()                         | Blokuje wątek wywołujący, aż dany wątek się zakończy, obsługując COM i SendMessage            |
| MemoryBarrier()                | Synchronizuje dostęp do pamięci                                                               |
| ResetAbort()                   | Anuluje przerwanie wątku                                                                      |
| SetData(LocalDataStoreSlot, Object) | Ustawia dane w gnieździe bieżącego wątku (lepiej używać ThreadStatic)                    |
| Start()                        | Rozpoczyna działanie wątku                                                                    |
| Sleep(int)                     | Zatrzymuje wątek na określony czas (w milisekundach)                                          |
| SpinWait(int)                  | Zmusza wątek do czekania przez określoną liczbę iteracji                                      |
| VolatileRead(ref type)         | Odczytuje ostatnią wartość pola zapisaną przez dowolny procesor (różne przeciążenia)          |
| VolatileWrite(ref type, value) | Zapisuje wartość pola, widoczna od razu dla wszystkich procesorów (różne przeciążenia)        |
| Yield()                        | Przerywa wątek, by uruchomić inny gotowy na tym samym procesorze, jeśli taki istnieje         |


To pozwala na kontrolowanie, jak wątek wpływa na aplikację, np. czy powinien działać w tle i nie blokować zamknięcia programu.  

### Rozróżniamy kilka poziomów wątków. 
1. Lowest - Najniższy priorytet. Wątek będzie wykonywany tylko wtedy, gdy inne wątki o wyższych priorytetach nie konkurują o czas procesora.
2. BelowNormal - Priorytet poniżej normalnego. Wątek ustępuje miejsca wątkom o priorytecie normalnym i wyższym. 
3. Normal(domyślny) - Standardowy priorytet, wszystkie nowo tworzone wątki mają ten priorytet, jeśli nie określono inaczej.
4. AboveNormal - Priorytet ponad poprzednikami, stosowany dla zadań wymagających nieco szybszego wykonania, ale nie krytycznych.
5. Highest - Najwyższy priorytet. Wątek będzie preferowany przez *scheduler** w stosunku do wszystkich innych priorytetów. Przede wszystkim używany do zadań czasu rzeczywistego, ale należy stosować ostrożnie, aby nie zmniejszyć wydajności.

*Zarządza przydziałem czasu procesora dla wątków na podstawie ich priorytetów, MIMO TO ostateczne decyzje zależą TYLKO od systemu operacyjnego.

[Docsy microsoftu o priorytetach wątków dla ambitnych](https://learn.microsoft.com/en-us/dotnet/api/system.threading.threadpriority?view=net-9.0)
[Redditowy wątek](https://www.reddit.com/r/dotnet/comments/1c0hnxe/difference_between_multithread_and_async_tell_me/?tl=pl)

##### Priorytet ustawia się za pomocą właściwości Priority obiektu Thread. Przykład:
```csharp
Thread thread = new Thread(SomeMethod);
thread.Priority = ThreadPriority.AboveNormal;
thread.Start();
```

## ThreadPool
ThreadPool w C# to taka "gotowa paczka" wątków, które czekają na zadania. Zamiast samemu tworzyć nowy wątek za każdym razem, gdy masz coś do zrobienia, dajesz zadanie do ThreadPool, a on wybiera wolny wątek i wykonuje je. Jak praca się kończy, wątek wraca do puli i czeka na kolejne zadanie.
```csharp
using System;
using System.Diagnostics;
using System.Threading;

class Program
{
    static void Main()
    {
        ThreadPool.QueueUserWorkItem(Zadanie);
        Console.WriteLine("JAM JEST GŁÓWNY WĄTEK, JA DZIAŁAM DALEJ");
        Console.ReadLine();
    }

    static void Zadanie(object state)
    {
        Console.WriteLine("ALE JAZDA PŁYWAM W WĄTKO-BASENIE!!11!!1!1");
        Thread.Sleep(1000);
        Console.WriteLine("ratownik mnie wytargał, ponoć nie wolno sikać :( ");
    }
}
```
Wynikiem powyższego kodu będzie: 

![{WYNIK KODU1}](https://github.com/user-attachments/assets/bcffe95d-5170-449e-8cf4-02c833169d9a)

Tu spoko przykład który znalazłem w prezentacji ze studiów, QueueUserWorkItem wrzuca metodę Zadanie do ThreadPool przekazując parametr. Główny wątek idzie dalej, a zadanie robi się w tle.

```csharp
using System;
using System.Diagnostics;
using System.Threading;

class Program
{
    static void Main()
    {
        string text = Console.ReadLine();
        ThreadPool.QueueUserWorkItem(Zadanie, text);
        Console.WriteLine("Dzialam dalej");
    }

    static void Zadanie(object state)
    {
        Console.WriteLine($"Dostałem wiadomość: {state}");
    }
}
```

Wynikiem powyższego kodu będzie: 

![{WYNIK KODU2}](https://github.com/user-attachments/assets/6391e7c3-1b6d-46fb-a218-18f4473b5c26)

## Lock
Lock sprawia, że tylko jeden wątek na raz może wejść do kawałka kodu. Jak jeden wątek go używa, inne muszą czekać, aż skończy i go odblokuje.

Spoko, tylko po co to?

Żeby wątki nie przeszkadzały sobie nawzajem. Bez locka dwa wątki mogą np. jednocześnie zapisywać do tej samej zmiennej i wynik będzie błędny.
Lock to podstawowe narzędzie w C# do synchronizacji wątków, idealne do prostych scenariuszy, gdzie potrzebna jest wyłączność dostępu.

Przykład od AI:
```csharp
using System;
using System.Threading;

class Program
{
    static int licznik = 0;
    static object klucz = new object();

    static void Main()
    {
        Thread w1 = new Thread(Dodaj);
        Thread w2 = new Thread(Dodaj);
        w1.Start();
        w2.Start();
        w1.Join();
        w2.Join();
        Console.WriteLine($"Licznik: {licznik}"); // Powinno w wyniku być 200
    }

    static void Dodaj()
    {
        for (int i = 0; i < 100; i++)
        {
            lock (klucz)
            {
                licznik++; // Tylko jeden wątek na raz tu wchodzi
            }
        }
    }
}
```

Wynikiem powyższego kodu będzie:

![{WYNIK KODU3}](https://github.com/user-attachments/assets/c78de0c5-9341-4cfa-bf2c-4cd49087e9e6)

[Wątek o Locku z StackOverflow, chłop w odpowiedzi wyczerpał większość pytań](https://stackoverflow.com/questions/21544823/what-precisely-does-a-lock-lock)

**A co najciekawsze, Lock to tak naprawdę skrót od używania klasy Monitor**

## Mutex

Mutex (skrót od "mutual exclusion", czyli wzajemne wykluczenie) pilnuje, żeby tylko jeden wątek albo proces na raz mógł używać jakiegoś zasobu. Działa jak lock, ale jest mocniejszy, bo może działać nie tylko w jednym programie, ale też między różnymi procesami na komputerze. To taki strażnik texasu, który pilnuje porządku.

Spoko, tylko po co to?

Żeby uniknąć sytuacji w której wiele wątków lub programów chce zmieniać to samo na raz – np. zapisywać do tego samego pliku albo używać tej samej pamięci. Jeszcze prościej mówiąc, tworzysz Mutex, dajesz mu nazwę (jeśli ma działać między procesami) i używasz go, żeby "zamknąć" dostęp. Wątek, który go trzyma, może działać, a inne czekają, aż się zwolni.
Używaj go, gdy potrzebujesz synchronizacji na poziomie systemu, ale pamiętaj, że jest wolniejszy niż Lock.

Prosty przykład:
```csharp
using System;
using System.Threading;

class Program
{
    static Mutex mutex = new Mutex(); // Tworzymy mutex
    static int licznik = 0;

    static void Main()
    {
        Thread w1 = new Thread(Dodaj);
        Thread w2 = new Thread(Dodaj);
        w1.Start();
        w2.Start();
        w1.Join();
        w2.Join();
        Console.WriteLine($"Licznik: {licznik}");
    }

    static void Dodaj()
    {
        for (int i = 0; i < 100; i++)
        {
            mutex.WaitOne(); 
            licznik++; 
            mutex.ReleaseMutex();
        }
    }
}
```

Wynikiem powyższego kodu będzie:

![{WYNIK KODU4}](https://github.com/user-attachments/assets/c0c80a24-ad7a-404e-a69c-92a201eddd51)

Ważniejsze metody:
- WaitOne() - Wątek czeka na wolne miejsce i zabiera je. Można podać limit czasu (np. WaitOne(1000) – czeka 1 sekundę).
- ReleaseMutex(): Zwalnia mutex, ale tylko wątek, który go zajął, może to zrobić (inaczej wyjątek).

## Semaphore 
Semaphore to licznik miejsc, który kontroluje, ile wątków na raz może coś robić. Przykładowo, jeśli są dostępne 3 wątki i posiadamy 5 zadań, wykorzysta dostępne wątki na tylko 3 zadania podczas gdy pozostałe 2 będą musiały czekać na swoją kolej. To sposób, żeby nie robić tłoku tam, gdzie zasoby są ograniczone.
Tworzysz Semaphore i mówisz, ile "miejsc" ma być. Wątek wchodzi, biorąc jedno miejsce, a jak kończy, oddaje je z powrotem.

Trochę praktyki: 

```csharp
using System;
using System.Threading;

class Program
{
    static Semaphore semafor = new Semaphore(2, 2); // 2 miejsca, max 2
    static void Main()
    {
        Thread[] watki = new Thread[5];
        for (int i = 0; i < 5; i++)
        {
            watki[i] = new Thread(Pracuj);
            watki[i].Start(i + 1);
        }
        foreach (var w in watki) w.Join();
    }

    static void Pracuj(object numer)
    {
        Console.WriteLine($"Wątek {numer} czeka...");
        semafor.WaitOne();
        Console.WriteLine($"Wątek {numer} wchodzi!");
        Thread.Sleep(2000);
        Console.WriteLine($"Wątek {numer} wychodzi!");
        semafor.Release(); 
    }
}
```

Wynikiem powyższego kodu będzie:

![{WYNIK KODU5}](https://github.com/user-attachments/assets/a999bf95-3e2b-4775-9c64-8ad99534aca4)

Szczerze chuj wi czemu ten wynik taki brzydki jest, mimo wszystko dobrze obrazuje o co chodzi d-_-b

Ważniejsze metody:
- Release() - Oddaje miejsce z powrotem. Można oddać więcej miejsc niż wzięto (np. Release(2)), ale ostrożnie, bo może rzucić wyjątek, jeśli przekroczysz maximum.
- WaitOne() - Jak poprzednio.

Bardziej jako ciekawostka ale istnieje coś taiego jak SemaphoreSlim - lżejsza wersja Semaphore, tylko dla jednego procesu, przede wszystkim szybsza.

# ASYNCHRONICZNOŚĆ

Asynchroniczność to sposób, żeby program robił kilka rzeczy na raz, ale bez czekania, aż każda się skończy. Zamiast stać w miejscu i czekać (np. na pobranie pliku z internetu), program deleguje zadanie do wykonania w tle podczas gdy idzie dalej. Dzięki temu aplikacja działa płynnie, nie zawiesza się i może robić inne rzeczy w międzyczasie. 
Asynchroniczność opiera się na słowach kluczowych async i await oraz na typie Task. Zamiast robić coś od razu, (obrazowo mówiąc) tworzysz zamiar zrobienia tego (Task), a potem czekasz na wynik tylko wtedy, gdy jest potrzebny (await), nie blokując reszty programu.
``` csharp
using System;
using System.Net.Http;
using System.Threading.Tasks;

class Program
{
    static async Task Main()
    {
        Console.WriteLine("Pobieram dane...");
        string wynik = await PobierzStrone();
        Console.WriteLine($"Dostałem: {wynik.Substring(0, 50)}...");
    }

    static async Task<string> PobierzStrone()
    {
        using (HttpClient client = new HttpClient())
        {
            return await client.GetStringAsync("https://www.google.com");
        }
    }
}
```

Task w tym wypadku to tylko deklaracja że coś się wykona. Może być w tle, na wątku z ThreadPool, albo w ogóle bez wątku.

Wynikiem powyższego kodu będzie: 

![{WYNIK KODU6}](https://github.com/user-attachments/assets/6cda33f6-374f-4cf4-a7e8-991c959e2d46)


### Różnica od wątków
- Thread - Tworzysz nowy wątek i ręcznie nim zarządzasz.
- Task z async - System sam decyduje, jak to zrobić (często bez nowych wątków). Łatwiejsze i wydajniejsze ale mniej elastyczne.

### Potencjalne problemy z asynchronicznością
- Deadlock - Jeśli źle użyjesz await w głównym wątku i zablokujesz go (np. .Result zamiast await), program się zawiesi.
- Zbyt wiele awaitów - Może spowolnić, jeśli niepotrzebnie czekasz na każdą rzecz po kolei zamiast równolegle.

Poza podstawowymi instrukcjami istnieje jeszcze Task.WhenAll: 

```csharp
static async Task Main()
{
    Task<int> zadanie1 = PoliczCos(1);
    Task<int> zadanie2 = PoliczCos(2);
    int[] wyniki = await Task.WhenAll(zadanie1, zadanie2);
    Console.WriteLine($"Wyniki: {wyniki[0]}, {wyniki[1]}");
}

static async Task<int> PoliczCos(int x)
{
    await Task.Delay(1000);
    return x * 10;
}
```

Wynik powyższego kodu: 

![{WYNIK KODU7}](https://github.com/user-attachments/assets/110aa4e6-8223-4ee8-a588-b9c93db4d227)
[Podstawy asynchroniczności](https://modestprogrammer.pl/asynchronicznosc-w-csharpie-podstawy-programowania-wielowatkowego)
[Szczegółowe rozpisanie asynchronicznego C# i jak wygląda to w procesorze](https://cezarywalenciuk.pl/blog/programing/asynchroniczny-c)

Podsumowując czym się różnią Task a Thread:
Thread daje pełną kontrolę nad wątkami, ale wymaga więcej pracy, jest kosztowny w tworzeniu i pochodzi z czasów .NET 1.0. Task jest nowszy (od .NET 4.0), prostszy w użyciu dzięki async/await, wydajniejszy dzięki ThreadPool i pozwala systemowi samemu zarządzać wykonaniem.




