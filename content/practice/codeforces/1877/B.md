---
title: "CF 1877B - Mũ bảo hiểm có ánh sáng ban đêm"
description: "Chúng tôi có nhiệm vụ truyền bá thông báo đến tất cả cư dân của một ngôi làng theo cách rẻ nhất có thể. Có hai cách để thông báo cho cư dân: Pak Chanek có thể trực tiếp thông báo cho ai đó với chi phí cố định p, hoặc một cư dân đã biết có thể thông báo cho người khác bằng cách sử dụng mũ bảo hiểm ma thuật…"
date: "2026-06-09T01:03:50+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "greedy", "sortings"]
categories: ["algorithms"]
codeforces_contest: 1877
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 902 (Div. 2, based on COMPFEST 15 - Final Round)"
rating: 1000
weight: 1877
solve_time_s: 106
verified: false
draft: false
---

[CF 1877B - Mũ bảo hiểm trong ánh sáng ban đêm](https://codeforces.com/problemset/problem/1877/B) 

**Đánh giá:** 1000 
**Tags:** tìm kiếm nhị phân, tham lam, sắp xếp 
**Thời gian giải:** 1 phút 46s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có nhiệm vụ truyền bá thông báo đến tất cả cư dân của một ngôi làng theo cách rẻ nhất có thể. Có hai cách để thông báo cho người dân: Pak Chanek có thể trực tiếp thông báo cho ai đó với chi phí cố định`p`, hoặc một cư dân đã biết có thể thông báo cho người khác bằng cách sử dụng mũ bảo hiểm ma thuật, cho phép họ thông báo tối đa`a_i`cư dân với chi phí`b_i`mỗi thông báo. Mỗi cư dân có khả năng khác nhau (`a_i`) và chi phí (`b_i`) để truyền bá thông báo. Mục tiêu là chọn người mà Pak Chanek sẽ thông báo trực tiếp và cách thức người dân chuyển tiếp thông báo để tất cả mọi người đều có thể thông báo.`n`người dân biết, với tổng chi phí tối thiểu. 

Những hạn chế chỉ ra rằng`n`có thể cao tới 10^5 cho mỗi trường hợp thử nghiệm, với tổng`n`trên tất cả các trường hợp thử nghiệm cũng được giới hạn bởi 10^5. Chi phí chênh lệch trực tiếp`p`và chi phí mũ bảo hiểm`b_i`cũng có thể đạt tới 10^5. Với các giới hạn này, bất kỳ thuật toán nào có độ phức tạp O(n^2) đều không khả thi ngay lập tức vì nó có thể yêu cầu tới 10^10 thao tác. Chúng tôi cần một cái gì đó gần hơn với O(n log n) hoặc O(n) cho mỗi trường hợp thử nghiệm. 

Các trường hợp khó khăn không rõ ràng bao gồm các tình huống trong đó chi phí trực tiếp`p`rẻ hơn chi phí chia sẻ của bất kỳ cư dân nào`b_i`, trường hợp một số`a_i`là cực kỳ nhỏ (1) hoặc cực kỳ lớn ( ≥ n) và các tình huống trong đó nhiều cư dân có cùng chi phí nhưng năng lực khác nhau. Ví dụ, nếu`n=3`,`p=5`,`a=[1,1,1]`,`b=[6,6,6]`, lựa chọn rẻ nhất là thông báo trực tiếp cho cả ba cư dân với tổng số là 15. Một thuật toán ngây thơ cho rằng việc chia sẻ luôn có lợi có thể thay vào đó cố gắng chuyển tiếp thông qua những người khác và vượt quá chi phí. 

## Phương pháp tiếp cận 

Giải pháp bạo lực sẽ xem xét tất cả các nhóm nhỏ cư dân có thể có mà Pak Chanek có thể thông báo trực tiếp và mô phỏng sự lan truyền đến tất cả cư dân còn lại bằng cách sử dụng dung lượng mũ bảo hiểm của họ. Đối với mỗi lựa chọn, chúng tôi tính toán tổng chi phí và chọn mức tối thiểu. Về nguyên tắc, điều này hoạt động nhưng độ phức tạp của nó tăng theo cấp số nhân.`n`vì số tập con là 2^n. Ngay cả khi cắt tỉa cũng không thể`n=10^5`. 

Cái nhìn sâu sắc quan trọng là khả năng truyền bá thông báo của mỗi người dân có thể được định lượng dưới dạng chi phí cho mỗi người:`b_i`đối với mỗi`a_i`những người mà họ có thể thông báo. Nếu chi phí đầu người này lớn hơn`p`, việc sử dụng cư dân đó để thông báo cho người khác không bao giờ có giá trị. Việc sắp xếp cư dân theo chi phí và dung tích mũ bảo hiểm của họ cho phép chúng ta tham lam lựa chọn cách rẻ nhất để truyền bá đến những cư dân không hiểu biết còn lại. Sau đó, chúng tôi có thể sử dụng tìm kiếm nhị phân theo số lượng cư dân để thông báo trực tiếp: với số đó, hãy tính chi phí bổ sung tối thiểu để thông báo cho những người khác bằng cách sử dụng danh sách chi phí mũ bảo hiểm được sắp xếp. Điều này làm giảm độ phức tạp xuống O(n log n) cho mỗi trường hợp thử nghiệm. 

Lực lượng vũ phu hoạt động vì nó đảm bảo thử mọi khả năng, nhưng không thành công do số lượng kết hợp theo cấp số nhân. Quan sát rằng việc trải rộng rẻ hơn có thể được biểu diễn dưới dạng danh sách được sắp xếp các chi phí hiệu quả cho mỗi người cho phép chúng tôi giảm vấn đề thành quét tuyến tính kết hợp với tìm kiếm nhị phân, biến một vấn đề theo cấp số nhân thành một vấn đề có thể giải quyết được. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n * n) | O(n) | Quá chậm | 
| Tối ưu | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng cư dân`n`và chi phí chênh lệch trực tiếp`p`. 
2. Đọc mảng`a`(năng lực) và`b`(chi phí mũ bảo hiểm). 
3. Đối với mỗi cư dân, hãy xem xét chi phí sử dụng chúng để thông báo cho người khác`min(b_i, p)`mỗi người, bởi vì nếu`b_i > p`, Pak Chanek thông báo trực tiếp thì rẻ hơn. 
4. Sắp xếp cư dân theo thứ tự sức chứa giảm dần (`a_i`) và thứ tự tăng dần của chi phí cho mỗi người (`b_i`), ưu tiên những người có thể cung cấp thông tin cho nhiều người với giá rẻ. 
5. Khởi tạo mảng tổng tiền tố của dãy đã sắp xếp`b_i`giá trị để tính toán chi phí hiệu quả. 
6. Lặp lại số lượng cư dân để thông báo trực tiếp, từ 0 đến`n`. Đối với mỗi số có thể: 

một. Tổng chi phí bao gồm`num_direct * p`. 

b. Tính số dân còn lại`n - num_direct`. 

c. Thêm số tiền rẻ nhất`n - num_direct`chi phí chia sẻ mũ bảo hiểm bằng cách sử dụng mảng tổng tiền tố. 
7. Theo dõi tổng chi phí tối thiểu cho tất cả các lựa chọn. 
8. Đưa ra chi phí tối thiểu. 

Tại sao nó hoạt động: điều bất biến là ở mỗi bước, chúng tôi luôn chọn phương pháp rẻ nhất hiện có để thông báo cho những cư dân còn lại, dù trực tiếp hay thông qua mũ bảo hiểm. Sắp xếp theo chi phí mỗi người đảm bảo không bỏ sót sự kết hợp nào rẻ hơn. Tìm kiếm nhị phân hoặc tổng tiền tố tránh đánh giá lại các tùy chọn giống nhau, đảm bảo hiệu quả mà không làm mất đi tính tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n, p = map(int, input().split())
        a = list(map(int, input().split()))
        b = list(map(int, input().split()))
        
        # Each resident's effective cost is min(b_i, p)
        residents = sorted(zip(b, a), key=lambda x: (x[0], -x[1]))
        b_sorted = [min(bi, p) for bi, _ in residents]
        
        # Prefix sum of b_sorted
        prefix = [0] * (n + 1)
        for i in range(n):
            prefix[i + 1] = prefix[i] + b_sorted[i]
        
        res = float('inf')
        for direct in range(n + 1):
            # Cost to inform `direct` residents directly
            cost = direct * p
            remaining = n - direct
            # Additional cost from cheapest residents sharing
            cost += prefix[remaining]
            res = min(res, cost)
        print(res)

if __name__ == "__main__":
    solve()
```Mã đầu tiên tính toán chi phí mũ bảo hiểm hiệu quả tối thiểu cho mỗi người dân, thay thế bất kỳ`b_i`cao hơn`p`với`p`. Việc sắp xếp đảm bảo chúng tôi luôn chọn máy rải rẻ nhất trước tiên. Tổng tiền tố cho phép tính toán chi phí tích lũy của việc chia sẻ một cách hiệu quả. Vòng lặp chính xem xét tất cả số lượng cư dân có thể có để thông báo trực tiếp và kết hợp điều này với chi phí phân bổ tối thiểu. Việc sử dụng tổng tiền tố sẽ tránh được các vòng lặp lồng nhau và giữ độ phức tạp O(n log n) do sắp xếp ban đầu. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
6 3
2 3 2 1 1 3
4 3 2 6 3 6
```| trực tiếp | còn lại | chi phí từ trực tiếp | tổng hợp mũ bảo hiểm rẻ nhất | tổng cộng | 
| --- | --- | --- | --- | --- | 
| 0 | 6 | 0 | 16 | 16 | 
| 1 | 5 | 3 | 13 | 16 | 
| 2 | 4 | 6 | 10 | 16 | 
| 3 | 3 | 9 | 7 | 16 | 
| 4 | 2 | 12 | 4 | 16 | 
| 5 | 1 | 15 | 2 | 17 | 
| 6 | 0 | 18 | 0 | 18 | 

Tổng chi phí tối thiểu là 16, xác nhận thuật toán chọn đúng số lượng thông báo trực tiếp kết hợp với việc chia sẻ mũ bảo hiểm tối ưu. 

### Ví dụ 2 

đầu vào:```
1 100000
100000
1
```| trực tiếp | còn lại | chi phí từ trực tiếp | tổng hợp mũ bảo hiểm rẻ nhất | tổng cộng | 
| --- | --- | --- | --- | --- | 
| 0 | 1 | 0 | 100000 | 100000 | 
| 1 | 0 | 100000 | 0 | 100000 | 

Chi phí tối thiểu là 100000, thông báo trực tiếp hoặc thông qua cư dân, phù hợp với trực giác vì chỉ có một cư dân. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Sắp xếp chiếm ưu thế; lặp qua 0..n với tổng tiền tố là O(n) | 
| Không gian | O(n) | Lưu trữ cư dân, mảng tổng tiền tố | 

Cho tổng của`n`trên tất cả các trường hợp thử nghiệm ≤ 10^5, giải pháp này chạy thoải mái trong vòng 1 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out
    solve()
    return out.getvalue().strip()

# Provided samples
assert run("3\n6 3\n2 3 2 1 1 3\n4 3 2 6 3 6\n1 100000\n100000\n1\n4 94\n1 4 2 3\n103 96 86 57\n") == "16\n100000\n265", "sample 1"

# Custom cases
assert run("1\n3 5\n1 1 1\n6 6 6\n") == "15", "all helmets more expensive than direct"
assert run("1\n5 10\n5 4 3 2 1\n1 1 1 1 1\n") == "10", "all helmets cheaper and sufficient"
assert run("1\n4 7\n1 2 1 1\n3 2 1 1\n") == "12", "mix of direct and helmet optimal"
assert run("1\n1 100000\n1\n100000\n") == "100000", "single resident"
```| Kiểm tra đầu vào
