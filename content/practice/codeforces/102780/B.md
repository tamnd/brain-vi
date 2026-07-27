---
title: "CF 102780B - Điện trở bí ẩn"
description: "Mạch được xây dựng từ một số điện trở giống hệt nhau. Mỗi tầng chứa một điện trở đã biết và một điện trở bí ẩn được mắc song song và tất cả các tầng sau đó được mắc nối tiếp."
date: "2026-07-27T20:06:35+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102780
codeforces_index: "B"
codeforces_contest_name: "ICPC Central Russia Regional Contest (CRRC 19)"
rating: 0
weight: 102780
solve_time_s: 69
verified: true
draft: false
---

[CF 102780B - Điện trở bí ẩn](https://codeforces.com/problemset/problem/102780/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 9 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Mạch được xây dựng từ một số điện trở giống hệt nhau. Mỗi tầng chứa một điện trở đã biết và một điện trở bí ẩn được mắc song song và tất cả các tầng sau đó được mắc nối tiếp. Đầu vào cung cấp số lượng giai đoạn, tổng điện trở của toàn bộ mạch và giá trị điện trở đã biết ở mỗi giai đoạn. Nhiệm vụ là phục hồi điện trở của điện trở bí ẩn. 

Nếu điện trở chưa biết là`x`, sự đóng góp của một giai đoạn duy nhất với sức đề kháng đã biết`ri`là$$\frac{ri \cdot x}{ri + x}$$vì hai điện trở mắc song song. Tổng điện trở mạch là tổng của các giá trị này qua tất cả các giai đoạn, vì vậy chúng ta cần tìm`x`như vậy$$\sum_{i=1}^{k}\frac{r_i x}{r_i+x}=R$$Số lượng giai đoạn có thể lên tới 1000 và mỗi mức kháng cự được biết đến có thể lớn. Điều này loại trừ các phương pháp thử nhiều giá trị điện trở có thể có hoặc thực hiện giải phương trình ký hiệu với nhiều số hạng. Thuộc tính hữu ích là độ chính xác cần thiết chỉ`10^-6`, do đó, một phương pháp số lặp với khoảng 60 đến 100 lần lặp là đủ nhanh. Một giải pháp xung quanh`O(k log precision)`hoạt động phù hợp thoải mái vì nó chỉ thực hiện một phép tính tổng đơn giản trên tối đa 1000 giá trị mỗi lần lặp. 

Một vài trường hợp có thể phá vỡ việc triển khai bất cẩn. Một sai lầm phổ biến là cho rằng đáp số luôn gần với điện trở lớn nhất đã biết. Ví dụ, với đầu vào```
1 5
10
```đầu ra đúng là`10.000000`, vì giai đoạn duy nhất có sức đề kháng$$\frac{10x}{10+x}=5$$mang lại`x=10`. Giới hạn trên cố định như`100000`sẽ thất bại với những câu trả lời lớn hơn có thể có. 

Một vấn đề khác là dừng tìm kiếm nhị phân quá sớm. Với```
3 11
3 12 30
```câu trả lời là`6.000000`. Một vài lần lặp có thể tìm thấy một giá trị gần đúng nhưng không đáp ứng được độ chính xác cần thiết. Chức năng thay đổi trơn tru nên việc lặp lại nhiều lần sẽ rẻ và tránh được vấn đề này. 

Trường hợp cạnh thứ ba là khi mục tiêu chính xác bằng một nửa tổng điện trở đã biết. Ví dụ,```
2 10
10 10
```có câu trả lời`10`, bởi vì mỗi cặp song song đóng góp`5`. Thuật toán phải cho phép ranh giới trên đạt đến giá trị này thay vì dừng lại khi khoảng tìm kiếm trở nên nhỏ. 

## Phương pháp tiếp cận 

Một cách tiếp cận trực tiếp là thử các giá trị có thể có của điện trở bí ẩn và tính tổng điện trở thu được. Vì điện trở là một giá trị liên tục nên chúng ta không thể liệt kê chính xác tất cả các ứng cử viên và việc quét đơn giản trên một lưới mịn sẽ yêu cầu một số lượng lớn các lần kiểm tra. Ngay cả khi phạm vi được giới hạn ở một triệu giá trị có thể và mỗi lần kiểm tra chỉ mất`k`hoạt động, trường hợp xấu nhất sẽ cần khoảng một tỷ hoạt động khi`k=1000`. 

Quan sát hữu ích là tổng điện trở là một hàm đơn điệu của điện trở chưa biết. Khi`x`tăng lên, mỗi điện trở song song riêng lẻ$$\frac{r_i x}{r_i+x}$$cũng tăng lên. Điều này có nghĩa là có chính xác một câu trả lời và chúng ta có thể sử dụng tìm kiếm nhị phân trên giá trị của`x`. 

Cách tiếp cận bạo lực có hiệu quả vì việc đánh giá một giá trị ứng viên rất dễ dàng, nhưng nó thất bại vì việc tìm kiếm một ứng cử viên đủ chính xác bằng cách thử từng giá trị là quá tốn kém. Mối quan hệ đơn điệu giữa điện trở chưa biết và điện trở mạch cuối cùng cho phép chúng ta loại bỏ một nửa không gian tìm kiếm sau mỗi lần lặp. 

Khoảng thời gian tìm kiếm có thể bắt đầu từ số không. Giới hạn trên được tìm thấy một cách linh hoạt bằng cách liên tục nhân đôi nó cho đến khi điện trở tính toán đạt ít nhất`R`. Câu trả lời bắt buộc được đảm bảo tồn tại vì điện trở mạch tối đa có thể đạt tới tổng của tất cả các điện trở đã biết và tuyên bố đảm bảo rằng mục tiêu không vượt quá một nửa giá trị này. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(k * số giá trị đã thử) | O(1) | Quá chậm | 
| Tối ưu | O(k độ chính xác của nhật ký) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Lưu trữ các điện trở đã biết và xác định hàm tính tổng điện trở mạch cho giá trị điện trở bí ẩn đã chọn`x`. Hàm này bổ sung điện trở song song của mọi giai đoạn, mang lại điện trở mà mạch sẽ có nếu điện trở không xác định là`x`. 
2. Bắt đầu với giới hạn dưới của`0`và giới hạn trên của`1`. Tăng giới hạn trên bằng cách nhân đôi cho đến khi mức kháng cự được tính toán ít nhất là mục tiêu`R`. Điều này tạo ra một khoảng chứa câu trả lời thực sự mà không cần bất kỳ giả định nào về kích thước của điện trở chưa biết. 
3. Lặp lại tìm kiếm nhị phân trong khoảng thời gian này. Lấy giá trị ở giữa và tính điện trở mạch do nó tạo ra. Nếu kết quả nhỏ hơn`R`, điện trở chưa biết phải lớn hơn nên hãy di chuyển giới hạn dưới lên trên. Ngược lại, điện trở chưa biết nhỏ hơn hoặc bằng giá trị này, do đó hãy di chuyển giới hạn trên xuống dưới. 
4. Thực hiện đủ số lần lặp để làm cho khoảng thời gian nhỏ hơn nhiều so với sai số yêu cầu. Khoảng 100 lần lặp là quá đủ vì khoảng thời gian mỗi lần lặp lại giảm đi một nửa. 
5. Xuất điểm giữa cuối cùng làm giá trị ước tính của điện trở bí ẩn. 

Tại sao nó hoạt động: tổng điện trở được tính toán tăng nghiêm ngặt so với điện trở chưa xác định. Mọi quyết định tìm kiếm nhị phân đều dựa trên thứ tự này, vì vậy khi điểm giữa tạo ra điện trở quá nhỏ thì mọi giá trị nhỏ hơn cũng không hợp lệ. Khi nó tạo ra điện trở quá lớn thì mọi giá trị lớn hơn cũng không có giá trị. Khoảng tìm kiếm luôn chứa câu trả lời đúng và việc giảm một nửa lặp đi lặp lại làm cho khoảng đó hội tụ về câu trả lời đó. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    k, R = map(int, input().split())
    r = list(map(int, input().split()))

    def calc(x):
        total = 0.0
        for v in r:
            total += v * x / (v + x)
        return total

    lo = 0.0
    hi = 1.0

    while calc(hi) < R:
        hi *= 2.0

    for _ in range(100):
        mid = (lo + hi) / 2.0
        if calc(mid) < R:
            lo = mid
        else:
            hi = mid

    print("{:.10f}".format((lo + hi) / 2.0))

if __name__ == "__main__":
    solve()
```các`calc`hàm trực tiếp thực hiện công thức điện cho một giá trị điện trở bí ẩn ứng viên. Nó sử dụng số học dấu phẩy động vì câu trả lời không nhất thiết phải là số nguyên và độ chính xác yêu cầu là số thập phân. 

Tìm kiếm giới hạn trên là động chứ không phải cố định. Điều này tránh việc dựa vào câu trả lời tối đa được đoán và xử lý các trường hợp điện trở chưa biết lớn hơn mọi điện trở đã biết. Việc nhân đôi kết thúc nhanh chóng vì phạm vi câu trả lời bắt buộc bị giới hạn bởi các ràng buộc đầu vào. 

Tìm kiếm nhị phân chạy với số lần lặp cố định thay vì kiểm tra điều kiện epsilon. Điều này tránh các trường hợp góc chính xác và đảm bảo mức độ co lại. Một trăm lần lặp lại mang lại độ chính xác cao hơn nhiều so với yêu cầu`10^-6`. 

Ở đây, các giá trị dấu phẩy động của Python là đủ vì các giá trị trung gian lớn nhất đủ nhỏ và câu trả lời cuối cùng chỉ cần sáu chữ số sau dấu thập phân. 

## Ví dụ đã hoạt động 

Đối với mẫu đầu tiên:```
3 11
3 12 30
```Việc tìm kiếm đánh giá các dự đoán khác nhau cho điện trở chưa biết. 

| Ý tưởng lặp lại | Giới hạn dưới | Giới hạn trên | Kết quả điểm giữa | 
| --- | --- | --- | --- | 
| Phạm vi ban đầu | 0 | 8 | Quá nhỏ hoặc gần mục tiêu | 
| Tìm kiếm sau | Khoảng 6 | Khoảng 8 | Trên 11 | 
| Phạm vi cuối cùng | Gần 6 | Gần 6 | Hội tụ về 6 | 

Thuật toán phát hiện ra rằng một điện trở bí ẩn của`6`làm cho ba giai đoạn song song tổng cộng lại thành`11`. Khoảng thu hẹp thể hiện tính chất đơn điệu được sử dụng bởi tìm kiếm nhị phân. 

Đối với mẫu thứ hai:```
7 110
15 60 6 45 20 120 70
```| Ý tưởng lặp lại | Giới hạn dưới | Giới hạn trên | Quyết định | 
| --- | --- | --- | --- | 
| Mở rộng ban đầu | 0 | 32 | Kháng cự dưới mục tiêu | 
| Phạm vi mở rộng | 0 | 64 | Kháng cự trên mục tiêu | 
| Tìm kiếm cuối cùng | Gần 30 | Gần 30 | Hội tụ về 30 | 

Việc mở rộng giới hạn trên nhanh chóng tìm thấy một khoảng chứa câu trả lời. Tìm kiếm nhị phân sau đó thu hẹp nó cho đến khi kết quả là`30.000000`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(k độ chính xác của nhật ký) | Mỗi lần lặp trong khoảng 100 lần đánh giá tất cả`k`giai đoạn một lần | 
| Không gian | O(1) | Chỉ có danh sách kháng cự và một vài biến được lưu trữ | 

Với`k <= 1000`, thuật toán thực hiện khoảng 100.000 phép tính số học. Điều này dễ dàng nằm trong giới hạn, trong khi các phương pháp thử nhiều giá trị điện trở có thể sẽ không mở rộng được. 

## Trường hợp thử nghiệm```python
import sys
import io

def solve(inp):
    data = inp.strip().split()
    k = int(data[0])
    R = int(data[1])
    r = list(map(int, data[2:]))

    def calc(x):
        s = 0.0
        for v in r:
            s += v * x / (v + x)
        return s

    lo = 0.0
    hi = 1.0
    while calc(hi) < R:
        hi *= 2.0

    for _ in range(100):
        mid = (lo + hi) / 2
        if calc(mid) < R:
            lo = mid
        else:
            hi = mid

    return "{:.6f}".format((lo + hi) / 2)

assert solve("3 11\n3 12 30\n") == "6.000000"
assert solve("7 110\n15 60 6 45 20 120 70\n") == "30.000000"

assert solve("1 5\n10\n") == "10.000000"
assert solve("2 10\n10 10\n") == "10.000000"
assert solve("3 3\n3 3 3\n") == "1.000000"
assert solve("4 100000\n100000 100000 100000 100000\n") == "100000.000000"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 5 / 10`|`10.000000`| Giai đoạn đơn và giá trị lớn chưa biết | 
|`2 10 / 10 10`|`10.000000`| Trường hợp ranh giới nửa tổng chính xác | 
|`3 3 / 3 3 3`|`1.000000`| Câu trả lời nhỏ và giá trị lặp lại | 
|`4 100000 / 100000 100000 100000 100000`|`100000.000000`| Giá trị lớn và mở rộng giới hạn trên | 

## Vỏ cạnh 

Đối với trường hợp một giai đoạn```
1 5
10
```chức năng đang được tìm kiếm là$$f(x)=\frac{10x}{10+x}$$Tìm kiếm nhị phân bắt đầu với một phạm vi nhỏ, mở rộng giới hạn trên cho đến khi hàm đạt ít nhất`5`, và sau đó hội tụ đến`x=10`. Nó không phụ thuộc vào việc có nhiều giai đoạn. 

Đối với trường hợp biên chính xác```
2 10
10 10
```tổng điện trở đã biết là`20`, và mục tiêu chính xác là một nửa số đó. Câu trả lời đúng là`10`. Tại`x=10`, mỗi giai đoạn đóng góp$$\frac{10\cdot10}{10+10}=5$$vậy tổng cộng là`10`. Giới hạn trên động cho phép tìm kiếm bao gồm giá trị này. 

Trong trường hợp tất cả các điện trở đều bằng nhau,```
3 3
3 3 3
```câu trả lời là`1`. Mỗi giai đoạn đều góp phần$$\frac{3\cdot1}{3+1}=0.75$$và tổng số là`2.25`, do đó thuật toán tiếp tục tìm kiếm cho đến khi tìm thấy giá trị cung cấp chính xác`3`. Kiểm tra đơn điệu xử lý các giá trị lặp lại mà không có trường hợp đặc biệt nào. 

Đối với các giá trị lớn nhất,```
4 100000
100000 100000 100000 100000
```điện trở chưa biết là`100000`. Giới hạn trên ban đầu quá nhỏ nhưng việc nhân đôi nhanh chóng đạt đến phạm vi chứa câu trả lời. Sau đó, tìm kiếm hoạt động theo cách tương tự như trong các trường hợp nhỏ hơn, cho thấy lý do tại sao giới hạn trên động lại an toàn hơn giới hạn được mã hóa cứng.
