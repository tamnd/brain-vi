---
title: "CF 1090M - Bước Đi Vui Vẻ"
description: "Chúng ta được cho một dãy nhà, mỗi dãy nhà được sơn một màu nguyên. Ta muốn chọn một đoạn liền kề của đường này sao cho trong đoạn đã chọn không có hai ngôi nhà lân cận nào có cùng màu."
date: "2026-06-13T04:03:49+07:00"
tags: ["codeforces", "competitive-programming", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1090
codeforces_index: "M"
codeforces_contest_name: "2018-2019 Russia Open High School Programming Contest (Unrated, Online Mirror, ICPC Rules, Teams Preferred)"
rating: 1000
weight: 1090
solve_time_s: 256
verified: true
draft: false
---

[CF 1090M - Chuyến đi thú vị](https://codeforces.com/problemset/problem/1090/M) 

**Đánh giá:** 1000 
**Thẻ:** triển khai 
**Thời gian giải:** 4 phút 16s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một dãy nhà, mỗi dãy nhà được sơn một màu nguyên. Ta muốn chọn một đoạn liền kề của đường này sao cho trong đoạn đã chọn không có hai ngôi nhà lân cận nào có cùng màu. Trong số tất cả các phân khúc hợp lệ như vậy, chúng ta được yêu cầu tối đa hóa số lượng nhà trong phân khúc đó. 

Đầu vào chỉ đơn giản là một mảng màu sắc. Đầu ra là độ dài của mảng con liền kề dài nhất trong đó mỗi cặp phần tử liền kề đều khác nhau. 

Các ràng buộc ngay lập tức hướng dẫn không gian thiết kế. Với tối đa 100.000 ngôi nhà, bất kỳ giải pháp nào thử tất cả các mảng con và kiểm tra tính hợp lệ từ đầu sẽ liên quan đến thứ tự$n^2$phân khúc và có khả năng$O(n)$xác minh trên mỗi phân đoạn, vượt xa những gì phù hợp với giới hạn một giây. Do đó, chúng tôi đang tìm kiếm giải pháp quét tuyến tính hoặc kiểu hai con trỏ để xử lý từng phần tử với số lần không đổi. 

Trường hợp cạnh tinh tế xuất hiện khi mảng xen kẽ hoàn hảo, chẳng hạn như`1 2 1 2 1`. Câu trả lời đúng là độ dài đầy đủ. Một cách tiếp cận ngây thơ khởi động lại quá mạnh mẽ có thể cắt bớt phân đoạn một cách không chính xác. Một trường hợp cạnh khác là một mảng hoàn toàn thống nhất như`5 5 5 5`, trong đó mọi phân đoạn hợp lệ đều có độ dài 1. Mọi giải pháp đều phải đặt lại chính xác cho mọi vi phạm. 

## Phương pháp tiếp cận 

Chiến lược brute-force sẽ liệt kê mọi vị trí bắt đầu có thể có và mở rộng đoạn sang bên phải trong khi vẫn duy trì ràng buộc rằng các phần tử liền kề khác nhau. Đối với mỗi chỉ số bắt đầu$i$, chúng tôi quét về phía trước cho đến khi tìm thấy chỉ mục$j$như vậy$a[j] = a[j-1]$, sau đó ghi lại độ dài$j - i$. 

Điều này đúng vì nó trực tiếp xây dựng tất cả các phân đoạn hợp lệ tối đa. Tuy nhiên, trong trường hợp xấu nhất khi tất cả các phần tử là riêng biệt hoặc xen kẽ nhau, mỗi vị trí bắt đầu sẽ chạy gần đến cuối, cho ra kết quả đại khái là$n + (n-1) + \dots + 1 = O(n^2)$hoạt động. Với$n = 100000$, đây là theo thứ tự của$10^{10}$, điều đó là không thể thực hiện được. 

Quan sát quan trọng là tính hợp lệ chỉ phụ thuộc vào tính liền kề cục bộ. Khi chúng ta gặp một cặp hàng xóm ngang nhau, bất kỳ phân đoạn nào vượt qua ranh giới đó sẽ trở thành không hợp lệ. Điều này có nghĩa là mảng được phân chia tự nhiên thành các khối tối đa trong đó các phần tử liền kề luôn khác nhau. Bên trong mỗi khối, toàn bộ khối là hợp lệ nên chúng ta chỉ cần độ dài khối tối đa. 

Điều này làm giảm vấn đề xuống còn một lần quét tuyến tính trong đó chúng tôi theo dõi độ dài chuỗi hiện tại của vùng lân cận hợp lệ và đặt lại nó bất cứ khi nào chúng tôi gặp phải sự bằng nhau. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^2) | O(1) | Quá chậm | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi duy trì độ dài hoạt động của phân đoạn hợp lệ hiện tại kết thúc ở mỗi vị trí. 

1. Bắt đầu với ngôi nhà đầu tiên. Độ dài phân đoạn hợp lệ hiện tại là 1 vì một phần tử luôn hợp lệ. 
2. Lặp lại mảng từ trái sang phải bắt đầu từ chỉ số 1. Tại mỗi vị trí, so sánh màu hiện tại với màu trước đó. 
3. Nếu màu hiện tại khác với màu trước đó, chúng ta có thể mở rộng phân đoạn hợp lệ hiện tại lên 1. Điều này an toàn vì điều kiện duy nhất là lân cận và chúng ta vừa xác minh phân đoạn kề mới là hợp lệ. 
4. Nếu màu hiện tại giống với màu trước đó thì phân đoạn hợp lệ sẽ bị hỏng tại thời điểm này. Chúng tôi đặt lại độ dài đoạn hiện tại thành 1 vì đoạn mới có thể bắt đầu từ vị trí này. 
5. Theo dõi độ dài phân đoạn tối đa nhìn thấy trong quá trình quét. 

Câu trả lời là giá trị tối đa mà chiều dài chạy này đạt được. 

### Tại sao nó hoạt động 

Tại bất kỳ vị trí nào, thuật toán duy trì độ dài của đoạn hợp lệ dài nhất kết thúc chính xác tại vị trí đó. Điều này có hiệu quả vì hạn chế duy nhất liên quan đến các phần tử liền kề. Miễn là cặp liền kề cuối cùng hợp lệ, tất cả các cặp trước đó trong cửa sổ hiện tại đã được xác thực khi cửa sổ được mở rộng. Khi xảy ra vi phạm, không có phân đoạn hợp lệ nào có thể vượt qua nó, do đó việc khởi động lại ở chỉ mục đó sẽ duy trì tính chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, k = map(int, input().split())
    a = list(map(int, input().split()))

    best = 1
    cur = 1

    for i in range(1, n):
        if a[i] != a[i - 1]:
            cur += 1
        else:
            cur = 1
        if cur > best:
            best = cur

    print(best)

if __name__ == "__main__":
    solve()
```Giải pháp đọc mảng và duy trì hai biến. Biến`cur`theo dõi độ dài của phân đoạn hợp lệ hiện tại kết thúc tại chỉ mục`i`. Biến`best`lưu trữ giá trị tối đa như vậy trên tất cả các vị trí. 

Quá trình chuyển đổi rất đơn giản. Khi các phần tử liên tiếp khác nhau, chúng ta mở rộng`cur`. Khi chúng khớp nhau, chúng tôi đặt lại. Chi tiết triển khai quan trọng là việc đặt lại phải đặt`cur`thành 1, không phải 0, vì một phần tử luôn tự tạo thành một phân đoạn hợp lệ. 

Câu trả lời được cập nhật sau mỗi bước để đảm bảo chúng tôi nắm bắt được phân khúc tốt nhất ở bất kỳ đâu trong mảng. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
8
1 2 3 3 2 1 2 2
```| tôi | một [tôi] | một[i-1] | cur | tốt nhất | 
| --- | --- | --- | --- | --- | 
| 0 | 1 | - | 1 | 1 | 
| 1 | 2 | 1 | 2 | 2 | 
| 2 | 3 | 2 | 3 | 3 | 
| 3 | 3 | 3 | 1 | 3 | 
| 4 | 2 | 3 | 2 | 3 | 
| 5 | 1 | 2 | 3 | 3 | 
| 6 | 2 | 1 | 4 | 4 | 
| 7 | 2 | 2 | 1 | 4 | 

Dấu vết này cho thấy phân đoạn được mở rộng liên tục như thế nào cho đến khi một lần lặp lại phá vỡ nó ở chỉ số 3, sau đó một phân đoạn mới bắt đầu. Khoảng thời gian hợp lệ dài nhất xảy ra ngay trước lần lặp lại cuối cùng. 

### Ví dụ 2 

đầu vào:```
5
5 5 5 5 5
```| tôi | một [tôi] | một[i-1] | cur | tốt nhất | 
| --- | --- | --- | --- | --- | 
| 0 | 5 | - | 1 | 1 | 
| 1 | 5 | 5 | 1 | 1 | 
| 2 | 5 | 5 | 1 | 1 | 
| 3 | 5 | 5 | 1 | 1 | 
| 4 | 5 | 5 | 1 | 1 | 

Mọi vùng lân cận sẽ bị ngắt ngay lập tức, do đó phân đoạn tốt nhất không bao giờ vượt quá độ dài 1. Điều này xác nhận logic đặt lại hoạt động chính xác trên các mảng thống nhất. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi phần tử được xử lý một lần bằng cách so sánh và cập nhật liên tục theo thời gian | 
| Không gian | O(1) | Chỉ một số bộ đếm được sử dụng bất kể kích thước đầu vào | 

Quá trình quét tuyến tính vừa vặn thoải mái trong giới hạn cho 100.000 phần tử và việc sử dụng bộ nhớ liên tục sẽ tránh được mọi lo ngại về chi phí. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# provided sample
assert run("8 3\n1 2 3 3 2 1 2 2\n") == "4"

# all distinct
assert run("5 5\n1 2 3 4 5\n") == "5"

# all equal
assert run("4 2\n7 7 7 7\n") == "1"

# alternating
assert run("6 2\n1 2 1 2 1 2\n") == "6"

# single element
assert run("1 10\n5\n") == "1"

# break in middle
assert run("7 3\n1 2 2 3 4 4 5\n") == "3"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 2 3 4 5 | 5 | tăng hiệu lực một cách chặt chẽ | 
| 7 7 7 7 | 1 | hành vi thiết lập lại đầy đủ | 
| 1 2 1 2 1 2 | 6 | phân đoạn hợp lệ xen kẽ đầy đủ | 
| trường hợp 1 phần tử | 1 | độ đúng của điều kiện biên | 
| 2 2 trường hợp chia | 3 | xử lý nhiều lần nghỉ giải lao | 

## Vỏ cạnh 

Một mảng hoàn toàn thống nhất như`5 5 5 5`gây ra việc thiết lập lại ở mỗi bước. Thuật toán xử lý nó dưới dạng một chuỗi các đoạn có độ dài 1 độc lập, luôn cập nhật`best`thành 1 và không bao giờ hợp nhất không chính xác qua các ranh giới không hợp lệ. 

Một mảng xen kẽ như`1 2 1 2 1 2`không bao giờ kích hoạt thiết lập lại. Độ dài chạy tăng liên tục lên 6, xác nhận rằng các chuyển đổi hợp lệ lặp lại được tích lũy chính xác mà không cần cắt bớt nhân tạo. 

Một trường hợp có các cụm trùng lặp như`1 2 2 3 4 4 5`thể hiện sự tăng trưởng và thiết lập lại xen kẽ. Quá trình quét tạo ra độ dài phân đoạn`2`, sau đó đặt lại, sau đó`3`, cho thấy thuật toán tách biệt chính xác các đoạn hợp lệ tối đa mà không cần logic phân đoạn rõ ràng.
