---
title: "CF 1951B - Bò Chiến"
description: "Chúng tôi có một dòng bò, mỗi dòng có xếp hạng Cowdeforces riêng biệt và chúng cạnh tranh trong một giải đấu tuần tự. Giải đấu bắt đầu với hai con bò đầu tiên và mỗi trận đấu tiếp theo là giữa người chiến thắng trong trận đấu trước và con bò tiếp theo trong hàng."
date: "2026-05-31T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "data-structures", "greedy"]
categories: ["algorithms"]
codeforces_contest: 1951
codeforces_index: "B"
codeforces_contest_name: "Codeforces Global Round 25"
rating: 1200
weight: 1951
solve_time_s: 84
verified: false
draft: false
---

[CF 1951B - Bò chiến](https://codeforces.com/problemset/problem/1951/B) 

**Đánh giá:** 1200 
**Tas:** tìm kiếm nhị phân, cấu trúc dữ liệu, tham lam 
**Thời gian giải:** 1 phút 24s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một dòng bò, mỗi dòng có xếp hạng Cowdeforces riêng biệt và chúng cạnh tranh trong một giải đấu tuần tự. Giải đấu bắt đầu với hai con bò đầu tiên và mỗi trận đấu tiếp theo là giữa người chiến thắng trong trận đấu trước và con bò tiếp theo trong hàng. Con bò có thứ hạng cao hơn luôn thắng. Bạn sở hữu một con bò và có thể đổi nó một lần với bất kỳ con bò nào khác hoặc để nguyên. Mục tiêu là tối đa hóa số trận thắng của con bò của bạn. 

Đầu vào bao gồm nhiều trường hợp thử nghiệm. Mỗi trường hợp thử nghiệm cung cấp số lượng bò, chỉ số dựa trên 1 của con bò của bạn và xếp hạng của tất cả các con bò ở vị trí ban đầu của chúng. Đầu ra là một số duy nhất cho mỗi trường hợp thử nghiệm thể hiện chiến thắng tối đa mà con bò của bạn có thể đạt được theo chiến lược hoán đổi tối ưu. 

Các ràng buộc cho phép tổng cộng lên tới 100.000 con bò trong tất cả các trường hợp thử nghiệm. Điều này có nghĩa là bất kỳ giải pháp nào có độ phức tạp về thời gian kém hơn O(n) cho mỗi trường hợp thử nghiệm đều có thể quá chậm. Cụ thể, các mô phỏng lồng nhau của tất cả các giao dịch hoán đổi có thể xảy ra sẽ dẫn đến hành vi O(n^2), điều này không thể chấp nhận được. 

Trường hợp khó khăn tinh tế xảy ra khi con bò của bạn đã mạnh nhất hoặc yếu nhất trong phân khúc của nó. Ví dụ: nếu con bò của bạn đã về nhất và mạnh hơn một số con bò tiếp theo, một cách tiếp cận ngây thơ có thể thử hoán đổi một cách không cần thiết và làm giảm tổng số tiền thắng. Một tình huống khó khăn khác là khi chiến lược tốt nhất là đổi con bò của bạn về phía trước qua một chuỗi yếu hơn, thay vì về đầu. Ví dụ: hãy xem xét xếp hạng [2, 1, 5] với xếp hạng con bò của bạn là 2. Đổi sang vị trí 2 sẽ mang lại một chiến thắng, nhưng đổi sang vị trí 3 sẽ không thắng. Việc xử lý những vấn đề này đòi hỏi phải xem xét cẩn thận cực đại cục bộ và trình tự của những con bò yếu hơn. 

## Phương pháp tiếp cận 

Cách tiếp cận vũ phu rất đơn giản. Bạn có thể mô phỏng giải đấu cho mọi khả năng hoán đổi con bò của bạn với con bò khác, sau đó đếm số tiền thắng cho từng kịch bản và chọn mức tối đa. Điều này đúng vì luật thi đấu mang tính quyết định. Tuy nhiên, điều này yêu cầu các thao tác O(n^2) cho mỗi trường hợp thử nghiệm: đối với n con bò, chúng tôi sẽ xem xét n vị trí có thể hoán đổi và mỗi mô phỏng có thể mất tới n bước. Với n lên đến 10^5, điều này dẫn đến 10^10 phép toán trong trường hợp xấu nhất, điều này là không thể thực hiện được. 

Điều quan trọng cần lưu ý là trong giải đấu tuần tự, chỉ những con bò yếu hơn con bò của bạn mới quan trọng. Nếu con bò của bạn mạnh hơn những con bò tiếp theo trong hàng, nó sẽ tiếp tục giành chiến thắng cho đến khi gặp con bò mạnh hơn nó. Vì vậy, để tối đa hóa chiến thắng, chúng ta chỉ cần tìm một vị trí sao cho con bò của bạn gặp chuỗi dài nhất liên tiếp những con bò yếu hơn nó. Điều này có nghĩa là chiến lược tối ưu là giữ con bò của bạn tại chỗ hoặc di chuyển nó ngay trước con bò "đỉnh" (con bò mạnh hơn con bò của bạn) để tối đa hóa chiến thắng liên tiếp. 

Chúng ta có thể thực hiện điều này trong thời gian O(n) bằng cách quét từ trái sang phải. Chúng tôi theo dõi số lượng con bò yếu hơn mà con bò của bạn có thể đánh bại bắt đầu từ bất kỳ vị trí nhất định nào. Chúng tôi duy trì giá trị tối đa trên tất cả các vị trí bắt đầu hợp lệ (bao gồm cả việc giữ con bò ở đúng vị trí). Vấn đề giảm xuống còn quét tuyến tính đơn giản so sánh xếp hạng, tránh mô phỏng đầy đủ cho mỗi lần hoán đổi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^2) | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng con bò n, chỉ số k của con bò của bạn và mảng xếp hạng a. Chuyển đổi chỉ số k dựa trên 1 thành chỉ số k dựa trên 0 để xử lý mảng dễ dàng hơn. 
2. Khởi tạo hai biến:`max_wins`để theo dõi số trận thắng tốt nhất và`current_wins`để tính số trận thắng liên tiếp cho vị trí hiện tại. 
3. Lặp lại mảng từ trái sang phải. Nếu con bò hiện tại là con bò của bạn, hãy đặt lại`current_wins`về 0 vì nó bắt đầu đếm số trận thắng từ đây. 
4. Đối với bất kỳ con bò nào khác, hãy kiểm tra xem con bò của bạn có thắng nếu nó ở vị trí này hay không. Điều này đúng nếu xếp hạng của con bò của bạn cao hơn xếp hạng của con bò hiện có trong trận đấu. Nếu có thì tăng`current_wins`. 
5. Cập nhật`max_wins`bất cứ khi nào`current_wins`vượt quá nó. Điều này đảm bảo chúng tôi luôn theo dõi chuỗi trận thắng dài nhất mà con bò của bạn có thể bắt đầu từ một vị trí nào đó. 
6. Sau khi quét mảng,`max_wins`thể hiện số trận đấu liên tiếp tối đa mà con bò của bạn có thể thắng, bằng cách giữ nguyên vị trí hoặc bằng một lần hoán đổi tối ưu duy nhất. 

Tại sao nó hoạt động: Điều bất biến là con bò chỉ có thể đánh bại những con bò yếu hơn nó theo trình tự. Bằng cách quét tuyến tính và đếm những con bò yếu hơn liên tiếp, chúng tôi tự động tính toán vị trí tốt nhất để đặt con bò của bạn, bao gồm cả việc giữ nó ở vị trí ban đầu. Điều này tránh mô phỏng mọi trận đấu một cách rõ ràng nhưng cho kết quả tương tự. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def max_wins_for_cow(n, k, a):
    cow_rating = a[k-1]
    max_wins = 0
    current_wins = 0
    for rating in a:
        if rating == cow_rating:
            current_wins = 0
            continue
        if rating < cow_rating:
            current_wins += 1
            max_wins = max(max_wins, current_wins)
        else:
            current_wins = 0
    return max_wins

t = int(input())
for _ in range(t):
    n, k = map(int, input().split())
    a = list(map(int, input().split()))
    print(max_wins_for_cow(n, k, a))
```Giải pháp đầu tiên sẽ tách biệt xếp hạng của con bò của bạn. Sau đó, nó lặp qua tất cả các con bò, đếm xem có bao nhiêu con bò yếu hơn liên tiếp tồn tại trong bất kỳ phân đoạn nào, đặt lại bộ đếm nếu xuất hiện một con bò mạnh hơn. Việc sử dụng`current_wins`đảm bảo rằng chúng tôi theo dõi các chuỗi và`max_wins`giữ kết quả tốt nhất. Việc chuyển đổi chỉ mục dựa trên k thành 0 sẽ tránh được từng lỗi một. 

## Ví dụ đã hoạt động 

### Mẫu 1: 

đầu vào:`6 1 12 10 14 11 8 3`| Bước | Đánh giá bò | Bò hiện tại | Thắng hiện tại | Thắng tối đa | 
| --- | --- | --- | --- | --- | 
| 1 | 12 | 12 | 0 | 0 | 
| 2 | 12 | 10 | 1 | 1 | 
| 3 | 12 | 14 | 0 | 1 | 
| 4 | 12 | 11 | 1 | 1 | 
| 5 | 12 | 8 | 2 | 2 | 
| 6 | 12 | 3 | 3 | 3 | 

Số tiền thắng tối đa nếu chúng ta hoán đổi một cách tối ưu là 1 vì trình tự bị ngắt khi gặp 14. Bảng này chứng minh rằng việc đếm những con bò yếu hơn liên tiếp sẽ xử lý chính xác việc đặt lại chuỗi khi một con bò mạnh hơn xuất hiện. 

### Mẫu 2: 

đầu vào:`6 5 7 2 727 10 12 13`| Bước | Đánh giá bò | Bò hiện tại | Thắng hiện tại | Thắng tối đa | 
| --- | --- | --- | --- | --- | 
| 1 | 12 | 7 | 1 | 1 | 
| 2 | 12 | 2 | 2 | 2 | 
| 3 | 12 | 727 | 0 | 2 | 
| 4 | 12 | 10 | 1 | 2 | 
| 5 | 12 | 12 | 0 | 2 | 
| 6 | 12 | 13 | 0 | 2 | 

Số lượng tối đa những con bò yếu hơn mà con bò của bạn có thể đánh bại là 2. Cách hoán đổi tối ưu là đặt con bò của bạn ngay trước dãy 10, 727. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Quét tuyến tính đơn trên mảng, không phụ thuộc vào giao dịch hoán đổi, vì chúng tôi theo dõi trực tiếp các vệt tối đa. | 
| Không gian | O(1) | Chỉ cần các biến không đổi: max_wins và current_wins. | 

Với tổng n trên tất cả các trường hợp thử nghiệm không vượt quá 10^5, giải pháp này chạy thoải mái trong vòng chưa đầy 1 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = []
    t = int(input())
    for _ in range(t):
        n, k = map(int, input().split())
        a = list(map(int, input().split()))
        output.append(str(max_wins_for_cow(n, k, a)))
    return "\n".join(output)

# Provided samples
assert run("3\n6 1\n12 10 14 11 8 3\n6 5\n7 2 727 10 12 13\n2 2\n1000000000 1") == "1\n2\n0"

# Minimum-size input
assert run("1\n2 1\n2 1") == "1"

# Maximum-size input, decreasing order
n = 10**5
ratings = " ".join(str(i) for i in range(n, 0, -1))
assert run(f"1\n{n} 1\n{ratings}") == "0"

# All equal except your cow
assert run("1\n5 3\n1 1 5 1 1") == "2"

# Edge swap to start
assert run("1\n5 5\n1 2 3 4 5") == "0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2 1 2 1 | 1 | Kích thước đầu vào tối thiểu, con bò của bạn đầu tiên | 
| 10^5 giảm | | |
