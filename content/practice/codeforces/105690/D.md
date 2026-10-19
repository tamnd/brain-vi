---
title: "CF 105690D - Vũ công sư tử"
description: "Hành lang là một chuỗi các cột. Cột i chứa các bệ ki xếp chồng lên nhau theo chiều dọc và tất cả n vũ công phải đứng trên các bệ riêng biệt trong cột đó."
date: "2026-06-26T09:03:19+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105690
codeforces_index: "D"
codeforces_contest_name: "UTPC Contest 1-29-25 Div. 1 (Advanced)"
rating: 0
weight: 105690
solve_time_s: 40
verified: true
draft: false
---

[CF 105690D - Vũ công sư tử](https://codeforces.com/problemset/problem/105690/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải quyết:** 40s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Hành lang là một chuỗi các cột. Cột`i`chứa`k_i`các nền tảng được xếp chồng lên nhau theo chiều dọc và tất cả`n`người nhảy phải đứng trên các bục riêng biệt ở cột đó. Các vũ công được sắp xếp theo chiều cao nên vũ công`1`phải luôn là người chiếm giữ nền tảng và vũ công thấp nhất`n`phải luôn là nền tảng được chiếm dụng cao nhất. Nhiệm vụ là đếm xem có thể có bao nhiêu màn biểu diễn hoàn chỉnh, trong đó một màn biểu diễn là sự lựa chọn các vị trí vũ công ở mỗi cột. 

Đối với một cột duy nhất với`k_i`nền tảng, quyết định thực sự duy nhất là`n`vị trí nền tảng được chiếm đóng. Khi các vị trí đó được chọn, quy tắc đặt hàng sẽ tự động chỉ định vị trí được chọn thấp nhất cho vũ công.`1`, người tiếp theo là vũ công`2`, vân vân. Số khả năng xảy ra của cột đó chính là số cách chọn`n`vị trí từ`k_i`, đó là hệ số nhị thức`C(k_i, n)`. 

Các cột không ảnh hưởng lẫn nhau. Người nhảy chỉ cần giữ nguyên trật tự trong mỗi cột và không hạn chế việc kết nối các bệ đã chọn ở cột này với các bệ đã chọn ở cột tiếp theo. Tổng số phần trình diễn là tích của số lựa chọn ở mỗi cột. 

Các ràng buộc được thiết kế xung quanh quan sát này. Số lượng cột có thể đạt tới`100000`, trong khi`n`nhiều nhất là`100`. Một giải pháp cố gắng liệt kê các vị trí vũ công hoặc mô phỏng tất cả các cách sắp xếp có thể là không thể bởi vì ngay cả một cột cũng có thể có một số lượng lớn các trạng thái hợp lệ. Chúng ta cần một cách tiếp cận trong đó mỗi cột được xử lý trong thời gian không đổi hoặc rất nhỏ. Từ`n`nhỏ, việc kết hợp tính toán với vòng lặp ngắn là đủ. 

Một số trường hợp nguy hiểm có thể phá vỡ việc triển khai bất cẩn. Khi`n = 1`, mỗi cột đóng góp chính xác`k_i`khả năng vì một vũ công có thể đứng ở bất cứ đâu. Ví dụ:```
Input:
1 3
2 5 4

Output:
40
```Một giải pháp coi quy tắc đặt hàng là yêu cầu một sự sắp xếp đặc biệt có thể xuất ra một cái gì đó nhỏ hơn một cách không chính xác, nhưng câu trả lời chỉ đơn giản là`2 * 5 * 4`. 

Khi một cột có chính xác`n`nền tảng, mọi nền tảng phải được sử dụng và cột đó chỉ đóng góp một lựa chọn. Ví dụ:```
Input:
3 2
3 5

Output:
10
```Cột đầu tiên đóng góp`C(3,3) = 1`, trong khi thứ hai đóng góp`C(5,3) = 10`. Việc quên trường hợp này có thể dẫn đến một phép trừ không cần thiết hoặc giả định không chính xác rằng luôn có nhiều lựa chọn. 

Giá trị lớn cũng cần được chăm sóc. Một cột có thể có`100000`nền tảng, do đó giai thừa lên đến`100000`có thể được sử dụng, nhưng chỉ khi phép tính nghịch đảo mô-đun được xử lý chính xác. Từ`n`chỉ là`100`, tính toán mỗi tổ hợp với tích tối đa là`100`thuật ngữ tránh sự phức tạp không cần thiết. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực trực tiếp là tạo ra mọi tập hợp có thể`n`các nền tảng được sử dụng cho mỗi cột, sau đó kết hợp các lựa chọn đó trên các cột. Điều này đúng vì mỗi màn trình diễn hợp lệ chính xác là một lựa chọn nền tảng trên mỗi cột. Tuy nhiên, số lựa chọn trong một cột đã`C(k_i, n)`. Với`k_i = 100000`Và`n = 50`, con số này vượt xa mọi thứ có thể liệt kê được. Cách tiếp cận bạo lực sẽ cần phải xử lý một số lượng lớn các trạng thái. 

Quan sát quan trọng là danh tính của các vũ công không thêm các lựa chọn bổ sung. Khi một tập hợp các nền tảng được chọn, quy tắc đặt hàng sẽ bắt buộc phải phân công. Một cột với`k_i`nền tảng chỉ đóng góp giá trị kết hợp chứ không phải giá trị hoán vị. 

Điều này biến toàn bộ vấn đề thành những đóng góp độc lập được nhân lên. Bởi vì`n`nhỏ, chúng ta có thể tính được`C(k_i, n)`trực tiếp như:`k_i * (k_i - 1) * ... * (k_i - n + 1) / n!`Phép chia phải được thực hiện modulo`1e9 + 7`, vì vậy chúng tôi tính toán trước nghịch đảo mô đun của`n!`và nhân mỗi đóng góp của cột với nó. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(tích của C(k_i, n)) | O(tích của C(k_i, n)) | Quá chậm | 
| Tối ưu | O(m * n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc`n`,`m`và chiều cao của tất cả các cột. Câu trả lời được xây dựng bằng cách xem xét từng cột một vì mỗi lựa chọn cột là độc lập. 
2. Tính toán`n!`modulo`1e9 + 7`và tìm nghịch đảo mô-đun của nó bằng cách sử dụng lũy ​​thừa nhanh. Mọi phép tính kết hợp đều cần chia cho`n!`và nghịch đảo mô-đun cung cấp phép toán tương đương theo số học modulo. 
3. Đối với mỗi chiều cao cột`k_i`, tính số cách chọn các sân ga bị chiếm dụng. Bắt đầu với`1`và nhân với`n`các giá trị liên tiếp từ`k_i - n + 1`bởi vì`k_i`. Sau đó nhân với nghịch đảo của`n!`. 
4. Nhân phần đóng góp của cột này vào modulo câu trả lời chung`1e9 + 7`. Phép nhân hợp lệ vì việc chọn nền tảng trong một cột không bao giờ hạn chế các lựa chọn trong cột khác. 
5. In sản phẩm cuối cùng. 

Tại sao nó hoạt động: Đối với mỗi cột, thuật toán sẽ tính chính xác số lượng nền tảng được sử dụng có thể có. Điều kiện đặt hàng không tạo ra các khả năng bổ sung vì mỗi nhóm được chọn có chính xác một sự phân công của các vũ công từ bệ thấp nhất đến bệ cao nhất. Vì một màn biểu diễn đầy đủ bao gồm việc chọn độc lập một trạng thái hợp lệ cho mỗi cột nên quy tắc nhân sẽ đưa ra số lần biểu diễn chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 10 ** 9 + 7

def mod_pow(a, b):
    res = 1
    while b:
        if b & 1:
            res = res * a % MOD
        a = a * a % MOD
        b >>= 1
    return res

def solve():
    n, m = map(int, input().split())
    k = list(map(int, input().split()))

    fact = 1
    for i in range(1, n + 1):
        fact = fact * i % MOD

    inv_fact = mod_pow(fact, MOD - 2)

    ans = 1
    for height in k:
        cur = 1
        for x in range(n):
            cur = cur * (height - x) % MOD
        cur = cur * inv_fact % MOD
        ans = ans * cur % MOD

    print(ans)

if __name__ == "__main__":
    solve()
```Việc tính toán giai thừa chỉ đi đến`n`, không phải chiều cao cột tối đa. Đây là cố ý bởi vì`n`là số lượng nền tảng được chọn và công thức chỉ cần mẫu số`n!`. 

Nghịch đảo mô đun được tính toán bằng định lý nhỏ Fermat vì`1e9 + 7`là nguyên tố. số mũ`MOD - 2`đưa ra nghịch đảo của`n!`theo số học modulo. 

Đối với mỗi cột, vòng lặp nhân chính xác`n`các giá trị. Giá trị đầu tiên là`height`, tiếp theo là`height - 1`, vân vân. Điều này trực tiếp xây dựng tử số của công thức kết hợp. Phạm vi dừng lại sau`n`giá trị, do đó không có sai sót nào. 

Số nguyên Python không bị tràn, nhưng mỗi phép nhân đều được giảm modulo`MOD`ngay lập tức để giữ các giá trị nhỏ và phù hợp với số học cần thiết. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
2 3
2 3 2
```| Cột | Chiều cao | Sản phẩm dành cho tử số | Kết hợp | Trả lời cho đến nay | 
| --- | --- | --- | --- | --- | 
| Bắt đầu | | | | 1 | 
| 1 | 2 | 2 × 1 | 1 | 1 | 
| 2 | 3 | 3 × 2 | 3 | 3 | 
| 3 | 2 | 2 × 1 | 1 | 3 | 

Cột giữa có ba cặp bệ có thể có. Các cột khác buộc cả hai vũ công phải sử dụng tất cả các nền tảng có sẵn, vì vậy mỗi người họ đóng góp một khả năng. 

### Mẫu 2 

đầu vào:```
1 5
3 2 1 4 2
```| Cột | Chiều cao | Tử số | Kết hợp | Trả lời cho đến nay | 
| --- | --- | --- | --- | --- | 
| Bắt đầu | | | | 1 | 
| 1 | 3 | 3 | 3 | 3 | 
| 2 | 2 | 2 | 2 | 6 | 
| 3 | 1 | 1 | 1 | 6 | 
| 4 | 4 | 4 | 4 | 24 | 
| 5 | 2 | 2 | 2 | 48 | 

Với một vũ công, việc chọn bệ là quyết định duy nhất, vì vậy mỗi cột đều đóng góp toàn bộ chiều cao của nó. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(m * n) | Mỗi trong số`m`cột yêu cầu`n`phép nhân để tính toán sự kết hợp của nó. | 
| Không gian | O(n) | Chỉ có một vài biến và giai thừa nghịch đảo được lưu trữ. | 

Các giá trị tối đa là`m = 100000`Và`n = 100`, đưa ra khoảng mười triệu thao tác đơn giản. Điều này phù hợp thoải mái trong giới hạn thời gian, đồng thời chỉ lưu trữ dữ liệu bổ sung liên tục giúp giảm mức sử dụng bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys
import io

MOD = 10 ** 9 + 7

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    result = sys.stdout.getvalue()
    sys.stdin = old_stdin
    sys.stdout = old_stdout
    return result

def mod_pow(a, b):
    res = 1
    while b:
        if b & 1:
            res = res * a % MOD
        a = a * a % MOD
        b >>= 1
    return res

def solve():
    n, m = map(int, sys.stdin.readline().split())
    k = list(map(int, sys.stdin.readline().split()))

    fact = 1
    for i in range(1, n + 1):
        fact = fact * i % MOD

    inv_fact = mod_pow(fact, MOD - 2)

    ans = 1
    for height in k:
        cur = 1
        for x in range(n):
            cur = cur * (height - x) % MOD
        cur = cur * inv_fact % MOD
        ans = ans * cur % MOD

    print(ans)

assert run("""2 3
2 3 2
""") == "3\n", "sample 1"

assert run("""1 5
3 2 1 4 2
""") == "48\n", "sample 2"

assert run("""3 2
3 5
""") == "10\n", "exactly n platforms in first column"

assert run("""1 3
2 5 4
""") == "40\n", "single dancer"

assert run("""2 4
2 2 2 2
""") == "1\n", "all columns forced"

assert run("""100 1
100
""") == "1\n", "maximum n with minimum column height"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2 3 / 2 3 2`|`3`| Cung cấp mẫu và phép nhân kết hợp thông thường | 
|`1 5 / 3 2 1 4 2`|`48`| Hành vi vũ công đơn lẻ | 
|`3 2 / 3 5`|`10`| Một cột có chính xác`n`nền tảng | 
|`2 4 / 2 2 2 2`|`1`| Tất cả các cột chỉ có một cách sắp xếp | 
|`100 1 / 100`|`1`| Lớn`n`ranh giới | 

## Vỏ cạnh 

Khi chỉ có một vũ công, công thức kết hợp sẽ trở thành`C(k_i, 1) = k_i`. Đối với đầu vào:```
1 3
2 5 4
```thuật toán tính toán đóng góp của cột`2`,`5`, Và`4`. Sản phẩm là`40`, phù hợp với thực tế là vũ công chọn độc lập một nền tảng trong mỗi cột. 

Khi một cột có số bục bằng số lượng vũ công thì mọi bục đều phải có người. Vì:```
3 2
3 5
```cột đầu tiên đưa ra`C(3, 3) = 1`. Thứ hai cho`C(5, 3) = 10`. Thuật toán nhân các giá trị này và trả về`10`. 

Khi mỗi cột có chiều cao bằng`n`, mọi chuyển động đều bị ép buộc. Vì:```
2 4
2 2 2 2
```mỗi cột đóng góp`C(2, 2) = 1`, vậy đáp án cuối cùng vẫn là`1`. Điều này xác nhận rằng thuật toán không vô tình tính các nhiệm vụ của vũ công khác nhau thành các màn trình diễn riêng biệt.
