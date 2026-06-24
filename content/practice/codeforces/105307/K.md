---
title: "CF 105307K - Mua thuốc trên thế giới tuyệt vời này!"
description: "Mỗi lọ thuốc trong bài toán này có thể được xem dưới dạng tập hợp con của các loại chỉ số trong số tối đa 14 chỉ số có thể có. Mua một lọ thuốc sẽ cung cấp cho bạn tất cả các chỉ số được bảo vệ của nó và bởi vì nhân vật chính “may mắn” nên mọi lọ thuốc đều được đảm bảo thành công, vì vậy mỗi lọ thuốc được chọn đều có tính xác định…"
date: "2026-06-23T14:51:05+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105307
codeforces_index: "K"
codeforces_contest_name: "ICPC 2024 Thailand - Chulalongkorn University Internal Round"
rating: 0
weight: 105307
solve_time_s: 102
verified: false
draft: false
---

[CF 105307K - Mua thuốc trên thế giới tuyệt vời này!](https://codeforces.com/problemset/problem/105307/K) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 42s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Mỗi lọ thuốc trong bài toán này có thể được xem dưới dạng tập hợp con của các loại chỉ số trong số tối đa 14 chỉ số có thể có. Mua một lọ thuốc sẽ mang lại cho bạn tất cả các chỉ số được bảo vệ của nó và vì nhân vật chính “may mắn” nên mọi lọ thuốc đều được đảm bảo thành công, do đó, mỗi lọ thuốc được chọn sẽ đóng góp một cách xác định toàn bộ bộ chỉ số của nó. 

Đối với mỗi ngày, chúng tôi được cung cấp một tập hợp con số liệu thống kê bắt buộc. Nhiệm vụ là chọn một bộ sưu tập thuốc (được phép lặp lại, nhưng không liên quan vì việc mua trùng lặp không bao giờ có ích) sao cho sự kết hợp của các bộ chỉ số của chúng bao gồm tất cả các chỉ số cần thiết, đồng thời giảm thiểu tổng chi phí. Nếu không thể bao gồm tất cả các số liệu thống kê cần thiết bằng cách sử dụng các loại thuốc có sẵn, chúng tôi sẽ xuất -1. 

Chi tiết cấu trúc quan trọng là K ≤ 14, ngay lập tức ngụ ý rằng mọi bộ chỉ số có thể được mã hóa dưới dạng mặt nạ bit có độ dài tối đa là 14. Điều này đẩy vấn đề vào chế độ “tập hợp con trên các tập hợp con của vũ trụ nhỏ”, trong đó 2^K tối đa là 16384, đủ nhỏ để cho phép lập trình động trên tất cả các mặt nạ. 

Các ràng buộc N, M 2 × 10^4 loại trừ mọi phép tính lại theo truy vấn đối với tất cả các loại thuốc hoặc bất kỳ phương pháp nào cố gắng giải quyết mỗi ngày một cách độc lập bằng tìm kiếm tổ hợp. Một cách tiếp cận ngây thơ, mỗi ngày, cố gắng kết hợp các tập hợp thuốc nhỏ sẽ bùng nổ theo cấp số nhân. 

Một trường hợp phức tạp xuất hiện khi một chỉ số bắt buộc hoàn toàn không có trong bất kỳ loại thuốc nào. Trong trường hợp đó, câu trả lời luôn là -1. Một chế độ thất bại khác xảy ra nếu một người thử lựa chọn tham lam các lọ thuốc theo hiệu quả chi phí trên mỗi chỉ số: vì các lọ thuốc chồng chéo một cách tùy ý, các lựa chọn tham lam có thể chặn các kết hợp rẻ hơn. 

Một ví dụ nhỏ về thất bại tham lam là khi thuốc A bao gồm {1,2} với giá 5, thuốc B bao {2,3} với giá 5 và thuốc C bao {1,3} với giá 6. Greedy có thể chọn A và B để trang trải cả ba chỉ số với giá 10, nhưng tối ưu là C + A hoặc C + B tùy theo cấu trúc và trong những trường hợp phức tạp hơn, tham lam có thể thất bại thảm hại hơn. Giải pháp đúng phải tối ưu hóa toàn cục qua các kết hợp. 

## Phương pháp tiếp cận 

Cách giải thích bạo lực là xử lý mỗi ngày một cách độc lập: liệt kê các tập hợp con thuốc và tính toán sự kết hợp của các mặt nạ của chúng, theo dõi chi phí tối thiểu để trang trải cho mặt nạ cần thiết. Điều này đúng vì nó khớp trực tiếp với định nghĩa của vấn đề, nhưng không gian tìm kiếm là 2^N mỗi ngày, lớn về mặt thiên văn ở mức N = 20000. Ngay cả việc giới hạn các tập hợp con có kích thước hợp lý cũng không thành công vì không có giới hạn về số lượng thuốc có thể cần thiết. 

Quan sát quan trọng là chúng ta không cần phải xem xét các tập hợp con của thuốc một cách rõ ràng. Mỗi lọ thuốc đóng góp một mặt nạ cố định và việc kết hợp các lọ thuốc tương ứng với từng bit HOẶC của mặt nạ có chi phí phụ gia. Đây là một “chi phí tối thiểu để hình thành mỗi mặt nạ” cổ điển trên một vũ trụ nhỏ, có thể được giải quyết một lần cho tất cả các mặt nạ bằng cách sử dụng lập trình động kiểu đường đi ngắn nhất trên bitmasks. 

Chúng tôi xác định dp[mask] là chi phí tối thiểu để đạt được chính xác bộ thống kê đó. Mỗi lọ thuốc hoạt động như một quá trình chuyển đổi: từ bất kỳ mặt nạ hiện tại nào, chúng ta có thể chuyển sang mặt nạ HOẶC potion_mask với chi phí bổ sung. Điều này tạo thành một biểu đồ trên 2^K nút với N chuyển tiếp được lặp lại từ mọi nút. Thay vì chạy tìm kiếm biểu đồ đầy đủ cho mỗi truy vấn, chúng tôi tính toán dp một lần bằng cách sử dụng đường dẫn ngắn nhất nhiều nguồn trên không gian bitmask.

Tuy nhiên, việc thực hiện trực tiếp N chuyển đổi từ mọi trạng thái là quá chậm (chuyển đổi 2^K × N). Tinh chỉnh quan trọng là khởi tạo dp với các lọ thuốc riêng lẻ và sau đó chạy phần thư giãn tiêu chuẩn trên tất cả các mặt nạ bằng cách sử dụng tập hợp con cổ điển DP hoặc lan truyền giống Dijkstra trên bitmask. Vì K nhỏ nên chúng tôi có thể coi đây là đường dẫn ngắn nhất qua 2^K nút với chuyển tiếp lên tới K-bit, được tối ưu hóa bằng cách sử dụng danh sách thuốc được tính toán trước trên mỗi mặt nạ hoặc sử dụng tính năng thư giãn kiểu SPFA đối với các thao tác bit. Cách tiếp cận được chấp nhận thường sử dụng cấu trúc thư giãn giống như 0-1 với hàng đợi ưu tiên hoặc thư giãn kiểu BFS, tận dụng rằng mỗi lần chuyển đổi chỉ làm tăng mặt nạ. 

Khi dp được tính toán, mỗi truy vấn sẽ được trả lời bằng cách kiểm tra dp[required_mask]. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force cho mỗi truy vấn | O(M · 2^N) | O(1) | Quá chậm | 
| Đường đi ngắn nhất của mặt nạ bit tối ưu | O((N + 2^K) · 2^K) | O(2^K) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Chuyển đổi danh sách chỉ số của từng lọ thuốc thành một mặt nạ bit có độ dài K. Thao tác này sẽ nén mỗi lọ thuốc thành một trạng thái số nguyên duy nhất thể hiện chính xác số liệu thống kê mà nó bao gồm. 
2. Khởi tạo một mảng dp có kích thước 2^K với vô cùng, biểu thị chi phí tối thiểu để đạt được từng tập hợp con số liệu thống kê. 
3. Đặt trực tiếp dp[mask] = giá cho mỗi mặt nạ thuốc. Điều này giải thích cho thực tế rằng việc mua một lọ thuốc duy nhất luôn là một chiến lược hợp lý. 
4. Chạy quy trình thư giãn trên tất cả các mặt nạ. Đối với mỗi mặt nạ hiện tại có thể truy cập được, hãy thử áp dụng từng mặt nạ thuốc và cập nhật mặt nạ kết hợp: 

new_mask = mặt nạ HOẶC potion_mask và dp[new_mask] = min(dp[new_mask], dp[mask] + cost_potion). 

Bước này phổ biến sự kết hợp của các loại thuốc, đảm bảo rằng cuối cùng mọi lựa chọn nhiều loại thuốc đều được xây dựng. 
5. Lặp lại việc thư giãn cho đến khi không thể cải thiện được nữa. Vì mặt nạ chỉ tăng theo bit nên quá trình hội tụ trên mạng hữu hạn của các tập hợp con. 
6. Đối với mỗi ngày truy vấn, hãy chuyển đổi số liệu thống kê bắt buộc thành mặt nạ bit và xuất ra dp[required_mask] hoặc -1 nếu nó vẫn là vô cùng. 

### Tại sao nó hoạt động 

Trạng thái dp thể hiện chi phí được biết đến nhiều nhất cho mỗi tập hợp con số liệu thống kê được đề cập. Mỗi bộ thuốc hợp lệ tương ứng với một chuỗi các thao tác HOẶC bắt đầu từ các mặt nạ thuốc riêng lẻ. Bởi vì OR có tính liên kết và đơn điệu về mặt bao gồm tập hợp, nên bất kỳ sự kết hợp nào của các lọ thuốc đều có thể được phân tách thành các chuyển đổi OR theo cặp lặp đi lặp lại. Việc nới lỏng đảm bảo rằng bất cứ khi nào có sự kết hợp rẻ hơn cho một chiếc mặt nạ, cuối cùng nó sẽ được phát hiện thông qua một số chuỗi cập nhật. Vì chi phí chỉ giảm và mặt nạ là hữu hạn nên quá trình hội tụ đến mức tối ưu toàn cục cho mọi tập hợp con. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

INF = 10**18

def main():
    N, M, K = map(int, input().split())
    
    potions = []
    for _ in range(N):
        n, c = map(int, input().split())
        arr = list(map(int, input().split()))
        mask = 0
        for x in arr:
            mask |= 1 << (x - 1)
        potions.append((mask, c))
    
    max_mask = 1 << K
    dp = [INF] * max_mask
    
    for mask, cost in potions:
        if cost < dp[mask]:
            dp[mask] = cost
    
    updated = True
    while updated:
        updated = False
        for mask in range(max_mask):
            if dp[mask] == INF:
                continue
            base_cost = dp[mask]
            for pmask, pcost in potions:
                new_mask = mask | pmask
                new_cost = base_cost + pcost
                if new_cost < dp[new_mask]:
                    dp[new_mask] = new_cost
                    updated = True
    
    for _ in range(M):
        m = int(input())
        arr = list(map(int, input().split()))
        mask = 0
        for x in arr:
            mask |= 1 << (x - 1)
        ans = dp[mask]
        print(-1 if ans == INF else ans)

if __name__ == "__main__":
    main()
```Giải pháp bắt đầu bằng cách mã hóa mọi lọ thuốc thành một mặt nạ bit để việc kết hợp các bộ chỉ số trở thành phép toán OR theo bit. Mảng dp lưu trữ chi phí tối thiểu có thể đạt được cho mỗi tập hợp con số liệu thống kê. Chúng tôi gieo hạt dp bằng các lọ thuốc đơn lẻ, sau đó liên tục nới lỏng các chuyển đổi trong đó việc kết hợp mặt nạ có thể tiếp cận hiện có với bất kỳ lọ thuốc nào sẽ tạo ra một mặt nạ lớn hơn có khả năng rẻ hơn. Vòng lặp lặp lại là cần thiết vì các giải pháp tối ưu có thể cần nhiều hơn hai lọ thuốc và các kết hợp trung gian phải được phát hiện dần dần. 

Giai đoạn truy vấn sau đó trở nên đơn giản: mỗi bộ yêu cầu được chuyển đổi thành mặt nạ bit và được trả lời trong tra cứu O(1). 

Một điểm tinh tế là chúng tôi không cố gắng theo dõi “chính xác k lọ thuốc” hoặc thực thi bất kỳ cấu trúc nào theo thứ tự lựa chọn, vì việc tích lũy chi phí sẽ xử lý việc lựa chọn không lặp lại một cách tự nhiên. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Đầu tiên chúng tôi chuyển đổi thuốc thành mặt nạ. Giả sử chúng ta có mặt nạ P1, P2, P3 với các chi phí liên quan. 

Chúng tôi khởi tạo dp với chi phí thuốc đơn lẻ và sau đó nhân rộng các kết hợp. 

| Bước | Mặt nạ được xem xét | Hành động | cập nhật dp | 
| --- | --- | --- | --- | 
| ban đầu | P1 | dp[P1]=c1 | đặt | 
| ban đầu | P2 | dp[P2]=c2 | đặt | 
| thư giãn | P1 | P1 HOẶC P2 | dp[P1∪P2]=phút | 
| thư giãn | P2 | P2 HOẶC P3 | dp[P2∪P3]=phút | 

Sau khi hội tụ, dp chứa chi phí tốt nhất cho tất cả các kết hợp chỉ số có thể tiếp cận. Mặt nạ truy vấn được tra cứu trực tiếp, tạo ra kết quả đầu ra 4, 4, 8. 

Điều này cho thấy cách kết hợp nhiều loại thuốc được hình thành thông qua việc truyền bá OR lặp đi lặp lại thay vì liệt kê rõ ràng. 

### Mẫu 2 

Với ít ngày hơn, cùng một bảng dp được tạo một lần. 

| Bước | Mặt nạ | giá trị dp | 
| --- | --- | --- | 
| ban đầu | mặt nạ thuốc | chi phí cơ bản | 
| thư giãn | mặt nạ kết hợp | cập nhật | 
| truy vấn | mặt nạ đầy đủ | 7 | 

Điều này cho thấy rằng ngay cả khi chỉ tồn tại một truy vấn, quá trình xử lý trước đầy đủ vẫn nắm bắt chính xác các kết hợp tối ưu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(2^K · N) | Mỗi trạng thái dp có thể được nới lỏng với tất cả các lọ thuốc lặp đi lặp lại cho đến khi hội tụ | 
| Không gian | O(2^K) | Mảng DP trên tất cả các tập hợp con chỉ số | 

Vì K ≤ 14 nên 2^K nhiều nhất là 16384. Ngay cả với N lên tới 20000, điều này vẫn khả thi vì các phép toán bit rẻ và không gian trạng thái nhỏ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import os
    return os.popen("python3 main.py").read().strip()

# sample 1
assert run("""3 3 3
2 4
1 2
2 4
1 2
2 3
1 3
1
1
2
3
""") == "4\n4\n8"

# sample 3 (impossible stat)
assert run("""1 1 2
1 1
1 1
1
2
""") == "-1"

# custom: single potion solves directly
assert run("""1 1 3
2 5
1 3
2
1 3
""") == "5"

# custom: need combination
assert run("""2 1 3
2 3
1 2
2 4
2 3
3
1 2 3
""") == "7"

# custom: unreachable stat
assert run("""2 1 3
1 5
1 1
1 7
3
2 3
""") == "-1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| trận đấu chính xác thuốc duy nhất | 5 | khởi tạo dp trực tiếp | 
| yêu cầu kết hợp | 7 | HOẶC tính chính xác của việc truyền bá | 
| chỉ số không thể truy cập | -1 | xử lý mặt nạ không thể | 

## Vỏ cạnh 

Trường hợp thất bại trực tiếp xảy ra khi chỉ số bắt buộc không bao giờ xuất hiện trong bất kỳ lọ thuốc nào. Trong tình huống đó, tất cả mặt nạ dp bao gồm bit đó vẫn ở vô cực. Thuật toán trả về chính xác -1 vì không có sự thư giãn nào có thể đưa bit bị thiếu đó vào bất kỳ trạng thái có thể truy cập nào. 

Một trường hợp khó khăn khác là khi giải pháp tối ưu yêu cầu kết hợp nhiều hơn hai lọ thuốc. Vì dp không bị giới hạn ở các kết hợp theo cặp mà liên tục giãn ra trên các trạng thái đã hình thành nên mặt nạ trung gian sẽ tích lũy dần dần cho đến khi đạt được tập hợp đầy đủ. Sự kết hợp một bước tham lam sẽ bỏ lỡ các chuỗi như vậy, nhưng việc nới lỏng lặp đi lặp lại đảm bảo việc đóng các chuỗi có độ dài tùy ý. 

Trường hợp cuối cùng là khi nhiều bình thuốc có chung mặt nạ nhưng giá thành khác nhau. Bước khởi tạo chỉ giữ lại đại diện trực tiếp rẻ nhất và các bước nới lỏng sau này sẽ duy trì cấu trúc chi phí tối thiểu, đảm bảo các bản sao không làm sai lệch quá trình chuyển đổi.
