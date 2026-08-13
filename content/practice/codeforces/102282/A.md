---
title: "CF 102282A - \u041f\u0435\u0440\u0432\u0430\u044f \u0437\u0430\u0434\u0430\u0447\u0430"
description: "Chúng ta cần chọn một số nguyên dương (x) sao cho (x^n) chia hết cho (m) và trong số tất cả các số nguyên dương như vậy, chúng ta cần số nguyên dương nhỏ nhất. Nếu không có (x) như vậy tồn tại, chúng tôi in Vắng mặt."
date: "2026-08-13T16:12:51+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102282
codeforces_index: "A"
codeforces_contest_name: "2011, \u041e\u0442\u0431\u043e\u0440\u043e\u0447\u043d\u044b\u0439 \u043a\u043e\u043d\u0442\u0435\u0441\u0442 \u0421\u0413\u0410\u0423 \u043d\u0430 \u0447\u0435\u0442\u0432\u0435\u0440\u0442\u044c\u0444\u0438\u043d\u0430\u043b ACM ICPC"
rating: 0
weight: 102282
solve_time_s: 65
verified: true
draft: false
---

[CF 102282A - \u041f\u0435\u0440\u0432\u0430\u044f \u0437\u0430\u0434\u0430\u0447\u0430](https://codeforces.com/problemset/problem/102282/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 5s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta cần chọn một số nguyên dương (x) sao cho (x^n) chia hết cho (m) và trong số tất cả các số nguyên dương như vậy, chúng ta cần số nguyên dương nhỏ nhất. Nếu không có (x) như vậy tồn tại, chúng tôi in`ABSENT`. 

Giá trị (n) có thể lớn bằng (10^9), do đó, thuật toán phụ thuộc tuyến tính vào (n) là không cần thiết. Quan trọng hơn, (m) không phải là tùy ý. Nó chỉ có thể là một trong các giá trị được liệt kê, cụ thể là (1,2,3,5,7,11,13,17) hoặc (1000000007). Mọi giá trị ngoại trừ (1) đều là số nguyên tố. Hạn chế này là quan sát trung tâm của vấn đề. 

Câu trả lời không bao giờ có thể yêu cầu chúng ta tìm kiếm trong một dãy số nguyên lớn. Nếu (m>1) và (n>0), việc chọn (x=m) ngay lập tức có hiệu quả vì (m^n) chia hết cho (m). Vì bản thân (m) là số nguyên dương nhỏ nhất chia hết cho (m), nên không có (x) nhỏ hơn có thể hoạt động khi (n=1), và đối với các số mũ lớn hơn, chúng ta vẫn cần (x) để chứa thừa số nguyên tố (m), vì vậy (x\ge m). 

Các trường hợp ranh giới đáng được quan tâm riêng biệt. Đối với đầu vào`0 2`, ta có (x^0=1) với mọi số dương (x) và (1) không chia hết cho (2), nên câu trả lời là`ABSENT`. Một giải pháp bất cẩn luôn in (m) cho (n=0) sẽ xuất ra`2`, điều đó là sai. 

Đối với đầu vào`0 1`, câu trả lời là`1`, bởi vì mọi số nguyên dương lũy ​​thừa bậc 0 đều bằng (1) và (1) chia hết cho (1). Một giải pháp coi (n=0) là không thể tự động sẽ thất bại ở đây. 

Đối với đầu vào`1 1`, câu trả lời cũng là`1`. Vì (1) chia hết mọi số nguyên nên số dương nhỏ nhất có thể (x) là đủ. 

Ngoài ra còn có vấn đề về định dạng trong đoạn trích câu lệnh được cung cấp: phần ví dụ được hiển thị chứa`2 2`Và`3 3`không có sự tách biệt đáng tin cậy giữa đầu vào và đầu ra. Theo điều kiện toán học nêu trong bài toán,`2 2`có câu trả lời`2`, trong khi`3 3`có câu trả lời`3`. Giải pháp bên dưới tuân theo tuyên bố chính thức, không phải định dạng ví dụ bị hỏng. 

## Phương pháp tiếp cận 

Một giải pháp bạo lực trực tiếp sẽ thử (x=1,2,3,\ldots), tính toán xem (x^n) có chia hết cho (m) hay không và dừng ở giá trị thành công đầu tiên. Điều này đúng vì các ứng viên được kiểm tra theo thứ tự tăng dần, do đó ứng viên hợp lệ đầu tiên nhất thiết phải là tối thiểu. 

Vấn đề là trường hợp xấu nhất. Hãy xem xét (n=1) và (m=1000000007). (x) hợp lệ đầu tiên là (1000000007), do đó, tìm kiếm mạnh mẽ sẽ kiểm tra chính xác (1000000007) ứng viên. Ngay cả khi mọi kiểm tra tính chia hết được giảm xuống thành một hoạt động liên tục trong thời gian, thì điều đó vẫn vượt xa những gì giải pháp cuộc thi một giây nên làm. Việc tính toán trực tiếp các lũy thừa sẽ còn tệ hơn vì (x^n) có thể trở nên rất lớn. 

Quan sát làm cho việc tìm kiếm biến mất là mọi phép (m>1) đều là số nguyên tố. Giả sử (m=p), trong đó (p) là số nguyên tố và (n>0). Nếu (p\mid x^n), thì (p\mid x). Điều này xuất phát từ tính chất cơ bản của số nguyên tố: nếu một số nguyên tố chia một tích thì nó phải chia một trong các thừa số của nó. Vì (x^n=x\cdot x\cdots x), lũy thừa chia hết cho (p) buộc (p) phải chia (x). 

Số dương nhỏ nhất (x) chia hết cho (p) chính xác là (p). Vì vậy câu trả lời đơn giản là (m). 

Ngoại lệ duy nhất là (m=1), trong đó số nguyên dương nhỏ nhất luôn là (1). Trường hợp ngoại lệ còn lại là (n=0), vì mọi dương (x) đều thỏa mãn (x^0=1). Do đó, với (m>1) không có nghiệm khi (n=0). 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(m)) ứng viên, với quyền lực/phân chia công việc cho mỗi ứng viên | (O(1)) | Quá chậm đối với (m=1000000007) | 
| Tối ưu | (O(1)) | (O(1)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc (n) và (m). Chúng ta chỉ cần hai giá trị này vì tập hợp giới hạn các giá trị (m) có thể cung cấp trực tiếp cho chúng ta thuộc tính lý thuyết số được yêu cầu. 
2. Nếu (m=1), in`1`. Mọi số nguyên dương được nâng lên lũy thừa không âm ít nhất được xác định ở đây là (1) khi (n=0) và (1) chia hết cho (1). Do đó, số nguyên dương nhỏ nhất là câu trả lời. 
3. Nếu (m>1) và (n=0), in`ABSENT`. Với mọi số nguyên dương (x), (x^0=1) và không có (m>1) chia hết (1). 
4. Ngược lại (m>1) và (n>0). Vì mọi giá trị được phép của (m) đều là số nguyên tố nên mọi (x) thỏa mãn (m\mid x^n) bản thân nó phải chia hết cho (m). Giá trị dương nhỏ nhất (x) là (m), nên in ra (m). 

### Tại sao nó hoạt động 

Với (m=1), câu trả lời hiển nhiên là (1). Với (m>1) và (n=0), mọi ứng viên đều có lũy thừa bậc 0 bằng (1), nên không có ứng viên nào làm việc. Với (m>1) và (n>0), các giá trị cho phép của (m) là số nguyên tố. Nếu (m\mid x^n), lực nguyên tố (m\mid x), thì mọi (x) hợp lệ ít nhất là (m). Đồng thời, (x=m) hợp lệ vì (m^n) chia hết cho (m). Do đó (m) chính xác là mức tối thiểu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())

    if m == 1:
        print(1)
    elif n == 0:
        print("ABSENT")
    else:
        print(m)

solve()
```Nhánh đầu tiên xử lý (m=1) trước khi nhìn vào (in). Thứ tự này quan trọng đối với đầu vào`0 1`, vì câu trả lời là`1`, không`ABSENT`. 

Nhánh thứ hai xử lý tình huống bất khả thi duy nhất. Khi (m>1), (x^0) luôn là (1), do đó không có số nguyên dương hợp lệ. 

Nhánh cuối cùng bao gồm mọi trường hợp còn lại. Ở đây (n>0) và (m) là một trong những số nguyên tố được phép, do đó bản thân số đó là cơ sở hợp lệ tối thiểu. 

Không có lũy thừa trong việc thực hiện. Điều này tránh được cả những công việc không cần thiết và khả năng xây dựng các số nguyên cực lớn. Python cũng có các số nguyên có độ chính xác tùy ý, nhưng việc dựa vào đó sẽ không giải quyết được vấn đề thuật toán về khả năng thực hiện các phép tính lớn. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Đối với mẫu được nêu về mặt toán học`2 2`, ta có (n=2) và (m=2). 

| Bước | (n) | (m) | Tình trạng | Trả lời | 
| --- | --- | --- | --- | --- | 
| Đọc đầu vào | 2 | 2 | (m\ne1) | | 
| Kiểm tra (n=0) | 2 | 2 | sai | | 
| Chi nhánh cuối cùng | 2 | 2 | (m) là số nguyên tố và (n>0) | 2 | 

Cơ số hợp lệ nhỏ nhất là (2), vì (2^2=4) chia hết cho (2). Kết quả là`2`. 

### Mẫu 2 

cho`3 3`, ta có (n=3) và (m=3). 

| Bước | (n) | (m) | Tình trạng | Trả lời | 
| --- | --- | --- | --- | --- | 
| Đọc đầu vào | 3 | 3 | (m\ne1) | | 
| Kiểm tra (n=0) | 3 | 3 | sai | | 
| Chi nhánh cuối cùng | 3 | 3 | (m) là số nguyên tố và (n>0) | 3 | 

Ở đây (3^3=27), chia hết cho (3). Bất kỳ số nguyên dương nhỏ hơn nào cũng là (1) hoặc (2) và không có lũy thừa thứ ba chia hết cho (3). Kết quả là`3`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(1)) | Chỉ có một số lượng so sánh không đổi và một thao tác đầu ra được thực hiện. | 
| Không gian | (O(1)) | Chỉ có hai số nguyên đầu vào và một vài giá trị tạm thời được lưu trữ. | 

Giới hạn (n\le10^9) không ảnh hưởng đến thời gian chạy vì thuật toán không bao giờ lặp lại (n) và không bao giờ tính toán (x^n). Tương tự như vậy, ngay cả giá trị lớn nhất có thể (m), (1000000007), cũng không gây ra tìm kiếm nào. Giải pháp thoải mái phù hợp với giới hạn thời gian và bộ nhớ đã nêu. 

## Trường hợp thử nghiệm```python
import sys
import io

def solve():
    n, m = map(int, input().split())

    if m == 1:
        print(1)
    elif n == 0:
        print("ABSENT")
    else:
        print(m)

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    try:
        solve()
        return sys.stdout.getvalue().strip()
    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout

# Provided examples as they appear mathematically in the supplied statement.
assert run("2 2\n") == "2", "sample 1"
assert run("3 3\n") == "3", "sample 2"

# Minimum-size input: n = 0, m = 1.
assert run("0 1\n") == "1", "m = 1 must always return 1"

# Boundary case n = 0 with a prime m.
assert run("0 2\n") == "ABSENT", "x^0 is always 1"

# All-equal values.
assert run("7 7\n") == "7", "prime m with positive exponent"

# Maximum allowed n and maximum allowed m.
assert run("1000000000 1000000007\n") == "1000000007", "maximum n and m"

# Boundary between n = 0 and n = 1.
assert run("1 17\n") == "17", "positive exponent must return m"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`0 1`|`1`| Ước số đặc biệt (m=1) phải được xử lý trước trường hợp (n=0). | 
|`0 2`|`ABSENT`| Số lũy thừa thứ 0 không thể chia hết cho số nguyên tố lớn hơn (1). | 
|`7 7`|`7`| Bằng (n) và (m), với số mũ dương. | 
|`1000000000 1000000007`|`1000000007`| Giá trị cực đại và thực tế là thuật toán không phụ thuộc vào (n). | 
|`1 17`|`17`| Số mũ dương nhỏ nhất và ước số nguyên tố. | 

## Vỏ cạnh 

cho`0 2`, trước tiên thuật toán sẽ kiểm tra xem (m=1) có sai không. Sau đó nó nhìn thấy (n=0) và in`ABSENT`. Điều này đúng vì mọi số dương (x) đều thỏa mãn (x^0=1) và (2\nmid1). 

Vì`0 1`, điều kiện đầu tiên thành công ngay lập tức và in`1`. Thứ tự của những lần kiểm tra này giúp trường hợp hợp lệ (m=1) không bị phân loại sai thành trường hợp không thể chỉ vì (n=0). 

Vì`1 1`, điều kiện đầu tiên lại in`1`. Vì (1) chia hết mọi số nguyên nên cơ số dương nhỏ nhất luôn đủ. 

Vì`1 17`, thuật toán đến nhánh cuối cùng và in`17`. Ở đây (17^1=17) và không có số nguyên dương nào nhỏ hơn (17) chia hết cho (17). 

Vì`1000000000 1000000007`, thuật toán vẫn chỉ thực hiện hai phép so sánh trước khi in kết quả. Số mũ khổng lồ không bao giờ gây ra vòng lặp và số nguyên tố khổng lồ không bao giờ gây ra tìm kiếm. Đầu ra là`1000000007`. 

Bất biến trung tâm trên tất cả các nhánh rất đơn giản: một lần (m>1) và (n>0), mọi cơ sở hợp lệ phải là bội số của số nguyên tố (m). Bội số nhỏ nhất có thể có là chính nó (m), nên không còn gì để tìm kiếm.
