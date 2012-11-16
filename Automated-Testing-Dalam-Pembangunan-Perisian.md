_Testing_ adalah sebahagian daripada proses pembangunan perisian komputer. Setiap kali baris-baris kod ditulis, diubah atau dipadam, ia mesti melalui proses testing bagi memastikan program tersebut masih lagi berjalan sebagaimana yang diharapkan. Ini bermakna kita sama sekali tidak boleh mengelak daripada melakukan _testing_ terhadap program atau aplikasi yang kita bangunkan.

## Manual Testing
Testing biasanya dilakukan secara manual dengan pengaturcara cuba menjalankan aturcara atau aplikasi yang dibangunkan dan memerhatikan sama ada ia mengeluarkan output yang dikehendaki ataupun semua _features_ berfungsi sebagai mana diharapkan. Ambil contoh aturcara ringkas berikut:-

```python
    function add(num1, num2) {
        return num1 + num2;
    }

    print add(1, 2) . "\n" # akan paparkan 3
    print add(2, 2) . "\n" # akan paparkan 4
```

Andaikan aturcara diatas ditulis menggunakan bahasa PHP dan disimpan dalam fail bernama `add.php`. Untuk menguji aturcara ini, pengaturcara akan `execute` fail tersebut dan memerhatikan sama ada ia memaparkan out yang diharapkan iaitu:-

    3
    4

Aturcara tersebut mungkin boleh dijalankan seperti berikut:-

```bash
    php add.php
    3
    4
```

Di atas kita dapati aturcara tersebut memaparkan output yang kita kehendaki. Sekarang kita andaikan berlaku sedikit kesilapan dalam kod tersebut. Katakan ia ditulis seperti berikut:-

```python
    function add(num1, num2) {
        return num1 * num2;
    }

    print add(1, 2) . "\n"; # akan paparkan 3
    print add(2, 2) . "\n"; # akan paparkan 4
```

Apabila dijalankan, kita akan dapati outputnya berlainan:-

    2
    4

Ini bermakna program tersebut gagal dalam proses _testing_ kita. Cara _testing_ seperti diatas walaupun mudah untuk difahami dan diamalkan ia menjadi amat tidak efektif apabila program semakin berkembang dan kompleks. Ia juga tidak efektif kerana bergantung semata-mata kepada keupayaan mata kita untuk mengesan perbezaan pada output yang dipaparkan. Bukan mahu menidakkan keupayaan mata namun manusia sememangnya tidak sesuai untuk melakukan kerja-kerja leceh dan remeh seperti ini. Disinilah fungsi komputer supaya manusia boleh menumpukan kepada kerja-kerja yang lebih memerlukan daya fikir, intelek serta kreativiti yang tinggi.

## Automated Testing
Dalam proses _testing_ secara manual di atas, kita bergantung kepada tenaga manusia untuk memerhatikan semasa program dijalankan, ia menghasilkan output yang dikehendaki atau tidak. Dalam _automated testing_ kita akan menulis satu aturcara lain bagi menguji aturcara yang dibangunkan. Ini membolehkan kita untuk _delegate_ tugas-tugas menguji tersebut kepada komputer yang sudah semestinya lebih efisyen untuk melaksanakannya.

Contoh kod sebelum ini boleh ditulis seperti berikut untuk <fill in>:-

```php
    function add($num1, $num2) {
        return $num1 * $num2;
    }

    assert(add(1, 2) === 3);
    assert(add(2, 2) === 4);
```

Apabila dijalankan kita akan melihat output seperti berikut:-

    Warning: assert(): Assertion failed in /home/rkiteratai/add.php on line 6

Aah, sekarang anda sudah dapat melihat bagaimana komputer sudah mula mengambil peranan manusia dalam menguji kod aturcara.

## Kategori _Test_
* Unit Test
* Functional Test
* Integration Test
* Stress Test
...

## Test Framework
...

## Contoh Test mengikut bahasa pengaturcaraan

### Unit Test dalam C# (.NET)

Dalam dunia .NET antara _unit test framework_ yang paling awal dan paling meluas digunakan ialah NUnit. Berikut adalah contoh bagaimana ia digunakan.

Katakan kod yang ingin diuji adalah seperti berikut:

```csharp
class Calculator
{
    public int Add(int operand1, int operand2)
    {
        return operand1 + operand2;
    }
}
```

Kita boleh membuat _test fixture_ seperti ini:

```csharp
using NUnit.Framework

[TestFixture]
public class CalculatorTest
{
    [Test]
    public void AddShouldReturnSum()
    {
        var calc = new Calculator();
        var result = calc.Add(1, 2);

        Assert.AreEqual(3, result);
    }
}
```

_Compile_ _test code_ di atas menjadi menjadi _assembly_ (DLL) atau _executable_, dan larikan assembly tersebut menggunakan sama ada _console runner_ (unit-console.exe) atau _GUI runnner_ (nunit-gui.exe).

Contoh antaramuka NUnit GUI Runner
![Contoh antaramuka NUnit GUI Runner](http://www.nunit.org/docs/2.2/img/gui-screenshot.gif)


## Soalan Lazim
### Apa itu _assert_ ?
_Assertion_ merupakan satu proses menguji nilai pada bahagian-bahagian tertentu aturcara bagi memastikan ia sentiasa benar sebagaimana yang diharapkan oleh pengaturcara. Kegagalan pada _assertion_ boleh dianggap sebagai kegagalan langsung aturcara tersebut dan ia mesti dibetulkan. Ini berbeza dengan _error handling_ dimana ia sesuatu yang dijangka akan berlaku dan pengaturcara hanya perlu memastikan langkah-langkah sepatutnya dilakukan untuk menangani _error_ tersebut.[^1]

## Penulis
Nama-nama di bawah adalah penyumbang kepada tulisan ini:-

* Mohd. Kamal bin Mustafa (k4ml) - http://k4ml.github.com/
* Mohd Amree (amree) - http://ieatbinary.com/
* Mior Muhammad Zaki (crynobone) - http://crynobone.com/
* Irwan Azam Ahmad (ryzam) - https://github.com/ryzam

[^1]: http://en.wikipedia.org/wiki/Assertion_(computing)