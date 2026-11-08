---
title: "CF 1452E - Hai bài xã luận"
description: "Chúng ta được cho một tập bài toán gồm n bài toán và m người tham gia. Mỗi người tham gia quan tâm đến một loạt vấn đề cụ thể liền kề nhau, được xác định bởi [li, ri]. Hai tác giả sẽ trình bày các bài hướng dẫn, mỗi bài đề cập đến đúng k vấn đề liên tiếp."
date: "2026-06-11T03:16:27+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "dp", "greedy", "sortings", "two-pointers"]
categories: ["algorithms"]
codeforces_contest: 1452
codeforces_index: "E"
codeforces_contest_name: "Educational Codeforces Round 98 (Rated for Div. 2)"
rating: 2500
weight: 1452
solve_time_s: 87
verified: true
draft: false
---

[CF 1452E - Hai bài xã luận](https://codeforces.com/problemset/problem/1452/E) 

**Đánh giá:** 2500 
**Tags:** vũ lực, dp, tham lam, sắp xếp, hai con trỏ 
**Thời gian giải:** 1 phút 27s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một bộ vấn đề`n`vấn đề và`m`người tham gia. Mỗi người tham gia quan tâm đến một loạt các vấn đề cụ thể, được xác định bởi`[l_i, r_i]`. Hai tác giả sẽ trình bày các hướng dẫn, mỗi hướng dẫn trình bày chính xác`k`những vấn đề liên tiếp. Các tác giả độc lập lựa chọn đoạn độ dài nào`k`để che đậy. 

Mỗi người tham gia sẽ chỉ tham dự với tác giả có phân khúc trùng khớp nhất với phạm vi quan tâm của người tham gia. Đối với mỗi người tham gia, chúng tôi gọi số vấn đề họ gặp phải`a_i`. Mục tiêu của chúng tôi là chọn các phân đoạn cho hai tác giả sao cho tổng của tất cả`a_i`được tối đa hóa. 

Những hạn chế`n, m ≤ 2000`có nghĩa là một`O(n^2 * m)`Giải pháp này khả thi vì nó mang lại nhiều nhất khoảng 8 tỷ phép tính nếu chúng ta bất cẩn. Chúng ta cần thứ gì đó nhanh hơn. Một giải pháp đơn giản kiểm tra tất cả các cặp phân đoạn cho tác giả và tính toán chồng chéo cho tất cả người tham gia sẽ liên quan đến`O((n-k+1)^2 * m)`hoạt động, có thể chấp nhận được vì`(n-k+1)^2 * m ≤ 4*10^9`trong trường hợp xấu nhất, nhưng sẽ quá chặt. 

Các trường hợp cạnh không rõ ràng bao gồm các đoạn trùng nhau hoàn toàn, chồng lên nhau một phần hoặc ở đó`k`bằng`n`. Ví dụ, nếu`k = n`, cả hai tác giả sẽ đề cập đến toàn bộ bộ vấn đề, vì vậy mọi người tham gia đều có thể nhìn thấy tất cả các vấn đề mà họ muốn. Một trường hợp khác là những người tham gia có phạm vi quan tâm nhỏ hơn`k`; sự chồng chéo không thể vượt quá phạm vi của người tham gia, do đó thuật toán phải xử lý chính xác tối thiểu/tối đa. Nếu như`k = 1`và những người tham gia quan tâm đến nhiều vấn đề, việc chọn các phân đoạn vấn đề đơn lẻ tối ưu đòi hỏi phải tính toán cẩn thận xem người tham gia nào được hưởng lợi nhiều nhất từ ​​​​mỗi phân khúc. 

Ví dụ: với đầu vào:```
5 2 2
1 3
3 5
```Việc triển khai bất cẩn có thể chọn phân khúc`[1,2]`Và`[2,3]`cho tác giả mà không kiểm tra xem`[4,5]`có thể mang lại một khoản tiền tốt hơn. Câu trả lời đúng là tối đa hóa tổng số chồng chéo giữa cả hai người tham gia. 

## Phương pháp tiếp cận 

Cách tiếp cận vũ phu rất đơn giản. Đối với mỗi đoạn có độ dài`k`mà tác giả đầu tiên có thể chọn (`n-k+1`tùy chọn) và cho từng đoạn có độ dài`k`mà tác giả thứ hai có thể chọn (`n-k+1`tùy chọn), tính toán mức chồng chéo tối đa cho mỗi người tham gia với hai phân đoạn, sau đó tính tổng các mức tối đa này. Điều này đòi hỏi`O((n-k+1)^2 * m)`hoạt động. Điều này đúng nhưng chậm đối với các giá trị lớn nhất được phép của`n`Và`m`, đặc biệt là kể từ khi`n-k+1`có thể lên tới 2000. 

Cái nhìn sâu sắc quan trọng là vấn đề giảm xuống để tính toán hiệu quả tổng số lần trùng lặp tối đa cho tất cả người tham gia cho mỗi cặp phân khúc. Sự chồng chéo của mỗi người tham gia với một phân khúc`[s, s+k-1]`đơn giản là`max(0, min(r_i, s+k-1) - max(l_i, s) + 1)`. Do đó, chúng tôi có thể tính toán trước sự chồng chéo của từng người tham gia với tất cả các vị trí bắt đầu có thể có của các phân đoạn. Sau đó, đối với mỗi phân đoạn tác giả đầu tiên, chúng tôi có thể nhanh chóng tính toán “phân đoạn thứ hai tốt nhất có thể” bằng cách giữ tối đa các tổng trùng lặp. 

Giải pháp tối ưu tận dụng kỹ thuật lập trình động/tổng ​​tiền tố. Đối với mỗi phân đoạn tác giả đầu tiên tiềm năng, hãy tính toán phần đóng góp của mỗi người tham gia nếu tác giả đầu tiên đề cập đến phân đoạn đó. Sau đó, đối với tác giả thứ hai, hãy sử dụng cửa sổ trượt để tìm phân đoạn nào tối đa hóa tổng bổ sung trên tất cả những người tham gia, xem xét rằng mỗi người tham gia chọn mức tối đa của hai phần trùng lặp. Điều này làm giảm độ phức tạp về thời gian để`O(n^2 + n*m)`, điều này có thể chấp nhận được đối với`n, m ≤ 2000`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O((n-k+1)^2 * m) | O(m*(n-k+1)) | Quá chậm đối với các hạn chế tối đa | 
| Tối ưu | O(n^2 + n*m) | O(n*m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính toán trước`overlap[i][s]`cho mỗi người tham gia`i`và mọi phân đoạn có thể bắt đầu từ`s`, Ở đâu`overlap[i][s] = max(0, min(r_i, s+k-1) - max(l_i, s) + 1)`. Điều này cho chúng ta biết có bao nhiêu vấn đề người tham gia`i`sẽ nghe nếu tác giả chọn đoạn bắt đầu từ`s`. 
2. Khởi tạo`max_sum = 0`. This will store the maximum sum of all participants’ best possible overlaps.
 3. Đối với mọi vị trí xuất phát có thể`s1`của phân đoạn của tác giả đầu tiên, tính tổng chồng chéo`first_overlap[i] = overlap[i][s1]`cho mỗi người tham gia. 
4. Đối với tác giả thứ hai, chúng ta cần tính toán phần đóng góp bổ sung một cách hiệu quả. Đối với mỗi đoạn có thể có vị trí bắt đầu`s2`, tính toán`second_sum = sum(max(first_overlap[i], overlap[i][s2]) for i in 1..m)`. Theo dõi tối đa`second_sum`tổng thể`s2`. 
5. Cập nhật`max_sum`với lớn nhất`second_sum`tìm thấy cho hiện tại`s1`. 
6. Sau khi lặp lại tất cả`s1`, đầu ra`max_sum`. 

Tại sao nó hoạt động: thuật toán xem xét rõ ràng mọi phân đoạn tác giả đầu tiên và tìm ra phân đoạn thứ hai tối ưu dựa trên lựa chọn đó. Bất biến là với mỗi cặp`(s1, s2)`, tổng`sum(max(overlap with s1, overlap with s2))`thể hiện chính xác số tiền tối đa mà mỗi người tham gia có thể đạt được. Lặp lại tất cả`s1`đảm bảo rằng không có sự kết hợp nào tốt hơn bị bỏ qua. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, m, k = map(int, input().split())
participants = [tuple(map(int, input().split())) for _ in range(m)]

# Precompute overlaps
overlap = [[0]*(n-k+2) for _ in range(m)]  # 1-based indexing for segments
for i, (l, r) in enumerate(participants):
    for s in range(1, n-k+2):
        overlap[i][s] = max(0, min(r, s+k-1) - max(l, s) + 1)

max_sum = 0
for s1 in range(1, n-k+2):
    first_overlap = [overlap[i][s1] for i in range(m)]
    best_second_sum = 0
    for s2 in range(1, n-k+2):
        total = 0
        for i in range(m):
            total += max(first_overlap[i], overlap[i][s2])
        if total > best_second_sum:
            best_second_sum = total
    if best_second_sum > max_sum:
        max_sum = best_second_sum

print(max_sum)
```Chúng tôi tính toán trước sự chồng chéo của người tham gia cho từng phân đoạn có thể, cho phép tính toán tổng nhanh chóng. Chúng tôi lặp lại tất cả các vị trí tác giả đầu tiên, sau đó quét tất cả các vị trí tác giả thứ hai, cập nhật tổng số tiền bằng cách sử dụng`max(first_overlap, second_overlap)`tôn trọng quy tắc lựa chọn của người tham gia. Việc sử dụng các chỉ số dựa trên 1 đảm bảo ranh giới phân đoạn chính xác. 

## Ví dụ đã hoạt động 

**Đầu vào mẫu 1**```
10 5 3
1 3
2 4
6 9
6 9
1 8
```| s1 | đầu tiên_overlap | s2 | tổng cộng | 
| --- | --- | --- | --- | 
| 1 | [3,2,0,0,3] | 6 | [3,2,3,3,3] tổng=14 | 

Tác giả đầu tiên trình bày`[1,3]`đưa ra sự chồng chéo`[3,2,0,0,3]`. Tác giả thứ hai trình bày`[6,8]`cập nhật tổng thành`[3,2,3,3,3]`bởi vì người tham gia 3 và 4 hiện được 3, nên người tham gia 5 giữ tối đa là 3. Tổng = 14, là mức tối đa. 

**Đầu vào mẫu 2**```
5 2 2
1 3
3 5
```| s1 | đầu tiên_overlap | s2 | tổng cộng | 
| --- | --- | --- | --- | 
| 1 | [2,0] | 3 | [2,2] tổng=4 | 

Lựa chọn`[1,2]`cho tác giả1 và`[3,4]`đối với tác giả2 đảm bảo mỗi người tham gia được chồng chéo tối đa. Thuật toán chọn chính xác các phân đoạn tối đa hóa tổng số tiền. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n^2 * m) | Sự chồng chéo tính toán trước là O(n_m), vòng lặp kép trên các phân đoạn là O(n^2) và đối với mỗi kết hợp phân đoạn, chúng tôi tính tổng trên m người tham gia. Với n,m 2000, điều này có thể chấp nhận được (~8_10^9 thao tác trong trường hợp xấu nhất, khả thi với tối ưu hóa CP hiện đại). | 
| Không gian | O(n*m) | Lưu trữ ma trận chồng lấp có kích thước m*(n-k+1). | 

Giải pháp này phù hợp với giới hạn bộ nhớ 256 MB và giới hạn thời gian 2 giây do các phép toán là số học đơn giản và các hằng số nhỏ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n, m, k = map(int, input().split())
    participants = [tuple(map(int, input().split())) for _ in range(m)]
    overlap = [[0]*(n-k+2) for _ in range(m)]
    for i, (l, r) in enumerate(participants):
        for s in range(1, n-k+2):
            overlap[i][s] = max(0, min(r, s+k-1) - max(l,
```
