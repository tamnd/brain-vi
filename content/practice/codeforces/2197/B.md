---
title: "CF 2197B - Mảng và hoán vị"
description: "Chúng ta được cho một hoán vị p có độ dài n và một mảng a có cùng độ dài. Hoán vị là một mảng các số nguyên riêng biệt từ 1 đến n, theo một thứ tự nào đó."
date: "2026-06-09T04:44:06+07:00"
tags: ["codeforces", "competitive-programming", "implementation", "schedules", "sortings", "two-pointers"]
categories: ["algorithms"]
codeforces_contest: 2197
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 1079 (Div. 2)"
rating: 1100
weight: 2197
solve_time_s: 81
verified: true
draft: false
---

[CF 2197B - Mảng và Hoán vị](https://codeforces.com/problemset/problem/2197/B) 

**Đánh giá:** 1100 
**Thẻ:** thực hiện, lịch trình, sắp xếp, hai con trỏ 
**Thời gian giải:** 1 phút 21s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một hoán vị`p`chiều dài`n`và một mảng`a`có cùng độ dài. Một hoán vị là một mảng các số nguyên khác nhau từ`1`ĐẾN`n`, theo một thứ tự nào đó. Chúng tôi muốn xác định xem liệu`a`có thể đã được sản xuất từ ​​​​`p`sử dụng một chuỗi các thao tác trong đó trong mỗi thao tác chúng ta chọn hai phần tử liền kề trong`p`và sao chép giá trị của cái này sang cái khác. Nghĩa là, với bất kỳ cặp liền kề nào`(p[i], p[i+1])`, chúng ta có thể đặt`p[i] = p[i+1]`hoặc`p[i+1] = p[i]`. 

Điều quan trọng là các hoạt động này chỉ có thể **truyền bá các số hiện có cho hàng xóm**, không bao giờ giới thiệu số mới. Vì vậy, mọi số xuất hiện trong`a`phải tồn tại ở một vị trí nào đó trong`p`. Hơn nữa, khi một số được đưa vào một vị trí, nó có thể "mở rộng" sang trái hoặc sang phải để ghi đè lên các số khác, do đó, các khối liền kề có cùng số đó sẽ`a`được tạo ra bởi một lần xuất hiện ban đầu duy nhất trong`p`. 

Những hạn chế là như vậy`n`có thể lên đến`2 * 10^5`, và tổng của`n`trên tất cả các trường hợp thử nghiệm không vượt quá`2 * 10^5`. Điều này loại trừ bất kỳ giải pháp nào có độ phức tạp tuyến tính kém hơn cho mỗi trường hợp thử nghiệm. Mô phỏng lực lượng mạnh mẽ của các hoạt động sao chép sẽ là bậc hai trong`n`trong trường hợp xấu nhất và sẽ hết thời gian. 

Các trường hợp phức tạp bao gồm các mảng trong đó tất cả các phần tử đều bằng nhau, các mảng trong đó các số lặp lại nhưng không liền kề nhau.`a`và các mảng trong đó phần tử cuối cùng hoặc phần tử đầu tiên cần được truyền từ xa. Ví dụ,`p = [1, 2, 3]`Và`a = [2, 2, 2]`là không thể vì số`2`không thể truyền tới vị trí đầu tiên nếu ban đầu nó không ở đó. 

## Phương pháp tiếp cận 

Một cách tiếp cận đơn giản là mô phỏng trực tiếp các thao tác: lặp qua`p`và với mọi sự không khớp với`a`, thực hiện các thao tác sao chép cho đến khi khớp. Về nguyên tắc, điều này hoạt động vì các phép toán cho phép trải rộng số sang các vị trí liền kề, nhưng trong trường hợp xấu nhất, nó sẽ yêu cầu các phép toán O(n^2). Ví dụ, nếu`p`là`[1,2,3,...,n]`Và`a`là`[1]*n`, chúng tôi sẽ liên tục ghi đè lên từng phần tử một. Với`n`lên tới 2 * 10^5, tốc độ này quá chậm. 

Quan sát quan trọng cho một cách tiếp cận tối ưu là ** khi một số tồn tại trong`p`, nó có thể lan truyền qua các đoạn liền kề**. Vì vậy, chúng ta chỉ cần kiểm tra xem dãy số trong`a`có thể được hình thành bằng cách mở rộng số lượng có trong`p`theo thứ tự. Cụ thể, chúng tôi đi qua`a`từ trái sang phải và duy trì một con trỏ ở`p`đại diện cho vị trí sẵn có đầu tiên cho một số mới. Nếu như`a[i]`khớp với số hiện tại trong`p`, chúng ta tiến con trỏ. Nếu như`a[i]`lặp lại số trước đó trong`a`, chúng tôi tiếp tục. Nếu như`a[i]`không khớp với số trước đó hoặc số có sẵn tiếp theo trong`p`, mảng là không thể. 

Điều này làm giảm vấn đề xuống chỉ còn một lần đi qua`a`trong khi duy trì một con trỏ trong`p`, đưa ra giải pháp O(n) tuyến tính cho mỗi trường hợp thử nghiệm. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^2) | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc hoán vị`p`và mảng`a`. Khởi tạo một con trỏ`j = 0`vì`p`. 
2. Duyệt mảng`a`từ trái sang phải. Cho phép`prev`lưu trữ phần tử trước đó trong`a`, ban đầu được đặt thành`None`. 
3. Đối với mỗi phần tử`a[i]`, nếu nó bằng`prev`, Tiếp tục. Điều này có nghĩa là chúng ta đang ở trong một khối liền kề và không có số mới từ`p`là cần thiết. 
4. Nếu`a[i]`không bằng`prev`, chúng tôi cố gắng tìm nó trong`p`bắt đầu từ chỉ mục`j`. Nếu như`p[j]`bằng`a[i]`, tăng`j`và cập nhật`prev`ĐẾN`a[i]`. 
5. Nếu`a[i]`không bằng`prev`và không khớp`p[j]`, mảng`a`không thể được tạo ra từ`p`. đầu ra`NO`. 
6. Nếu chúng ta đi đến cuối`a`không có xung đột, đầu ra`YES`. 

Tại sao nó hoạt động: con trỏ`j`đảm bảo rằng chúng tôi chỉ sử dụng từng phần tử trong`p`một lần như một nguồn cho các số mới. Sự lan truyền của các số lặp lại trong`a`được xử lý bằng cách so sánh với`prev`. Nếu một số trong`a`không thể khớp với số có sẵn tiếp theo trong`p`và không phải là sự lặp lại, điều đó có nghĩa là không có trình tự hợp pháp nào của hoạt động sao chép có thể tạo ra nó, đảm bảo tính chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n = int(input())
    p = list(map(int, input().split()))
    a = list(map(int, input().split()))
    
    j = 0
    prev = None
    possible = True
    
    for num in a:
        if num == prev:
            continue
        while j < n and p[j] != num:
            j += 1
        if j == n:
            possible = False
            break
        prev = num
        j += 1
    
    print("YES" if possible else "NO")
```Giải pháp đọc đầu vào hiệu quả cho nhiều trường hợp thử nghiệm. Con trỏ`j`tiến bộ thông qua`p`chỉ khi có số mới trong`a`gặp phải, đảm bảo từng yếu tố của`p`được sử dụng nhiều nhất một lần. Các số lặp lại trong`a`được xử lý bởi`prev`, vì vậy chúng tôi không sử dụng các yếu tố mới của`p`một cách không cần thiết. các`while`vòng lặp đảm bảo chúng tôi bỏ qua các số trong`p`đã được thông qua và không thể đóng góp vào con số hiện tại trong`a`. 

## Ví dụ đã hoạt động 

Hãy xem xét đầu vào mẫu đầu tiên:```
p = [1,2,3], a = [1,2,2]
```| tôi | một [tôi] | trước | j | hành động | 
| --- | --- | --- | --- | --- | 
| 0 | 1 | Không có | 0 | p[0] = 1, khớp với a[0], prev = 1, j = 1 | 
| 1 | 2 | 1 | 1 | p[1] = 2, khớp với a[1], prev = 2, j = 2 | 
| 2 | 2 | 2 | 2 | trận đấu trước, tiếp tục | 

Đầu ra là CÓ. Dấu vết cho thấy các số lặp lại lan truyền mà không tiến lên`p`. 

Mẫu thứ hai:```
p = [3,1,2,4], a = [3,4,2,2]
```| tôi | một [tôi] | trước | j | hành động | 
| --- | --- | --- | --- | --- | 
| 0 | 3 | Không có | 0 | p[0] = 3, khớp với a[0], prev = 3, j = 1 | 
| 1 | 4 | 3 | 1 | p[1] = 1, bỏ qua -> j=2, p[2]=2 !=4, j=3, p[3]=4 trùng khớp, prev=4, j=4 | 
| 2 | 2 | 4 | 4 | j == n, không thể khớp a[2], break | 

Đầu ra là KHÔNG. Điều này xác nhận thuật toán phát hiện chính xác sự lan truyền không thể. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Mỗi phần tử trong`a`được xử lý một lần, con trỏ`j`TRONG`p`chỉ tiến về phía trước | 
| Không gian | O(n) | Lưu trữ`p`Và`a`mảng | 

Với tổng số`n`trên tất cả các trường hợp thử nghiệm được giới hạn bởi 2 * 10^5, giải pháp này phù hợp thoải mái trong giới hạn thời gian 2 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    
    # solution function
    t = int(input())
    for _ in range(t):
        n = int(input())
        p = list(map(int, input().split()))
        a = list(map(int, input().split()))
        
        j = 0
        prev = None
        possible = True
        
        for num in a:
            if num == prev:
                continue
            while j < n and p[j] != num:
                j += 1
            if j == n:
                possible = False
                break
            prev = num
            j += 1
        
        print("YES" if possible else "NO")
    
    sys.stdout = sys.__stdout__
    return output.getvalue().strip()

# Provided samples
assert run("6\n3\n1 2 3\n1 2 2\n4\n3 1 2 4\n3 4 2 2\n5\n1 3 2 5 4\n3 3 3 5 4\n7\n3 7 4 2 1 6 5\n3 3 4 4 5 6 5\n7\n1 2 3 4 5 6 7\n
```
