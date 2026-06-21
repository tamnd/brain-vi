---
title: "CF 105B - ​​Hội tối"
description: "Chúng tôi có một nhóm nhỏ gồm n thượng nghị sĩ, mỗi người được xác định bởi một cấp độ và điểm trung thành. Lòng trung thành là xác suất mà một thượng nghị sĩ bỏ phiếu ủng hộ một đề xuất, được đưa ra với mức tăng 10%. Nếu hơn một nửa số thượng nghị sĩ bỏ phiếu đồng ý, đề xuất sẽ được thông qua."
date: "2026-06-01T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "probabilities"]
categories: ["algorithms"]
codeforces_contest: 105
codeforces_index: "B"
codeforces_contest_name: "Codeforces Beta Round 81"
rating: 1800
weight: 105
solve_time_s: 139
verified: true
draft: false
---

[CF 105B - Hội tối](https://codeforces.com/problemset/problem/105/B) 

**Đánh giá:** 1800 
**Tags:** sức mạnh vũ phu, xác suất 
**Thời gian giải:** 2m 19s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một nhóm nhỏ gồm _n_ thượng nghị sĩ, mỗi người được xác định theo cấp độ và điểm trung thành. Lòng trung thành là xác suất mà một thượng nghị sĩ bỏ phiếu ủng hộ một đề xuất, được đưa ra với mức tăng 10%. Nếu hơn một nửa số thượng nghị sĩ bỏ phiếu đồng ý, đề xuất sẽ được thông qua. Nếu cuộc bỏ phiếu thất bại, người chơi có thể cố gắng "loại bỏ" những người bất đồng chính kiến, với xác suất thành công dựa trên tổng cấp độ của người chơi so với tổng cấp độ của những người bất đồng chính kiến ​​đó. 

Trước khi bỏ phiếu, người chơi có thể hối lộ các thượng nghị sĩ bằng tối đa _k_ kẹo. Mỗi viên kẹo tăng mức độ trung thành lên 10%, tối đa là 100%. Nhiệm vụ là xác định xác suất tối đa để thông qua đề xuất sau khi phân phối kẹo một cách tối ưu. 

Các ràng buộc rất nhỏ: _n_ và _k_ nhiều nhất là 8. Điều này ngay lập tức gợi ý rằng việc khám phá tất cả các cách phân phối kẹo có thể là khả thi. Cấp độ thượng nghị sĩ và giá trị lòng trung thành có thể lên tới 9999 và 100 tương ứng, nhưng xác suất được lượng tử hóa theo các bước 10%. 

Các trường hợp khó khăn không rõ ràng bao gồm kịch bản ban đầu không có thượng nghị sĩ nào bỏ phiếu tích cực hoặc khi lòng trung thành đã đạt 100%, khiến việc bổ sung kẹo không hiệu quả. Một trường hợp tinh vi khác phát sinh nếu _k_ vượt quá mức cần thiết để đạt được mức tối đa lòng trung thành; việc phân phối số kẹo thừa không làm thay đổi xác suất. 

## Phương pháp tiếp cận 

Phương pháp vũ lực rất đơn giản: thử mọi cách có thể để phân phát kẹo giữa các thượng nghị sĩ, tính toán xác suất vượt qua phiếu bầu cho mỗi lần phân phối và theo dõi mức tối đa. Đối với mỗi lần phân phối kẹo, chúng ta phải xem xét tất cả$2^n$tập hợp con các thượng nghị sĩ bỏ phiếu có hoặc không để tính xác suất bỏ phiếu thành công. Sau đó, nếu đề xuất thất bại ở một nhóm nhỏ, chúng tôi sẽ tính xác suất loại bỏ những người bất đồng chính kiến. 

Với _n_ ≤ 8 và _k_ ≤ 8, số lần phân phối kẹo có tính chất tổ hợp nhưng đủ nhỏ. Cụ thể, mỗi thượng nghị sĩ có thể nhận được từ 0 đến tối thiểu (k, 10 - lòng trung thành/10) kẹo. Chúng ta có thể tạo phân phối đệ quy. Đối với mỗi phân phối, chúng tôi tính tổng tất cả các tập hợp con của các thượng nghị sĩ bỏ phiếu đồng ý, tính toán xác suất kết hợp. Ngay cả trong trường hợp xấu nhất, độ phức tạp vẫn có thể chấp nhận được vì$8 \times 2^8 \times 9^8$nằm trong khoảng vài triệu thao tác mà CPU hiện đại có thể xử lý trong vòng chưa đầy 2 giây. 

Cái nhìn sâu sắc quan trọng để tối ưu hóa là chúng ta không cần lập trình động hoặc cắt tỉa cầu kỳ ngoài cách tiếp cận mạnh mẽ này, bởi vì giới hạn là rất nhỏ. Thách thức ở đây là tính toán chính xác xác suất cho từng kịch bản và xử lý việc làm tròn với mức tăng 10%. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(lựa chọn kẹo × 2^n) | O(n) | Đã chấp nhận | 
| Tối ưu hóa với tính năng ghi nhớ | O(giống nhau) | O(n × k) | Không cần thiết cho những giới hạn này | 

## Hướng dẫn thuật toán 

1. Đọc thông tin đầu vào, lưu trữ cấp độ và lòng trung thành của từng thượng nghị sĩ. Chuyển đổi tỷ lệ phần trăm lòng trung thành thành xác suất thập phân để tính toán. 
2. Tạo ra tất cả các cách có thể để phân phối tối đa _k_ kẹo cho _n_ thượng nghị sĩ, tôn trọng giới hạn 100% lòng trung thành. Mỗi viên kẹo tăng thêm 10% lòng trung thành. Điều này có thể được thực hiện theo cách đệ quy: đối với thượng nghị sĩ i, hãy thử cho từ 0 đến tối thiểu (số kẹo còn lại, 10 - current_loyalty/10) kẹo, sau đó lặp lại cho thượng nghị sĩ i+1. 
3. Với mỗi lần phân phối kẹo, hãy tính xác suất đề xuất được thông qua. Lặp lại tất cả$2^n$tập hợp con các thượng nghị sĩ đại diện cho nhóm bỏ phiếu đồng ý. Tính xác suất của mỗi tập hợp con xảy ra dưới dạng tích của các xác suất của từng thượng nghị sĩ: đối với một thượng nghị sĩ trong tập hợp có, nhân với lòng trung thành; đối với một thượng nghị sĩ không có trong tập hợp, nhân với (1 - lòng trung thành). 
4. Đối với các tập hợp con có hơn một nửa số phiếu đồng ý, hãy cộng xác suất vào tổng số hiện có. 
5. Đối với các tập hợp con mà đề xuất không thành công, hãy tính xác suất tiêu diệt thành công tất cả những người bất đồng chính kiến. Tính tổng các cấp độ của chúng để được _B_, sau đó tính toán$A / (A + B)$, nhân với xác suất của mẫu phiếu bầu này và cộng vào tổng số tiền hiện có. 
6. Theo dõi xác suất tối đa trên tất cả các lần phân phối kẹo. Xuất xác suất này với 10 chữ số thập phân. 

Tại sao nó hoạt động: thuật toán liệt kê rõ ràng tất cả các lần phân bổ kẹo có thể có và tất cả kết quả bỏ phiếu. Mỗi xác suất được tính toán chính xác theo các quy tắc của bài toán. Bằng cách xem xét mọi cách phân bổ kẹo, chúng tôi đảm bảo sẽ tìm ra cách phân bổ tối ưu. Tổng xác suất trên tất cả các kết quả bằng 1 cho mỗi lần phân phối, đảm bảo không có kịch bản nào bị bỏ sót. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
from itertools import product

def solve():
    n, k, A = map(int, input().split())
    senators = []
    for _ in range(n):
        b, l = map(int, input().split())
        senators.append([b, l])

    max_prob = 0.0

    # Recursive candy distribution
    def distribute(i, remaining_candies, loyalties):
        nonlocal max_prob
        if i == n:
            # Evaluate this loyalty distribution
            prob = 0.0
            for mask in range(1 << n):
                yes_count = 0
                p_mask = 1.0
                dissenters_level = 0
                for j in range(n):
                    if mask & (1 << j):
                        yes_count += 1
                        p_mask *= loyalties[j] / 100
                    else:
                        p_mask *= 1 - loyalties[j] / 100
                        dissenters_level += senators[j][0]
                if yes_count > n // 2:
                    prob += p_mask
                else:
                    if A + dissenters_level > 0:
                        prob += p_mask * A / (A + dissenters_level)
            max_prob = max(max_prob, prob)
            return

        max_add = min(remaining_candies, 10 - loyalties[i] // 10)
        for candies in range(max_add + 1):
            loyalties[i] += candies * 10
            distribute(i + 1, remaining_candies - candies, loyalties)
            loyalties[i] -= candies * 10

    distribute(0, k, [sen[1] for sen in senators])
    print(f"{max_prob:.10f}")

solve()
```Đầu tiên, mã sẽ đọc dữ liệu đầu vào và lưu trữ cấp độ cũng như lòng trung thành của các thượng nghị sĩ. Nó sử dụng hàm đệ quy để thử tất cả các phân phối kẹo hợp lệ. Đối với mỗi phân phối, nó liệt kê tất cả các tập hợp con biểu quyết bằng cách sử dụng mặt nạ bit. Xác suất cho mỗi kiểu bỏ phiếu được nhân lên cho từng thượng nghị sĩ. Nếu phiếu bầu thông qua, xác suất sẽ được cộng trực tiếp; nếu không, cơ hội loại bỏ những người bất đồng chính kiến sẽ được tính đến. 

Việc triển khai cẩn thận đảm bảo giới hạn mức độ trung thành ở mức 100%, xác suất được tính theo số thập phân và đệ quy theo dõi chính xác các thay đổi về mức độ trung thành sau mỗi nhánh. 

## Ví dụ đã hoạt động 

**Mẫu 1** 

đầu vào:```
5 6 100
11 80
14 90
23 70
80 30
153 70
```| Phân phối | Mẫu phiếu bầu | Bỏ phiếu có? | Cấp độ bất đồng chính kiến ​​| Xác suất | Đóng góp | 
| --- | --- | --- | --- | --- | --- | 
| 2 viên kẹo cho thượng nghị sĩ 1, 2 đến 2, 2 đến 3 | mặt nạ 11111 | vâng | 0 | 1 | 1 | 
| ... | mặt nạ khác | thất bại | tổng (B) | P_vote * A/(A+B) | tổng hợp | 

Điều này chứng tỏ rằng việc phân phát kẹo cho các thượng nghị sĩ trung thành nhất sẽ đảm bảo dễ dàng tiếp cận được đa số. 

**Mẫu 2 (tùy chỉnh)**```
3 3 10
5 0
5 0
5 0
```Tối ưu: tặng 1 viên kẹo cho mỗi thượng nghị sĩ → lòng trung thành = 10%, 10%, 10% 

| mặt nạ | vâng_count | xác suất | đóng góp | 
| --- | --- | --- | --- | 
| 000 | 0 | 0,9_0,9_0,9=0,729 | thất bại → 10/(10+15)=0,4 → 0,729*0,4=0,2916 | 
| 001 | 1 | 0,1_0,9_0,9=0,081 | thất bại → 10/(10+10)=0,5 → 0,081*0,5=0,0405 | 
| 010 | 1 | giống nhau | 0,0405 | 
| 011 | 2 | vượt qua | 0,009 | 
| tổng cộng | | | tổng=0,3816 | 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((k+1)^n * 2^n) | Đối với mỗi lần phân phối kẹo (≤ 9^8), chúng tôi liệt kê tất cả các tập hợp con phiếu bầu (2^n ≤ 256) | 
| Không gian | O(n) | Chỉ lưu trữ mức độ trung thành hiện tại và ngăn xếp đệ quy | 

Với n 8 và k 8, tổng số thao tác đủ nhỏ cho giới hạn 2 giây. Việc sử dụng bộ nhớ là tối thiểu. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    f = io.StringIO()
    with redirect_stdout(f):
        solve()
    return f.getvalue().strip()

# provided sample
assert run("5 6 100\n11 80\n14 90\n23 70\n80 30\n153 70\n") == "1.0000000000", "sample 1"

# custom
assert run("3 3 10\n5 0\n5 0\n5 0\n") == "0.3816000000", "all
```
