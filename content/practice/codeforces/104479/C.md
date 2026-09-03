---
title: "CF 104479C - Tích chập"
description: "Chúng ta được cho hai dãy và xác định một dãy mới hoạt động giống như một tích chập: mỗi vị trí k trong kết quả được hình thành bằng cách tính tổng tất cả các tích của các cặp phần tử có tổng chỉ số bằng k."
date: "2026-06-30T12:44:02+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104479
codeforces_index: "C"
codeforces_contest_name: "Adam G\u0105sienica\u2011Samek Contest 1"
rating: 0
weight: 104479
solve_time_s: 78
verified: true
draft: false
---

[CF 104479C - Tích chập](https://codeforces.com/problemset/problem/104479/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 18s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có hai chuỗi và xác định một chuỗi mới hoạt động giống như một phép tích chập: mỗi vị trí`k`trong kết quả được hình thành bằng cách tính tổng tất cả các tích của các cặp phần tử có chỉ số cộng lại bằng`k`. Tác vụ không yêu cầu chúng ta xuất toàn bộ tích chập này mà chỉ xuất tổng của tất cả các giá trị trong chuỗi tích chập đó. 

Vì vậy, thay vì tính toán từng khoản đóng góp theo cặp được nhóm theo tổng chỉ số, chúng ta chỉ cần đóng góp tổng hợp của tất cả các cặp trên tất cả các vị trí. 

Các ràng buộc rất lớn, lên tới 100.000 phần tử trong mỗi mảng, điều này ngay lập tức loại trừ bất kỳ phép liệt kê bậc hai nào của các cặp. Một vòng lặp kép ngây thơ sẽ tạo ra tới 10^10 phép nhân trong trường hợp xấu nhất, vượt xa mọi giới hạn thời gian. 

Một trường hợp khó nhận thấy là các giá trị có thể bằng 0, không đóng góp gì và tất cả các đóng góp đều độc lập và bổ sung, nghĩa là việc sắp xếp lại hoặc nhóm lại không làm thay đổi câu trả lời cuối cùng. 

## Phương pháp tiếp cận 

Việc giải thích vũ phu tính toán mọi`c[k]`một cách rõ ràng bằng cách lặp lại tất cả các giá trị hợp lệ`(i, j)`cặp như vậy`i + j = k`. Điều này đã ngụ ý một cấu trúc lồng nhau trên tất cả các cặp`(i, j)`trong hai mảng. Ngay cả khi chúng ta tính tổng theo`k`, mỗi cặp`(i, j)`vẫn được truy cập đúng một lần, đưa ra`O(nm)`quá trình. Điều này trở nên không thể ở quy mô lớn. 

Quan sát quan trọng là số lượng yêu cầu cuối cùng không phụ thuộc vào cách các thuật ngữ được nhóm theo`k`. Mỗi sản phẩm`a[i] * b[j]`xuất hiện đúng một lần trên tất cả`c[k]`, bởi vì mỗi cặp đóng góp vào đúng một chỉ mục`k = i + j`. Do đó, tổng của tất cả`c[k]`chỉ đơn giản là tổng của tất cả các tích theo cặp. 

Điều này làm sụp đổ hoàn toàn cấu trúc tích chập. Thay vì suy luận về tổng chỉ số, chúng ta chỉ cần tính tổng tích của tất cả các cặp. Điều đó còn đơn giản hóa hơn nữa vì biểu thức phân tích thành nhân tử: 

Mỗi phần tử của`a`được nhân với tổng của tất cả các phần tử trong`b`, và ngược lại. 

Vậy câu trả lời cuối cùng là:```
(sum of a) * (sum of b)
```Điều này làm giảm vấn đề về thời gian tuyến tính. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Bảng liệt kê cặp Brute Force | O(nm) | O(1) | Quá chậm | 
| Phân tích tổng | O(n + m) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi tính tổng của chuỗi đầu tiên, sau đó là tổng của chuỗi thứ hai. Cuối cùng, chúng tôi nhân chúng. 

1. Đọc hai số nguyên`n`Và`m`, cung cấp độ dài của mảng. 
2. Đọc mảng`a`và tính toán`sum_a`bằng cách tích lũy tất cả các yếu tố của nó. 
3. Đọc mảng`b`và tính toán`sum_b`tương tự. 
4. Đầu ra`sum_a * sum_b`. 

Bước lập luận quan trọng là nhận ra rằng tích chập phân bố trên tổng, do đó tổng khối lượng của tích chập chỉ là tích của tổng khối lượng của các chuỗi đầu vào. 

### Tại sao nó hoạt động 

Mỗi cặp`(i, j)`đóng góp chính xác`a[i] * b[j]`đến đúng một số hạng`c[i+j]`. Vì chúng ta đang tổng hợp tất cả`c[k]`, mỗi cặp được tính đúng một lần trong câu trả lời cuối cùng. Do đó, kết quả là tổng của tất cả các cặp, được phân tích thành tích của các tổng độc lập. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())
    a = list(map(int, input().split()))
    b = list(map(int, input().split()))

    sum_a = sum(a)
    sum_b = sum(b)

    print(sum_a * sum_b)

if __name__ == "__main__":
    solve()
```Giải pháp đọc cả hai mảng theo thời gian tuyến tính và tích lũy tổng của chúng. Phép nhân xảy ra một lần, do đó không có nguy cơ tràn trong Python ngoài việc xử lý số nguyên lớn thông thường. Điều tinh tế quan trọng là chúng ta không bao giờ cố gắng xây dựng tích chập một cách rõ ràng, điều này sẽ quá chậm. 

## Ví dụ đã hoạt động 

Hãy xem xét một ví dụ nhỏ: 

đầu vào:```
n = 3, m = 2
a = [1, 2, 3]
b = [4, 5]
```Chúng tôi tính toán: 

| Bước | tổng_a | tổng_b | trạng thái đang chạy | 
| --- | --- | --- | --- | 
| đọc một | 1 → 6 | - | tích lũy | 
| đọc b | 6 | 4 → 9 | tích lũy | 
| cuối cùng | 6 | 9 | 54 | 

Bản thân phép tích chập sẽ tạo ra nhiều`c[k]`, nhưng tổng hợp chúng sẽ thu gọn mọi thứ thành 54. 

Một ví dụ thứ hai:```
a = [0, 1, 2], b = [3, 0, 4]
```| Bước | tổng_a | tổng_b | kết quả | 
| --- | --- | --- | --- | 
| tính tổng | 3 | 7 | 21 | 

Mặc dù nhiều số hạng tích chập bằng 0 hoặc phân bố, tổng số vẫn là 21, xác nhận rằng các số 0 và phân bố không ảnh hưởng đến tổng toàn cục. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + m) | Mỗi mảng được quét một lần để tính tổng | 
| Không gian | O(1) | Chỉ tổng số đang chạy mới được lưu trữ | 

Điều này dễ dàng phù hợp với các ràng buộc lên tới 100.000 phần tử trên mỗi mảng vì chúng tôi chỉ thực hiện công việc tuyến tính. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys as _sys
    from io import StringIO
    backup = _sys.stdin
    _sys.stdin = StringIO(inp)

    n, m = map(int, input().split())
    a = list(map(int, input().split()))
    b = list(map(int, input().split()))
    ans = sum(a) * sum(b)
    _sys.stdin = backup
    return str(ans)

# provided sample
assert run("4 5\n2 1 3 7\n4 2 0 6 9\n") == str((2+1+3+7)*(4+2+0+6+9))

# custom cases
assert run("1 1\n5\n7\n") == "35", "single element"
assert run("3 3\n0 0 0\n1 2 3\n") == "0", "zeros case"
assert run("2 2\n1 2\n3 4\n") == "21", "small mixed"
assert run("5 1\n1 2 3 4 5\n10\n") == "150", "single column"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1x1 | sản phẩm | trường hợp cơ sở | 
| tất cả số không | 0 | không lan truyền | 
| hỗn hợp nhỏ | 21 | hành vi bình thường | 
| cột đơn | 150 | chiều không đối xứng | 

## Vỏ cạnh 

Khi một mảng chỉ chứa các số 0, mọi số hạng tích chập sẽ trở thành 0 và thuật toán tạo ra số 0 một cách chính xác vì tổng của mảng đó bằng 0. Khi cả hai mảng đều chứa một phần tử duy nhất, tích chập sẽ thu gọn thành một tích duy nhất, phù hợp với cả định nghĩa và công thức đơn giản hóa. Khi mảng lớn nhưng thưa thớt, việc tính toán vẫn tuyến tính vì chỉ tính tổng mới quan trọng chứ không phải các mẫu phân phối hoặc thưa thớt.
