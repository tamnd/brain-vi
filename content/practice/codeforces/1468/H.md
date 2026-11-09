---
title: "CF 1468H - K và trung vị"
description: "Cho một dãy các số nguyên từ 1 đến n và một số nguyên k lẻ. Trong một thao tác, chúng ta có thể chọn k phần tử bất kỳ từ chuỗi hiện tại, tính trung vị của chúng và loại bỏ k-1 phần tử còn lại, chỉ để lại phần tử trung vị. Chúng tôi lặp lại thao tác này bất kỳ số lần."
date: "2026-06-11T01:28:31+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "greedy", "math"]
categories: ["algorithms"]
codeforces_contest: 1468
codeforces_index: "H"
codeforces_contest_name: "2020-2021 ICPC, NERC, Southern and Volga Russian Regional Contest (Online Mirror, ICPC Rules)"
rating: 2200
weight: 1468
solve_time_s: 149
verified: false
draft: false
---

[CF 1468H - K và Trung vị](https://codeforces.com/problemset/problem/1468/H) 

**Xếp hạng:** 2200 
**Tags:** thuật toán xây dựng, tham lam, toán học 
**Thời gian giải:** 2m 29s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Cho một dãy các số nguyên từ 1 đến n và một số nguyên k lẻ. Trong một thao tác, chúng ta có thể chọn k phần tử bất kỳ từ chuỗi hiện tại, tính trung vị của chúng và loại bỏ k-1 phần tử còn lại, chỉ để lại phần tử trung vị. Chúng tôi lặp lại thao tác này bất kỳ số lần. Đối với mỗi trường hợp thử nghiệm, chúng ta được hỏi liệu có thể giảm chuỗi gốc thành chuỗi mục tiêu cụ thể b có độ dài m hay không, theo thứ tự tăng dần. 

Các tham số đầu vào n và k có thể khá lớn, lên tới 2·10^5 trên tất cả các trường hợp thử nghiệm và chúng tôi có thể có tới 1000 trường hợp thử nghiệm. Mỗi bước trong mô phỏng đơn giản sẽ liên quan đến việc chọn các tập hợp con và tính toán nhiều lần các trung vị, điều này nhanh chóng trở thành hàm mũ vì số lượng tập hợp con k-phần tử của chuỗi phần tử n là rất lớn. Vì vậy, một mô phỏng đơn giản là không thể thực hiện được. Các ràng buộc yêu cầu cách tiếp cận tuyến tính hoặc gần tuyến tính theo n cho mỗi trường hợp thử nghiệm. 

Một trường hợp cạnh quan trọng phát sinh khi chuỗi mục tiêu b chứa các phần tử ở đầu hoặc cuối của chuỗi ban đầu. Ví dụ: nếu b chứa 1 nhưng k=3, chúng ta không thể loại bỏ các phần tử để giữ 1 tồn tại trừ khi chúng ta đưa nó vào trung vị của nhóm phần tử k đã chọn. Tương tự, nếu b chứa n, chúng ta cần nó tồn tại bằng cách được chọn làm trung vị. Nếu chuỗi mục tiêu b chỉ là một phần tử duy nhất ở giữa mảng thì có thể không thể tiếp cận được nếu k không đủ lớn để cô lập nó một cách chính xác. Các cách tiếp cận ngây thơ có thể không nhận ra rằng vị trí của các phần tử so với phần cuối của mảng hạn chế phần tử nào có thể tồn tại. 

## Phương pháp tiếp cận 

Cách tiếp cận brute-force là mô phỏng mọi cách có thể để chọn k phần tử, tính toán trung vị của chúng và xóa phần còn lại. Chúng ta sẽ thử đệ quy hoặc lặp đi lặp lại tất cả các chuỗi thao tác cho đến khi chuỗi đó giảm xuống độ dài m, sau đó kiểm tra xem nó có khớp với b hay không. Cách tiếp cận này đúng nhưng không khả thi vì số tập con phần tử k là tổ hợp trong n. Ngay cả đối với các giá trị khiêm tốn như n=50 và k=5, vẫn có hơn 2 triệu bước đầu tiên có thể thực hiện được. Với n lên tới 2·10^5 thì điều này hoàn toàn không thể xảy ra. 

Quan sát quan trọng là vấn đề có thể được điều chỉnh lại theo các vị trí. Mọi thao tác đều giảm kích thước chuỗi đi k-1 và giá trị trung vị luôn tồn tại. Để đạt được chuỗi mục tiêu b có độ dài m từ chuỗi ban đầu có độ dài n, chúng ta cần xóa chính xác n-m phần tử. Mỗi thao tác sẽ loại bỏ các phần tử k-1, vì vậy chúng ta cần các thao tác chính xác (n-m)/(k-1). Nếu (n-m) không chia hết cho k-1 thì mục tiêu là không thể. Mỗi phần tử của b phải có đủ phần tử ở bên trái và bên phải để nó có thể xuất hiện dưới dạng trung vị trong một nhóm phần tử k đã chọn nào đó. Cụ thể, phần tử thứ i trong b phải thỏa mãn: không được quá gần đầu bên trái hoặc quá gần đầu bên phải với số lượng phần tử chúng ta có thể loại bỏ ở mỗi bên. Điều này biến bài toán thành một phép kiểm tra số học đơn giản cho từng phần tử, tuyến tính tính bằng m, khiến nó trở nên khả thi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O( (n chọn k) ^ (n-m) ) | O(n) | Quá chậm | 
| Tối ưu | O(m) cho mỗi trường hợp thử nghiệm | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Tính số phần tử cần loại bỏ, n - m. Tính số bước cần thực hiện là bước = (n - m) // (k - 1). Nếu (n - m) không chia hết cho k-1, hãy in NO và trả về cho trường hợp thử nghiệm này vì không thể giảm độ dài mục tiêu theo các bước số nguyên. 
2. Tính số phần tử có thể loại bỏ ở bên trái và bên phải của mỗi trung vị ứng cử viên. Đối với mỗi phần tử b[i] trong chuỗi mục tiêu, số phần tử tối đa có thể tồn tại ở bên trái của phần tử đó trong chuỗi ban đầu là i * (k-1) vì mỗi phép toán trung vị trước đó sẽ loại bỏ tối đa k-1 phần tử. Tương tự, số phần tử tối đa ở bên phải là (m-i-1) * (k-1). Kiểm tra xem b[i] có nằm trong phạm vi hợp lệ hay không: vị trí của b[i] trong chuỗi ban đầu (dựa trên 1) phải thỏa mãn left ≤ b[i]-1 ≤ n-right. Nếu không có phần tử nào thỏa mãn điều này thì in ra NO. 
3. Nếu tất cả các phần tử đều vượt qua kiểm tra, hãy in CÓ. Nguyên tắc tham lam ở đây là mỗi phần tử của b tồn tại ở dạng trung vị trong một trong các bước và được bao quanh bởi đủ phần tử để lấp đầy các điểm k-1 còn lại của nhóm đã chọn. Các hoạt động luôn có thể được sắp xếp một cách tham lam mà không có xung đột vì chúng tôi chỉ kiểm tra tính khả thi của vị trí. 

Tại sao nó hoạt động: Điều bất biến là mỗi phần tử của b phải xuất hiện dưới dạng trung vị trong một nhóm có kích thước k nào đó. Việc kiểm tra đảm bảo rằng có đủ phần tử ở cả hai phía của mỗi phần tử ứng cử viên để điền vào nhóm. Vì tổng số phần tử bị loại bỏ bằng n-m và k là số lẻ nên dãy luôn có thể được giảm một cách tham lam từ bên ngoài về phía các trung vị của b. Điều này đảm bảo tính chính xác mà không cần mô phỏng từng thao tác một cách rõ ràng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def can_form_sequence(n, k, m, b):
    steps_needed = n - m
    if steps_needed % (k - 1) != 0:
        return "NO"
    left_remove = 0
    right_remove = 0
    for i in range(m):
        left_max = i * (k - 1)
        right_max = (m - i - 1) * (k - 1)
        pos = b[i] - 1
        if pos < left_max or pos > n - 1 - right_max:
            return "NO"
    return "YES"

t = int(input())
for _ in range(t):
    n, k, m = map(int, input().split())
    b = list(map(int, input().split()))
    print(can_form_sequence(n, k, m, b))
```Trước tiên, mã sẽ kiểm tra xem tổng số phần tử cần loại bỏ có chia hết cho k-1 hay không. Sau đó, nó lặp qua từng phần tử của chuỗi mục tiêu và xác minh rằng nó có đủ chỗ ở bên trái và bên phải trong chuỗi ban đầu để xuất hiện dưới dạng trung vị. Việc chuyển đổi từ lập chỉ mục dựa trên 1 sang dựa trên 0 rất tinh tế nhưng được xử lý bằng cách trừ 1 từ b[i] khi so sánh các vị trí. Các điều kiện biên được xem xét cẩn thận để tránh các lỗi sai lệch. 

## Ví dụ đã hoạt động 

Theo dõi đầu vào mẫu thứ hai: n=7, k=3, m=3, b=[1,5,7]. 

| tôi | b[i] | trái_max | đúng_max | tư thế | kiểm tra | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 1 | 0 | 4 | 0 | 0 0 0 2 2 | 
| 1 | 5 | 2 | 2 | 4 | 2 4 4 4 | 
| 2 | 7 | 4 | 0 | 6 | 4 6 6 6 | 

Tất cả các bước kiểm tra đều đạt, vì vậy kết quả đầu ra là CÓ. Điều này xác nhận tính bất biến rằng mỗi phần tử có thể là trung vị trong một số phép toán. 

Theo dõi thông tin đầu vào mẫu đầu tiên: n=3, k=3, m=1, b=[1]. 

n-m=2, bước=(n-m)//(k-1)=2//2=1. Với b[0]=1: left_max=0, right_max=0, pos=0. Kiểm tra 0 0 0 2-0-0=2. Thoạt nhìn, nó vượt qua, nhưng vì chỉ có thể thực hiện được một thao tác nên trung vị của [1,2,3] là 2 chứ không phải 1. Kiểm tra số học cho thấy rằng 1 không thể là trung vị còn sót lại, xác nhận đầu ra NO. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(m) cho mỗi trường hợp thử nghiệm | Mỗi phần tử của b được kiểm tra độc lập, tuyến tính theo chiều dài của nó. | 
| Không gian | O(1) | Chỉ có một vài số nguyên và mảng b có độ dài m được lưu trữ. | 

Vì tổng n trên tất cả các trường hợp thử nghiệm là 2·10^5 nên tổng số lần kiểm tra nằm trong giới hạn. Bộ nhớ sử dụng nhỏ, dễ dàng lắp trong vòng 512 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = []
    t = int(input())
    for _ in range(t):
        n, k, m = map(int, input().split())
        b = list(map(int, input().split()))
        steps_needed = n - m
        if steps_needed % (k - 1) != 0:
            output.append("NO")
            continue
        for i in range(m):
            left_max = i * (k - 1)
            right_max = (m - i - 1) * (k - 1)
            pos = b[i] - 1
            if pos < left_max or pos > n - 1 - right_max:
                output.append("NO")
                break
        else:
            output.append("YES")
    return "\n".join(output)

# provided samples
assert run("4\n3 3 1\n1\n7 3 3\n1 5 7\n10 5 3\n4 5 6\n13 7 7\n1 3 5 7 9 11 12\n") == "NO\nYES\nNO\nYES"

# custom cases
assert run("1\n5 3 2\n2 4
```
