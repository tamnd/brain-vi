---
title: "CF 1899A - Trò chơi với số nguyên"
description: "Chúng ta được yêu cầu phân tích một trò chơi hai người chơi đơn giản với các số nguyên. Trò chơi bắt đầu với một số $n$. Vanya di chuyển trước và mỗi người chơi có thể tăng hoặc giảm số đó đi 1 trong lượt của mình. Vanya thắng ngay lập tức nếu sau khi di chuyển, số đó chia hết cho 3."
date: "2026-06-08T21:24:21+07:00"
tags: ["codeforces", "competitive-programming", "games", "math", "number-theory"]
categories: ["algorithms"]
codeforces_contest: 1899
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 909 (Div. 3)"
rating: 800
weight: 1899
solve_time_s: 91
verified: true
draft: false
---

[CF 1899A - Trò chơi với số nguyên](https://codeforces.com/problemset/problem/1899/A) 

**Đánh giá:** 800 
**Tags:** trò chơi, toán học, lý thuyết số 
**Thời gian giải:** 1 phút 31s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu phân tích một trò chơi hai người chơi đơn giản với các số nguyên. Trò chơi bắt đầu với một con số$n$. Vanya di chuyển trước và mỗi người chơi có thể tăng hoặc giảm số đó đi 1 trong lượt của mình. Vanya thắng ngay lập tức nếu sau nước đi của mình, số đó chia hết cho 3. Nếu Vanya không thắng sau 10 nước đi (mỗi nước 5 lượt), Vova thắng. Mục tiêu là xác định, với một số bắt đầu nhất định$n$, người chơi nào sẽ thắng nếu cả hai đều chơi tối ưu. 

Đầu vào bao gồm nhiều trường hợp thử nghiệm, mỗi trường hợp có một số nguyên duy nhất$n$nằm trong khoảng từ 1 đến 1000. Vì$t \le 100$, tổng số thao tác còn khiêm tốn. Các ràng buộc đủ nhỏ để chúng ta có thể khám phá mọi chuỗi di chuyển có thể nếu cần, nhưng vẫn có cách tiếp cận thông minh hơn vì điều kiện thắng của trò chơi phụ thuộc hoàn toàn vào khả năng chia hết cho 3. 

Một trường hợp cạnh không rõ ràng phát sinh khi$n$vốn đã chia hết cho 3. Trong trường hợp này, Vanya không thể tăng hoặc giảm mà không cho Vova phản ứng ngay, nhưng anh ta vẫn di chuyển trước nên không thể thực hiện nước đi để giữ số chia hết cho 3; anh ấy phải cố gắng điều chỉnh nó. Một sự tinh tế khác là việc cộng hoặc trừ 1 sẽ xen kẽ phần dư theo modulo 3. Ví dụ: nếu$n \% 3 = 1$, Vanya có thể trực tiếp chuyển sang bội số của 3 bằng cách trừ đi 1. Những cân nhắc về modulo này quyết định cách chơi tối ưu. Một cách tiếp cận ngây thơ mô phỏng các nước đi mà không nhận ra mô hình mô-đun này có thể dự đoán sai người chiến thắng. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ liệt kê tất cả các chuỗi nước đi lên đến 10 bước, xen kẽ giữa những người chơi và kiểm tra xem có chuỗi nước đi nào dẫn đến bội số của 3 sau các nước đi của Vanya hay không. Vì mỗi nước đi có hai lựa chọn nên tổng số bước đi trong trường hợp xấu nhất là$2^{10} = 1024$mỗi trường hợp thử nghiệm. Điều này khả thi nhưng không cần thiết vì trò chơi hoàn toàn được xác định bởi số modulo 3. 

Cái nhìn sâu sắc quan trọng là người chiến thắng hoàn toàn được xác định bởi phần còn lại$r = n \% 3$. Nếu như$r = 0$, Vanya không thể thắng ngay lập tức vì bất kỳ nước đi ±1 nào cũng dẫn đến$r = 1$hoặc$2$. Nếu như$r = 1$hoặc$2$, Vanya có thể ngay lập tức di chuyển đến bội số của 3 trong lượt đầu tiên của mình bằng cách trừ hoặc cộng 1. Phần còn lại của giới hạn 10 nước đi là không liên quan vì Vanya thắng ngay lập tức hoặc không thể đạt bội số của 3 trong một nước đi. Điều này làm giảm vấn đề xuống việc kiểm tra modulo đơn giản. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^10 * t) | O(1) | Có thể chấp nhận được nhưng không cần thiết | 
| Tối ưu | O(t) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case$t$. 
2. Với mỗi test, hãy đọc số nguyên$n$. 
3. Tính số dư$r = n \% 3$. 
4. Nếu$r = 0$, in "Thứ hai". Điều này là do Vanya không thể thực hiện$n$chia hết cho 3 ở nước đi đầu tiên nên Vova cuối cùng sẽ thắng. 
5. Nếu$r = 1$hoặc$2$, in "Đầu tiên". Vanya có thể điều chỉnh ngay lập tức$n$cho ±1 để chia hết cho 3, thắng ngay lập tức. 

Tại sao nó hoạt động: Điều bất biến ở đây là thông tin liên quan duy nhất về số nguyên hiện tại là số dư theo modulo 3. Mỗi bước di chuyển sẽ dịch chuyển phần dư đi ±1. Vanya đi trước nên có thể chọn nước đi đánh 0 modulo 3 nếu có thể. Nếu anh ta không thể, điều đó có nghĩa là số dư đã bằng 0 và anh ta không thể thắng ngay lập tức. Vì số nước đi được phép (10) lớn hơn 1 và cả hai người chơi đều chơi tối ưu nên modulo 3 còn lại hoàn toàn quyết định kết quả. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n = int(input())
    r = n % 3
    if r == 0:
        print("Second")
    else:
        print("First")
```Giải pháp đọc đầu vào hiệu quả với`sys.stdin.readline`cho nhiều trường hợp thử nghiệm. Đối với mỗi số, nó tính phần dư theo modulo 3, trực tiếp xác định người chiến thắng. Không cần trạng thái hoặc mô phỏng bổ sung, điều này tránh được lỗi từng bước một hoặc việc đếm nước đi không chính xác. 

## Ví dụ đã hoạt động 

Hãy xem xét đầu vào mẫu đầu tiên:```
n = 1
r = 1 % 3 = 1 → "First"
```

```
n = 3
r = 3 % 3 = 0 → "Second"
```

```
n = 5
r = 5 % 3 = 2 → "First"
```Vì`n = 5`, Vanya trừ 2 để được 3, chia hết cho 3. Dấu vết xác nhận bất biến rằng`n % 3 == 0`Vanya không thể đạt được nếu anh ấy bắt đầu bằng`r = 0`. 

| n | r | Người chiến thắng | 
| --- | --- | --- | 
| 1 | 1 | Đầu tiên | 
| 3 | 0 | Thứ hai | 
| 5 | 2 | Đầu tiên | 

Những ví dụ này xác nhận rằng quyết định dựa trên modulo là đủ và nắm bắt được động lực của trò chơi một cách hoàn hảo. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(t) | Mỗi trường hợp thử nghiệm yêu cầu một phép tính và so sánh modulo duy nhất. | 
| Không gian | O(1) | Chỉ có một số biến được lưu trữ; không có cấu trúc dữ liệu bổ sung. | 

Được cho$t \le 100$Và$n \le 1000$, giải pháp này chạy trong thời gian không đáng kể và sử dụng bộ nhớ tối thiểu. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = []
    t = int(input())
    for _ in range(t):
        n = int(input())
        output.append("Second" if n % 3 == 0 else "First")
    return "\n".join(output)

# Provided samples
assert run("6\n1\n3\n5\n100\n999\n1000\n") == "First\nSecond\nFirst\nFirst\nSecond\nFirst", "sample 1"

# Custom cases
assert run("3\n2\n4\n6\n") == "First\nFirst\nSecond", "modulo edge cases"
assert run("2\n3\n300\n") == "Second\nSecond", "multiples of 3"
assert run("4\n1\n2\n1000\n999\n") == "First\nFirst\nFirst\nSecond", "mixed values"
assert run("1\n7\n") == "First", "small odd number"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2,4,6 | Thứ nhất, Thứ nhất, Thứ hai | Tính toán modulo đúng | 
| 3.300 | Thứ hai, Thứ hai | Xử lý bội số của 3 một cách chính xác | 
| 1,2,1000,999 | Thứ nhất, Thứ nhất, Thứ nhất, Thứ hai | Giá trị lớn và nhỏ hỗn hợp | 
| 7 | Đầu tiên | Trường hợp giá trị đơn | 

## Vỏ cạnh 

Nếu`n`chính xác là chia hết cho 3, ví dụ`n = 3`, sau đó`n % 3 = 0`. Theo thuật toán của chúng tôi, nó sẽ xuất ra "Thứ hai". Dấu vết: Nước đi đầu tiên của Vanya có thể tăng hoặc giảm`n`thành 2 hoặc 4. Cả hai đều không chia hết cho 3 nên Vova có thể phản ứng tối ưu và cuối cùng giành chiến thắng trong vòng 10 nước đi. Thuật toán dự đoán chính xác kết quả này mà không cần mô phỏng tất cả các nước đi. 

Nếu như`n = 1000`,`n % 3 = 1`, Vanya có thể trừ 1 để đạt 999, chia hết cho 3. Đầu ra là "Đầu tiên". Điều này xác nhận rằng việc kiểm tra modulo xử lý các số lớn trong phạm vi và chứng tỏ rằng giải pháp mở rộng đến các giới hạn trên. 

Điều này hoàn thành việc lý luận và thực hiện đầy đủ cho vấn đề.
