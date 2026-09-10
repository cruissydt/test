# Perl 基本功能完整教學

## 目錄
1. 簡介與安裝
2. 第一個程式
3. 變數與資料型態
4. 運算子
5. 字串處理
6. 陣列 (Array)
7. 雜湊 (Hash)
8. 條件判斷
9. 迴圈
10. 子程式 (Subroutine)
11. 參照 (Reference)
12. 正規表示式
13. 檔案讀寫
14. 特殊變數
15. 模組與 CPAN
16. 錯誤處理
17. 簡易物件導向

---

## 1. 簡介與安裝

Perl 是一種泛用的直譯式腳本語言，特別擅長文字處理、系統管理、正規表示式與快速原型開發。

**安裝確認：**
```bash
perl -v          # 檢查版本
which perl       # 確認路徑
```

macOS / Linux 通常已內建 Perl。Windows 使用者可安裝 [Strawberry Perl](http://strawberryperl.com/)。

---

## 2. 第一個程式

建立 `hello.pl`：

```perl
#!/usr/bin/perl
use strict;
use warnings;

print "Hello, World!\n";
```

執行：
```bash
perl hello.pl
```

> **重要慣例**：幾乎每支 Perl 程式開頭都應該加上 `use strict;` 與 `use warnings;`，
> 這會強制宣告變數、並在出錯時給出警告，能省下大量除錯時間。

---

## 3. 變數與資料型態

Perl 有三種主要變數型態，靠**符號（sigil）**區分：

| 符號 | 型態 | 範例 |
|------|------|------|
| `$`  | 純量 (scalar，單一值) | `$name` |
| `@`  | 陣列 (array，多個值) | `@list` |
| `%`  | 雜湊 (hash，鍵值對) | `%map` |

```perl
use strict;
use warnings;

my $name = "Alice";      # 字串
my $age  = 30;           # 數字
my $pi   = 3.14159;

print "$name is $age years old.\n";  # 雙引號會做變數內插
print '$name is not interpolated', "\n";  # 單引號不會
```

- `my` 用來宣告一個「詞法作用域（lexical scope）」的變數，這是現代 Perl 撰寫時的標準做法。

---

## 4. 運算子

```perl
my $a = 10;
my $b = 3;

print $a + $b, "\n";   # 加 13
print $a - $b, "\n";   # 減 7
print $a * $b, "\n";   # 乘 30
print $a / $b, "\n";   # 除 3.33333...
print $a % $b, "\n";   # 取餘數 1
print $a ** $b, "\n";  # 次方 1000

# 字串運算子
my $s1 = "Hello";
my $s2 = "World";
print $s1 . " " . $s2, "\n";  # 字串串接 . 
print $s1 x 3, "\n";          # 字串重複 x -> HelloHelloHello

# 比較運算子（數字用符號，字串用文字）
print "equal\n" if $a == 10;      # 數字相等
print "equal\n" if $s1 eq "Hello"; # 字串相等
# 數字：== != < > <= >= <=>
# 字串：eq ne lt gt le ge cmp
```

---

## 5. 字串處理

```perl
my $str = "Hello, Perl!";

print length($str), "\n";              # 字串長度
print uc($str), "\n";                  # 轉大寫
print lc($str), "\n";                  # 轉小寫
print substr($str, 0, 5), "\n";        # 取子字串 "Hello"
print index($str, "Perl"), "\n";       # 找位置
print reverse($str), "\n";             # 反轉字串（在 scalar 情境下）

my @parts = split(/,\s*/, $str);       # 分割字串
print join(" | ", @parts), "\n";       # 合併字串

# sprintf 格式化
my $formatted = sprintf("Name: %-10s Age: %3d", "Bob", 25);
print "$formatted\n";
```

---

## 6. 陣列 (Array)

```perl
my @fruits = ("apple", "banana", "cherry");

print $fruits[0], "\n";        # 存取單一元素 apple
print scalar(@fruits), "\n";   # 陣列長度 3
print "@fruits\n";              # 印出整個陣列（用空白分隔）

push(@fruits, "date");         # 尾端加入
pop(@fruits);                  # 移除尾端
unshift(@fruits, "avocado");   # 開頭加入
shift(@fruits);                # 移除開頭

my @sorted = sort(@fruits);            # 排序
my @reversed = reverse(@fruits);       # 反轉
my @sliced = @fruits[0..1];            # 切片 (slice)

foreach my $fruit (@fruits) {
    print "水果: $fruit\n";
}

# map 與 grep（函數式操作）
my @lengths = map { length($_) } @fruits;      # 轉換每個元素
my @long = grep { length($_) > 5 } @fruits;    # 篩選符合條件的元素
```

---

## 7. 雜湊 (Hash)

```perl
my %person = (
    name => "Alice",
    age  => 30,
    city => "Taipei",
);

print $person{name}, "\n";     # 存取值
$person{job} = "Engineer";     # 新增/修改

foreach my $key (keys %person) {
    print "$key => $person{$key}\n";
}

foreach my $value (values %person) {
    print "值: $value\n";
}

if (exists $person{age}) {
    print "有 age 這個鍵\n";
}

delete $person{city};          # 刪除鍵值
```

---

## 8. 條件判斷

```perl
my $score = 85;

if ($score >= 90) {
    print "優等\n";
} elsif ($score >= 60) {
    print "及格\n";
} else {
    print "不及格\n";
}

# unless 是 if 的相反
print "分數不到 60\n" unless $score >= 60;

# 三元運算子
my $result = ($score >= 60) ? "Pass" : "Fail";
print "$result\n";

# 邏輯運算子
print "ok\n" if $score > 0 && $score <= 100;
```

---

## 9. 迴圈

```perl
# while
my $i = 0;
while ($i < 5) {
    print "i = $i\n";
    $i++;
}

# until
$i = 0;
until ($i >= 5) {
    print "i = $i\n";
    $i++;
}

# C 風格 for
for (my $j = 0; $j < 5; $j++) {
    print "j = $j\n";
}

# foreach（最常用於走訪陣列）
foreach my $n (1..5) {
    print "n = $n\n";
}

# 迴圈控制
foreach my $n (1..10) {
    next if $n % 2 == 0;   # 跳過偶數
    last if $n > 7;        # 大於 7 就跳出
    print "$n\n";
}
```

---

## 10. 子程式 (Subroutine)

```perl
sub add {
    my ($a, $b) = @_;   # 參數會被打包進 @_
    return $a + $b;
}

my $sum = add(3, 4);
print "$sum\n";  # 7

# 沒有 return 時，最後一個運算式的值會被當作回傳值
sub multiply {
    my ($a, $b) = @_;
    $a * $b;
}

# 預設值範例
sub greet {
    my ($name) = @_;
    $name //= "訪客";   # // 是 "定義則使用，否則用預設值"
    print "哈囉，$name！\n";
}
greet();          # 哈囉，訪客！
greet("小明");     # 哈囉，小明！
```

---

## 11. 參照 (Reference)

參照類似其他語言的指標，讓你可以建立巢狀資料結構（陣列的陣列、雜湊的雜湊等）。

```perl
my @array = (1, 2, 3);
my $aref = \@array;          # 建立陣列參照
print $aref->[0], "\n";      # 取值方式一
print $$aref[0], "\n";       # 取值方式二
print @$aref, "\n";          # 還原成整個陣列

my %hash = (a => 1, b => 2);
my $href = \%hash;
print $href->{a}, "\n";

# 匿名資料結構（常用於複雜資料）
my $data = {
    name    => "Alice",
    scores  => [90, 85, 78],
    address => { city => "Taipei", zip => "100" },
};

print $data->{name}, "\n";
print $data->{scores}[0], "\n";
print $data->{address}{city}, "\n";
```

---

## 12. 正規表示式

Perl 的正規表示式功能非常強大，是它最著名的特色之一。

```perl
my $text = "My email is test\@example.com";

if ($text =~ /(\w+)\@(\w+\.\w+)/) {
    print "使用者: $1, 網域: $2\n";
}

# 取代
(my $censored = $text) =~ s/\@example\.com/@[hidden]/;
print "$censored\n";

# 全域比對
my $str = "cat bat hat mat";
my @matches = ($str =~ /(\w)at/g);
print "@matches\n";  # c b h m

# 常用修飾符：g(全域) i(不分大小寫) m(多行) s(讓 . 比對換行)
print "match\n" if "HELLO" =~ /hello/i;
```

---

## 13. 檔案讀寫

```perl
# 寫入檔案
open(my $fh, '>', 'output.txt') or die "無法開啟檔案: $!";
print $fh "第一行\n";
print $fh "第二行\n";
close($fh);

# 讀取檔案（逐行）
open(my $in, '<', 'output.txt') or die "無法開啟檔案: $!";
while (my $line = <$in>) {
    chomp($line);   # 移除行尾換行符號
    print "讀到: $line\n";
}
close($in);

# 一次讀取所有行到陣列
open($in, '<', 'output.txt') or die "無法開啟: $!";
my @lines = <$in>;
close($in);

# 附加寫入
open(my $append, '>>', 'output.txt') or die "無法開啟: $!";
print $append "附加的一行\n";
close($append);
```

---

## 14. 特殊變數

| 變數 | 說明 |
|------|------|
| `$_` | 預設變數，許多函數在未指定變數時會使用它 |
| `@_` | 子程式的參數列表 |
| `$0` | 執行中的程式名稱 |
| `@ARGV` | 命令列參數 |
| `%ENV` | 環境變數 |
| `$/` | 輸入的行分隔字元 |
| `$\` | 輸出的行分隔字元 |

```perl
foreach (1..3) {
    print "$_\n";   # 直接使用 $_
}

foreach (@ARGV) {
    print "參數: $_\n";
}

print "使用者: $ENV{USER}\n";
```

---

## 15. 模組與 CPAN

Perl 的一大優勢是 CPAN（Comprehensive Perl Archive Network）擁有數十萬個現成模組。

```perl
use strict;
use warnings;
use List::Util qw(sum max min);   # 內建常用模組

my @nums = (3, 7, 2, 9, 4);
print sum(@nums), "\n";   # 25
print max(@nums), "\n";   # 9
print min(@nums), "\n";   # 2

use Data::Dumper;          # 除錯時印出複雜資料結構很好用
my %h = (a => 1, b => 2);
print Dumper(\%h);
```

安裝第三方模組：
```bash
cpan Module::Name
# 或使用 cpanm（更快更簡單）
cpanm Module::Name
```

---

## 16. 錯誤處理

```perl
# die 會拋出錯誤並終止程式
open(my $fh, '<', 'nofile.txt') or die "找不到檔案: $!";

# eval 用來攔截錯誤（類似 try/catch）
eval {
    die "發生錯誤！\n";
};
if ($@) {
    print "捕捉到錯誤: $@";
}

# 現代 Perl (5.34+) 也支援原生 try/catch
use feature 'try';
no warnings 'experimental::try';
try {
    die "出錯了\n";
} catch ($e) {
    print "捕捉到: $e";
}
```

---

## 17. 簡易物件導向

Perl 的傳統 OOP 是用套件（package）+ 參照 + `bless` 實現：

```perl
package Animal;

sub new {
    my ($class, %args) = @_;
    my $self = { name => $args{name} || "unknown" };
    return bless $self, $class;
}

sub speak {
    my ($self) = @_;
    print $self->{name}, " makes a sound.\n";
}

package main;

my $dog = Animal->new(name => "Rex");
$dog->speak();   # Rex makes a sound.
```

現代 Perl（5.38+）也內建更簡潔的 `class` 語法（實驗性功能）：

```perl
use v5.38;
use feature 'class';
no warnings 'experimental::class';

class Animal {
    field $name :param;
    method speak {
        print "$name makes a sound.\n";
    }
}

my $dog = Animal->new(name => "Rex");
$dog->speak();
```

---

## 小結與下一步

學完以上內容，你已經掌握了 Perl 的核心功能：變數、控制流程、子程式、參照、正規表示式與檔案處理。建議接下來可以：

1. 練習用正規表示式處理文字檔（Perl 最擅長的領域）。
2. 學習 CPAN 上熱門模組，例如 `Moose`／`Moo`（更現代的物件導向框架）。
3. 嘗試撰寫一個小工具，例如日誌分析器或批次改檔名工具。
