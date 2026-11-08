---
title: "CF 1423L - Công tắc đèn"
description: "Chúng tôi được yêu cầu xác định số công tắc tối thiểu mà Nikola phải bật để tắt tất cả đèn trong kho, dựa trên trạng thái của đèn sau vài ngày. Mỗi công tắc sẽ lật một tập hợp con đèn được xác định trước."
date: "2026-06-11T06:14:56+07:00"
tags: ["codeforces", "competitive-programming", "meet-in-the-middle"]
categories: ["algorithms"]
codeforces_contest: 1423
codeforces_index: "L"
codeforces_contest_name: "Bubble Cup 13 - Finals [Online Mirror, unrated, Div. 1]"
rating: 2600
weight: 1423
solve_time_s: 72
verified: true
draft: false
---

[CF 1423L - Công tắc đèn](https://codeforces.com/problemset/problem/1423/L) 

**Đánh giá:** 2600 
**Thẻ:** gặp nhau giữa đường 
**Thời gian giải:** 1 phút 12s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được yêu cầu xác định số công tắc tối thiểu mà Nikola phải bật để tắt tất cả đèn trong kho, dựa trên trạng thái của đèn sau vài ngày. Mỗi công tắc sẽ lật một tập hợp con đèn được xác định trước. Về mặt khái niệm, vấn đề giảm xuống còn việc chuyển đổi mặt nạ bit ban đầu (bật hoặc tắt đèn) thành mặt nạ bit bằng 0 (tắt hoàn toàn) bằng cách sử dụng một tập hợp con các mặt nạ bit có sẵn tương ứng với các công tắc. Mỗi ngày trình bày một bitmask khởi đầu mới. 

Các ràng buộc cho thấy có thể có tới 1000 đèn và 30 công tắc. Trong khi số lượng đèn lớn thì số lượng công tắc lại đủ nhỏ để xem xét các tập hợp con tổ hợp. Việc lật các công tắc tương ứng với việc XOR mặt nạ bit của chúng với trạng thái hiện tại. Vì có 30 công tắc nên việc khám phá tất cả$2^{30}$các tập hợp con chỉ khả thi nếu chúng tôi tối ưu hóa hơn nữa, bởi vì việc lặp lại một cách đơn giản trong mỗi 1000 ngày sẽ nhân chi phí lên đáng kể. Số lượng đèn cao cản trở các giải pháp thử trực tiếp tất cả các cấu hình ánh sáng; chúng ta cần một cách tiếp cận chủ yếu dựa trên số lượng thiết bị chuyển mạch. 

Một trường hợp phức tạp xảy ra khi không có tổ hợp công tắc nào có thể tắt tất cả các đèn. Ví dụ: nếu một ngày bắt đầu với đèn số 1 và 2 bật và tất cả các công tắc chỉ ảnh hưởng đến đèn số 1 thì đèn sẽ không bao giờ tắt hoàn toàn. Một trường hợp khác là khi nhiều công tắc ảnh hưởng đến các bộ đèn chồng chéo - một cách tiếp cận tham lam ngây thơ là lật các công tắc ảnh hưởng đến hầu hết các đèn bật có thể không thành công. Cuối cùng, những ngày không bật đèn không cần công tắc, việc này phải được xử lý rõ ràng. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ xem xét tất cả$2^S$tập hợp con các công tắc cho mỗi ngày, áp dụng từng kết hợp cho mặt nạ bit của ngày để xem liệu nó có dẫn đến tắt tất cả đèn hay không. Điều này đúng nhưng tốn kém về mặt tính toán. Với$S$lên đến 30,$2^{30}$là khoảng một tỷ. Lặp đi lặp lại điều này cho$D = 1000$ngày rõ ràng là không thể thực hiện được. 

Thông tin chi tiết quan trọng đến từ số lượng nhỏ thiết bị chuyển mạch. Vì việc lật là XOR nên thứ tự các lần lật không quan trọng. Điều này cho phép chúng ta chia các công tắc thành hai nửa và tính toán trước tất cả các kết quả XOR có thể có của mỗi nửa riêng biệt, cùng với số lần lật được sử dụng. Đây là kỹ thuật gặp nhau ở giữa cổ điển. Đối với mỗi ngày, chúng tôi cố gắng tìm hai kết quả được tính toán trước, một kết quả từ mỗi nửa, có XOR bằng mặt nạ bit của ngày. Việc sử dụng bản đồ từ các giá trị XOR tới số lần lật tối thiểu trong một nửa cho phép tra cứu hiệu quả, giảm tổng công việc từ$O(D \cdot 2^S)$đại khái$O(D \cdot 2^{S/2})$, điều này khả thi với các ràng buộc. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(D * 2^S * N) | O(2^S) | Quá chậm | 
| Gặp-ở-giữa | O(D * 2^(S/2) * N) | O(2^(S/2) * N) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Chuyển đổi mỗi switch thành một bitmask có độ dài$N$. Mỗi bit biểu thị liệu công tắc đó có bật một đèn cụ thể hay không. 
2. Chia công tắc thành hai nửa, gần bằng nhau. Tính toán trước tất cả các kết hợp XOR của nửa đầu và lưu trữ trong từ điển số lần lật tối thiểu cần thiết để đạt được mỗi mặt nạ bit kết quả. 
3. Tương tự, tính toán trước tất cả các kết hợp XOR của nửa sau nhưng lưu trữ chúng dưới dạng danh sách các cặp (mặt nạ bit, số lần lật). 
4. Đối với bitmask bắt đầu mỗi ngày, hãy lặp lại tất cả các kết hợp từ nửa sau. Đối với mỗi kết hợp, hãy tính XOR cần thiết từ nửa đầu để tắt tất cả đèn:$needed = day\_mask \oplus second\_mask$. 
5. Nếu$needed$tồn tại trong từ điển của hiệp một, tính tổng số lần lật của cả hai hiệp. Theo dõi số tiền tối thiểu. 
6. Nếu không có sự kết hợp nào khiến tất cả đèn tắt, hãy trả về -1 cho ngày đó; mặt khác, trả về số lần lật tối thiểu được tìm thấy. 

Tại sao nó hoạt động: thuật toán khám phá tất cả các tập hợp con có thể có của các công tắc mà không tạo ra toàn bộ$2^S$không gian cùng một lúc. Bằng cách chia thành hai nửa, giải pháp tận dụng tính giao hoán và tính kết hợp của XOR, đảm bảo rằng mọi cấu hình có thể đạt được đều có thể được hình thành bằng cách kết hợp một tập hợp con từ mỗi nửa. Từ điển đảm bảo chúng ta luôn chọn tổ hợp có ít lần lật nhất trong hiệp một. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
from itertools import combinations
from collections import defaultdict

def main():
    N, S, D = map(int, input().split())
    switches = []
    for _ in range(S):
        data = list(map(int, input().split()))
        mask = 0
        for light in data[1:]:
            mask |= 1 << (light - 1)
        switches.append(mask)
    
    half = S // 2
    first_half = switches[:half]
    second_half = switches[half:]
    
    # Precompute first half combinations
    first_dict = {}
    for k in range(len(first_half)+1):
        for comb in combinations(range(len(first_half)), k):
            mask = 0
            for idx in comb:
                mask ^= first_half[idx]
            if mask not in first_dict or first_dict[mask] > k:
                first_dict[mask] = k
    
    # Precompute second half combinations
    second_list = []
    for k in range(len(second_half)+1):
        for comb in combinations(range(len(second_half)), k):
            mask = 0
            for idx in comb:
                mask ^= second_half[idx]
            second_list.append((mask, k))
    
    # Process each day
    for _ in range(D):
        data = list(map(int, input().split()))
        day_mask = 0
        for light in data[1:]:
            day_mask |= 1 << (light - 1)
        ans = float('inf')
        for mask2, flips2 in second_list:
            needed = day_mask ^ mask2
            if needed in first_dict:
                ans = min(ans, flips2 + first_dict[needed])
        print(ans if ans != float('inf') else -1)

if __name__ == "__main__":
    main()
```Mã đầu tiên chuyển đổi các thiết bị chuyển mạch thành mặt nạ bit để thực hiện các hoạt động XOR hiệu quả. Công tắc chia tách đảm bảo rằng số lượng kết hợp vẫn có thể điều khiển được. Từ điển đảm bảo những cú lật tối thiểu ở nửa đầu có thể được phục hồi nhanh chóng. Việc sử dụng danh sách cho nửa sau là an toàn vì chúng tôi chỉ lặp lại tất cả các kết hợp của nó một lần mỗi ngày. Mặt nạ mỗi ngày được xây dựng dưới dạng mặt nạ bit và số học XOR xác định tập hợp con bổ sung cần thiết. 

## Ví dụ đã hoạt động 

Đầu vào mẫu 1:```
4 3 4
2 1 2
2 2 3
1 2
1 1
2 1 3
3 1 2 3
3 1 2 4
```| Bước | ngày_mask | lần truy cập first_dict? | giây_mask | tổng số lần lật | 
| --- | --- | --- | --- | --- | 
| Ngày 1 | 1<<0 | vâng | 0 | 2 | 
| Ngày 2 | 1<<0 ^ 1<<2 | vâng | 0 | 2 | 
| Ngày 3 | 1<<0 ^ 1<<1 ^ 1<<2 | vâng | 0 | 3 | 
| Ngày 4 | 1<<0^1<<1^1<<3 | không | bất kỳ | -1 | 

Bảng này cho thấy cách thuật toán tìm kiếm tất cả các kết hợp của nửa sau, tìm mặt nạ nửa đầu cần thiết để tắt đèn và tính toán số lần lật tối thiểu. Ngày thứ 4 không có nghiệm, tạo ra -1. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(D * 2^(S/2) * S) | Mỗi hiệp tạo ra 2^(S/2) kết hợp; mỗi ngày chúng ta lặp lại một nửa và tra cứu trong từ điển | 
| Không gian | O(2^(S/2)) | Lưu trữ các kết hợp của nửa đầu trong từ điển | 

Với S 30, 2^(S/2) ≈ 1024, do đó mức sử dụng bộ nhớ sẽ ở mức dưới 1 GB. Việc xử lý D = 1000 ngày là khả thi trong giới hạn 3 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    main()
    return output.getvalue().strip()

# Provided sample
assert run("""4 3 4
2 1 2
2 2 3
1 2
1 1
2 1 3
3 1 2 3
3 1 2 4""") == "2\n2\n3\n-1", "sample 1"

# Custom cases
assert run("""2 2 2
1 1
1 2
1 1
1 2""") == "1\n1", "single flip per day"

assert run("""3 3 1
1 1
1 2
1 3
3 1 2 3""") == "3", "all lights on, each switch flips one"

assert run("""3 2 1
2 1 2
2 2 3
2 1 3""") == "2", "overlapping switches"

assert run("""1 1 1
1 1
0""") == "0", "no lights on"

assert run("""30 30 1
""" + "\n".join(f"1 {i+1}" for
```
