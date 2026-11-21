---
title: "CF 224B - Mảng"
description: "Chúng ta được cho một mảng các số nguyên và một số k. Nhiệm vụ là tìm một phân đoạn hoặc phân đoạn liền kề sao cho nó chứa chính xác k số nguyên phân biệt."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "bitmasks", "implementation", "two-pointers"]
categories: ["algorithms"]
codeforces_contest: 224
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 138 (Div. 2)"
rating: 1500
weight: 224
solve_time_s: 62
verified: true
draft: false
---

[CF 224B - Mảng](https://codeforces.com/problemset/problem/224/B) 

**Đánh giá:** 1500 
**Thẻ:** bitmask, triển khai, hai con trỏ 
**Thời gian giải:** 1m 2s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một mảng các số nguyên và một số _k_. Nhiệm vụ là tìm một phân đoạn hoặc phân đoạn liền kề sao cho nó chứa chính xác _k_ số nguyên riêng biệt. Trong số tất cả các phân đoạn thỏa mãn điều kiện này, chúng tôi được yêu cầu tìm một phân đoạn **tối thiểu bằng cách bao gồm**, nghĩa là chúng tôi không thể xóa các phần tử khỏi đầu hoặc cuối phân đoạn mà không vi phạm yêu cầu có chính xác _k_ số nguyên riêng biệt. Nếu không có phân đoạn như vậy tồn tại, chúng ta nên quay lại`-1 -1`. 

Mảng có thể có tối đa 10^5 phần tử và mỗi giá trị phần tử cũng có thể lớn tới 10^5. Vì _n_ là 10^5 và giới hạn thời gian là 2 giây, nên mọi giải pháp kiểm tra tất cả các mảng con có thể có một cách rõ ràng sẽ mất thời gian O(n^2), quá chậm. Điều này buộc chúng ta phải xem xét các phương pháp tiếp cận tuyến tính hoặc gần tuyến tính. Các trường hợp cạnh bao gồm các mảng có tất cả các phần tử giống hệt nhau, các mảng trong đó _k_ lớn hơn số phần tử riêng biệt và các mảng trong đó phân đoạn tối thiểu xuất hiện ở các biên. 

Ví dụ, nếu mảng là`[1, 2, 2, 3]`và _k_ = 2, đoạn`[1, 2]`là hợp lệ và tối thiểu bởi vì`[1, 2]`chứa chính xác hai số phân biệt và bất kỳ phân đoạn nào ngắn hơn sẽ có ít hơn 2 số phân biệt. Nếu _k_ = 4 thì đầu ra sẽ là`-1 -1`vì mảng chỉ chứa 3 số phân biệt. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ xem xét mọi phân khúc có thể`[l, r]`trong mảng, đếm các phần tử riêng biệt trong phân đoạn đó và kiểm tra xem nó có bằng _k_ hay không. Việc đếm các phần tử riêng biệt có thể được thực hiện bằng một tập hợp. Cách tiếp cận này đúng về nguyên tắc, nhưng việc kiểm tra tất cả các phân đoạn O(n^2) và tính toán tập hợp các phần tử riêng biệt trong mỗi phân đoạn có thể mất tới O(n^3) trong trường hợp xấu nhất nếu chúng ta xây dựng lại tập hợp mỗi lần. Ngay cả khi chúng tôi sử dụng nhóm đang chạy, tổng công việc vẫn là O(n^2) cho tất cả các phân đoạn, quá chậm đối với n = 10^5. 

Quan sát quan trọng là chúng ta có thể duy trì **cửa sổ trượt** tăng hoặc giảm trong khi theo dõi số lượng phần tử riêng biệt. Nếu chúng ta sử dụng từ điển tần số để theo dõi số lần mỗi số xuất hiện trong cửa sổ hiện tại, chúng ta có thể thêm các phần tử ở bên phải và xóa các phần tử ở bên trái một cách hiệu quả, cập nhật số lượng các số riêng biệt trong O(1) cho mỗi thao tác. Điều này cho phép chúng tôi tìm thấy một phân đoạn hợp lệ trong thời gian **O(n)**, vì mỗi phần tử được thêm và xóa nhiều nhất một lần. 

Thông tin chi tiết giúp cửa sổ trượt có thể thực hiện được là vấn đề đếm các phần tử riêng biệt trên các mảng con có **thuộc tính đơn điệu**: việc mở rộng cửa sổ sang bên phải chỉ có thể tăng số lượng phần tử riêng biệt và thu nhỏ cửa sổ từ bên trái chỉ có thể làm giảm số lượng phần tử riêng biệt. Thuộc tính này đảm bảo rằng chúng ta có thể di chuyển hai con trỏ mà không bỏ lỡ bất kỳ phân đoạn bao gồm tối thiểu nào. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^2) | O(n) | Quá chậm | 
| Cửa sổ trượt / Hai con trỏ | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo hai con trỏ`l`Và`r`ở mức 0, đại diện cho phân khúc hiện tại`[l, r)`. Khởi tạo một từ điển`count`để theo dõi tần số của từng số trong phân đoạn hiện tại và một biến`distinct`để theo dõi số lượng các phần tử riêng biệt. 
2. Di chuyển con trỏ sang phải`r`chuyển tiếp trong một vòng lặp cho đến khi số phần tử riêng biệt trong`[l, r]`đạt hoặc vượt _k_. Đối với mỗi phần tử`a[r]`, tăng tần số của nó lên`count`. Nếu phần tử này xuất hiện lần đầu tiên, hãy tăng`distinct`. 
3. Một lần`distinct`bằng _k_, kiểm tra xem đoạn này`[l, r]`là tối thiểu bằng cách đưa vào. Trong khi phần tử ngoài cùng bên trái`a[l]`có tần số lớn hơn 1, hãy giảm tần số và di chuyển`l`bên phải. Điều này đảm bảo không còn yếu tố không cần thiết nào ở đầu phân đoạn. 
4. Nếu`distinct`bằng *k` sau khi thu nhỏ, hãy ghi lại phân đoạn này dưới dạng câu trả lời hợp lệ. Chúng tôi có thể quay lại ngay lập tức vì bất kỳ phân đoạn hợp lệ nào được tìm thấy sau đó sẽ không nhỏ hơn khi đưa vào; chúng tôi chỉ cần **một** phân khúc theo từng mức tối thiểu. 
5. Nếu vòng lặp kết thúc và`distinct`không bao giờ đạt _k_, đầu ra`-1 -1`. 

### Tại sao nó hoạt động 

Điều bất biến là cửa sổ trượt`[l, r)`luôn chứa tối đa _k_ số khác nhau. Di chuyển con trỏ sang phải sẽ mở rộng cửa sổ, có khả năng tăng`distinct`. Thu nhỏ từ bên trái đảm bảo bao gồm tối thiểu. Mỗi phần tử vào và ra khỏi cửa sổ nhiều nhất một lần, đảm bảo độ phức tạp O(n) và từ điển tần số đảm bảo chúng tôi duy trì số lượng khác biệt chính xác một cách hiệu quả. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def find_segment(n, k, a):
    count = {}
    distinct = 0
    l = 0
    for r in range(n):
        if a[r] not in count:
            count[a[r]] = 0
        count[a[r]] += 1
        if count[a[r]] == 1:
            distinct += 1
        
        while distinct > k:
            count[a[l]] -= 1
            if count[a[l]] == 0:
                distinct -= 1
            l += 1
        
        if distinct == k:
            # shrink from left to make it minimal by inclusion
            while count[a[l]] > 1:
                count[a[l]] -= 1
                l += 1
            return l + 1, r + 1  # converting to 1-based index
    return -1, -1

def main():
    n, k = map(int, input().split())
    a = list(map(int, input().split()))
    l, r = find_segment(n, k, a)
    print(l, r)

if __name__ == "__main__":
    main()
```Mã duy trì một từ điển tần số`count`để theo dõi các phần tử trong cửa sổ hiện tại. Mỗi lần thêm hoặc bớt sẽ cập nhật số lượng riêng biệt. Việc thu nhỏ con trỏ bên trái đảm bảo phân đoạn được bao gồm ở mức tối thiểu. Một điểm tinh tế là chuyển đổi các chỉ số thành đầu ra dựa trên 1, đó là lý do tại sao`l + 1`Và`r + 1`được trả lại. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:`[1, 2, 2, 3]`, k = 2 

| tôi | r | khác biệt | đếm | hành động | 
| --- | --- | --- | --- | --- | 
| 0 | 0 | 1 | {1:1} | thêm 1 | 
| 0 | 1 | 2 | {1:1,2:1} | cộng 2, khác biệt == k | 
| 0 | 1 | 2 | {1:1,2:1} | co lại bên trái? 1 đếm=1 | 
| 0 | 1 | 2 | {1:1,2:1} | tìm thấy phân đoạn tối thiểu | 

Đầu ra:`1 2`Dấu vết này xác nhận rằng thuật toán tìm chính xác phân đoạn tối thiểu theo bao gồm bắt đầu từ chỉ mục 1 và kết thúc ở chỉ mục 2. 

### Mẫu 2 

đầu vào:`[1, 1, 1, 1]`, k = 2 

| tôi | r | khác biệt | đếm | hành động | 
| --- | --- | --- | --- | --- | 
| 0 | 0 | 1 | {1:1} | thêm 1 | 
| 0 | 1 | 1 | {1:2} | thêm 1 | 
| 0 | 2 | 1 | {1:3} | thêm 1 | 
| 0 | 3 | 1 | {1:4} | thêm 1 | 

khác biệt không bao giờ đạt đến k. Đầu ra:`-1 -1`. 

Điều này thể hiện việc xử lý trường hợp có ít phần tử khác biệt hơn k. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi phần tử được thêm và xóa nhiều nhất một lần khỏi cửa sổ, tổng số thao tác O(n) | 
| Không gian | O(n) | Từ điển count có thể chứa tối đa n phần tử riêng biệt trong trường hợp xấu nhất | 

Giải pháp vừa vặn thoải mái trong giới hạn 2 giây và bộ nhớ 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline
    
    def find_segment(n, k, a):
        count = {}
        distinct = 0
        l = 0
        for r in range(n):
            if a[r] not in count:
                count[a[r]] = 0
            count[a[r]] += 1
            if count[a[r]] == 1:
                distinct += 1
            while distinct > k:
                count[a[l]] -= 1
                if count[a[l]] == 0:
                    distinct -= 1
                l += 1
            if distinct == k:
                while count[a[l]] > 1:
                    count[a[l]] -= 1
                    l += 1
                return f"{l+1} {r+1}"
        return "-1 -1"
    
    n, k = map(int, input().split())
    a = list(map(int, input().split()))
    return find_segment(n, k, a)

# provided samples
assert run("4 2\n
```
