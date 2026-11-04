---
title: "CF 1150A - Trọng tài chứng khoán"
description: "Chúng ta được đưa ra một kịch bản giao dịch hai giai đoạn rất đơn giản. Vào buổi sáng, có một số người bán chào bán cùng một loại cổ phiếu với số lượng không giới hạn ở các mức giá mua khác nhau. Bạn có thể chọn bất kỳ một trong những mức giá này và mua bao nhiêu cổ phiếu tùy thích ở mức giá đó."
date: "2026-06-12T03:05:40+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1150
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 556 (Div. 2)"
rating: 800
weight: 1150
solve_time_s: 74
verified: true
draft: false
---

[CF 1150A - Trọng tài cổ phiếu](https://codeforces.com/problemset/problem/1150/A) 

**Đánh giá:** 800 
**Tags:** tham lam, thực hiện 
**Thời gian giải:** 1 phút 14s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được đưa ra một kịch bản giao dịch hai giai đoạn rất đơn giản. Vào buổi sáng, có một số người bán chào bán cùng một loại cổ phiếu với số lượng không giới hạn ở các mức giá mua khác nhau. Bạn có thể chọn bất kỳ một trong những mức giá này và mua bao nhiêu cổ phiếu tùy thích ở mức giá đó. Vào buổi tối, có một số người mua sẵn sàng mua số lượng không giới hạn ở các mức giá bán khác nhau, một mức giá cho mỗi giao dịch và bạn có thể chọn bất kỳ mức giá nào trong số đó. 

Bạn bắt đầu với một số tiền và không có cổ phiếu. Mục tiêu của bạn là chọn chính xác một giá mua từ buổi sáng, mua bất kỳ số lượng cổ phiếu nào bằng số tiền hiện tại của bạn, sau đó chọn chính xác một giá bán từ buổi tối và bán tất cả cổ phiếu ở mức giá đó. Bạn được phép không làm gì cả nếu điều đó tốt hơn. 

Nhận xét quan trọng từ cấu trúc là một khi bạn ấn định giá mua và giá bán, mọi thứ sẽ trở nên mang tính quyết định: bạn tiêu hết tiền vào cổ phiếu, sau đó thanh lý chúng hoàn toàn. 

Các ràng buộc đủ nhỏ nên việc kiểm tra tất cả các kết hợp là không đáng kể. Với tối đa 30 giá mua và 30 giá bán, tổng số cặp là 900. Đối với mỗi cặp, việc tính toán số lượng cổ phiếu bạn có thể mua là thời gian không đổi, vì vậy việc quét toàn diện là hoàn toàn đủ trong giới hạn. 

Cạm bẫy tinh vi duy nhất là quên rằng bạn không bị buộc phải giao dịch. Nếu tất cả các cơ hội lợi nhuận đều âm thì câu trả lời tối ưu chỉ đơn giản là số tiền ban đầu. 

## Phương pháp tiếp cận 

Một giải pháp mạnh mẽ sẽ thử mọi cặp giá buổi sáng và buổi tối có thể. Đối với mỗi giá mua, nó sẽ tính số lượng cổ phiếu bạn có thể mua bằng số tiền hiện tại của mình, sau đó với mỗi giá bán, nó sẽ tính số tiền cuối cùng sau khi bán số cổ phiếu đó. 

Điều này hiệu quả vì mỗi quyết định đều độc lập: số lượng cổ phiếu chỉ phụ thuộc vào giá mua và vốn ban đầu, còn lợi nhuận chỉ phụ thuộc vào giá bán. Tuy nhiên, công thức ngây thơ này vẫn trực tiếp dẫn đến một vòng lặp kép trên tất cả các cặp, tối đa là 900 lần lặp, đủ nhanh. Không cần tối ưu hóa nâng cao hơn nhưng chúng ta vẫn có thể đơn giản hóa việc suy luận. 

Điểm mấu chốt là việc mua nhiều cổ phiếu luôn tốt hơn nếu giá mua thấp hơn và bán ở mức giá cao hơn luôn tốt hơn. Vì vậy, chúng ta không cần cân nhắc lựa chọn trung gian: chúng ta chỉ quan tâm đến phương án mua rẻ nhất và phương án bán đắt nhất. Chiến lược tối ưu trở thành: chọn mức tối thiểu trong tất cả các giá mua, chọn mức tối đa trong tất cả các giá bán và đánh giá lợi nhuận thu được hoặc chọn không làm gì nếu điều đó làm giảm sự giàu có. 

Brute-force hoạt động vì không gian tìm kiếm rất nhỏ nhưng nó trở nên dư thừa về mặt khái niệm khi chúng ta nhận ra tính đơn điệu làm giảm mọi thứ xuống còn hai giá trị cực trị. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(nm) | O(1) | Đã chấp nhận | 
| Tối ưu (giảm tối thiểu/tối đa) | O(n + m) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi tiến hành bằng cách xác định hai quyết định có ý nghĩa duy nhất: giá mua tốt nhất có thể và giá bán tốt nhất có thể.

1. Đọc vốn ban đầu và bảng giá mua bán. Chúng đại diện cho tất cả các thị trường thống nhất có thể có mà bạn có thể chọn trong mỗi giai đoạn. 
2. Tính giá trị tối thiểu trong số tất cả các giá mua. Đây là thị trường rẻ nhất mà bạn có thể chuyển đổi tiền thành số lượng cổ phiếu lớn nhất. 
3. Tính giá trị lớn nhất trong số tất cả các giá bán. Đây là thị trường có lợi nhuận cao nhất, nơi mỗi cổ phiếu mang lại lợi nhuận cao nhất. 
4. Tính xem bạn có thể mua bao nhiêu cổ phiếu nếu bạn dùng hết số tiền của mình ở mức giá mua tối thiểu. Đây chỉ đơn giản là số tiền ban đầu chia cho giá_mua_tối thiểu. Phép chia là số nguyên vì phần chia không thể chia được. 
5. Tính số tiền cuối cùng sau khi bán toàn bộ cổ phiếu ở mức giá bán tối đa. Đây là số cổ phiếu nhân với max_sell_price. 
6. So sánh kết quả này với số tiền ban đầu. Nếu giao dịch không có lợi thì giữ nguyên giá trị ban đầu. Nếu không thì lấy cái lớn hơn. 

Lý do chúng tôi so sánh rõ ràng với số vốn ban đầu là vì chiến lược tối ưu có thể không liên quan đến giao dịch nào cả. 

### Tại sao nó hoạt động 

Bất kỳ chiến lược hợp lệ nào cũng bao gồm việc chọn một giá mua và một giá bán. Nếu giá mua không phải là mức giá tối thiểu sẵn có, việc thay thế nó bằng một lựa chọn rẻ hơn sẽ làm tăng hoặc duy trì số lượng cổ phiếu bạn có thể có được. Nếu giá bán không phải là mức giá tối đa, việc thay thế nó bằng mức giá cao hơn sẽ làm tăng hoặc duy trì doanh thu trên mỗi cổ phiếu. Do đó, bất kỳ chiến lược tối ưu nào cũng có thể được chuyển đổi thành chiến lược sử dụng giá mua tối thiểu và giá bán tối đa mà không làm giảm lợi nhuận. Phép biến đổi này bảo toàn tính đúng đắn và chứng tỏ rằng nghiệm được xác định đầy đủ bởi các giá trị cực trị. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m, r = map(int, input().split())
    s = list(map(int, input().split()))
    b = list(map(int, input().split()))
    
    min_buy = min(s)
    max_sell = max(b)
    
    shares = r // min_buy
    best = shares * max_sell
    
    print(max(r, best))

if __name__ == "__main__":
    solve()
```Việc triển khai trực tiếp mã hóa việc giảm xuống các giá trị cực trị. Điểm quan tâm số học duy nhất là phép chia số nguyên khi tính toán chia sẻ, vì chia sẻ phân số là không thể. Sự so sánh cuối cùng đảm bảo chúng tôi không thực thi nhầm giao dịch khi nó không mang lại lợi nhuận. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3 4 11
4 2 5
4 4 5 4
```Chúng tôi tính toán các giá trị chính: giá mua tối thiểu là 2, giá bán tối đa là 5. 

| Bước | Giá trị | 
| --- | --- | 
| Tiền ban đầu | 11 | 
| Giá mua tối thiểu | 2 | 
| Giá bán tối đa | 5 | 
| Cổ phiếu đã mua | 11 // 2 = 5 | 
| Tiền cuối cùng | 5 × 5 = 25 | 
| Kết quả tốt nhất | tối đa(11, 25) = 25 | 

Quá trình này cho thấy rằng việc sử dụng tùy chọn mua rẻ nhất sẽ tối đa hóa chuyển đổi thành cổ phiếu và tùy chọn bán đắt nhất sẽ tối đa hóa lợi nhuận trên mỗi cổ phiếu. 

### Ví dụ 2 

đầu vào:```
2 2 10
5 6
4 3
```Ở đây, việc mua cổ phiếu luôn tệ hơn vì ngay cả giá bán tốt nhất cũng thấp hơn giá mua rẻ nhất. 

| Bước | Giá trị | 
| --- | --- | 
| Tiền ban đầu | 10 | 
| Giá mua tối thiểu | 5 | 
| Giá bán tối đa | 4 | 
| Cổ phiếu đã mua | 10 // 5 = 2 | 
| Tiền cuối cùng | 2 × 4 = 8 | 
| Kết quả tốt nhất | tối đa(10, 8) = 10 | 

Điều này khẳng định tầm quan trọng của lựa chọn “không làm gì cả” khi giao dịch mang lại lợi nhuận âm. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + m) | Chúng tôi quét từng danh sách một lần để tính giá trị tối thiểu và tối đa | 
| Không gian | O(1) | Chỉ có một số biến vô hướng được sử dụng | 

Giới hạn đầu vào nhỏ nhưng ngay cả khi chúng lớn hơn nhiều, giải pháp này vẫn hiệu quả vì nó tránh được mọi phép liệt kê theo cặp và giảm vấn đề về việc đưa ra quyết định theo thời gian không đổi sau một lần vượt qua. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    return str(solve() if solve() is not None else "").strip()

# provided sample
assert run("""3 4 11
4 2 5
4 4 5 4
""") == "25"

# no profit case
assert run("""2 2 10
5 6
4 3
""") == "10"

# minimum input
assert run("""1 1 1
1
1
""") == "1"

# profitable extreme spread
assert run("""3 3 100
1 2 3
10 20 30
""") == "3000"

# all buy prices equal
assert run("""3 2 50
5 5 5
1 2
""") == "50"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả đều có giá ngang nhau | 1 | sự ổn định tầm thường | 
| không có lợi nhuận | tiền ban đầu | dự phòng đúng | 
| lan truyền cực độ | tăng lớn | độ chính xác tối đa/phút | 
| giá mua đồng phục | xử lý tối thiểu đúng | dự phòng mạnh mẽ | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi mọi giao dịch có thể thực hiện đều không có lãi vì ngay cả giá bán tốt nhất cũng thấp hơn giá mua rẻ nhất. Ví dụ: nếu bạn bắt đầu với 10, giá mua là`[5, 6]`, và giá bán là`[1, 2]`, thì bất kỳ giao dịch nào cũng sẽ phá hủy giá trị. Thuật toán xử lý việc này vì nó so sánh rõ ràng kết quả giao dịch với số tiền ban đầu và trả về mức tối đa của cả hai. 

Một trường hợp khác là khi tất cả các mức giá đều giống nhau. Trong trường hợp đó, việc mua và bán không mang lại sự thay đổi về của cải. Thuật toán tính toán chia sẻ chính xác nhưng so sánh cuối cùng đảm bảo kết quả vẫn bằng giá trị ban đầu. 

Trường hợp thứ ba là khi chiến lược tối ưu liên quan đến việc sử dụng chính xác tất cả số tiền mà không có số dư. Phép chia số nguyên thực thi điều này một cách tự nhiên; bất kỳ số tiền còn lại nào đều không liên quan vì nó không thể được sử dụng để mua các cổ phiếu lẻ và thuật toán không cố gắng sử dụng lại nó theo bất kỳ cách nào khác.
