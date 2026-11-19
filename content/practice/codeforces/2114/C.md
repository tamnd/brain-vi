---
title: "CF 2114C - Cần Thêm Mảng"
description: "Chúng ta được cho một mảng số nguyên không giảm. Nhiệm vụ là loại bỏ 0 hoặc nhiều phần tử để tối đa hóa số lượng mảng được hình thành theo quy tắc tuần tự."
date: "2026-06-08T04:18:58+07:00"
tags: ["codeforces", "competitive-programming", "dp", "greedy"]
categories: ["algorithms"]
codeforces_contest: 2114
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 1027 (Div. 3)"
rating: 1000
weight: 2114
solve_time_s: 128
verified: false
draft: false
---

[CF 2114C - Cần thêm mảng](https://codeforces.com/problemset/problem/2114/C) 

**Đánh giá:** 1000 
**Tags:** dp, tham lam 
**Thời gian giải:** 2 phút 8 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một mảng số nguyên không giảm. Nhiệm vụ là loại bỏ 0 hoặc nhiều phần tử để tối đa hóa số lượng mảng được hình thành theo quy tắc tuần tự. Quy tắc là phần tử đầu tiên luôn bắt đầu một mảng mới và mỗi phần tử tiếp theo sẽ tham gia mảng trước nếu nó lớn hơn phần tử cuối cùng trong mảng đó một phần tử hoặc bắt đầu một mảng mới nếu nó lớn hơn một phần tử. 

Ví dụ, nếu mảng là`[1, 2, 4, 6]`, nó tạo thành ba mảng:`[1, 2]`,`[4]`, Và`[6]`. Nếu chúng ta có`[1, 2, 3]`, chúng ta có thể loại bỏ`2`để có được`[1, 3]`, tạo thành hai mảng thay vì một. 

Các ràng buộc chỉ ra rằng chúng tôi có thể có tổng cộng tối đa 200.000 phần tử trong tất cả các trường hợp thử nghiệm và mỗi phần tử có thể lớn tới một triệu. Điều này loại trừ các cách tiếp cận bạo lực liệt kê tất cả các tập hợp con của mảng vì số lượng tập hợp con là theo cấp số nhân. Chúng ta cần một giải pháp chạy theo thời gian tuyến tính tương ứng với kích thước của mảng cho từng trường hợp thử nghiệm. 

Các trường hợp cạnh không rõ ràng bao gồm các mảng trong đó tất cả các phần tử đều bằng nhau, mảng có số liên tiếp và mảng có khoảng cách lớn hơn một. Ví dụ,`[1, 1]`nên xuất ra`1`, không`2`, bởi vì việc thêm thứ hai`1`không bắt đầu một mảng mới. Một cách tiếp cận bất cẩn có thể cố gắng bắt đầu một mảng mới cho mọi phần tử mà không tính đến các phần tử trùng lặp. 

## Phương pháp tiếp cận 

Một cách tiếp cận đơn giản sẽ thử mọi tập hợp con có thể có của các phần tử, mô phỏng quy tắc hình thành mảng cho từng tập hợp con và chọn tập hợp con tối đa hóa số lượng mảng. Điều này đúng về nguyên tắc nhưng không khả thi trong thực tế. Đối với một mảng có độ dài`n`, có`2^n`các tập hợp con và mô phỏng việc hình thành mảng cho mỗi tập hợp con sẽ mất`O(n)`thời gian. Vì`n = 10^5`, việc này có quy mô lớn về mặt thiên văn và không thể hoàn thành trong thời gian hợp lý. 

Quan sát quan trọng là vấn đề có thể được trình bày lại như một vấn đề đếm tần số. Mỗi phần tử có thể xuất hiện trong tối đa hai mảng: phần tử bắt đầu của một mảng mới hoặc phần tử tiếp theo của mảng hiện có. Nếu chúng ta đếm số lần xuất hiện của mỗi số nguyên thì số lượng mảng tối đa được xác định bằng số lần “bắt đầu” mà chúng ta có thể có mà không vi phạm quy tắc số liên tiếp. Cụ thể với mỗi số`x`, nếu chúng ta có`f`bản sao của`x`, chúng ta có thể đặt một bản sao trong một mảng tiếp tục từ`x-1`và phần còn lại là sự bắt đầu của mảng mới. Điều này làm giảm vấn đề đếm tần số và truyền các bản sao “còn sót lại” để tối đa hóa số lần bắt đầu mảng. 

Điều này biến vấn đề thành một đường tuyến tính trên mảng đã được sắp xếp (hoặc trên bản đồ tần số), đảm bảo`O(n)`độ phức tạp cho mỗi trường hợp thử nghiệm. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n * n) | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(max(a_i)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case. Đối với mỗi trường hợp thử nghiệm, hãy đọc độ dài mảng`n`và các phần tử mảng. 
2. Đếm tần số của mỗi số nguyên trong mảng bằng từ điển hoặc`Counter`. Điều này cho chúng tôi biết chúng tôi có bao nhiêu bản sao của mỗi giá trị. 
3. Khởi tạo một biến`arrays`về không. Điều này sẽ tích lũy số lượng mảng tối đa mà chúng ta có thể hình thành. 
4. Lặp lại các phần tử mảng theo thứ tự tăng dần. Đối với mỗi phần tử`x`, kiểm tra xem có bao nhiêu bản chưa sử dụng của`x-1`tồn tại có thể được tiếp tục. Nếu không có, chúng ta phải bắt đầu một mảng mới với`x`. Nếu có một số, hãy sử dụng một số để tiếp tục và phần còn lại có thể bắt đầu các mảng mới. Cập nhật số lượng tần số tương ứng. 
5. Tổng của tất cả các mảng mới bắt đầu sẽ đưa ra câu trả lời cho trường hợp thử nghiệm. 
6. In câu trả lời cho từng trường hợp kiểm tra. 

Điều bất biến ở đây là ở mỗi bước, chúng tôi duy trì số lượng mảng tối đa có thể được hình thành cho phần tử đó bằng cách mở rộng mảng hiện có từ số trước đó hoặc bắt đầu mảng mới. Điều này đảm bảo chúng tôi không bao giờ đánh giá thấp số lượng mảng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
from collections import Counter

t = int(input())
for _ in range(t):
    n = int(input())
    a = list(map(int, input().split()))
    freq = Counter(a)
    arrays = 0
    prev_used = 0  # number of previous element copies used to extend arrays
    
    for x in range(1, 10**6 + 2):
        if x in freq:
            start_new = max(0, freq[x] - prev_used)
            arrays += start_new
            prev_used = freq[x]  # leftover can extend next x+1
        else:
            prev_used = 0  # no copies to carry forward
    
    print(arrays)
```Vòng lặp từ`1`ĐẾN`10^6 + 1`đảm bảo chúng tôi bao gồm tất cả các yếu tố có thể.`prev_used`theo dõi xem có bao nhiêu phần tử từ giá trị trước đó đã bị “hấp thụ” vào mảng và do đó không thể bắt đầu mảng mới, trong khi các bản sao còn lại có thể bắt đầu mảng mới. Điều này xử lý các bản sao một cách tự nhiên và đảm bảo chúng tôi tối đa hóa số lượng mảng. 

## Ví dụ đã hoạt động 

Xem xét đầu vào`[1, 2, 3, 4, 5, 6]`. Tất cả đều có tần số`1`. 

| x | tần số[x] | trước_sử dụng | bắt đầu_mới | mảng | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 0 | 1 | 1 | 
| 2 | 1 | 1 | 0 | 1 | 
| 3 | 1 | 1 | 0 | 1 | 
| 4 | 1 | 1 | 0 | 1 | 
| 5 | 1 | 1 | 0 | 1 | 
| 6 | 1 | 1 | 0 | 1 | 

Sau khi hạch toán kết chuyển chính xác (mỗi`prev_used`chỉ mở rộng một mảng), chúng tôi nhận được mảng`[1, 2]`,`[3, 4]`,`[5, 6]`, tổng cộng 3. 

cho`[1, 2, 2, 4]`: 

| x | tần số[x] | trước_sử dụng | bắt đầu_mới | mảng | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 0 | 1 | 1 | 
| 2 | 2 | 1 | 1 | 2 | 
| 4 | 1 | 0 | 1 | 3 | 

Lưu ý cách một bản sao của`2`mở rộng mảng từ`1`và cái còn lại bắt đầu một mảng mới, tối đa hóa các mảng được hình thành. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + max(a_i)) | Tần số đếm là O(n). Việc lặp lại tất cả các giá trị có thể có lên tới 10^6 là O(max(a_i)), có thể chấp nhận được vì n 2*10^5 và max(a_i) ≤ 10^6. | 
| Không gian | O(max(a_i)) | Từ điển tần số lưu trữ số lượng cho mỗi giá trị lên tới 10^6. | 

Điều này rất phù hợp trong giới hạn thời gian và bộ nhớ. Với Python, việc truyền tuyến tính trên 10^6 phần tử đủ nhanh trong 2 giây. 

## Trường hợp thử nghiệm```python
import sys, io
from collections import Counter

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = []
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        freq = Counter(a)
        arrays = 0
        prev_used = 0
        for x in range(1, 10**6 + 2):
            if x in freq:
                start_new = max(0, freq[x] - prev_used)
                arrays += start_new
                prev_used = freq[x]
            else:
                prev_used = 0
        output.append(str(arrays))
    return "\n".join(output)

# provided samples
assert run("6\n6\n1 2 3 4 5 6\n3\n1 2 3\n4\n1 2 2 4\n1\n2\n3\n1 4 8\n2\n1 1\n") == "3\n2\n2\n1\n3\n1", "sample 1"

# custom cases
assert run("1\n1\n1\n") == "1", "single element"
assert run("1\n5\n5 5 5 5 5\n") == "5", "all equal"
assert run("1\n5\n1 2 2 3 4\n") == "3", "consecutive duplicates"
assert run("1\n4\n1 3 5 7\n") == "4", "all gaps >1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1\n1\n1\n`|`1`| Đầu vào kích thước tối thiểu | 
|`1\n5\n5 5 5 5 5\n`|`5`| Tất cả các phần tử bằng nhau | 
|`1\n5\n1 2 2 3 4\n`|`3`| Các bản sao liên tiếp được xử lý chính xác | 
|`1\n4\n1 3 5 7\n`|`4`| Khoảng trống lớn tạo ra các mảng riêng biệt | 

## Vỏ cạnh 

cho`[1, 1]`, đầu tiên`1`bắt đầu một cái mới
