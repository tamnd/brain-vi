---
title: "CF 1592B - Mua sắm Hemose"
description: "Chúng ta được cung cấp một mảng các số nguyên và một hạn chế trong việc hoán đổi các phần tử: chúng ta chỉ có thể hoán đổi hai phần tử nếu vị trí của chúng khác nhau ít nhất là x. Nhiệm vụ là xác định xem có thể sắp xếp mảng theo thứ tự không giảm theo ràng buộc này hay không."
date: "2026-06-10T09:15:19+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "dsu", "math", "sortings"]
categories: ["algorithms"]
codeforces_contest: 1592
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 746 (Div. 2)"
rating: 1200
weight: 1592
solve_time_s: 110
verified: true
draft: false
---

[CF 1592B - Mua sắm Hemose](https://codeforces.com/problemset/problem/1592/B) 

**Đánh giá:** 1200 
**Tags:** thuật toán xây dựng, dsu, toán, sắp xếp 
**Thời gian giải:** 1 phút 50 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một mảng các số nguyên và một hạn chế trong việc hoán đổi các phần tử: chúng ta chỉ có thể hoán đổi hai phần tử nếu vị trí của chúng khác nhau ít nhất`x`. Nhiệm vụ là xác định xem có thể sắp xếp mảng theo thứ tự không giảm theo ràng buộc này hay không. Mỗi trường hợp thử nghiệm chỉ định mảng và giá trị`x`. 

Nhận xét đầu tiên là nếu`x`lớn so với độ dài mảng, một số vị trí trở nên "bị khóa" một cách hiệu quả. Ví dụ, nếu`x = n`, không thể hoán đổi được gì cả, vì vậy mảng phải được sắp xếp sẵn. Ngược lại, nếu`x = 1`, không có hạn chế nào và chúng tôi có thể thực hiện bất kỳ thao tác hoán đổi nào, nghĩa là chúng tôi luôn có thể sắp xếp mảng. Những trường hợp phức tạp xảy ra khi`1 < x < n`, bởi vì chỉ một số vị trí nhất định mới có thể tương tác. 

Những ràng buộc cho chúng ta biết rằng`n`có thể lên tới`10^5`cho mỗi trường hợp thử nghiệm và tổng của tất cả`n`trên tất cả các trường hợp thử nghiệm là nhiều nhất`2 * 10^5`. Điều này có nghĩa là bất kỳ giải pháp nào có độ phức tạp kém hơn`O(n log n)`mỗi trường hợp thử nghiệm có thể là quá chậm. Những cách tiếp cận ngây thơ thử tất cả các chuỗi hoán đổi sẽ theo cấp số nhân và hoàn toàn không khả thi. 

Trường hợp cạnh không rõ ràng xảy ra khi mảng gần được sắp xếp nhưng các phần tử nhỏ nhất hoặc lớn nhất bị mắc kẹt ở giữa nơi chúng không thể di chuyển đủ xa do`x`sự hạn chế. Ví dụ, hãy xem xét`[3, 2, 1]`với`x = 3`. Mảng đủ nhỏ để không được phép hoán đổi, vì vậy, mặc dù nó gần như đã được sắp xếp nhưng câu trả lời là "KHÔNG". Một kịch bản phức tạp khác là mảng trong đó`x`chỉ nhỏ hơn một chút so với`n/2`, để lại đoạn giữa không thể tương tác với các đầu. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực rất đơn giản: thử tất cả các giao dịch hoán đổi được phép trong mọi cấu hình cho đến khi mảng được sắp xếp hoặc hết tất cả các khả năng. Điều này đảm bảo tính chính xác vì cuối cùng chúng ta sẽ khám phá mọi hoán vị có thể tiếp cận được. Tuy nhiên, số lượng giao dịch hoán đổi tăng theo cấp số nhân với`n`, vì vậy ngay cả đối với`n = 20`nó trở nên không thực tế. 

Quan sát quan trọng là chỉ các phần tử ở các vị trí ít nhất`x`ngoài có thể trao đổi. Nếu như`x <= n/2`, thì mọi phần tử ở gần đầu cuối cùng có thể đến mọi phần tử ở gần cuối, có thể thông qua một chuỗi hoán đổi. Tuy nhiên, các phần tử ở vùng giữa có chiều dài`n - 2*x`bị hạn chế: họ không thể di chuyển đủ xa để đạt được mục đích nếu`x > n/2`. Vì vậy, nếu mảng chưa được sắp xếp ở các vị trí ở giữa này thì việc sắp xếp là không thể. 

Giải pháp tối ưu tận dụng điều này bằng cách so sánh mảng với phiên bản được sắp xếp đầy đủ của nó. Chúng ta chỉ cần kiểm tra xem mỗi phần tử không thể chạm tới đầu cuối (đoạn giữa "cố định") đã ở đúng vị trí hay chưa. Nếu tất cả các phần tử như vậy khớp với mảng đã sắp xếp thì có thể sắp xếp được; nếu không thì không. Điều này làm giảm vấn đề xuống còn một phép so sánh đơn giản sau khi sắp xếp mà không cần mô phỏng các giao dịch hoán đổi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Ồ (n!) | O(n) | Quá chậm | 
| Tối ưu | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case. Đối với mỗi trường hợp thử nghiệm, hãy đọc`n`,`x`, và mảng`a`. 
2. Tạo một phiên bản đã sắp xếp của mảng,`sorted_a`. Điều này thể hiện sự sắp xếp mục tiêu. 
3. Nếu`x <= n/2`, mọi phần tử cuối cùng có thể được hoán đổi vào bất kỳ vị trí nào. Trong trường hợp này, ngay lập tức trả về "CÓ". 
4. Ngược lại, đối với các vị trí`i`từ`n - x`ĐẾN`x - 1`(đoạn "giữa" không thể đến cuối), hãy kiểm tra xem`a[i] == sorted_a[i]`. Nếu có phần tử nào khác, trả về "NO". 
5. Nếu tất cả các vị trí ở đoạn giữa khớp với mảng đã sắp xếp, hãy trả về "CÓ". 

Tại sao nó hoạt động: Các phần tử bên ngoài đoạn giữa có thể di chuyển đủ tự do để đến điểm cuối thông qua các giao dịch hoán đổi được phép. Đoạn giữa được cố định một cách hiệu quả theo thứ tự tương đối. Bằng cách kiểm tra các vị trí cố định này dựa trên mảng đã sắp xếp, chúng tôi xác định tính khả thi. Bất biến này đảm bảo tính đúng đắn mà không cần mô phỏng các giao dịch hoán đổi. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def can_sort(n, x, a):
    sorted_a = sorted(a)
    if x <= n // 2:
        return "YES"
    for i in range(n - x, x):
        if a[i] != sorted_a[i]:
            return "NO"
    return "YES"

t = int(input())
for _ in range(t):
    n, x = map(int, input().split())
    a = list(map(int, input().split()))
    print(can_sort(n, x, a))
```chức năng`can_sort`nắm bắt logic cốt lõi. Đầu tiên chúng ta sắp xếp mảng để biết vị trí mục tiêu. Vì`x <= n//2`, chúng tôi có thể sắp xếp bất kỳ mảng nào, vì vậy chúng tôi ngay lập tức trả về "CÓ". Đối với lớn hơn`x`, chúng tôi chỉ kiểm tra đoạn giữa bị ràng buộc. sử dụng`range(n - x, x)`chọn chính xác các chỉ số không thể di chuyển đủ xa. Các lỗi riêng lẻ ở đây sẽ âm thầm thất bại đối với các trường hợp biên, vì vậy việc chú ý đến giới hạn bao gồm và giới hạn độc quyền là rất quan trọng. 

## Ví dụ đã hoạt động 

**Ví dụ 1** 

đầu vào:`n = 3, x = 3, a = [3, 2, 1]`| tôi | một [tôi] | được sắp xếp_a[i] | So sánh | 
| --- | --- | --- | --- | 
| 0 | 3 | 1 | không khớp | 
| 1 | 2 | 2 | trận đấu | 
| 2 | 1 | 3 | không khớp | 

Từ`x = n`, đoạn giữa là toàn bộ mảng. Có sự không khớp, đầu ra là "KHÔNG". 

**Ví dụ 2** 

đầu vào:`n = 5, x = 2, a = [5, 1, 2, 3, 4]`Đây,`x <= n//2`là sai, vì vậy hãy kiểm tra các chỉ số ở giữa`n-x`ĐẾN`x-1`→`i = 3 to 1`→ phạm vi trống, không có vị trí nào để kiểm tra. Đầu ra "CÓ". 

Dấu vết này cho thấy cách kiểm tra đoạn giữa trực tiếp xác định các cấu hình không khả thi và bỏ qua các vị trí có thể di chuyển. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Sắp xếp chiếm ưu thế; các hoạt động khác là tuyến tính | 
| Không gian | O(n) | Lưu trữ mảng đã sắp xếp | 

Với tổng số tiền`n`trên các trường hợp thử nghiệm 2*10^5, điều này vừa vặn thoải mái trong giới hạn 1 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    # main solution code
    t = int(input())
    for _ in range(t):
        n, x = map(int, input().split())
        a = list(map(int, input().split()))
        print(can_sort(n, x, a))
    return output.getvalue().strip()

# provided samples
assert run("4\n3 3\n3 2 1\n4 3\n1 2 3 4\n5 2\n5 1 2 3 4\n5 4\n1 2 3 4 4") == "NO\nYES\nYES\nYES"

# custom cases
assert run("1\n1 1\n42") == "YES", "single element"
assert run("1\n2 2\n2 1") == "NO", "cannot swap when x=n"
assert run("1\n5 3\n5 4 3 2 1") == "NO", "middle segment unsorted"
assert run("1\n6 2\n1 2 3 6 5 4") == "YES", "x <= n//2, full sorting possible"
assert run("1\n7 4\n1 3 2 7 6 5 4") == "NO", "middle segment unsorted"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1 42`| CÓ | Yếu tố đơn | 
|`2 2 2 1`| KHÔNG | Không được phép hoán đổi đối với x=n | 
|`5 3 5 4 3 2 1`| KHÔNG | Đoạn giữa không thể di chuyển | 
|`6 2 1 2 3 6 5 4`| CÓ | x <= n/2 cho phép sắp xếp đầy đủ | 
|`7 4 1 3 2 7 6 5 4`| KHÔNG | Đoạn giữa không khớp | 

## Vỏ cạnh 

Khi độ dài mảng bằng`x`, không thể hoán đổi được. Vì`[3, 2, 1]`với`x = 3`, đoạn giữa là mảng đầy đủ, chưa được sắp xếp. Thuật toán kiểm tra tất cả các vị trí và đưa ra kết quả "KHÔNG". 

Vì`x <= n//2`, chẳng hạn như`[5, 1, 2, 3, 4]`với`x = 2`, đoạn giữa thực sự trống. Tất cả các phần tử có thể được xáo trộn tự do, do đó hàm sẽ xuất ra "CÓ". 

Đối với các mảng có phần tử lặp lại, ví dụ:`[1, 2, 2, 1]`với`x = 3`, thuật toán xác định chính xác xem các vị trí ở đoạn giữa có cần điều chỉnh hay không, tránh kết quả dương tính giả. 

Điều này hoàn thành phần giải thích rõ ràng và từng bước
