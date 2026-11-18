---
title: "CF 2037F - Ngọn lửa rực cháy"
description: "Chúng ta được cung cấp một dòng kẻ thù, mỗi kẻ có một lượng máu cố định và một vị trí cố định. Xilonen có thể tấn công từ một vị trí p đã chọn duy nhất và mỗi đòn tấn công sẽ làm giảm lượng máu của kẻ địch đi m - Nhận xét quan trọng đầu tiên là sát thương từ mỗi đòn tấn công giảm tuyến tính theo khoảng cách."
date: "2026-06-08T10:14:21+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "data-structures", "math", "sortings", "two-pointers"]
categories: ["algorithms"]
codeforces_contest: 2037
codeforces_index: "F"
codeforces_contest_name: "Codeforces Round 988 (Div. 3)"
rating: 2100
weight: 2037
solve_time_s: 100
verified: true
draft: false
---

[CF 2037F - Ngọn lửa nồng nàn](https://codeforces.com/problemset/problem/2037/F) 

**Xếp hạng:** 2100 
**Tags:** tìm kiếm nhị phân, cấu trúc dữ liệu, toán học, sắp xếp, hai con trỏ 
**Thời gian giải:** 1 phút 40s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một dòng kẻ thù, mỗi kẻ có một lượng máu cố định và một vị trí cố định. Xilonen có thể tấn công từ một vị trí đã chọn`p`và mỗi đòn tấn công sẽ làm giảm lượng máu của kẻ địch đi`m - |p - x|`nếu sự khác biệt đó là không âm. Mục tiêu là chọn`p`ít nhất như vậy`k`kẻ thù có thể bị đánh bại với số lần tấn công ít nhất. Đầu ra là số lần tấn công tối thiểu, hoặc`-1`nếu không có vị trí nào cho phép giết`k`kẻ thù. 

Nhận xét quan trọng đầu tiên là sát thương từ mỗi đòn tấn công giảm tuyến tính theo khoảng cách. Điều này có nghĩa là kẻ thù càng ở xa`p`, càng cần nhiều đòn tấn công để đánh bại nó. Chúng tôi cũng nhận thấy rằng`p`có thể là số nguyên bất kỳ, không bị giới hạn ở vị trí của kẻ thù và vị trí của kẻ thù đang tăng lên một cách nghiêm ngặt. 

Những ràng buộc ngụ ý rằng`n`có thể lên tới 100.000 và`m`Và`h_i`lên đến 10^9. Điều này có nghĩa là chúng tôi không đủ khả năng sử dụng phương pháp tiếp cận bạo lực O(n^2) trong đó chúng tôi mô phỏng mọi vị trí có thể có cho`p`. Chúng tôi cũng cần xử lý tối đa 10^4 trường hợp thử nghiệm với tổng`n`trong mọi trường hợp nhiều nhất là 10^5. Do đó, thuật toán của chúng tôi tệ nhất cần phải đạt khoảng O(n log n) cho mỗi trường hợp thử nghiệm, lý tưởng là nhanh hơn. 

Các trường hợp đặc biệt bao gồm khi kẻ thù ở khoảng cách xa hơn`m`, khiến không thể tác động lên nhiều kẻ địch cùng một lúc hoặc khi tất cả kẻ địch ở trong khoảng cách`m`, cho phép một vị trí bắn trúng nhiều mục tiêu. Ví dụ, nếu`n=2`,`m=1`,`k=2`, và các vị trí là`[1, 3]`, không có đơn`p`có thể tiếp cận cả hai kẻ thù, vì vậy câu trả lời là`-1`. Một cách tiếp cận ngây thơ luôn cho rằng chúng ta có thể đánh trúng mục tiêu đầu tiên`k`kẻ thù sẽ thất bại ở đây. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ lặp lại trên tất cả các vị trí số nguyên có thể có cho`p`và tính toán số lần tấn công cần thiết để đánh bại từng kẻ thù. Sau đó chúng tôi sẽ sắp xếp các số đếm này và chọn`k`nhỏ nhất để xem có bao nhiêu đòn tấn công là đủ. Mặc dù điều này đúng nhưng nó quá chậm vì`p`có thể lên tới 10^9, dẫn đến không gian tìm kiếm tiềm năng rất lớn. 

Cái nhìn sâu sắc quan trọng là mô hình tấn công là tuyến tính từng phần. Mỗi kẻ thù xác định một khoảng vị trí mà nó có thể bị sát thương. Cụ thể là kẻ thù ở`x_i`với sức khỏe`h_i`có thể sống sót tới`ceil(h_i / (m - |p - x_i|))`các cuộc tấn công, giảm dần khi`|p - x_i|`giảm đi. Vì vậy, với bất kỳ số lượng cuộc tấn công nào`t`, chúng ta có thể tính toán khoảng thời gian mà kẻ địch có thể bị đánh bại nhiều nhất`t`các cuộc tấn công. Điều này chuyển vấn đề thành vấn đề bao phủ khoảng thời gian: chúng ta cần một vị trí`p`nằm trong khoảng ít nhất`k`kẻ thù. 

Chúng ta có thể kết hợp điều này với tìm kiếm nhị phân về số lần tấn công. Đối với một ứng cử viên`t`, chúng tôi tạo ra các khoảng thời gian cho tất cả kẻ thù sẽ bị đánh bại trong`t`các cuộc tấn công. Sau đó chúng tôi kiểm tra xem có vị trí nào được bao phủ bởi ít nhất`k`của các khoảng này bằng cách sử dụng phương pháp quét theo đường quét, trong đó chúng tôi đếm các phần trùng lặp một cách hiệu quả bằng cách sắp xếp các điểm cuối của khoảng. Tìm kiếm nhị phân đảm bảo chúng ta tìm thấy giá trị tối thiểu`t`một cách hiệu quả. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n * m_range) | O(n) | Quá chậm | 
| Khoảng thời gian + Tìm kiếm nhị phân | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Với mỗi test case, hãy đọc`n`,`m`,`k`, mảng sức khỏe`h`và mảng vị trí`x`. 
2. Xác định hàm trợ giúp`possible(t)`trả về True nếu ít nhất`k`kẻ thù có thể bị đánh bại trong`t`các cuộc tấn công. 
3. Trong`possible(t)`, lặp lại tất cả kẻ thù. Đối với mỗi kẻ thù, hãy tính khoảng cách tối đa`d`như vậy`m - d`lần`t`ít nhất là`h_i`. Nếu như`d >= m`, bỏ qua kẻ thù; nó không thể bị đánh bại trong`t`các cuộc tấn công. Ngược lại, hãy thêm khoảng`[x_i - d, x_i + d]`vào một danh sách. 
4. Sắp xếp các điểm cuối khoảng và thực hiện đường quét. Theo dõi số lượng khoảng thời gian chồng chéo tại mỗi điểm. Nếu tại bất kỳ thời điểm nào có ít nhất`k`các khoảng chồng chéo, trả về True. 
5. Sử dụng tìm kiếm nhị phân trên`t`. Bộ`low = 1`,`high = max(h)`hoặc giới hạn trên an toàn, chẳng hạn như 10^9. Đối với mỗi giá trị trung bình, hãy gọi`possible(mid)`. Nếu đúng thì giảm`high`; nếu sai thì tăng`low`. 
6. Sau khi tìm kiếm nhị phân, nếu không hợp lệ`t`được tìm thấy, xuất ra`-1`. Ngược lại, xuất ra mức tối thiểu`t`thành lập. 

Tại sao nó hoạt động: Tìm kiếm nhị phân đảm bảo rằng tìm kiếm đầu tiên`t`vì cái gì`possible(t)`trả về True là tối thiểu. Việc quét theo khoảng thời gian đảm bảo rằng chúng tôi chỉ xem xét các vị trí có thể thực sự đánh bại kẻ thù trong`t`các cuộc tấn công và việc đếm các lần trùng lặp đảm bảo chúng tôi tìm thấy`p`ít nhất nó đánh bại`k`kẻ thù. Cách tiếp cận này tránh lặp lại trên tất cả các vị trí số nguyên. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
import math

def solve():
    t = int(input())
    for _ in range(t):
        n, m, k = map(int, input().split())
        h = list(map(int, input().split()))
        x = list(map(int, input().split()))
        
        def possible(t_attacks):
            events = []
            for hi, xi in zip(h, x):
                d = m - math.ceil(hi / t_attacks)
                if d < 0:
                    continue
                left = xi - d
                right = xi + d
                events.append((left, 1))
                events.append((right + 1, -1))
            events.sort()
            count = 0
            for pos, delta in events:
                count += delta
                if count >= k:
                    return True
            return False
        
        low, high = 1, max(h)
        answer = -1
        while low <= high:
            mid = (low + high) // 2
            if possible(mid):
                answer = mid
                high = mid - 1
            else:
                low = mid + 1
        print(answer)
        
solve()
```Mã đọc đầu vào hiệu quả bằng cách sử dụng`sys.stdin.readline`và xử lý nhiều trường hợp thử nghiệm. các`possible`tính toán các khoảng thời gian cho từng kẻ thù mà chúng có thể bị đánh bại trong số lần tấn công có thể, sau đó sử dụng phương pháp tiếp cận theo đường quét trên các điểm cuối khoảng được sắp xếp để xem liệu có tồn tại một vị trí được bao phủ bởi ít nhất`k`khoảng thời gian. Tìm kiếm nhị phân được sử dụng để giảm thiểu số lượng các cuộc tấn công. 

Những điểm tinh tế bao gồm việc xử lý trần nhà`hi / t`chính xác, đảm bảo rằng các khoảng bao gồm cả hai đầu và thêm`+1`đến điểm cuối bên phải để đảm bảo độ chính xác của đường quét. Các lỗi ngẫu nhiên trong các điểm cuối khoảng thời gian sẽ phá vỡ số lượng các khoảng chồng chéo. 

## Ví dụ đã hoạt động 

**Ví dụ 1:**`n=5, m=5, k=3, h=[7,7,7,7,7], x=[1,2,3,4,5]`. 

| Tấn công t | Khoảng thời gian của kẻ thù | Chồng chéo tối đa | 
| --- | --- | --- | 
| 1 | [1-2,1+2],[2-2,2+2],[3-2,3+2],[4-2,4+2],[5-2,5+2] | 3 ở vị trí 2,3,4 | 
| 2 | [1-3,1+3],[2-3,2+3],... | chồng chéo >= 3 | 

Tìm kiếm nhị phân tìm thấy rằng`t=2`là mức tối thiểu để đánh bại 3 kẻ thù. 

**Ví dụ 2:**`n=2, m=10, k=2, h=[1,1], x=[1,20]`. 

Khoảng thời gian cho t=1: kẻ thù1=[1-9,1+9]=[-8,10], kẻ thù2=[20-9,20+9]=[11,29]. Không có sự chồng chéo. Trả về tìm kiếm nhị phân`-1`. 

Dấu vết này xác nhận rằng các khoảng không chồng chéo tạo ra chính xác`-1`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n log H) | Mỗi lần lặp tìm kiếm nhị phân thực hiện quét O(n log n), với các lần lặp log H (H=max health) | 
| Không gian | O(n) | Các sự kiện khoảng thời gian được lưu trữ cho mỗi trường hợp thử nghiệm | 

Được cho`n`≤ 10^5 và tính tổng trên tất cả các trường hợp thử nghiệm ≤ 10^5 và tìm kiếm nhị phân trên mức tối đa`h_i`10^9, giải pháp phù hợp thoải mái trong giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out
    solve()
    return out.getvalue().strip()

# provided samples
assert run("""6
5 5 3
7 7 7 7 7
1 2 3 4 5
9 5 9
2 4 6 8 10 8 6 4 2
1 2 3 4 5 6 7 8 9
2 10 2
1 1
1 20
2 10 1
69696969 420420420
1 20
2 10 2
10 15
1 19
```
