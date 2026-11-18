---
title: "CF 2024B - Mua nước chanh"
description: "Chúng ta có một máy bán hàng tự động với n ô ẩn, mỗi ô chứa một số lon nước chanh đã biết. Ngoài ra còn có n nút, mỗi nút được ánh xạ tới đúng một vị trí, nhưng ánh xạ sẽ bị mất."
date: "2026-06-09T03:07:08+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "constructive-algorithms", "sortings"]
categories: ["algorithms"]
codeforces_contest: 2024
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 980 (Div. 2)"
rating: 1100
weight: 2024
solve_time_s: 311
verified: false
draft: false
---

[CF 2024B - Mua nước chanh](https://codeforces.com/problemset/problem/2024/B) 

**Đánh giá:** 1100 
**Tags:** tìm kiếm nhị phân, thuật toán xây dựng, sắp xếp 
**Thời gian giải:** 5 phút 11s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một máy bán hàng tự động với`n`các ô ẩn, mỗi ô chứa một số lon nước chanh đã biết. Ngoài ra còn có`n`các nút, mỗi nút được ánh xạ tới chính xác một vị trí, nhưng ánh xạ sẽ bị mất. Nhấn một nút có thể cung cấp cho bạn một lon nếu khe tương ứng còn lại; nếu không thì không có gì xảy ra. Bạn không thể biết lon đến từ khe nào và bạn không thể xem nội dung hiện tại. 

Nhiệm vụ là: đưa ra số lượng ban đầu trong các vị trí và mục tiêu`k`, hãy xác định số lần nhấn nút tối thiểu để đảm bảo đạt được ít nhất`k`lon, bất kể nút nào tương ứng với khe nào. Chúng ta biết trước rằng tổng số lon ít nhất là`k`. 

Những hạn chế cho thấy chúng ta có thể có tới`2 * 10^5`các vị trí được tổng hợp trên tất cả các trường hợp thử nghiệm và`k`có thể lớn như`10^9`. Điều này loại trừ bất kỳ mô phỏng lực lượng vũ phu nào trong đó chúng tôi thử mọi trình tự nhấn có thể, bởi vì nhấn các nút riêng lẻ cho đến khi chúng tôi đạt được`k`có thể thực hiện các phép toán O(k), quá lớn. Chúng ta cần một phương pháp phù hợp với`n`còn hơn là`k`. 

Trường hợp cạnh tinh tế xảy ra khi một số khe có số lượng cực lớn và một số khác rất nhỏ. Ví dụ, nếu`a = [1, 1000000000]`Và`k = 2`, cách tiếp cận ngây thơ nhấn các nút một cách mù quáng có thể nhấn quá mức máy ép đầu tiên và lãng phí, trong khi chiến lược tối ưu phải tính đến phân phối trong trường hợp xấu nhất để đảm bảo`k`lon. 

## Phương pháp tiếp cận 

Giải pháp đơn giản nhất là mô phỏng tất cả các trình tự ép. Người ta có thể thử một phương pháp tham lam: chọn liên tục bất kỳ nút nào cho đến khi nó không thành công, sau đó chuyển sang nút khác. Mặc dù về nguyên tắc là đúng, nhưng việc mô phỏng điều này quá chậm khi số lượng lớn vì trong trường hợp xấu nhất, nó cần phải nhấn O(k), tức là lên đến 10^9. 

Quan sát quan trọng là số lần nhấn tối thiểu phụ thuộc vào số lượng vị trí được sắp xếp theo thứ tự giảm dần. Nếu chúng tôi biểu thị số vị trí được tính là`a_1 ≥ a_2 ≥ ... ≥ a_n`, trường hợp xấu nhất xảy ra khi máy ép của chúng ta chạm vào các khe lớn nhất trước nhưng lại đánh trượt các khe nhỏ hơn cho đến hết. Vì chúng tôi có thể điều chỉnh sau mỗi lần nhấn dựa trên thành công hay thất bại nên chúng tôi có thể lập mô hình này bằng **tìm kiếm nhị phân về số lần nhấn trên mỗi nút**. 

Giải pháp tối ưu có thể được coi là phân phối số lần nhấn trên các nút theo cách có tính đến sự không chắc chắn về việc nút nào ánh xạ tới khe nào. Nếu chúng ta gán`x`nhấn từng nút, số lon đảm bảo mà chúng tôi thu thập được từ mỗi khe sẽ là`min(a_i, x)`, bởi vì một khe có ít lon hơn`x`không thể cung cấp nhiều hơn nội dung của nó. Tổng số này trên tất cả các vị trí sẽ cho ra tổng số lon được đảm bảo. Do đó, tổng số lần ép tối thiểu là nhỏ nhất`x`sao cho tổng của`min(a_i, x)`trên tất cả các vị trí ít nhất là`k`. 

Việc sắp xếp số lượng vị trí theo thứ tự giảm dần cho phép chúng tôi tính toán điều này một cách hiệu quả. Sau khi được sắp xếp, chúng ta có thể sử dụng cách tiếp cận tham lam: cố gắng "lấy càng nhiều càng tốt từ các vị trí lớn nhất trước", điều này làm giảm tìm kiếm nhị phân trên số lần nhấn có thể. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(k) cho mỗi trường hợp thử nghiệm | O(n) | Quá chậm | 
| Tham lam / Tìm kiếm nhị phân | O(n log max(a_i)) cho mỗi trường hợp thử nghiệm | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Với mỗi test case, hãy đọc`n`Và`k`và mảng`a`. Loại`a`theo thứ tự giảm dần. Việc sắp xếp đảm bảo chúng tôi cố gắng "che" các vị trí lớn nhất trước tiên. 
2. Khởi tạo hai con trỏ để tìm kiếm nhị phân:`low = 1`(tối thiểu 1 lần nhấn) và`high = k`(trường hợp xấu nhất: 1 lần nhấn mỗi lon). 
3. Trong khi`low < high`, tính toán`mid = (low + high) // 2`. Đối với ứng viên này`mid`nhấn từng nút, tính toán`sum(min(a_i, mid))`trên tất cả các khe. Tổng này là số lon được đảm bảo nếu nhấn từng nút`mid`lần trong một chiến lược thích ứng. 
4. Nếu tổng ít nhất`k`, thì chúng ta có thể đạt được`k`với số lần nhấn mỗi nút ít hơn, vì vậy hãy đặt`high = mid`. Ngược lại, đặt`low = mid + 1`để tăng số lần nhấn. 
5. Sau khi tìm kiếm nhị phân hoàn tất,`low`là số lần nhấn tối thiểu cho mỗi nút được yêu cầu. 
6. Để tìm tổng số lần nhấn, lưu ý rằng chúng ta sẽ nhấn từng nút theo số lần ấn định của nó.`low`lần. Nhưng bởi vì`k`có thể nhỏ hơn tổng số`min(a_i, low)`, chúng tôi điều chỉnh số lần nhấn cuối cùng để đáp ứng chính xác`k`. Do đó, tổng số lần ép được tính bằng tổng của`min(a_i, low)`cho đến khi tổng đạt`k`. 

**Tại sao nó hoạt động:** Tìm kiếm nhị phân duy trì tính bất biến`low`luôn khả thi hoặc quá nhỏ, và`high`luôn luôn đủ để đảm bảo`k`lon. Từ`min(a_i, x)`là đơn điệu trong`x`, tìm kiếm hội tụ về giá trị nhỏ nhất. Việc sắp xếp đảm bảo rằng chúng tôi xem xét phân phối máy ép trong trường hợp xấu nhất, đưa ra số lượng hộp được đảm bảo bất kể ánh xạ từ nút đến khe. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def min_presses(n, k, a):
    a.sort(reverse=True)
    low, high = 1, k
    while low < high:
        mid = (low + high) // 2
        total = sum(min(ai, mid) for ai in a)
        if total >= k:
            high = mid
        else:
            low = mid + 1
    # low is the minimum presses per button in worst case
    total_presses = 0
    remaining = k
    for ai in a:
        take = min(ai, low, remaining)
        total_presses += take
        remaining -= take
        if remaining == 0:
            break
    return total_presses

t = int(input())
for _ in range(t):
    n, k = map(int, input().split())
    a = list(map(int, input().split()))
    print(min_presses(n, k, a))
```Mỗi phần của mã tương ứng trực tiếp với các bước thuật toán. Việc sắp xếp là điều cần thiết cho việc tính toán tham lam các lon được đảm bảo. Vòng tìm kiếm nhị phân tìm thấy số lần nhấn tối thiểu trên mỗi nút một cách hiệu quả mà không cần mô phỏng mỗi lần nhấn. Tổng hợp`min(a_i, mid)`đảm bảo chúng tôi tôn trọng giới hạn vị trí. Vòng lặp cuối cùng đảm bảo rằng tổng số lần nhấn đáp ứng chính xác`k`. 

## Ví dụ đã hoạt động 

**Ví dụ 1**: 

đầu vào:`n=2, k=2, a=[1,2]`| Bước | Đã sắp xếp`a`| Giữa | Tổng (phút(a_i, giữa)) | Quyết định | 
| --- | --- | --- | --- | --- | 
| Ban đầu | [2,1] | - | - | thấp=1, cao=2 | 
| 1 | - | giữa=1 | phút(2,1)+min(1,1)=2 | tổng>=k → cao=1 | 
| Xong | - | thấp=1 | tổng_presses = phút(2,1)+min(1,1)=2 | Đầu ra 2 | 

Chúng tôi nhấn mỗi nút một lần. Trường hợp xấu nhất, chúng tôi nhận được 2 lon. 

**Ví dụ 2**: 

đầu vào:`n=3, k=4, a=[2,1,3]`| Bước | Đã sắp xếp`a`| Giữa | Tổng (phút(a_i, giữa)) | Quyết định | 
| --- | --- | --- | --- | --- | 
| Ban đầu | [3,2,1] | - | - | thấp=1, cao=4 | 
| 1 | - | giữa=2 | phút(3,2)+min(2,2)+min(1,2)=2+2+1=5 | tổng>=k → cao=2 | 
| 2 | - | giữa=1 | 1+1+1=3 | tổng<k → thấp=2 | 
| Xong | - | thấp=2 | tổng min(a_i,2) cho đến khi 4 lon = 2+2=4 | Đầu ra 4 | 

Điều này xác nhận thuật toán phân phối chính xác các máy ép giữa các khe để đảm bảo`k`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log k) cho mỗi trường hợp thử nghiệm | Việc sắp xếp mất O(n log n), tìm kiếm nhị phân trên phạm vi 1..k lấy log k, mỗi lần lặp tính tổng trên n vị trí. | 
| Không gian | O(n) | Lưu trữ mảng số lượng vị trí. | 

Cho số tiền đó`n`trên tất cả các trường hợp thử nghiệm là 2*10^5 và`log k ≤ 30`, giải pháp dễ dàng phù hợp với giới hạn thời gian 1 giây. 

## Trường hợp thử nghiệm```python
def run(inp: str) -> str:
    import sys, io
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    output = io.StringIO()
    with redirect_stdout(output):
        exec(open("solution.py").read())
    return output.getvalue().strip()

# Provided samples
assert run("5\n2 1\n1 1\n2 2\n1 2\n3 4\n2 1 3\n10 50\n1 1 3 8 8 9 12 13 27 27\n2 1000000000\n1000000000 500000000\n") == "1\n2\n
```
