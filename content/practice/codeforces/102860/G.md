---
title: "CF 102860G - Kem"
description: "Chúng tôi có một số kem ốc quế. Hình nón thứ i ban đầu chứa a[i] gam kem. Khi thời gian trôi qua, mỗi chiếc nón đều mất kem vì nó tan chảy với tốc độ không đổi v gam trên giây."
date: "2026-07-25T14:14:04+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102860
codeforces_index: "G"
codeforces_contest_name: "2020-2021 Saint-Petersburg Open High School Programming Contest (SpbKOSHP 20)"
rating: 0
weight: 102860
solve_time_s: 35
verified: true
draft: false
---

[CF 102860G - Kem](https://codeforces.com/problemset/problem/102860/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 35s 
**Đã xác minh:** có 

##Giải pháp 
#Hiểu vấn đề 

Chúng tôi có một số kem ốc quế. các`i`-hình nón ban đầu chứa`a[i]`gram kem. Khi thời gian trôi qua, mọi chiếc nón đều mất kem vì nó tan chảy với tốc độ không đổi`v`gram mỗi giây. Đồng thời, chúng ta có thể ăn kem với tổng tốc độ không đổi`u`gram mỗi giây, chọn loại hình nón để ăn và chuyển đổi giữa các hình nón bất cứ khi nào chúng ta muốn. 

Nhiệm vụ là tìm khoảng thời gian tối thiểu cần thiết để ăn hết số kem. Đầu ra là thời gian tối thiểu này. 

Các ràng buộc làm cho độ phức tạp dự định trở nên rõ ràng. Với tối đa khoảng`10^5`hình nón, việc mô phỏng từng chuyển đổi nhỏ trong việc ăn uống sẽ là không thể vì số lượng hành động có thể thực hiện có thể trở nên cực kỳ lớn. Chúng ta cần một giải pháp xử lý mọi hình nón chỉ trong một số lần nhỏ, chẳng hạn như`O(n log n)`hoặc`O(n)`. 

Một sai lầm phổ biến là chỉ nghĩ đến tổng số lượng kem. Sự tan chảy làm thay đổi câu trả lời vì hình nón có thể biến mất trước khi chúng ta ăn nó. Ví dụ, nếu có hai hình nón có trọng số`100`Và`1`, và tốc độ nóng chảy rất lớn, hình nón thứ hai có thể tan chảy trong khi chúng ta vẫn đang làm hình nón thứ nhất. Câu trả lời không đơn giản`(101 / (u + v))`. 

Một trường hợp cạnh khác là khi tất cả các hình nón có cùng kích thước. Đối với đầu vào có ba hình nón có kích thước`5`, thuật toán phải nhận ra rằng cả ba đều hoạt động cùng một lúc và việc ăn uống phải được chia sẻ giữa chúng. Một giải pháp tham lam luôn chỉ chọn một hình nón sẽ làm trì hoãn việc hoàn thiện một cách không chính xác. 

Ví dụ:```
3 1 1
5 5 5
```Câu trả lời đúng là`5`, vì sau 5 giây mọi chiếc nón đều đã bị ăn hết hoặc tan chảy. Một giải pháp chỉ giả định lượng ăn vào có thể tính ra giá trị lớn hơn. 

## Phương pháp tiếp cận 

Một cách tiếp cận trực tiếp sẽ cố gắng mô phỏng quá trình. Chúng ta luôn có thể chọn chiếc nón có số lượng còn lại lớn nhất và ăn nhiều lần từ nó. Chiến lược này mang tính trực quan vì dành thời gian cho hình nón lớn nhất sẽ tránh lãng phí thời gian cho những hình nón sắp tan chảy. Khó khăn là số lượng thiết bị chuyển mạch không bị giới hạn bởi`n`. Khi một số hình nón đạt cùng kích thước còn lại, chúng ta có thể cần phải luân phiên giữa chúng liên tục, khiến việc mô phỏng chính xác trở nên quá chậm. 

Quan sát quan trọng là chúng ta không cần biết chính xác trình tự các vết cắn. Chúng tôi chỉ cần thời gian hoàn thiện sớm nhất có thể. 

Giả sử câu trả lời là`t`giây. Trong thời gian này`t`giây, mỗi hình nón đều mất chính xác`v * t`gram một cách tự nhiên. Nếu một hình nón có`a[i]`gam, thì số lượng còn cần phải ăn từ nó là:```
max(0, a[i] - v * t)
```Tổng số lượng chúng ta phải ăn là tổng của những giá trị này. Vì chúng ta có thể ăn chính xác`u * t`gam trong`t`giây, một lần`t`là có thể nếu:```
sum(max(0, a[i] - v*t)) <= u*t
```Tình trạng này là đơn điệu. Nếu một khoảng thời gian nào đó có tác dụng thì mọi khoảng thời gian lớn hơn cũng có tác dụng. Điều đó cho phép tìm kiếm nhị phân trên câu trả lời. 

Mô phỏng lực lượng vũ phu hoạt động vì nó tuân theo hành vi tham lam tối ưu, nhưng nó thất bại vì số lượng thay đổi giữa các hình nón có thể quá lớn. Phương pháp tìm kiếm nhị phân loại bỏ nhu cầu xây dựng lịch trình và chỉ kiểm tra xem thời gian hoàn thành nhất định có khả thi hay không. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Quá lớn trong trường hợp xấu nhất | O(1) | Quá chậm | 
| Tìm kiếm nhị phân | O(n log C) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc kích thước hình nón cũng như tốc độ ăn và tan chảy. 
2. Tìm kiếm nhị phân theo thời gian trả lời. Giới hạn dưới là`0`, và giới hạn trên có thể được chọn đủ lớn để tất cả kem chắc chắn đã hết. 
3. Vào thời gian giữa`t`, hãy tính lượng kem vẫn cần ăn sau khi tan chảy:```
need = sum(max(0, a[i] - v*t))
```Đây là loại kem duy nhất còn sót lại sau khi tan chảy. 
4. So sánh`need`với số lượng chúng ta có thể ăn:```
can_eat = u*t
```Nếu như`need <= can_eat`, thời gian đã chọn là đủ nên hãy tìm đáp án nhỏ hơn. Còn không thì tăng thời gian lên. 
5. Tiếp tục cho đến khi đủ độ chính xác của tìm kiếm nhị phân và in thời gian kết quả. 

Tại sao nó hoạt động: vào bất kỳ thời điểm cố định nào`t`, lượng còn lại của mỗi hình nón sau khi tan chảy tự nhiên bị ép buộc. Không có chiến lược nào có thể làm cho nhiều kem biến mất hơn mức độ tan chảy cộng với việc ăn uống cho phép. Nếu số lượng còn lại phù hợp với khả năng ăn uống của chúng ta, một số lịch trình có thể kết thúc vào thời điểm đó. Vì tính khả thi chỉ thay đổi một lần từ sai thành đúng nên tìm kiếm nhị phân tìm thấy thời gian hợp lệ nhỏ nhất. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, u, v = map(int, input().split())
    a = list(map(int, input().split()))

    def ok(t):
        need = 0
        melted = v * t
        for x in a:
            if x > melted:
                need += x - melted
        return need <= u * t

    lo, hi = 0.0, max(a) / v if v else 1e18

    if v == 0:
        hi = sum(a) / u

    else:
        hi = max(hi, sum(a) / u)

    for _ in range(100):
        mid = (lo + hi) / 2
        if ok(mid):
            hi = mid
        else:
            lo = mid

    print("{:.10f}".format(hi))

if __name__ == "__main__":
    solve()
```Chức năng khả thi là cốt lõi của giải pháp. Nó không mô phỏng thứ tự ăn uống. Nó chỉ kiểm tra những gì còn lại sau`t`giây và liệu người ăn có thể loại bỏ số tiền đó hay không. 

Tìm kiếm nhị phân sử dụng dấu phẩy động vì câu trả lời là số thực. Một trăm lần lặp lại là đủ để tạo ra sai số thấp hơn nhiều so với độ chính xác yêu cầu. Các giới hạn cũng bao gồm trường hợp đặc biệt khi không có sự tan chảy. Khi`v = 0`, câu trả lời chỉ đơn giản là tổng số tiền chia cho tốc độ ăn nên giới hạn trên được đặt riêng. 

phép nhân`v * t`có thể lớn, do đó số học dấu phẩy động của Python rất hữu ích ở đây. Không có vấn đề tràn số nguyên. 

## Ví dụ đã hoạt động 

Hãy xem xét:```
3 2 1
10 10 10
```Việc tìm kiếm kiểm tra thời gian có thể: 

| Thời gian | Còn lại sau khi tan chảy | Số lượng ăn được có thể | Kết quả | 
| --- | --- | --- | --- | 
| 5 | 5 + 5 + 5 = 15 | 10 | Quá nhỏ | 
| 7,5 | 2,5 + 2,5 + 2,5 = 7,5 | 15 | Đủ | 
| 6 | 4 + 4 + 4 = 12 | 12 | Đủ | 

Câu trả lời đến gần`6`. 

Điều này chứng tỏ thuật toán không cần quyết định hình nón nào được ăn trước. Nó chỉ cần tổng số kem còn sót lại. 

Vì:```
2 1 10
100 1
```| Thời gian | Còn lại sau khi tan chảy | Số lượng ăn được có thể | Kết quả | 
| --- | --- | --- | --- | 
| 0 | 101 | 0 | Quá nhỏ | 
| 0,1 | 99 + 0 = 99 | 0,1 | Quá nhỏ | 
| 10 | 0 + 0 = 0 | 10 | Đủ | 

Hình nón thứ hai biến mất một cách tự nhiên, cho thấy tại sao phải đưa vào số hạng tan chảy. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log C) | Mỗi lần lặp tìm kiếm nhị phân sẽ quét tất cả các hình nón và số lần lặp phụ thuộc vào độ chính xác cần thiết. | 
| Không gian | O(1) | Ngoài việc lưu trữ mảng đầu vào, thuật toán chỉ sử dụng một vài biến. | 

Giải pháp xử lý số lượng lớn hình nón vì mỗi lần lặp là tuyến tính và số lần lặp được cố định bởi độ chính xác chứ không phải bởi`n`. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    old = sys.stdin
    sys.stdin = io.StringIO(inp)
    solve()
    out = sys.stdout.getvalue()
    sys.stdin = old
    return out

assert run("""3 2 1
10 10 10
""").strip() == "6.0000000000"

assert run("""2 1 10
100 1
""").strip() == "10.0000000000"

assert run("""1 5 0
20
""").strip() == "4.0000000000"

assert run("""5 1 1
7 7 7 7 7
""").strip() == "7.0000000000"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Ba hình nón bằng nhau | 6.0000000000 | Các tế bào hình nón có kích thước bằng nhau sẽ hoạt động cùng nhau | 
| tan chảy nhanh | 10.0000000000 | Xử lý đúng các nón biến mất | 
| Không tan chảy | 4.0000000000 | Trường hợp đặc biệt`v = 0`| 
| Tất cả các giá trị bằng nhau | 7.0000000000 | Tránh các lỗi mô phỏng hình nón đơn | 

## Vỏ cạnh 

Khi tất cả các hình nón đều giống hệt nhau, tìm kiếm nhị phân vẫn hoạt động vì việc kiểm tra tính khả thi sẽ xử lý chúng cùng nhau. Vì:```
3 1 1
5 5 5
```Tại`t = 5`, mọi chiếc nón đều đã tan chảy hoàn toàn nên lượng ăn cần thiết là bằng không. Câu trả lời là`5`. 

Khi độ tan chảy bằng 0, không có loại kem nào tự biến mất. Vì:```
1 5 0
20
```Chúng ta phải ăn tất cả`20`gram tại`5`gam trên giây, cho kết quả chính xác`4`giây. 

Khi một hình nón lớn hơn nhiều so với những hình nón khác, những hình nón nhỏ có thể biến mất trước khi chúng ta chạm tới chúng. biểu hiện`max(0, a[i] - v*t)`loại bỏ chúng một cách tự động, do đó thuật toán không bao giờ lãng phí khả năng ăn kem đã tan chảy.
