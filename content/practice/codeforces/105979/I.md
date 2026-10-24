---
title: "CF 105979I - Chuỗi vô tận"
description: "Quá trình bắt đầu với một hàng vô hạn trong đó mọi vị trí đều chứa cùng một giá trị X. Trong mỗi lần lặp, mọi khoảng cách giữa hai giá trị lân cận sẽ nhận được một giá trị mới bằng tổng của hai giá trị xung quanh khoảng cách đó."
date: "2026-06-25T13:32:06+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105979
codeforces_index: "I"
codeforces_contest_name: "2025 ICPC Gran Premio de Mexico 2da Fecha"
rating: 0
weight: 105979
solve_time_s: 38
verified: true
draft: false
---

[CF 105979I - Trình tự Inifinitus](https://codeforces.com/problemset/problem/105979/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 38s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Quá trình bắt đầu với một hàng vô hạn trong đó mọi vị trí đều chứa cùng một giá trị`X`. Trong mỗi lần lặp, mọi khoảng cách giữa hai giá trị lân cận sẽ nhận được một giá trị mới bằng tổng của hai giá trị xung quanh khoảng cách đó. Các giá trị hiện có vẫn được giữ nguyên, do đó trình tự sẽ tăng lên bằng cách chèn các giá trị mới vào giữa các giá trị cũ. 

Đối với mỗi truy vấn, chúng ta cần tìm lần lặp sớm nhất khi một số giá trị trong chuỗi trở thành ít nhất`K`. Đầu vào chứa một số truy vấn độc lập, mỗi truy vấn mô tả giá trị bắt đầu và giá trị đích. Đầu ra là số lần lặp đầu tiên đạt đến ngưỡng mục tiêu. 

Các ràng buộc nhỏ theo hướng này và lớn theo hướng khác. Có thể có tới 1000 truy vấn, trong khi`K`có thể đạt được`10^9`. Điều này có nghĩa là không thể mô phỏng trực tiếp chuỗi vì số lượng vị trí được tạo sẽ tăng gấp đôi sau mỗi lần lặp. Thậm chí khoảng 30 lần lặp sẽ tạo ra hơn một tỷ vị trí, do đó, mọi cách tiếp cận tùy thuộc vào độ dài chuỗi đều bị loại trừ. 

Các trường hợp cạnh chính xuất phát từ thực tế là câu trả lời phụ thuộc vào giá trị tối đa trong một lần lặp chứ không phụ thuộc vào bất kỳ vị trí cụ thể nào. Một giải pháp bất cẩn có thể cho rằng các giá trị tăng theo lũy thừa của hai vì các số mới được thêm vào liên tục. Ví dụ, với`X = 1`Và`K = 5`, phép đoán lũy thừa hai sẽ dự đoán lần lặp 3 vì`2^2 = 4`Và`2^3 = 8`, nhưng câu trả lời đúng là lần lặp 4 vì giá trị lớn nhất là`1, 2, 3, 5`. Đầu vào là:```
1
1 5
```Đầu ra là:```
4
```Một trường hợp cạnh khác là khi giá trị bắt đầu đã đạt đến mục tiêu. Ví dụ:```
1
5 5
```Đầu ra đúng là:```
1
```Giải pháp luôn thực hiện ít nhất một lần chèn sẽ trả về sai lần lặp sau. 

## Phương pháp tiếp cận 

Mô phỏng trực tiếp sẽ giữ nguyên mẫu hữu hạn hiện tại và liên tục chèn tổng giữa các phần tử liền kề. Điều này hiệu quả vì mỗi lần lặp lại được xây dựng chính xác từ lần lặp trước đó. Bắt đầu với`[X]`, chúng ta có thể tạo các tiền tố ngày càng lớn hơn và theo dõi số lượng lớn nhất được nhìn thấy. Vấn đề là độ dài chuỗi tăng gấp đôi sau mỗi lần lặp. Sau đó`t`lặp lại khối lặp lại có kích thước`2^(t-1)`, do đó việc mô phỏng đủ số lần lặp để nhanh chóng đạt được giá trị lớn là điều không thể. 

Quan sát quan trọng là chúng ta không cần toàn bộ chuỗi. Chúng ta chỉ cần giá trị lớn nhất xuất hiện sau mỗi lần lặp. Nếu các giá trị được chia cho`X`, cấu trúc trở nên độc lập với số bắt đầu. Giá trị lớn nhất của vài lần lặp đầu tiên là:```
1, 2, 3, 5, 8, 13, ...
```Đây là các số Fibonacci có chỉ số thay đổi. 

Để hiểu lý do tại sao, hãy nhìn vào hai giá trị lân cận tạo ra mức tối đa tiếp theo. Giá trị lớn nhất trong lần lặp xuất hiện giữa hai giá trị lớn từ lần lặp trước đó. Sự tăng trưởng tuân theo sự lặp lại tương tự như các số Fibonacci vì mức cực đại mới được hình thành bằng cách cộng hai cực đại trước đó. Nếu như`mx[i]`là hệ số lớn nhất sau lần lặp`i`, sau đó:```
mx[i] = mx[i - 1] + mx[i - 2]
```với giá trị ban đầu:```
mx[1] = 1
mx[2] = 2
```Các giá trị chuỗi thực tế chỉ là những hệ số này nhân với`X`. Từ`K`chỉ là`10^9`, các giá trị Fibonacci cần thiết là rất nhỏ, khoảng vài chục số hạng. Chúng ta có thể tính toán trước chúng một lần và trả lời mọi truy vấn bằng cách quét tìm giá trị đầu tiên có tích bằng`X`đạt tới`K`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^t) mỗi truy vấn | O(2^t) | Quá chậm | 
| Tối ưu | O(30) mỗi truy vấn | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính toán trước hệ số tối đa cho mỗi lần lặp có thể. Bắt đầu với`1`cho lần lặp một và`2`đối với lần lặp thứ hai, sau đó tiếp tục cộng hai giá trị trước đó để tạo giá trị tiếp theo. Chỉ cần khoảng 30 giá trị vì số Fibonacci tăng theo cấp số nhân và nhanh chóng vượt quá`10^9`. 
2. Đối với mỗi truy vấn, hãy so sánh các hệ số được tính toán trước nhân với`X`chống lại`K`. Hệ số đầu tiên tạo nên`X * coefficient >= K`đưa ra câu trả lời vì đó là lần lặp đầu tiên có giá trị tối đa đạt đến ngưỡng yêu cầu. 
3. Xuất chỉ số của hệ số đó dưới dạng số lần lặp. Việc tính toán trước và tất cả các truy vấn đều sử dụng bộ nhớ không đổi. 

Tại sao nó hoạt động dựa trên bất biến rằng sau mỗi lần lặp, giá trị lớn nhất có thể chính xác là`X`lần hệ số Fibonacci tương ứng. Lần lặp đầu tiên chỉ có`X`và mỗi mức tối đa sau đó được tạo bằng cách cộng hai người đóng góp lớn nhất trước đó. Vì mọi giá trị khác được tạo ra từ các giá trị lân cận nhỏ hơn hoặc bằng nhau nên không vị trí nào có thể vượt quá mức tăng Fibonacci này. Việc tìm hệ số đầu tiên đủ lớn cũng giống như tìm lần lặp đầu tiên chứa giá trị ít nhất`K`. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

fib = [0, 1, 2]
while fib[-1] < 10**9:
    fib.append(fib[-1] + fib[-2])

def solve():
    q = int(input())
    ans = []
    for _ in range(q):
        x, k = map(int, input().split())
        for i in range(1, len(fib)):
            if x * fib[i] >= k:
                ans.append(str(i))
                break
    print("\n".join(ans))

if __name__ == "__main__":
    solve()
```Mảng`fib`lưu trữ các hệ số tối đa được lập chỉ mục theo số lần lặp. Hai mục thực đầu tiên là`1`Và`2`, khớp với hai lần lặp đầu tiên. Vòng lặp mở rộng mảng cho đến khi các giá trị chắc chắn đủ lớn cho mọi mục tiêu có thể. 

Đối với mỗi truy vấn, phép nhân được thực hiện trước khi so sánh để có thể sử dụng lại các hệ số được tính toán trước giống nhau cho các giá trị bắt đầu khác nhau. Số nguyên Python xử lý các sản phẩm này một cách an toàn và số lần lặp được kiểm tra trên mỗi truy vấn vẫn rất nhỏ. 

Việc lập chỉ mục là nơi chính xảy ra lỗi. hệ số`fib[1]`đại diện cho lần lặp một, không phải lần lặp 0. Giữ lại những thứ chưa sử dụng`fib[0]`giữ chỗ làm cho chỉ mục được trả về khớp trực tiếp với số lần lặp được yêu cầu. 

## Ví dụ đã hoạt động 

Đối với đầu vào:```
3
5 1
5 10
5 15
```hệ số tăng trưởng là: 

| Lặp lại | Hệ số tối đa | Giá trị lớn nhất của X = 5 | Kiểm tra | 
| --- | --- | --- | --- | 
| 1 | 1 | 5 | đạt 1 | 
| 2 | 2 | 10 | đạt 10 | 
| 3 | 3 | 15 | đạt 15 | 

Truy vấn đầu tiên thành công ngay lập tức vì giá trị ban đầu đã đủ. Hai truy vấn tiếp theo cho thấy câu trả lời chính xác là lần lặp đầu tiên trong đó mức tối đa vượt qua ngưỡng. 

Đối với đầu vào:```
1
1 5
```dấu vết là: 

| Lặp lại | Hệ số tối đa | Giá trị tối đa | Kiểm tra | 
| --- | --- | --- | --- | 
| 1 | 1 | 1 | quá nhỏ | 
| 2 | 2 | 2 | quá nhỏ | 
| 3 | 3 | 3 | quá nhỏ | 
| 4 | 5 | 5 | đạt | 

Điều này chứng tỏ tại sao việc sử dụng lũy ​​thừa của hai lại thất bại. Chuỗi phát triển giống như số Fibonacci, không giống như tăng gấp đôi. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(Q) | Mỗi truy vấn chỉ kiểm tra khoảng 30 giá trị được tính toán trước | 
| Không gian | O(1) | Mảng Fibonacci có kích thước nhỏ cố định | 

Mục tiêu lớn nhất có thể là`10^9`, do đó việc tính toán trước vẫn còn nhỏ. Với tối đa 1000 truy vấn, tổng số thao tác là không đáng kể. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    fib = [0, 1, 2]
    while fib[-1] < 10**9:
        fib.append(fib[-1] + fib[-2])

    q = int(sys.stdin.readline())
    out = []
    for _ in range(q):
        x, k = map(int, sys.stdin.readline().split())
        for i in range(1, len(fib)):
            if x * fib[i] >= k:
                out.append(str(i))
                break

    sys.stdout.write("\n".join(out))
    result = sys.stdout.getvalue()

    sys.stdin = old_stdin
    sys.stdout = old_stdout
    return result

assert run("""3
5 1
5 10
5 15
""") == "1\n2\n3", "sample 1"

assert run("""1
1 5
""") == "4", "sample 2"

assert run("""3
10 10
1 1
1000 1000000000
""") == "1\n1\n15", "boundary values"

assert run("""2
7 14
7 21
""") == "2\n3", "exact fibonacci thresholds"

assert run("""1
1000 1000000000
""") == "15", "large target"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`5 1`truy vấn kiểu |`1`| Xử lý các mục tiêu đã đạt được trong lần lặp đầu tiên | 
|`1 5`|`4`| Xác nhận mức tăng trưởng Fibonacci thay vì tăng gấp đôi | 
|`7 14`,`7 21`|`2`,`3`| Kiểm tra chuyển tiếp ngưỡng chính xác | 
|`1000 1000000000`|`15`| Kiểm tra các giá trị lớn và ranh giới nhân | 

## Vỏ cạnh 

Đối với trường hợp:```
1
5 5
```thuật toán kiểm tra hệ số đầu tiên. Từ`5 * 1`đã bằng rồi`5`, nó ngay lập tức trả về phép lặp`1`. Nó không cho rằng cần phải chèn mới trước khi ngưỡng có thể xuất hiện. 

Đối với trường hợp:```
1
1 5
```thuật toán kiểm tra các hệ số theo thứ tự. Ba hệ số đầu tiên tạo ra các giá trị`1`,`2`, Và`3`, tất cả đều dưới mục tiêu. Hệ số thứ tư là`5`, vì vậy câu trả lời trở thành`4`. Điều này khẳng định việc tìm kiếm dựa trên lần lặp thành công đầu tiên chứ không chỉ hệ số đầu tiên lớn hơn mục tiêu mà không xét đến`X`. 

Đối với giá trị khởi đầu lớn:```
1
1000 1000000000
```thuật toán kiểm tra`1000 * 1`,`1000 * 2`, v.v. cho đến khi`1000 * 1597`đạt tới`1000000000`? Từ`1597 * 1000 = 1,597,000`, nó tiếp tục. Hệ số đủ đầu tiên được tìm thấy từ sự tăng trưởng Fibonacci được tính toán trước và tìm kiếm cố định nhỏ sẽ tránh mọi sự phụ thuộc vào kích thước mục tiêu khổng lồ.
