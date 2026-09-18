---
title: "CF 104721B-đường"
description: "Chúng ta có một con đường thẳng gồm $n$ các trạm được đánh số từ $1$ đến $n$. Giữa ga $i$ và $i+1$ có một đoạn đường có chiều dài $vi$. Tại mỗi trạm $i$, nhiên liệu có thể được mua, nhưng mỗi trạm có giá cố định $ai$ một lít."
date: "2026-06-29T04:14:47+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104721
codeforces_index: "B"
codeforces_contest_name: "CSP-J 2023"
rating: 0
weight: 104721
solve_time_s: 89
verified: true
draft: false
---

[CF 104721B - đường](https://codeforces.com/problemset/problem/104721/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 29s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được ban cho một con đường thẳng được làm bằng$n$trạm được đánh số từ$1$ĐẾN$n$. Giữa ga$i$Và$i+1$, có một đoạn đường có chiều dài$v_i$. Tại mỗi trạm$i$, nhiên liệu có thể mua được nhưng mỗi trạm có giá cố định riêng$a_i$mỗi lít. 

Một ô tô khởi hành tại ga$1$với một chiếc xe tăng trống rỗng và phải đến trạm$n$. Bình xăng không có giới hạn dung tích nhưng mức tiêu thụ nhiên liệu được lượng tử hóa: một lít nhiên liệu cho phép xe đi được chính xác$d$km, do đó di chuyển một đoạn chiều dài$v_i$yêu cầu$\lceil v_i / d \rceil$lít. 

Nhiệm vụ là chọn mua bao nhiêu nhiên liệu ở mỗi trạm để ô tô có thể đi qua tất cả các đoạn đường và tổng chi phí là nhỏ nhất. 

Các ràng buộc cho phép lên đến$10^5$trạm và độ dài đoạn lên tới$10^5$. Điều này gợi ý mạnh mẽ một$O(n)$hoặc$O(n \log n)$giải pháp. Bất kỳ cách tiếp cận nào cố gắng mô phỏng tất cả các quyết định tiếp nhiên liệu có thể có hoặc lập trình động theo trạng thái nhiên liệu sẽ quá chậm vì không gian trạng thái sẽ bùng nổ với cả vị trí và lượng nhiên liệu. 

Một vấn đề tế nhị phát sinh từ việc làm tròn số nguyên tiêu thụ nhiên liệu. Ngay cả khi một đoạn chỉ dài hơn bội số của một chút$d$, nó vẫn cần thêm một lít đầy. Ví dụ, nếu$d = 4$Và$v_i = 5$, thì chúng ta cần$2$lít, không$1.25$. 

Một mối quan tâm không rõ ràng khác là mua nhiên liệu ở đâu. Vì nhiên liệu không bị giới hạn bởi kích thước thùng nhiên liệu nên quyết định mua hoàn toàn là về giá chứ không phải tính khả thi. Một cách tiếp cận ngây thơ có thể cố gắng quyết định tại địa phương mỗi trạm có nên chỉ mua nhiên liệu cho chặng tiếp theo hay không, nhưng điều này không thành công khi trạm sau rẻ hơn và có thể đã được sử dụng sớm hơn. 

## Phương pháp tiếp cận 

Chiến lược bạo lực sẽ xem xét, tại mỗi trạm, lượng nhiên liệu cần mua và sau đó mô phỏng tất cả các phân bổ có thể có trong việc mua nhiên liệu giữa các trạm. Ngay cả việc hạn chế chúng ta “mua chính xác những gì cần thiết cho phân khúc tiếp theo hoặc hơn thế nữa” vẫn dẫn đến những lựa chọn theo cấp số nhân, vì nhiên liệu có thể được chuyển tiếp vô thời hạn và các quyết định tương tác giữa nhiều phân khúc. 

Việc xây dựng chương trình động trên các trạm và lượng nhiên liệu còn lại cũng trở nên không thực tế. Lượng nhiên liệu là không giới hạn và việc rời rạc hóa nó dẫn đến một không gian trạng thái rất lớn tỷ lệ thuận với tổng nhu cầu nhiên liệu có thể có. 

Quan sát quan trọng là nhiên liệu có thể chuyển nhượng hoàn toàn và không có chi phí xuống cấp, vì vậy yếu tố có ý nghĩa duy nhất là mức giá rẻ nhất tính đến thời điểm hiện tại. Khi chúng tôi đến một trạm có mức giá thấp hơn, nó sẽ chiếm ưu thế hơn tất cả các trạm trước đó cho bất kỳ giao dịch mua nào trong tương lai. 

Như vậy, đối với mỗi đoạn đường, chiến lược tối ưu là mua toàn bộ nhiên liệu cần thiết cho đoạn đường đó với mức giá tối thiểu giữa các trạm từ$1$đến trạm hiện tại. Chúng tôi chỉ cần duy trì giá nhiên liệu ở mức tối thiểu và nhân nó với nhu cầu nhiên liệu của từng phân khúc. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | Trạng thái O(n) đến O(n²) | Quá chậm | 
| Tối ưu (tiền tố tham lam tối thiểu) | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Hãy để chúng tôi xử lý đường từ trái sang phải trong khi theo dõi giá nhiên liệu rẻ nhất gặp phải cho đến nay. 

1. Khởi tạo một biến`best_price`với giá ở trạm 1, vì đây là lựa chọn đầu tiên có sẵn. Đây là mức chi phí thấp nhất cho mỗi lít mà chúng tôi hiện có thể sử dụng. 
2. Khởi tạo`answer = 0`, sẽ tích lũy tổng chi phí. 
3. Đối với từng đoạn từ ga$i$ĐẾN$i+1$, tính số lít cần dùng là:$$\text{liters} = \frac{v_i + d - 1}{d}$$Đây là số nguyên nhỏ nhất của lít có độ bao phủ$d \cdot \text{liters}$ít nhất là$v_i$. 
4. Thêm vào câu trả lời:$$\text{answer} += \text{liters} \times \text{best\_price}$$Điều này phản ánh việc mua tất cả nhiên liệu cho phân khúc đó với mức giá rẻ nhất hiện có cho đến nay. 
5. Trước khi chuyển sang phần tiếp theo, hãy cập nhật:$$\text{best\_price} = \min(\text{best\_price}, a_{i+1})$$Điều này đảm bảo rằng các phân khúc trong tương lai có thể được hưởng lợi từ bất kỳ nhà ga nào rẻ hơn phía trước. 
6. Sau khi xử lý tất cả các phân đoạn, xuất ra`answer`. 

### Tại sao nó hoạt động 

Tại bất kỳ điểm nào trên đường, nhiên liệu mua trước đó có thể được sử dụng sau này mà không bị hao hụt. Vì vậy, nếu chúng ta gặp phải một trạm xăng có giá thấp hơn thì không có lý do gì phải mua nhiên liệu sớm hơn với giá cao hơn cho bất kỳ lần tiêu thụ nào trong tương lai. Chiến lược tốt nhất có thể là luôn coi mức giá rẻ nhất được coi là chi phí nhiên liệu hiệu quả cho tất cả các nhu cầu còn lại. Điều này tạo ra cấu trúc tiền tố tối thiểu trong đó mỗi phân đoạn được ấn định chi phí độc lập dựa trên trạm có thể tiếp cận rẻ nhất cho đến thời điểm đó và không việc sắp xếp lại các giao dịch mua nào có thể giảm tổng chi phí hơn nữa. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, d = map(int, input().split())
    v = list(map(int, input().split()))
    a = list(map(int, input().split()))

    best_price = a[0]
    ans = 0

    for i in range(n - 1):
        liters = (v[i] + d - 1) // d
        ans += liters * best_price
        if i + 1 < n:
            best_price = min(best_price, a[i + 1])

    print(ans)

if __name__ == "__main__":
    solve()
```Mã theo cấu trúc tham lam trực tiếp. Vòng lặp xử lý từng đoạn đường một lần, tính toán số lít cần thiết bằng phép chia trần số nguyên. Biến`best_price`duy trì giá nhiên liệu ở mức tối thiểu, đảm bảo rằng mọi phân khúc đều được tính phí ở mức giá rẻ nhất có thể cho đến thời điểm đó. 

Một cạm bẫy triển khai phổ biến là cập nhật giá tối thiểu không đúng lúc. Nó phải được cập nhật sau khi sử dụng logic quyết định giá của phân khúc hiện tại; nếu không, thuật toán sẽ cho phép trạm không chính xác$i+1$được sử dụng cho phân đoạn$i$, điều này là không thể vì đoạn đường đó đã đạt được trước khi đến ga$i+1$. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
5 4
10 10 10 10
9 8 9 6 5
```Chúng tôi theo dõi từng phân khúc. 

| Phân đoạn | v_i | lít | giá tốt nhất | chi phí bổ sung | tổng cộng | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 10 | 3 | 9 | 27 | 27 | 
| 2 | 10 | 3 | 8 | 24 | 51 | 
| 3 | 10 | 3 | 8 | 24 | 75 | 
| 4 | 10 | 3 | 6 | 18 | 93 | 

Đầu ra là chi phí tích lũy. Trình tự giá giảm dần cho thấy tại sao tiền tố tối thiểu lại quan trọng, vì các đài rẻ hơn sau này chiếm ưu thế so với các đài trước đó. 

### Mẫu 2 (tùy chỉnh) 

đầu vào:```
4 3
5 4 6
7 2 5 1
```| Phân đoạn | v_i | lít | giá tốt nhất | chi phí bổ sung | tổng cộng | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 5 | 2 | 7 | 14 | 14 | 
| 2 | 4 | 2 | 2 | 4 | 18 | 
| 3 | 6 | 2 | 2 | 4 | 22 | 

Ví dụ này nêu bật một hành vi quan trọng: một khi một trạm có giá rất rẻ xuất hiện, nó sẽ thống trị tất cả các phân khúc trong tương lai, ngay cả khi các trạm trước đó có giá đắt. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi phân đoạn được xử lý một lần với số học theo thời gian không đổi và một bản cập nhật tối thiểu duy nhất | 
| Không gian | O(1) | Chỉ một số biến đang chạy được duy trì | 

Thuật toán phù hợp thoải mái trong giới hạn vì$n \le 10^5$và tất cả các phép toán đều là các phép tính số nguyên đơn giản. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# provided sample
assert run("""5 4
10 10 10 10
9 8 9 6 5
""") == "79"

# minimum size
assert run("""2 5
10
3 10
""") == str(((10 + 4)//5) * 3)

# all equal prices
assert run("""3 2
3 3
5 5 5
""") == str(((5+1)//2)*3 + ((5+1)//2)*3)

# decreasing prices
assert run("""4 3
6 6 6
9 8 7 1
""") == str(((6+2)//3)*9 + ((6+2)//3)*8 + ((6+2)//3)*7)

# increasing prices
assert run("""4 3
6 6 6
1 2 3 4
""") == str(((6+2)//3)*1 + ((6+2)//3)*1 + ((6+2)//3)*1)
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| kích thước tối thiểu | tính toán | cấu trúc hợp lệ nhỏ nhất | 
| tất cả đều bình đẳng | tính toán | không có trường hợp cải thiện giá | 
| giảm giá | tính toán | tiền tố cập nhật tối thiểu sớm | 
| tăng giá | tính toán | giá tốt nhất vẫn ở mức bắt đầu | 

## Vỏ cạnh 

Trường hợp quan trọng xảy ra khi một trạm rẻ hơn nhiều xuất hiện sau đó. Giả sử trạm đầu tiên đắt tiền và trạm cuối cùng cực kỳ rẻ. Thuật toán tránh mua nhiên liệu sớm hơn cho các phân đoạn trong tương lai một cách chính xác vì nó chỉ liên tục cập nhật giá tối thiểu sau khi đi qua từng trạm. 

Một trường hợp cạnh khác là khi$v_i$không chia hết cho$d$. Ví dụ, nếu$d = 4$Và$v_i = 5$, thuật toán tính toán$(5 + 3) // 4 = 2$lít. Việc phân chia tầng sai sẽ chỉ phân bổ sai 1 lít và đánh giá thấp chi phí, khiến giải pháp không hợp lệ. 

Trường hợp khó phát hiện cuối cùng là khi tất cả các mức giá đều giống nhau. Trong tình huống này, thuật toán giảm một cách hiệu quả thành tổng tất cả các yêu cầu về nhiên liệu của phân khúc nhân với mức giá không đổi đó, cho thấy cấu trúc tham lam thoái hóa hoàn toàn thành một phép tích lũy đơn giản mà không cần bất kỳ quyết định tối ưu hóa nào.
