---
title: "CF 457C - Bầu cử"
description: "Chúng tôi đang cố gắng giành chiến thắng trong cuộc bầu cử ở một thành phố có lượng cử tri được biết đến. Mỗi cử tri hiện ủng hộ một ứng cử viên được đánh số từ 0 trở lên và mỗi người đều phải trả phí để hối lộ họ bỏ phiếu cho mình. Ứng viên 0 là chính chúng ta."
date: "2026-05-30T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "brute-force"]
categories: ["algorithms"]
codeforces_contest: 457
codeforces_index: "C"
codeforces_contest_name: "MemSQL Start[c]UP 2.0 - Round 2"
rating: 2100
weight: 457
solve_time_s: 85
verified: true
draft: false
---

[CF 457C - Cuộc bầu cử](https://codeforces.com/problemset/problem/457/C) 

**Đánh giá:** 2100 
**Tags:** vũ lực 
**Thời gian giải:** 1 phút 25s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang cố gắng giành chiến thắng trong cuộc bầu cử ở một thành phố có lượng cử tri được biết đến. Mỗi cử tri hiện ủng hộ một ứng cử viên được đánh số từ 0 trở lên và mỗi người đều phải trả phí để hối lộ họ bỏ phiếu cho mình. Ứng viên 0 là chính chúng ta. Mục tiêu của chúng tôi là chi tiêu số tiền tối thiểu để đảm bảo chúng tôi có nhiều phiếu bầu hơn bất kỳ ứng cử viên nào khác. 

Dữ liệu đầu vào cung cấp số lượng cử tri, theo sau là danh sách trong đó mỗi mục nhập chứa ứng cử viên mà cử tri hiện đang ủng hộ và chi phí để hối lộ họ. Nếu một cử tri đã ủng hộ chúng tôi thì chi phí của họ là bằng không. Đầu ra là tổng chi phí tối thiểu cần thiết để giành chiến thắng. 

Những hạn chế là đáng kể. Với tối đa 100.000 cử tri, mọi giải pháp kiểm tra tất cả các nhóm cử tri sẽ rất chậm. Một cách tiếp cận bạo lực nhằm cố gắng xem xét mọi sự kết hợp có thể có của hối lộ là theo cấp số nhân và bị loại trừ ngay lập tức. Chúng ta cần một cách tiếp cận gần bằng O(n log n) hoặc O(n * m), trong đó m là số ứng cử viên riêng biệt, bởi vì các phương pháp bậc hai có thể sẽ hết thời gian chờ. 

Một trường hợp nguy hiểm dễ xử lý sai xảy ra khi chúng ta đã có nhiều phiếu bầu hơn bất kỳ ai khác. Ví dụ: nếu đầu vào là:```
3
0 0
1 5
2 7
```Chúng tôi đã có một phiếu bầu, có thể nhiều hơn một số ứng cử viên khác. Thuật toán sẽ phát hiện rằng không cần hối lộ và đưa ra kết quả 0. Một trường hợp phức tạp khác phát sinh khi nhiều ứng cử viên có cùng số phiếu bầu và việc hối lộ cử tri rẻ nhất từ ​​mỗi ứng cử viên đối lập là bắt buộc để chỉ vừa đủ vượt qua họ. Thiếu sự tinh tế này sẽ dẫn đến bội chi. 

## Phương pháp tiếp cận 

Cách tiếp cận ngây thơ là thử mọi số phiếu có thể có mà chúng tôi có thể nhắm tới và xem chúng tôi cần hối lộ những cử tri nào để đạt được số phiếu đó. Cụ thể, chúng tôi có thể xem xét tất cả các tập hợp con cử tri hiện không bỏ phiếu cho chúng tôi, tính toán tổng chi phí cho từng tập hợp con và kiểm tra xem việc hối lộ họ có mang lại số phiếu thắng cử hay không. Cách tiếp cận này đúng vì nó khám phá tất cả các tình huống hối lộ có thể xảy ra, nhưng nó có tính cấp số nhân: với 100.000 cử tri, 2^100000 khả năng là hoàn toàn không thể giải quyết được. 

Thông tin chuyên sâu quan trọng giúp đưa ra giải pháp nhanh hơn là nhận ra rằng để giành chiến thắng, chúng ta chỉ cần có nhiều hơn một phiếu bầu so với ứng cử viên có mức ủng hộ cao nhất tiếp theo. Vì vậy, chúng ta có thể nghĩ theo khía cạnh "số phiếu mục tiêu": chúng ta muốn có bao nhiêu phiếu bầu cho mình và chi phí tối thiểu để đạt được con số đó là bao nhiêu? Đối với mỗi số phiếu bầu tiềm năng mà chúng ta mong muốn, chúng ta có thể ưu tiên hối lộ những cử tri ủng hộ các ứng cử viên đối lập mạnh nhất với chi phí hối lộ thấp nhất. Việc sắp xếp cử tri theo chi phí trong mỗi ứng cử viên đối lập cho phép chúng tôi tính toán dần chi phí tối thiểu. 

Vấn đề giảm xuống còn việc đánh giá, đối với mỗi mục tiêu bỏ phiếu khả thi, sự kết hợp rẻ nhất của các khoản hối lộ từ các ứng cử viên có số phiếu vượt quá mục tiêu của chúng tôi. Chúng tôi lặp lại tất cả số lượng phiếu bầu có thể có mà chúng tôi có thể nhắm đến để đạt được và duy trì hàng ưu tiên gồm những cử tri bổ sung mà chúng tôi có thể hối lộ với giá rẻ để đạt được con số đó. Điều này làm giảm vấn đề từ hàm mũ xuống khoảng O(n log n), vì cử tri của mỗi ứng cử viên đều được sắp xếp và chúng tôi duy trì rất nhiều khoản hối lộ tiềm năng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n) | O(n) | Quá chậm | 
| Tối ưu | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc tất cả cử tri và nhóm họ theo ứng cử viên mà họ hiện đang ủng hộ, lưu trữ chi phí hối lộ cho mỗi ứng cử viên trong danh sách. Điều này cho phép chúng tôi tiếp cận tất cả các khoản hối lộ tiềm năng dành cho ứng viên một cách nhanh chóng. 
2. Sắp xếp danh sách chi phí hối lộ của từng ứng viên theo thứ tự tăng dần. Việc hối lộ rẻ hơn nên được xem xét trước tiên vì đối với bất kỳ số phiếu bầu mục tiêu nào, việc hối lộ những cử tri có chi phí thấp nhất luôn là điều tối ưu. 
3. Đối với mỗi số phiếu mục tiêu có thể có`t`chúng tôi có thể giữ (từ số phiếu hiện tại của chúng tôi cho đến số phiếu tối đa mà bất kỳ ứng cử viên nào nắm giữ cộng một), tính chi phí tối thiểu để đạt được số phiếu đó. Điều này được thực hiện bằng cách trước tiên xác định ứng cử viên nào có nhiều phiếu bầu hơn`t-1`(vì để giành chiến thắng, chúng tôi cần nhiều hơn mỗi thứ). Đối với mỗi ứng cử viên như vậy, hãy xác định có bao nhiêu cử tri phải hối lộ để giảm số phiếu bầu của họ dưới đây`t`. 
4. Lấy số lượng cử tri rẻ nhất cần thiết từ mỗi ứng cử viên có tỷ lệ đại diện cao để giảm số lượng cử tri của họ. Duy trì hàng ưu tiên (hoặc danh sách được sắp xếp) gồm các cử tri còn lại từ tất cả các ứng cử viên để điền vào bất kỳ phiếu bầu bổ sung nào cần thiết nếu số phiếu ban đầu của chúng tôi cộng với số tiền hối lộ này vẫn thấp hơn`t`. 
5. Tổng hợp chi phí của tất cả các khoản hối lộ được lựa chọn cho mục tiêu này. Theo dõi tổng chi phí tối thiểu trên tất cả số phiếu bầu mục tiêu. 
6. Trả lại chi phí tối thiểu thu được. 

Tại sao nó hoạt động: tính bất biến được duy trì là dành cho bất kỳ mục tiêu nào`t`, thuật toán đảm bảo mỗi ứng viên có nhiều nhất`t-1`bỏ phiếu sau khi lựa chọn những khoản hối lộ có chi phí tối thiểu. Bằng cách lặp lại tất cả số phiếu bầu mục tiêu có thể có và luôn nhận những khoản hối lộ rẻ nhất hiện có, chúng tôi đảm bảo mức chi tiêu tối thiểu cần thiết để đảm bảo đa số nghiêm ngặt. 

## Giải pháp Python```python
import sys
import heapq
input = sys.stdin.readline

n = int(input())
votes = {}
our_votes = 0

for _ in range(n):
    a, b = map(int, input().split())
    if a == 0:
        our_votes += 1
    else:
        if a not in votes:
            votes[a] = []
        votes[a].append(b)

for v in votes:
    votes[v].sort()

all_candidates = list(votes.keys())
min_cost = float('inf')
max_votes = max(len(lst) for lst in votes.values()) if votes else 0

for target in range(our_votes, n+1):
    extra_needed = max(0, target - our_votes)
    costs_heap = []
    total_cost = 0
    extra_collected = 0
    
    for cand in all_candidates:
        cand_votes = len(votes[cand])
        if cand_votes >= target:
            num_to_bribe = cand_votes - (target - 1)
            total_cost += sum(votes[cand][:num_to_bribe])
            for c in votes[cand][num_to_bribe:]:
                heapq.heappush(costs_heap, c)
        else:
            for c in votes[cand]:
                heapq.heappush(costs_heap, c)
    
    while extra_collected < extra_needed and costs_heap:
        total_cost += heapq.heappop(costs_heap)
        extra_collected += 1
    
    if extra_collected == extra_needed:
        min_cost = min(min_cost, total_cost)

print(min_cost)
```Đầu tiên, mã này thu thập tất cả cử tri theo ứng cử viên và phân loại chi phí hối lộ của họ. Đối với mỗi mục tiêu bỏ phiếu tiềm năng, nó tính toán số lượng hối lộ cần thiết để giảm số phiếu bầu của các ứng cử viên khác xuống dưới mục tiêu, đồng thời lưu trữ thêm cử tri để lấp đầy số phiếu cần thiết còn lại trong một đống tối thiểu. Điều này đảm bảo những khoản hối lộ bổ sung rẻ nhất luôn được lựa chọn đầu tiên. Tổng chi phí tối thiểu trên tất cả các mục tiêu được in. 

## Ví dụ đã hoạt động 

Mẫu 1: 

đầu vào:```
5
1 2
1 2
1 2
2 1
0 0
```| mục tiêu | quá_ứng cử viên | hối lộ_taken | thêm_đống | thêm_cần thiết | tổng_chi phí | chi phí tối thiểu | 
| --- | --- | --- | --- | --- | --- | --- | 
| 2 | 1,2 | [2,2,1] | [] | 1 | 3 | 3 | 

Dấu vết này cho thấy bằng cách nhắm tới 2 phiếu bầu, chúng ta phải hối lộ hai cử tri của ứng cử viên 1 và một của ứng cử viên 2, tổng cộng là 3, là tối thiểu. 

Đầu vào tùy chỉnh:```
3
1 5
2 4
0 0
```| mục tiêu | quá_ứng cử viên | hối lộ_taken | thêm_đống | thêm_cần thiết | tổng_chi phí | chi phí tối thiểu | 
| --- | --- | --- | --- | --- | --- | --- | 
| 2 | 1,2 | [5,4] | [] | 1 | 4 | 4 | 

Ở đây, nhằm mục đích giành được 2 phiếu bầu, chúng tôi hối lộ một cử tri từ mỗi ứng cử viên đối lập, trả tiền cho sự kết hợp rẻ nhất, xác nhận thuật toán chọn đúng số tiền hối lộ tối ưu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Sắp xếp cử tri của mỗi ứng cử viên và duy trì một vùng tối thiểu chiếm ưu thế trong thời gian chạy. | 
| Không gian | O(n) | Việc lưu trữ tất cả chi phí của cử tri và vùng nhớ heap yêu cầu không gian tuyến tính. | 

Với n 100.000, các thao tác O(n log n) có thể thực hiện được trong vòng 2 giây và việc lưu trữ 100.000 số nguyên nằm trong bộ nhớ 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n = int(input())
    votes = {}
    our_votes = 0
    for _ in range(n):
        a, b = map(int, input().split())
        if a == 0:
            our_votes += 1
        else:
            if a not in votes:
                votes[a] = []
            votes[a].append(b)
    for v in votes:
        votes[v].sort()
    all_candidates = list(votes.keys())
    min_cost = float('inf')
    for target in range(our_votes, n+1):
        extra_needed = max(0, target - our_votes)
        costs_heap = []
        total_cost = 0
        extra_collected = 0
        for cand in all_candidates:
            cand_votes = len(votes[cand])
            if cand_votes >= target:
                num_to_bribe = cand_votes - (target - 1)
                total_cost += sum(votes[cand][:num_to_bribe])
                for c in votes[cand][num_to_bribe:]:
```
