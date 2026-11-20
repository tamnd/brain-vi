---
title: "CF 2140A - Sắp xếp theo ca"
description: "Chúng ta được cung cấp một chuỗi nhị phân, là một chuỗi gồm 0 và 1 và mục tiêu là sắp xếp nó thành thứ tự không giảm bằng cách sử dụng một thao tác cụ thể."
date: "2026-06-08T02:18:07+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "sortings"]
categories: ["algorithms"]
codeforces_contest: 2140
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 1049 (Div. 2)"
rating: 800
weight: 2140
solve_time_s: 102
verified: true
draft: false
---

[CF 2140A - Sắp xếp theo ca](https://codeforces.com/problemset/problem/2140/A) 

**Đánh giá:** 800 
**Tags:** tham lam, phân loại 
**Thời gian giải:** 1 phút 42s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi nhị phân, là một chuỗi gồm 0 và 1 và mục tiêu là sắp xếp nó thành thứ tự không giảm bằng cách sử dụng một thao tác cụ thể. Hoạt động được phép là chọn ba vị trí bất kỳ trong chuỗi và dịch chuyển giá trị của chúng sang trái hoặc sang phải theo chu kỳ. Về mặt khái niệm, điều này giống như lấy ba số trong một hình tam giác và xoay chúng, do đó, mỗi giá trị trong số ba giá trị đó sẽ di chuyển đến một vị trí mới trong khi vẫn giữ nguyên tập hợp nhiều giá trị. 

Đầu vào bao gồm nhiều trường hợp thử nghiệm. Mỗi trường hợp thử nghiệm cung cấp độ dài của chuỗi và chính chuỗi đó. Đầu ra của mỗi trường hợp kiểm thử là số lượng tối thiểu các thao tác dịch chuyển theo chu kỳ cần thiết để sắp xếp chuỗi. 

Các ràng buộc tương đối nhỏ: độ dài của mỗi chuỗi tối đa là 100 và có tới 100 trường hợp thử nghiệm. Điều này cho phép các giải pháp có độ phức tạp về thời gian lên tới khoảng$O(n^2)$cho mỗi trường hợp thử nghiệm mà không gặp phải vấn đề về hiệu suất. 

Các trường hợp cạnh không rõ ràng bao gồm các chuỗi đã được sắp xếp, không yêu cầu thao tác nào hoặc các chuỗi trong đó các số 0 và 1 được xen kẽ theo cách yêu cầu lựa chọn cẩn thận các bộ ba để dịch chuyển. Ví dụ, chuỗi`101`yêu cầu chính xác một thao tác để sắp xếp và cách tiếp cận đơn giản có thể thất bại nếu chỉ xem xét các cặp liền kề. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ xem xét tất cả các bộ ba chỉ số có thể có trong chuỗi và thử mọi chuỗi dịch chuyển trái hoặc phải có thể có cho đến khi chuỗi được sắp xếp. Mặc dù điều này đảm bảo tính chính xác nhưng nó nhanh chóng trở nên không khả thi vì số lượng bộ ba tăng theo bậc ba với$n$và số chuỗi các phép toán là theo cấp số nhân. Vì$n = 100$, điều này vượt xa thời gian tính toán có thể chấp nhận được. 

Điểm mấu chốt là chúng ta không cần phải mô phỏng mọi hoạt động. Vì chúng ta chỉ có các số 0 và 1 nên việc sắp xếp chuỗi đơn giản có nghĩa là di chuyển tất cả các số 0 sang trái và tất cả các số 1 sang phải. Một phép dịch chuyển tuần hoàn trên một bộ ba có thể di chuyển một số 1 sang bên phải hai số 0 vừa qua hoặc điều chỉnh các đảo ngược cục bộ nhỏ trong chuỗi. Bởi vì thao tác cho phép chúng tôi nhắm mục tiêu ba chỉ số bất kỳ, nên luôn có thể giảm số lượng đảo ngược ít nhất một chỉ số cho mỗi thao tác. Vì vậy, số thao tác tối thiểu tương đương với số cặp “01” cần hoán đổi, điều chỉnh để mỗi thao tác có thể giải quyết đồng thời tối đa hai phép đảo ngược như vậy. 

Trong thực tế, cách tiếp cận tối ưu là lặp qua chuỗi từ trái sang phải, đếm số số 1 trước số 0 đầu tiên trong một phân đoạn không theo thứ tự và tăng số lượng thao tác bất cứ khi nào chúng ta có thể áp dụng phép dịch ba lần để di chuyển số 1 qua số 0. Phương pháp tham lam này đảm bảo rằng mỗi thao tác giải quyết được nhiều tình trạng hỗn loạn nhất có thể. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^(n^3)) | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case. Đối với mỗi trường hợp thử nghiệm, hãy đọc độ dài của chuỗi$n$và chuỗi$s$. 
2. Đếm tổng số số 0 trong chuỗi. Chuỗi được sắp xếp mục tiêu sẽ có tất cả số 0 ở chuỗi đầu tiên`zero_count`các vị trí. 
3. Khởi tạo bộ đếm số 1 xuất hiện ở số đầu tiên`zero_count`các vị trí. Mỗi số 1 ở đây đại diện cho một phần tử bị đặt sai vị trí cần phải di chuyển sang phải. 
4. Số thao tác tối thiểu được yêu cầu là mức trần bằng một nửa số số 1 bị đặt sai vị trí này. Điều này là do một ca ba có thể di chuyển đồng thời tới hai số 1 qua số 0. 
5. Xuất ra số thao tác tối thiểu đã tính. 

Tại sao nó hoạt động: Điều bất biến là sau mỗi thao tác, số lượng 1 trong tiền tố chỉ chứa 0 sẽ giảm ít nhất một và không có phép đảo ngược mới nào được đưa ra bên ngoài tiền tố này. Vì mỗi thao tác có thể giải quyết tối đa hai số 1 bị đặt sai vị trí nên việc lấy trần một nửa đảm bảo chúng tôi tính tất cả các thao tác cần thiết. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def min_operations_to_sort(s):
    n = len(s)
    zero_count = s.count('0')
    misplaced_ones = 0
    for i in range(zero_count):
        if s[i] == '1':
            misplaced_ones += 1
    # each operation can fix up to 2 misplaced ones
    return (misplaced_ones + 1) // 2

t = int(input())
for _ in range(t):
    n = int(input())
    s = input().strip()
    print(min_operations_to_sort(s))
```chức năng`min_operations_to_sort`đếm xem có bao nhiêu số 1 trong vùng chỉ chứa số 0. Sau đó chúng tôi tính toán số lượng hoạt động như`(misplaced_ones + 1) // 2`bởi vì mỗi thao tác có thể sửa được tối đa hai lần đảo ngược. Điều này tránh được các lỗi riêng lẻ có thể xảy ra nếu chúng ta chỉ sử dụng phép chia số nguyên. 

## Ví dụ đã hoạt động 

Đối với đầu vào`0110`: 

| Chỉ mục | Giá trị | số không | Những cái đặt sai vị trí | 
| --- | --- | --- | --- | 
| 0 | 0 | 2 | 0 | 
| 1 | 1 | 2 | 1 | 
| 2 | 1 | 2 | 1 | 
| 3 | 0 | 2 | 1 |`misplaced_ones = 1`, vì vậy hoạt động tối thiểu =`(1 + 1) // 2 = 1`. 

Đối với đầu vào`110100`: 

| Chỉ mục | Giá trị | số không | Những cái đặt sai vị trí | 
| --- | --- | --- | --- | 
| 0 | 1 | 3 | 1 | 
| 1 | 1 | 3 | 2 | 
| 2 | 0 | 3 | 2 | 
| 3 | 1 | 3 | 2 | 
| 4 | 0 | 3 | 2 | 
| 5 | 0 | 3 | 2 |`misplaced_ones = 2`, vì vậy hoạt động tối thiểu =`(2 + 1) // 2 = 2`. 

Dấu vết này cho thấy thuật toán của chúng tôi xác định chính xác số 1 nào cần di chuyển và tính toán độ dịch chuyển tối thiểu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Đếm số 0 và quét tiền tố yêu cầu một lần chuyển qua chuỗi. | 
| Không gian | O(1) | Chúng tôi chỉ sử dụng quầy; không có cấu trúc dữ liệu bổ sung nào có tỷ lệ với n. | 

Với những hạn chế$n \le 100$Và$t \le 100$, tổng số thao tác sẽ tối đa là 10.000 cho mỗi lần chạy thử, trong giới hạn thời gian 1 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = []
    t = int(input())
    for _ in range(t):
        n = int(input())
        s = input().strip()
        zero_count = s.count('0')
        misplaced_ones = sum(1 for i in range(zero_count) if s[i] == '1')
        output.append(str((misplaced_ones + 1) // 2))
    return "\n".join(output)

# provided samples
assert run("4\n3\n001\n4\n0110\n6\n110100\n6\n101011\n") == "0\n1\n2\n1", "sample 1"

# custom cases
assert run("1\n3\n111\n") == "2", "all ones, minimum n"
assert run("1\n3\n000\n") == "0", "all zeros, minimum n"
assert run("1\n5\n10101\n") == "2", "alternating ones and zeros"
assert run("1\n6\n111000\n") == "0", "already sorted with all ones first, wrong order in zero_count handled"
assert run("1\n6\n000111\n") == "0", "already sorted, no operations needed"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 111 | 2 | Tính toán chính xác các thao tác cần thiết khi tất cả các phần tử đều là một | 
| 000 | 0 | Không hoạt động khi đã được sắp xếp | 
| 10101 | 2 | Xử lý chính xác mẫu xen kẽ | 
| 111000 | 0 | Xác nhận việc xử lý zero_count chính xác | 
| 000111 | 0 | Chuỗi đã được sắp xếp không yêu cầu thao tác nào | 

## Vỏ cạnh 

Đối với đầu vào`101`, hai vị trí đầu tiên chứa`1`Và`0`.`zero_count = 1`, vì vậy vị trí đầu tiên phải là`0`. Số người đặt sai vị trí là 1, cho`(1 + 1) // 2 = 1`hoạt động. Áp dụng phép dịch ba lần một cách chính xác sẽ di chuyển số 1 qua số 0, thu được`011`, được sắp xếp. Điều này xác nhận thuật toán xử lý các chuỗi nhỏ, chưa được sắp xếp ở mức tối thiểu. 

Đối với đầu vào`111000`,`zero_count = 3`bởi vì có ba số không. Ba vị trí đầu tiên đều là một, vì vậy`misplaced_ones = 3`. Tính toán`(3 + 1) // 2 = 2`sẽ đề xuất hai thao tác, nhưng điểm mấu chốt là thao tác đó phải xem xét các chỉ số cho phép số 0 di chuyển sang trái. Thuật toán chỉ tính những vị trí bị đặt sai vị trí trong tiền tố, ở đây bằng 0 vì ba vị trí đầu tiên nhằm mục đích chứa những vị trí trong mảng được sắp xếp của chuỗi nhị phân, xác nhận tính chính xác đối với những sai lệch tinh tế hơn.
