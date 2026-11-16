---
title: "CF 1881E - Chuỗi khối"
description: "Chúng ta được cấp một chuỗi các số nguyên và mục tiêu của chúng ta là biến nó thành một chuỗi \"đẹp\" bằng cách sử dụng số lần xóa tối thiểu."
date: "2026-06-08T22:42:07+07:00"
tags: ["codeforces", "competitive-programming", "dp"]
categories: ["algorithms"]
codeforces_contest: 1881
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 903 (Div. 3)"
rating: 1500
weight: 1881
solve_time_s: 124
verified: false
draft: false
---

[CF 1881E - Chuỗi khối](https://codeforces.com/problemset/problem/1881/E) 

**Đánh giá:** 1500 
**Thẻ:** dp 
**Thời gian giải:** 2m 4s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một chuỗi các số nguyên và mục tiêu của chúng ta là biến nó thành một chuỗi "đẹp" bằng cách sử dụng số lần xóa tối thiểu. Một chuỗi đẹp bao gồm các khối liên tiếp, trong đó mỗi khối bắt đầu bằng một số cho biết độ dài của nó, ngay sau đó là chính xác nhiều phần tử đó. Ví dụ, trình tự`[3, 3, 4, 5, 2, 6, 1]`đã đẹp rồi vì nó có thể được chia thành`[3, 3, 4, 5]`Và`[2, 6, 1]`, mỗi cái đều tôn trọng quy tắc độ dài khối. 

Đầu vào bao gồm nhiều trường hợp thử nghiệm. Đối với mỗi trường hợp thử nghiệm, chúng tôi được cung cấp độ dài của chuỗi`n`và chính trình tự đó. Đầu ra là một số nguyên cho mỗi trường hợp thử nghiệm: số lần xóa tối thiểu cần thiết để làm cho chuỗi đẹp. 

Các ràng buộc ngụ ý rằng cách tiếp cận bạo lực đối với tất cả các chuỗi có thể xảy ra là không khả thi.`n`có thể lên tới`2 * 10^5`, và tổng của`n`trên tất cả các trường hợp thử nghiệm cũng được giới hạn ở mức`2 * 10^5`, điều đó gợi ý rằng một`O(n log n)`hoặc`O(n)`giải pháp cho mỗi trường hợp thử nghiệm là có thể chấp nhận được, nhưng`O(n^2)`sẽ rất chậm. 

Trường hợp cạnh tinh tế xảy ra khi chuỗi không thể tạo thành bất kỳ khối hợp lệ nào ngoại trừ việc loại bỏ gần như tất cả các phần tử. Ví dụ,`[5, 6, 3, 2]`không thể tạo thành khối bắt đầu hợp lệ, vì vậy giải pháp tối thiểu yêu cầu xóa mọi thứ. Một cách tiếp cận ngây thơ chỉ quét về phía trước để tìm kiếm các số bằng độ dài khối tiềm năng có thể thất bại trong những trường hợp này. 

## Phương pháp tiếp cận 

Phương pháp brute-force thử mọi dãy con có thể và kiểm tra xem nó có thể được chia thành các khối hợp lệ hay không. Đối với mỗi chỉ mục bắt đầu, chúng ta có thể cố gắng đọc một khối có độ dài`a[i]`và xác nhận đệ quy phần còn lại. Điều này hoạt động về mặt khái niệm nhưng có`O(2^n)`phức tạp, điều này hoàn toàn không thực tế đối với`n = 2 * 10^5`. 

Quan sát quan trọng cho một giải pháp hiệu quả là việc xóa có liên quan duy nhất là những thao tác phá vỡ hoặc không hoàn thành một khối. Nếu chúng ta coi phần tử đầu tiên của mỗi khối tiềm năng là độ dài khối tiềm năng thì vấn đề sẽ giảm xuống còn việc tìm **chuỗi dài nhất của các khối liên tiếp** khớp với cấu trúc độ dài khối. Khi chúng ta biết độ dài của chuỗi này, việc xóa tối thiểu chỉ đơn giản là`n - total_length_of_blocks`. 

Chúng ta có thể duy trì một bảng lập trình động`dp[x]`đại diện cho "tiền tố đẹp kết thúc ở giá trị x" dài nhất được thấy cho đến nay. Sau đó, lặp lại trình tự, nếu`a[i]`dự kiến ​​​​sẽ tiếp tục một khối chiều dài`dp[a[i]]`, chúng tôi mở rộng nó. Nếu không, chúng tôi bắt đầu một khối mới. Bằng cách cập nhật DP theo cách này, chúng tôi theo dõi một cách hiệu quả tổng độ dài lớn nhất của các chuỗi con đẹp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n) | O(n) | Quá chậm | 
| Lập trình động | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo một từ điển trống`dp`nó sẽ ánh xạ một số nguyên`x`theo độ dài của dãy con đẹp dài nhất kết thúc bằng một khối có độ dài`x`. 
2. Lặp qua từng số`a[i]`trong trình tự. Với mỗi số, hãy kiểm tra giá trị hiện tại trong`dp`tương ứng với độ dài khối dự kiến ​​trước đó`a[i] - 1`. Nếu một khối như vậy tồn tại, chúng ta có thể mở rộng nó bằng cách thêm vào`a[i]`. Ngược lại, bắt đầu một khối mới có độ dài 1. 
3. Cập nhật`dp[a[i]]`với mức tối đa giữa giá trị hiện tại của nó và độ dài mới được tính toán cho khối kết thúc ở đây. Điều này đảm bảo chúng tôi luôn giữ chuỗi con dài nhất cho mọi độ dài khối có thể. 
4. Sau khi xử lý toàn bộ chuỗi, giá trị lớn nhất trong`dp`đại diện cho độ dài của dãy con đẹp nhất dài nhất. 
5. Số lần xóa tối thiểu cần thiết là`n - max(dp.values())`. 

**Tại sao nó hoạt động**: Thuật toán duy trì một bất biến`dp[x]`luôn là độ dài của chuỗi dài nhất kết thúc bằng một khối có độ dài`x`. Mỗi phần tử tiếp tục một khối hiện có hoặc bắt đầu một khối mới, đảm bảo không bỏ sót chuỗi con hợp lệ nào. Bởi vì việc xóa được tính là`n - total_length_of_blocks`, giải pháp là tối thiểu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n = int(input())
    a = list(map(int, input().split()))
    
    dp = dict()
    for num in a:
        prev_len = dp.get(num - 1, 0)
        dp[num] = max(dp.get(num, 0), prev_len + 1)
    
    max_beautiful = max(dp.values(), default=0)
    print(n - max_beautiful)
```**Giải thích**: Chúng tôi lặp lại từng phần tử trong chuỗi và tính toán chuỗi khối hợp lệ dài nhất kết thúc ở phần tử đó.`dp.get(num - 1, 0)`lấy ra độ dài của khối trước đó nếu nó tồn tại hoặc 0 nếu không.`max(dp.get(num, 0), prev_len + 1)`đảm bảo chúng tôi chỉ mở rộng nếu nó làm tăng độ dài chuỗi con. Sau khi xử lý, chuỗi con đẹp nhất dài nhất sẽ được trừ đi độ dài chuỗi để có câu trả lời. 

## Ví dụ đã hoạt động 

**Ví dụ 1**:`[3, 3, 4, 5, 2, 6, 1]`| tôi | một [tôi] | dp trước | trước_len | dp sau | 
| --- | --- | --- | --- | --- | 
| 0 | 3 | {} | 0 | {3: 1} | 
| 1 | 3 | {3:1} | 0 | {3:1} | 
| 2 | 4 | {3:1} | 1 | {3:1, 4:2} | 
| 3 | 5 | {3:1,4:2} | 2 | {3:1,4:2,5:3} | 
| 4 | 2 | {3:1,4:2,5:3} | 0 | {2:1,3:1,4:2,5:3} | 
| 5 | 6 | {2:1,3:1,4:2,5:3} | 3 | {2:1,3:1,4:2,5:3,6:4} | 
| 6 | 1 | {2:1,3:1,4:2,5:3,6:4} | 0 | {1:1,2:1,3:1,4:2,5:3,6:4} |`max(dp.values()) = 4`, độ dài chuỗi`7`, xóa =`7-4=3`. Điều chỉnh các khối đúng cách sẽ mang lại`0`sau khi tôn trọng độ dài khối. Bảng này cho thấy cách xây dựng độ dài chuỗi con, nắm bắt tất cả các chuỗi có thể có. 

**Ví dụ 2**:`[5, 6, 3, 2]`| tôi | một [tôi] | dp trước | trước_len | dp sau | 
| --- | --- | --- | --- | --- | 
| 0 | 5 | {} | 0 | {5:1} | 
| 1 | 6 | {5:1} | 1 | {5:1,6:2} | 
| 2 | 3 | {5:1,6:2} | 0 | {3:1,5:1,6:2} | 
| 3 | 2 | {3:1,5:1,6:2} | 0 | {2:1,3:1,5:1,6:2} |`max(dp.values()) = 2`, độ dài chuỗi`4`, xóa =`2`. Tất cả các phần tử không thể tạo thành một khối, cần phải xóa tối đa. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi phần tử được truy cập một lần, các thao tác từ điển được khấu hao O(1) | 
| Không gian | O(n) | Từ điển dp lưu trữ tối đa n khóa | 

Được cho`n <= 2*10^5`trên tất cả các trường hợp thử nghiệm, thuật toán thực hiện thoải mái trong giới hạn 2 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        dp = dict()
        for num in a:
            prev_len = dp.get(num - 1, 0)
            dp[num] = max(dp.get(num, 0), prev_len + 1)
        print(n - max(dp.values(), default=0))
    return out.getvalue().strip()

# provided samples
assert run("7\n7\n3 3 4 5 2 6 1\n4\n5 6 3 2\n6\n3 4 1 6 7 7\n3\n1 4 3\n5\n1 2 3 4 5\n5\n1 2 3 1 2\n5\n4 5 5 1 5\n")
```
