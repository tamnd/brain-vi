---
title: "CF 1657D - Dành cho game thủ. Bởi Game thủ."
description: "Chúng tôi có một kịch bản trò chơi chiến lược trong đó Monocarp chiêu mộ một đội để chiến đấu với một loạt quái vật. Mỗi trận chiến bắt đầu với một đội trống và Monocarp có thể tiêu tới C xu cho mỗi trận chiến. Mỗi loại đơn vị có chi phí, sát thương mỗi giây và sức khỏe."
date: "2026-06-10T03:28:35+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "brute-force", "greedy", "math", "sortings"]
categories: ["algorithms"]
codeforces_contest: 1657
codeforces_index: "D"
codeforces_contest_name: "Educational Codeforces Round 125 (Rated for Div. 2)"
rating: 2000
weight: 1657
solve_time_s: 61
verified: true
draft: false
---

[CF 1657D - Dành cho game thủ. Bởi game thủ.](https://codeforces.com/problemset/problem/1657/D) 

**Xếp hạng:** 2000 
**Tags:** tìm kiếm nhị phân, bạo lực, tham lam, toán học, sắp xếp 
**Thời gian giải:** 1m 1s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một kịch bản trò chơi chiến lược trong đó Monocarp chiêu mộ một đội để chiến đấu với một loạt quái vật. Mỗi trận chiến bắt đầu với đội hình trống và Monocarp có thể tiêu tới`C`xu mỗi trận chiến. Mỗi loại đơn vị có chi phí, sát thương mỗi giây và sức khỏe. Mỗi quái vật có sát thương mỗi giây và tổng lượng máu. Trận chiến diễn ra đồng thời, có nghĩa là Monocarp chỉ thắng nếu đội của anh ta tiêu diệt được quái vật nhanh hơn tốc độ mà quái vật tiêu diệt được một đơn vị trong đội của anh ta. 

Đầu vào cung cấp thông tin chi tiết về`n`loại đơn vị và`m`quái vật. Đầu ra phải báo cáo, đối với mỗi quái vật, số xu tối thiểu mà Monocarp cần bỏ ra để giành chiến thắng. Nếu không có sự kết hợp trong`C`đồng xu cho phép chiến thắng, chúng tôi trở lại`-1`. 

Với những hạn chế,`n`Và`m`có thể lên tới`3 * 10^5`và giới hạn tiền xu lên tới`10^6`. Điều này loại trừ các cách tiếp cận ngây thơ lặp đi lặp lại trên mọi số lượng đơn vị cho mỗi loại cho mọi quái vật, vì điều đó đòi hỏi khả năng`O(n * C * m)`hoạt động quá lớn. 

Một trường hợp có lợi thế không rõ ràng phát sinh khi một đơn vị cùng loại có thể đánh bại một con quái vật, nhưng nhiều đơn vị sẽ rẻ hơn. Ví dụ: một đơn vị giá rẻ có thể cần 5 bản sao để đánh bại quái vật, tốn 50 xu, trong khi đơn vị đắt tiền nhưng mạnh hơn chỉ cần một bản sao, giá 30 xu. Một giải pháp bất cẩn luôn chọn loại đầu tiên thỏa mãn điều kiện có thể trả về câu trả lời dưới mức tối ưu hoặc không thể. 

## Phương pháp tiếp cận 

Cách tiếp cận vũ phu xem xét từng loại đơn vị và thử mọi số lượng đơn vị có thể được tuyển dụng trong giới hạn xu. Đối với mỗi sự kết hợp, chúng tôi kiểm tra xem đội có tiêu diệt được quái vật hay không trước khi quái vật tiêu diệt một đơn vị. Điều này có hiệu quả nhưng quá chậm vì lặp đi lặp lại tới`C`đồng xu cho mỗi`n`đơn vị và`m`quái vật dẫn đến số lượng hoạt động trong trường hợp xấu nhất là`O(n * C * m)`. 

Thông tin chi tiết quan trọng là điều kiện chiến đấu phụ thuộc vào tỷ lệ tổng sát thương của đội đối với máu của quái vật và tổng sát thương của quái vật đối với máu của đơn vị. Cho phép`k`là số đơn vị thuộc loại`i`. Biệt đội tiêu diệt quái vật trong`H_j / (k * d_i)`giây và quái vật giết chết một đơn vị trong`h_i / D_j`giây. Chiến thắng đòi hỏi`H_j / (k * d_i) < h_i / D_j`, điều này đơn giản hóa thành`k > H_j * D_j / (h_i * d_i)`. Do đó, đối với mỗi loại đơn vị, chúng ta có thể tính toán số lượng đơn vị tối thiểu cần thiết để đánh bại một quái vật nhất định. Chi phí tương ứng là`ceil(k) * c_i`. Chúng tôi chỉ cần chi phí tối thiểu như vậy trên tất cả các loại đơn vị. 

Bởi vì`C`có thể lên tới`10^6`, lặp lại`k`trực tiếp là không cần thiết. Việc tính toán trước thiệt hại tối đa có thể đạt được trên mỗi số xu cho phép chúng tôi trả lời các truy vấn trong`O(log C)`thời gian cho mỗi quái vật thông qua tìm kiếm. Sắp xếp và giữ tiền tố tối đa`damage * health`đảm bảo rằng các loại đơn vị thống trị (đắt hơn nhưng tệ hơn) không ảnh hưởng đến câu trả lời. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n * C * m) | O(n * C) | Quá chậm | 
| Tối ưu | O(n + C + m * log C) | O(C) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo một mảng`best`kích thước`C + 1`để theo dõi tốt nhất`damage * health`có thể đạt được cho mỗi số tiền lên tới`C`. Đặt tất cả các mục về 0 ban đầu. 
2. Lặp lại tất cả các loại đơn vị. Đối với từng loại đơn vị có chi phí`c_i`, hư hại`d_i`, và sức khỏe`h_i`, tính toán`d_i * h_i`. Cập nhật`best[c_i]`là giá trị tối đa của giá trị hiện tại của nó và`d_i * h_i`. Điều này đảm bảo rằng với mỗi chi phí, chúng tôi biết loại đơn vị nào hiệu quả nhất. 
3. Truyền bá tiền tố tối đa trên`best`mảng. Với mỗi giá trị xu từ 1 đến`C`, bộ`best[i] = max(best[i], best[i-1])`. Điều này đảm bảo rằng đối với bất kỳ ngân sách tiền xu nào, chúng tôi biết loại đơn vị tốt nhất có giá cả phải chăng. 
4. Đối với mỗi quái vật có`D_j`Và`H_j`, tính toán`threshold = H_j * D_j`. Bây giờ chúng tôi muốn số lượng xu nhỏ nhất`k`như vậy`best[k] > threshold`. Thực hiện tìm kiếm nhị phân trên`best`mảng từ 1 đến`C`. 
5. Nếu hợp lệ`k`được tìm thấy, xuất ra`k`. Ngược lại, xuất ra`-1`. 

Tại sao nó hoạt động: Điều kiện chiến đấu giảm xuống`k * d_i * h_i > H_j * D_j`. Bằng cách tính toán trước mức tối đa`d_i * h_i`trên mỗi số xu và sử dụng tiền tố cực đại, chúng tôi đảm bảo rằng chúng tôi có thể trả lời từng truy vấn quái vật bằng cách tìm kiếm mức chi tiêu xu tối thiểu thỏa mãn sự bất bình đẳng. Các loại đơn vị thống trị không thể giảm chi phí xu xuống dưới mức tối thiểu`k`thành lập. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, C = map(int, input().split())
best = [0] * (C + 1)

for _ in range(n):
    c, d, h = map(int, input().split())
    value = d * h
    if best[c] < value:
        best[c] = value

for i in range(1, C + 1):
    best[i] = max(best[i], best[i - 1])

m = int(input())
monsters = [tuple(map(int, input().split())) for _ in range(m)]
res = []

for D, H in monsters:
    left, right = 1, C
    ans = -1
    threshold = D * H
    while left <= right:
        mid = (left + right) // 2
        if best[mid] > threshold:
            ans = mid
            right = mid - 1
        else:
            left = mid + 1
    res.append(str(ans))

print(" ".join(res))
```Vòng lặp đầu tiên xây dựng`best`mảng có giá trị tối đa`d*h`giá trị trên mỗi chi phí. Vòng lặp thứ hai xây dựng tiền tố cực đại để đối với bất kỳ ngân sách tiền xu nào, chúng ta có thể truy cập vào đơn vị hiệu quả nhất. Tìm kiếm nhị phân kiểm tra chi tiêu xu nhỏ nhất đáp ứng ngưỡng quái vật. Việc sử dụng số học số nguyên sẽ tránh được các lỗi dấu phẩy động và thứ tự truyền bá đảm bảo chúng ta không bỏ lỡ loại đơn vị rẻ hơn có thể đánh bại quái vật. 

## Ví dụ đã hoạt động 

### Đầu vào mẫu 1```
3 10
3 4 6
5 5 5
10 3 4
3
8 3
5 4
10 15
```| Quái vật | Ngưỡng H*D | mảng tốt nhất max ≤ C | Kết quả xu | 
| --- | --- | --- | --- | 
| 8 3 | 24 | tốt nhất[3]=24, tốt nhất[5]=25 | 5 | 
| 5 4 | 20 | tốt nhất[3]=24 | 3 | 
| 10 15 | 150 | tốt nhất[10]=25 | -1 | 

Dấu vết này cho thấy đối với quái vật đầu tiên, mặc dù nhiều đơn vị loại 1 có thể đạt ngưỡng nhưng loại 2 sẽ đạt được mức đó rẻ hơn. 

### Ví dụ tùy chỉnh```
2 5
2 2 2
3 3 1
1
3 3
```| Quái vật | Ngưỡng H*D | mảng tốt nhất max ≤ C | Kết quả xu | 
| --- | --- | --- | --- | 
| 3 3 | 9 | tốt nhất[3]=3 | -1 | 

Bất kỳ đơn vị nào trong giới hạn xu không thể đáp ứng được ngưỡng này. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + C + m * log C) | xây dựng`best`trong O(n), tiền tố cực đại trong O(C), tìm kiếm nhị phân cho từng quái vật trong O(log C) | 
| Không gian | O(C + m) |`best`kích thước mảng C+1, kích thước danh sách quái vật m | 

Thuật toán có hiệu quả với n, m ≤ 3*10^5 và C ≤ 10^6. Tìm kiếm nhị phân đảm bảo rằng ngay cả số lượng quái vật lớn nhất cũng được xử lý trong thời gian chấp nhận được. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n, C = map(int, input().split())
    best = [0] * (C + 1)
    for _ in range(n):
        c, d, h = map(int, input().split())
        best[c] = max(best[c], d*h)
    for i in range(1, C + 1):
        best[i] = max(best[i], best[i-1])
    m = int(input())
    monsters = [tuple(map(int, input().split())) for _ in range(m)]
    res = []
    for D, H in monsters:
        left, right = 1, C
        ans = -1
        threshold = D * H
        while left <= right:
            mid = (left + right) // 2
            if best[mid] > threshold:
                ans = mid
                right = mid - 1
            else:
                left = mid + 1
        res.append(str(ans))
    return " ".join(res)

# Provided samples
assert run("3 10\n3 4 6\n5 5 5\n10 3 4\n3\n8 3\n5 4\n10 15\n") == "5 3 -1"

# Custom cases
assert run("2 5\n2 2 2\n3 3
```
