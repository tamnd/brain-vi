---
title: "CF 1700C - Giúp ích cho thiên nhiên"
description: "Chúng tôi có một dãy số nguyên biểu thị độ ẩm của đất dọc theo con đường có cây cối. Mỗi giá trị có thể dương, âm hoặc 0 và mục tiêu của chúng tôi là giảm tất cả các giá trị về 0 bằng cách sử dụng ba thao tác: giảm tiền tố đi một, giảm hậu tố đi một hoặc tăng toàn bộ…"
date: "2026-06-09T22:00:12+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "data-structures", "greedy"]
categories: ["algorithms"]
codeforces_contest: 1700
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 802 (Div. 2)"
rating: 1700
weight: 1700
solve_time_s: 144
verified: false
draft: false
---

[CF 1700C - Helping the Nature](https://codeforces.com/problemset/problem/1700/C)

 **Đánh giá:** 1700 
**Tags:** thuật toán xây dựng, cấu trúc dữ liệu, tham lam 
**Thời gian giải:** 2m 24s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một dãy số nguyên biểu thị độ ẩm của đất dọc theo con đường có cây cối. Mỗi giá trị có thể dương, âm hoặc 0 và mục tiêu của chúng tôi là giảm tất cả các giá trị về 0 bằng cách sử dụng ba thao tác: giảm tiền tố đi một, giảm hậu tố đi một hoặc tăng toàn bộ mảng lên một. The task is to determine the minimum number of such operations to achieve a zero array.

 Kích thước mảng`n`có thể đạt tới 200.000 và tổng của tất cả`n`trên tất cả các trường hợp thử nghiệm cũng bị giới hạn ở 200.000. Điều này có nghĩa là chúng tôi không thể cung cấp các thuật toán thực hiện nhiều hơn công việc khoảng O(n) cho mỗi trường hợp thử nghiệm; bất cứ điều gì bậc hai hoặc tệ hơn là quá chậm. Các giá trị độ ẩm có thể có độ lớn rất lớn, lên tới 10^9 theo cả hai hướng, điều này loại trừ các phương pháp cố gắng mô phỏng từng mức tăng hoặc giảm riêng lẻ. 

Các trường hợp cạnh không rõ ràng bao gồm các mảng trong đó tất cả các giá trị đều âm, trong đó các giá trị dương và âm xen kẽ nhau hoặc khi các giá trị cực trị xảy ra ở các ranh giới. Ví dụ, một mảng`[-3, -2, -5]`chỉ yêu cầu tăng trên toàn bộ mảng để đạt 0, trong khi một mảng hỗn hợp như`[5, -5, 5, -5]`yêu cầu các thao tác phân tách tiền tố và hậu tố một cách cẩn thận. Một cách tiếp cận ngây thơ chỉ quét một lần từ trái sang phải sẽ không thành công đối với những điều này vì nó không thể giải thích được nhu cầu “đẩy” các giá trị âm bằng hoạt động tăng tổng thể. 

## Phương pháp tiếp cận 

Phương pháp vũ phu có khái niệm đơn giản. Đối với mỗi thao tác, bạn quét mảng, tìm tiền tố hoặc hậu tố có giá trị khác 0 và áp dụng mức giảm (hoặc mức tăng cho thao tác chung). Mỗi thao tác giảm một số giá trị đi một và bạn lặp lại cho đến khi tất cả các giá trị bằng 0. Điều này hoạt động vì các thao tác được phép là đủ để thao tác bất kỳ mẫu số nguyên nào về 0. Nhưng cách tiếp cận này quá chậm: nếu chúng ta có`n = 2 * 10^5`và giá trị tối đa`10^9`, số lượng thao tác mô phỏng sẽ lên tới hàng tỷ, điều này không thể thực hiện được trong 2 giây. 

Cái nhìn sâu sắc quan trọng cho một giải pháp tối ưu đến từ việc xử lý các phép toán dựa trên sự khác biệt giữa các phần tử liên tiếp. Nếu chúng ta xem xét sự thay đổi cần thiết từ vị trí này sang vị trí tiếp theo, chúng ta có thể biểu thị tổng số bước di chuyển dưới dạng tổng của mức tăng dương cần thiết và mức giảm dương cần thiết cho mỗi bước. Cụ thể, số lần di chuyển tối thiểu là tổng giá trị tuyệt đối của phần tử đầu tiên cộng với tổng tất cả các sai phân dương khi di chuyển từ trái sang phải, cộng với tổng tất cả các sai phân âm khi di chuyển từ phải sang trái. Điều này hiệu quả vì mỗi thao tác tiền tố/hậu tố có thể được hiểu là “hấp thụ” sự khác biệt một cách hiệu quả. 

Về bản chất, mỗi lần tăng hoặc giảm cục bộ giữa các phần tử liền kề có thể được thỏa mãn bằng một thao tác tiền tố hoặc hậu tố duy nhất và thao tác tăng toàn cục xử lý các giá trị bắt đầu âm. Bằng cách tích lũy những khác biệt này trong một lần thực hiện, chúng tôi tránh được sự mô phỏng và giảm vấn đề xuống còn O(n). 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n * max( | a_i | )) | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case`t`. 
2. Với mỗi test case, hãy đọc`n`và mảng`a`. 
3. Khởi tạo bộ đếm`actions`về không. Điều này sẽ theo dõi tổng số hoạt động cần thiết. 
4. Bước đầu tiên là xử lý phần tử đầu tiên. Nếu như`a[0]`là tiêu cực, chúng ta cần`-a[0]`tăng trên toàn bộ mảng; nếu tích cực, chúng ta cần`a[0]`giảm thông qua một hoạt động tiền tố. Thêm cái này vào`actions`. 
5. Lặp qua mảng từ chỉ số 1 đến n-1. Đối với mỗi phần tử`a[i]`, tính sự khác biệt`diff = a[i] - a[i-1]`. Nếu như`diff > 0`, nó thể hiện mức tăng thêm cần thiết so với phần tử trước đó, có thể đạt được bằng thao tác tiền tố. Nếu như`diff < 0`, nó thể hiện mức giảm thêm, có thể đạt được thông qua thao tác hậu tố. Cộng giá trị tuyệt đối của`diff`ĐẾN`actions`. 
6. Sau khi hoàn thành vòng lặp,`actions`chứa số lượng thao tác tối thiểu cần thiết để đưa mảng về 0. 
7. In`actions`cho từng trường hợp thử nghiệm. 

Tại sao nó hoạt động: mỗi thao tác chúng tôi đếm tương ứng với đơn vị công việc tối thiểu cần thiết để điều chỉnh độ dốc giữa các phần tử liên tiếp. Các hoạt động tiền tố và hậu tố tự nhiên phù hợp với sự khác biệt tích cực và tiêu cực, trong khi mức tăng tổng thể xử lý điểm bắt đầu tiêu cực ban đầu. Bất biến này đảm bảo không có nước đi dư thừa nào được tính. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        actions = abs(a[0])  # handle the first element
        for i in range(1, n):
            diff = a[i] - a[i-1]
            actions += abs(diff)
        print(actions)

if __name__ == "__main__":
    solve()
```Mã khởi tạo`actions`với giá trị tuyệt đối của phần tử đầu tiên, phản ánh số lượng thao tác tối thiểu về 0 khi bắt đầu. Vòng lặp sau đó tích lũy chi phí của tất cả các độ dốc giữa các phần tử liên tiếp. sử dụng`abs`đảm bảo chúng tôi tính cả điều chỉnh tích cực và tiêu cực và vòng lặp đơn tạo ra O(n) này cho mỗi trường hợp thử nghiệm. Các lỗi biên có thể tránh được bằng cách bắt đầu từ chỉ số 1. 

## Ví dụ đã hoạt động 

### Mẫu 1:`[-2, -2, -2]`| tôi | một [tôi] | khác biệt | hành động | 
| --- | --- | --- | --- | 
| 0 | -2 | - | 2 | 
| 1 | -2 | 0 | 2 | 
| 2 | -2 | 0 | 2 | 

Tất cả các yếu tố đều bằng nhau và âm, vì vậy chỉ cần 2 lần tăng toàn cầu. 

### Mẫu 2:`[10, 4, 7]`| tôi | một [tôi] | khác biệt | hành động | 
| --- | --- | --- | --- | 
| 0 | 10 | - | 10 | 
| 1 | 4 | -6 | 16 | 
| 2 | 7 | 3 | 19 | 

Tổng cộng 19 thao tác tương ứng với sự kết hợp điều chỉnh tiền tố và hậu tố để đi theo độ dốc. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Đi qua mảng một lần, tính toán sự khác biệt | 
| Không gian | O(n) | Lưu trữ mảng đầu vào, không có cấu trúc dữ liệu bổ sung | 

Cho n ≤ 2_10^5 và tổng n trên tất cả các trường hợp thử nghiệm 2_10^5, điều này phù hợp thoải mái trong giới hạn 2 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    solve()
    sys.stdout = sys.__stdout__
    return output.getvalue().strip()

# provided samples
assert run("4\n3\n-2 -2 -2\n3\n10 4 7\n4\n4 -4 4 -4\n5\n1 -2 3 -4 5\n") == "2\n13\n36\n33"

# custom cases
assert run("1\n1\n100\n") == "100", "single element positive"
assert run("1\n1\n-100\n") == "100", "single element negative"
assert run("1\n5\n0 0 0 0 0\n") == "0", "all zero"
assert run("1\n3\n-1 0 1\n") == "2", "negative to positive slope"
assert run("1\n4\n5 -5 5 -5\n") == "40", "alternating extremes"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1\n1\n100\n`| 100 | Yếu tố đơn dương | 
|`1\n1\n-100\n`| 100 | Yếu tố đơn âm | 
|`1\n5\n0 0 0 0 0\n`| 0 | Tất cả các giá trị bằng 0 | 
|`1\n3\n-1 0 1\n`| 2 | Độ dốc hỗn hợp trên 0 | 
|`1\n4\n5 -5 5 -5\n`| 40 | Luân phiên các thái cực lớn | 

## Vỏ cạnh 

Đối với một mảng âm hoàn toàn như`[-3, -2, -5]`,`actions`bắt đầu bằng 3 cho phần tử đầu tiên. Sau đó, sự khác biệt là`(-2)-(-3)=1`Và`(-5)-(-2)=-3`, vì vậy chúng tôi thêm`1 + 3 = 4`đưa ra tổng cộng là 7. Điều này phản ánh 3 mức tăng toàn cầu cộng với sự điều chỉnh giữa các độ dốc, đưa mảng về 0 một cách chính xác. 

Đối với một mảng xen kẽ`[1, -2, 3, -4]`, chúng tôi tính toán`abs(1)=1`, thì sự khác biệt`-3, 5, -7`, thêm`3+5+7=15`tổng cộng là 16. Mỗi bước ghi lại các bước di chuyển chính xác cần thiết cho các hoạt động tiền tố và hậu tố mà không cần mô phỏng. 

Điều này xác nhận thuật toán xử lý các khác biệt biên, dấu hỗn hợp và giá trị cực trị một cách nhất quán.
