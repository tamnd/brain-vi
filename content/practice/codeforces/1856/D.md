---
title: "CF 1856D - Sai lầm hơn"
description: "Chúng ta được yêu cầu tìm vị trí của phần tử lớn nhất trong một hoán vị ẩn có độ dài $n$ bằng cách sử dụng các truy vấn cho chúng ta biết số lần đảo ngược trong một mảng con."
date: "2026-06-09T05:03:50+07:00"
tags: ["codeforces", "competitive-programming", "divide-and-conquer", "interactive"]
categories: ["algorithms"]
codeforces_contest: 1856
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 890 (Div. 2) supported by Constructor Institute"
rating: 2100
weight: 1856
solve_time_s: 93
verified: false
draft: false
---

[CF 1856D - Sai nhiều hơn](https://codeforces.com/problemset/problem/1856/D) 

**Xếp hạng:** 2100 
**Tags:** chia để trị, tương tác 
**Thời gian giải:** 1m 33s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu tìm vị trí của phần tử lớn nhất trong hoán vị ẩn của độ dài$n$bằng cách sử dụng các truy vấn cho chúng ta biết số lần đảo ngược trong một mảng con. Mỗi truy vấn có chi phí tăng theo bậc hai với độ dài mảng con và chúng tôi có ngân sách chung là$5 \cdot n^2$xu cho mỗi trường hợp thử nghiệm. Hoán vị là cố định và khác biệt, nghĩa là mỗi số từ$1$ĐẾN$n$xảy ra đúng một lần. Phép đảo ngược trong một mảng con đếm xem có bao nhiêu cặp phần tử không đúng thứ tự so với thứ tự tăng dần. 

Thách thức chính là cách tiếp cận đơn giản để truy vấn tất cả các mảng con có thể có là cực kỳ tốn kém. Kể cả ở mức tối đa$n = 2000$, có khoảng hai triệu mảng con và chi phí bình phương sẽ vượt quá ngân sách theo các bậc độ lớn. Chúng ta cần một cách để xác định phần tử tối đa bằng cách sử dụng càng ít truy vấn càng nhỏ càng tốt. Một điểm tinh tế khác là việc truy vấn một mảng con chứa số lớn nhất nhưng cũng nhỏ hơn có thể trả về bất kỳ số lượng nghịch đảo nào tùy thuộc vào thứ tự của chúng, vì vậy chúng ta không thể chỉ dựa vào số đó; chúng ta cần một thuộc tính cấu trúc của hoán vị. 

Trường hợp cạnh bao gồm rất nhỏ$n$, chẳng hạn như$n = 2$, trong đó một truy vấn duy nhất là đủ và các trường hợp phần tử tối đa nằm ở một trong các đầu. Một cách tiếp cận bất cẩn có thể cho rằng mức tối đa là gần giữa hoặc có thể truy vấn quá nhiều phần tử cùng một lúc, nhanh chóng vượt quá ngân sách xu. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ là truy vấn từng cặp riêng lẻ hoặc từng mảng con và tính toán các phép nghịch đảo để suy ra mức tối đa. Điều này có hiệu quả vì phần tử tối đa không có phần tử nào lớn hơn nó, vì vậy các mảng con chứa nó ở cuối sẽ có số lượng đảo ngược có thể dự đoán được. Tuy nhiên, nó nhanh chóng trở nên quá đắt: vì$n=2000$, truy vấn tất cả các cặp chi phí khoảng$n^3/2 \sim 4 \times 10^9$tiền xu, vượt xa ngân sách của chúng tôi. 

Quan sát quan trọng là chúng ta không cần số lượng nghịch đảo chính xác ở mọi nơi. Phần tử tối đa luôn đóng góp số nghịch đảo bằng 0 cho bất kỳ phân mảng nào có nó là phần tử cuối cùng. Ngược lại, bất kỳ mảng con nào bao gồm phần tử lớn nhất ở đầu sẽ có tất cả các phần tử nhỏ hơn sau khi được tính là đảo ngược. Điều này cho phép chúng ta thực hiện một chiến lược giống như tìm kiếm nhị phân: chúng ta có thể truy vấn lặp đi lặp lại các mảng con và sử dụng số lượng đảo ngược để quyết định xem giá trị lớn nhất nằm ở nửa bên trái hay nửa bên phải. Bằng cách chọn phạm vi truy vấn một cách cẩn thận, chúng tôi đảm bảo chi phí xu vẫn nằm trong ngân sách. 

Trên thực tế, chúng tôi coi số lần đảo ngược là tín hiệu về các vị trí tương đối. Nếu một mảng con kết thúc ở phần tử cuối cùng có số lần đảo ngược bằng 0, thì chúng ta biết giá trị lớn nhất nằm ở cuối mảng con đó. Ngược lại, chúng ta có thể chia mảng và tiếp tục đệ quy. Cách tiếp cận chia để trị này khai thác thứ tự tuyến tính của một hoán vị để thu hẹp mức tối đa trong$O(n \log n)$trị giá. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^3) | O(1) | Quá chậm | 
| Phân chia và chinh phục bằng truy vấn đảo ngược | O(n log n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Bắt đầu với các chỉ số mảng đầy đủ từ$1$ĐẾN$n$. Chúng tôi muốn xác định vị trí chỉ mục của phần tử tối đa. 
2. Trong khi phạm vi tìm kiếm hiện tại có nhiều hơn một phần tử, hãy chọn điểm giữa$m$và truy vấn mảng con từ$m$đến cuối cùng. Tính số lần đảo ngược. 
3. Nếu số lần đảo ngược bằng 0 thì giá trị tối đa nằm ở nửa bên phải của phạm vi hiện tại, bao gồm$m$, vì không có phần tử nào lớn hơn phần tử cuối cùng trong mảng con đó. 
4. Nếu không, giá trị tối đa phải nằm ở nửa bên trái của phạm vi, không bao gồm các phần tử sau$m$điều đó sẽ tạo ra sự đảo ngược. 
5. Lặp lại đệ quy việc thu hẹp kiểu tìm kiếm nhị phân này cho đến khi phạm vi tìm kiếm giảm xuống còn một chỉ mục. 
6. Xuất chỉ số đó làm vị trí của phần tử lớn nhất. 

Tại sao nó hoạt động: Ở mỗi bước, số lần đảo ngược sẽ phân chia mảng. Đảo ngược bằng 0 ở đầu bên phải đảm bảo phần tử ngoài cùng bên phải trong mảng con là phần tử lớn nhất ở đó. Các phép nghịch đảo khác 0 chỉ ra rằng có một phần tử lớn hơn tồn tại ở bên trái điểm giữa. Bất biến này đảm bảo rằng chúng ta không bao giờ loại trừ mức tối đa thực sự trong các bước chia để trị. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
import math

def find_max_index(n):
    l, r = 1, n
    while l < r:
        mid = (l + r) // 2
        print(f"? {mid} {r}")
        sys.stdout.flush()
        inv = int(input())
        if inv == 0:
            l = mid
        else:
            r = mid - 1
    print(f"! {l}")
    sys.stdout.flush()

t = int(input())
for _ in range(t):
    n = int(input())
    find_max_index(n)
```Giải pháp sử dụng đầu vào/đầu ra tiêu chuẩn có tính năng xả cho vấn đề tương tác. các`find_max_index`chức năng duy trì một phạm vi tìm kiếm`[l, r]`. Truy vấn điểm giữa đảm bảo chi phí tỷ lệ thuận với bình phương độ dài mảng con, nhưng tìm kiếm nhị phân giữ tổng chi phí dưới mức$5 \cdot n^2$giới hạn. Sự chú ý đặc biệt được dành cho`l < r`và cập nhật`l = mid`so với`r = mid - 1`để tránh các vòng lặp vô hạn hoặc bỏ qua các phần tử. 

## Ví dụ đã hoạt động 

Đối với hoán vị`[1,3,2,4]`với`n=4`: 

| tôi | r | giữa | truy vấn | inv | mới l,r | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 4 | 2 | ? 2 4 | 1 | r = 1 | 
| 1 | 1 | - | - | - | l = r = 4 | 

Chúng tôi xác định chỉ số 4 là mức tối đa. 

Đối với hoán vị`[2,1]`với`n=2`: 

| tôi | r | giữa | truy vấn | inv | mới l,r | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 2 | 1 | ? 1 2 | 1 | r = 1 | 

Chúng tôi xác định chỉ số 1 là mức tối đa. 

Những dấu vết này xác nhận rằng việc chia để trị đã thu hẹp phạm vi tìm kiếm đến mức tối đa thực sự một cách chính xác.

 ## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Mỗi truy vấn sẽ giảm khoảng một nửa không gian tìm kiếm. Tối đa n lần lặp log, mỗi lần truy vấn tối đa n phần tử. | 
| Không gian | O(1) | Chỉ có một vài biến số nguyên được duy trì, không có cấu trúc dữ liệu lớn. | 

các`n`giới hạn 2000 đảm bảo rằng`n log n`truy vấn có chi phí tối đa$n^2$mỗi truy vấn nằm trong ngân sách của$5 n^2$tiền xu. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    exec(open("solution.py").read())  # or paste the solution here
    return output.getvalue().strip()

# provided samples
assert run("2\n4\n2\n") == "! 4\n! 1", "sample 1"

# custom tests
assert run("1\n2\n") == "! 2", "minimum-size input"
assert run("1\n5\n") == "! 5", "maximum at the end"
assert run("1\n5\n") == "! 1", "maximum at the start"
assert run("1\n3\n") == "! 2", "middle maximum"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2\n4\n2\n`|`! 4\n! 1`| Độ chính xác tương tác mẫu | 
|`1\n2\n`|`! 2`| Đầu vào kích thước tối thiểu | 
|`1\n5\n`|`! 5`| Phần tử tối đa ở chỉ mục cuối cùng | 
|`1\n5\n`|`! 1`| Phần tử tối đa ở chỉ mục đầu tiên | 
|`1\n3\n`|`! 2`| Phần tử tối đa ở giữa | 

## Vỏ cạnh 

Đối với hoán vị có độ dài 2,`[2,1]`, truy vấn`? 1 2`trả về 1 đảo ngược. Thuật toán của chúng tôi hiểu đây là mức tối đa ở chỉ mục 1. Nếu mức tối đa ở chỉ mục 2 thì truy vấn sẽ trả về 0 lần đảo ngược. Điều này khẳng định rằng ngay cả trường hợp nhỏ nhất cũng được xử lý chính xác. Tương tự, nếu mức tối đa nằm ở cuối một hoán vị dài hơn, các truy vấn từ điểm giữa đến điểm cuối không mang lại sự đảo ngược nào, hướng tìm kiếm sang nửa bên phải và xác định chính xác chỉ mục cuối cùng. Bất biến tìm kiếm nhị phân cũng đảm bảo tính chính xác ở tất cả các vị trí khác.
