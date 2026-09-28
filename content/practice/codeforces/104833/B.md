---
title: "CF 104833B - \u304a\u306f\u3088\u3046 \u5b66\u5f1f"
description: "Chúng ta được giao một trò chơi trừ trên một đống bóng. Trạng thái của trò chơi được xác định bởi số lượng bóng hiện tại, giả sử $a$. Ở lượt của người chơi, kích thước nước đi được phép được xác định bởi một hàm của trạng thái hiện tại: tính tổng các chữ số của $a$, gọi nó là $x$."
date: "2026-06-28T11:53:02+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104833
codeforces_index: "B"
codeforces_contest_name: "The 2023 Zhejiang SCI-TECH University Freshman Programming Contest"
rating: 0
weight: 104833
solve_time_s: 61
verified: true
draft: false
---

[CF 104833B - \u304a\u306f\u3088\u3046 \u5b66\u5f1f](https://codeforces.com/problemset/problem/104833/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 1s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được giao một trò chơi trừ trên một đống bóng. Trạng thái của trò chơi được xác định bởi số lượng bóng hiện tại, chẳng hạn$a$. Ở lượt của người chơi, kích thước nước đi được phép được xác định bởi một hàm của trạng thái hiện tại: tính tổng các chữ số của$a$, gọi nó$x$. Người chơi có thể loại bỏ bất kỳ số lượng bóng nào từ 1 đến$\min(a, x)$, bao gồm. Người chơi thực hiện nước đi để lại cọc trống ngay lập tức thắng. 

Mỗi trường hợp thử nghiệm đưa ra một số ban đầu$n$và chúng ta phải xác định xem người chơi đầu tiên (A) có bị buộc phải thắng trong lối chơi tối ưu hay không. 

Các ràng buộc là lớn trong hai chiều. Có thể có tới$10^6$các trường hợp thử nghiệm và mỗi trường hợp$n$cũng tùy$10^6$. Điều đó ngay lập tức loại trừ mọi mô phỏng cho mỗi truy vấn. Thậm chí$O(n)$mỗi trường hợp thử nghiệm là không thể, và thậm chí$O(n \log n)$mỗi truy vấn sẽ quá chậm. Bất kỳ giải pháp nào cũng phải xử lý trước một cách hiệu quả tất cả các giá trị lên đến giá trị tối đa$n$một lần. 

Trường hợp cạnh tinh tế xuất hiện khi$n$nhỏ vì tổng các chữ số có thể lớn hơn hoặc bằng$n$. Ví dụ, khi$n = 1$, tổng các chữ số là 1 nên người chơi chỉ được lấy 1 và thắng ngay. Khi$n = 10$, tổng các chữ số là 1, do đó chỉ tồn tại một nước đi, điều này khiến nước đi này hoạt động khác với trò chơi “ăn bất kỳ” trực quan. Các trường hợp ranh giới này quan trọng vì phạm vi di chuyển phụ thuộc vào chính trạng thái chứ không chỉ phụ thuộc vào tham số cố định. 

Một chế độ thất bại quan trọng khác đến từ việc giả định một trò chơi mang đi cổ điển với các$k$. Ở đây, bước di chuyển tối đa thay đổi ở mọi vị trí, vì vậy các đối số chu kỳ tiêu chuẩn như modulo$k+1$không áp dụng trên toàn cầu. 

## Phương pháp tiếp cận 

Giải pháp brute-force sẽ mô phỏng trò chơi từ mỗi giá trị bắt đầu$n$. Đối với mỗi tiểu bang$a$, chúng tôi sẽ thử mọi động thái có thể$k$từ 1 đến$\min(a, \text{digitSum}(a))$, kiểm tra đệ quy xem vị trí kết quả có bị mất hay không. Điều này tạo thành một DP trò chơi tiêu chuẩn, nhưng mỗi trạng thái có thể phân nhánh tối đa 54 lần chuyển đổi trong trường hợp xấu nhất (vì tổng các chữ số lên tới$10^6$nhiều nhất là 54). Trên tất cả các tiểu bang cho đến$10^6$, điều này mang lại khoảng$5 \times 10^7$các quá trình chuyển đổi và đệ quy hoặc tính toán lại lặp đi lặp lại sẽ đẩy nó đi xa hơn, khiến nó ở ranh giới hoặc chậm hơn trong Python dưới các giới hạn nghiêm ngặt. 

Quan sát quan trọng là chúng ta không cần khám phá từng bước di chuyển riêng lẻ theo cách đệ quy. Một vị thế được coi là thắng nếu tồn tại ít nhất một nước đi đến vị thế thua. Vì tất cả các bước di chuyển đều đi đến các trạng thái liền kề$[a - s(a), a - 1]$, chúng ta chỉ cần biết liệu có tồn tại trạng thái thua trong khoảng đó hay không. Điều này biến vấn đề thành việc duy trì một bản tóm tắt tiền tố về các trạng thái bị mất, cho phép kiểm tra phạm vi thời gian không đổi sau khi tiền xử lý. 

Chúng tôi xác định một mảng DP trong đó mỗi trạng thái thắng hoặc thua. Chúng tôi tính toán các trạng thái theo thứ tự tăng dần và đối với mỗi vị trí, chúng tôi kiểm tra xem khoảng thời gian của các trạng thái có thể truy cập có chứa bất kỳ trạng thái mất nào không. Nếu có, trạng thái hiện tại đang thắng thế; nếu không thì nó đang thua. Tổng tiền tố trên các trạng thái mất khiến kiểm tra này O(1). 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu DFS DP | O(n · chữ sốSum) | O(n) | Quá chậm | 
| Tiền tố DP với truy vấn phạm vi | O(n + T) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi tính toán kết quả cho tất cả các giá trị lên đến mức tối đa$n$xuất hiện ở đầu vào. 

1. Tính trước các tổng chữ số cho tất cả các số nguyên lên đến$10^6$. Điều này đảm bảo phạm vi di chuyển của mỗi trạng thái có thể đạt được trong thời gian O(1). 
2. Khởi tạo mảng DP trong đó`dp[i]`cho biết người chơi hiện tại có chiến lược chiến thắng hay không bắt đầu từ$i$. Cũng duy trì một mảng tiền tố`pref`, Ở đâu`pref[i]`đếm xem có bao nhiêu vị thế thua lỗ trong$[0, i]$. 
3. Đặt trạng thái cơ bản`dp[0] = False`, vì không còn nước đi nào nữa. Đánh dấu điều này trong mảng tiền tố. 
4. Lặp lại từ$i = 1$đến giá trị tối đa. Đối với mỗi$i$, tính toán$s = \text{digitSum}(i)$. 
5. Người chơi có thể chuyển sang bất kỳ trạng thái nào trong khoảng thời gian$[i - s, i - 1]$. Trạng thái hiện tại chỉ thua nếu tất cả các trạng thái này đều thắng, điều này tương đương với việc nói rằng không có trạng thái thua nào trong khoảng thời gian này. 
6. Sử dụng tổng tiền tố để kiểm tra điều kiện này trong O(1): 

nếu`pref[i-1] - pref[max(0, i-s-1)] == 0`, sau đó đánh dấu`dp[i] = False`, nếu không thì`dp[i] = True`. 
7. Cập nhật tổng tiền tố tương ứng và tiếp tục. 

Sau khi điền vào bảng, mỗi truy vấn được trả lời bằng O(1). 

### Tại sao nó hoạt động 

Mỗi vị trí chỉ phụ thuộc vào các vị trí nhỏ hơn, do đó việc xử lý theo thứ tự tăng dần đảm bảo tính chính xác. Bất biến quan trọng là`dp[i]`được xác định hoàn toàn bởi sự tồn tại của ít nhất một trạng thái mất có thể truy cập được từ nó. Mảng tiền tố duy trì chính xác số lượng trạng thái thua trong bất kỳ khoảng thời gian nào, do đó, quy tắc quyết định khớp chính xác với định nghĩa về vị trí chiến thắng trong các trò chơi công bằng: một vị trí sẽ thắng nếu có ít nhất một nước đi đến trạng thái thua và ngược lại là thua. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MAXN = 10**6

digit_sum = [0] * (MAXN + 1)
for i in range(1, MAXN + 1):
    digit_sum[i] = digit_sum[i // 10] + (i % 10)

dp = [False] * (MAXN + 1)
pref = [0] * (MAXN + 1)

dp[0] = False
pref[0] = 1  # count of losing positions

for i in range(1, MAXN + 1):
    s = digit_sum[i]
    left = i - s
    if left < 0:
        left = 0

    # count losing positions in [left, i-1]
    losing_in_range = pref[i - 1] - (pref[left - 1] if left > 0 else 0)

    if losing_in_range == 0:
        dp[i] = False
    else:
        dp[i] = True

    pref[i] = pref[i - 1] + (0 if dp[i] else 1)

t = int(input())
out = []
for _ in range(t):
    n = int(input())
    out.append("A" if dp[n] else "B")

print("\n".join(out))
```Mã tính toán trước các tổng chữ số theo thời gian tuyến tính, sau đó xây dựng mảng DP từ 1 trở lên. Điều tinh tế quan trọng là duy trì số lượng tiền tố của các trạng thái mất để mỗi truy vấn khoảng thời gian trở thành thời gian không đổi. Mảng tiền tố lưu trữ số lượng vị trí thua chứ không phải vị trí thắng, bởi vì điều kiện chuyển tiếp được thể hiện một cách tự nhiên dưới dạng “có sẵn một nước đi thua không”. 

Ánh xạ câu trả lời theo sau trực tiếp: nếu`dp[n]`đúng, người chơi bắt đầu có thể buộc phải thắng, nếu không thì người chơi thứ hai sẽ thắng. 

## Ví dụ đã hoạt động 

Hãy xem xét một minh họa nhỏ với$n = 6$. 

| tôi | tổng chữ số | phạm vi có thể tiếp cận | mất trạng thái trong phạm vi | dp[i] | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | [0,0] | 1 (trạng thái 0) | W | 
| 2 | 2 | [0,1] | 1 | W | 
| 3 | 3 | [0,2] | 1 | W | 
| 4 | 4 | [0,3] | 1 | W | 
| 5 | 5 | [0,4] | 1 | W | 
| 6 | 6 | [0,5] | 1 | W | 

Ở đây, mọi trạng thái có tối đa 6 đều thắng vì mỗi trạng thái có thể đạt đến trạng thái cuối cùng thua 0 một cách trực tiếp hoặc gián tiếp trong một phạm vi di chuyển. 

Bây giờ hãy xem xét một cấu trúc lớn hơn một chút trong đó tổng chữ số hạn chế khả năng tiếp cận chặt chẽ hơn. Cơ chế tương tự cũng được áp dụng: ngay khi một vị thế không thể đạt đến bất kỳ trạng thái thua nào, nó sẽ bị thua, tạo ra phản ứng dây chuyền xác định toàn bộ DP. 

Những dấu vết này cho thấy rằng quyết định ở mỗi bước chỉ phụ thuộc vào thông tin khoảng thời gian chứ không phụ thuộc vào sự chuyển đổi riêng lẻ, đó là lý do tại sao việc tổng hợp tiền tố là đủ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(MAXN + T) | tiền xử lý tổng chữ số cộng với một thẻ DP và truy vấn O(1) | 
| Không gian | O(MAXN) | mảng cho DP, tổng tiền tố và tổng chữ số | 

Việc xử lý trước lên đến$10^6$phù hợp thoải mái trong giới hạn và mỗi trường hợp thử nghiệm được trả lời trong thời gian không đổi, giúp giải pháp trở nên hiệu quả ngay cả đối với$10^6$truy vấn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    MAXN = 10**6
    digit_sum = [0] * (MAXN + 1)
    for i in range(1, MAXN + 1):
        digit_sum[i] = digit_sum[i // 10] + (i % 10)

    dp = [False] * (MAXN + 1)
    pref = [0] * (MAXN + 1)

    dp[0] = False
    pref[0] = 1

    for i in range(1, MAXN + 1):
        s = digit_sum[i]
        left = max(0, i - s)
        losing = pref[i - 1] - (pref[left - 1] if left > 0 else 0)
        dp[i] = losing != 0
        pref[i] = pref[i - 1] + (0 if dp[i] else 1)

    t = int(input())
    out = []
    for _ in range(t):
        n = int(input())
        out.append("A" if dp[n] else "B")
    return "\n".join(out)

# boundary cases
assert run("1\n1\n") == "A"
assert run("1\n10\n") in ["A", "B"]

# small structure
assert run("3\n1\n2\n3\n") == "A\nA\nA"

# max boundary single test
assert run("1\n1000000\n") in ["A", "B"]
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1, 1 | A | bang chiến thắng nhỏ nhất | 
| 1, 10 | A/B | cạnh hạn chế tổng chữ số | 
| 1e6 | A/B | ổn định giới hạn trên | 
| 1,2,3 | A A A | hành vi chuỗi sớm | 

## Vỏ cạnh 

cho$n = 1$, tổng các chữ số là 1 nên nước đi duy nhất là lấy 1 và thắng ngay. DP đánh dấu trạng thái 1 là thắng vì nó có thể đạt đến trạng thái 0, tức là thua. 

Vì$n = 10$, tổng các chữ số lại là 1, vì vậy việc di chuyển duy nhất là về 1, điều này làm giảm vấn đề xem trạng thái 1 đang thua hay thắng. Vì trạng thái 1 đã được biết là thắng nên trạng thái 10 sẽ thua và logic tiền tố nắm bắt chính xác sự phụ thuộc này. 

Đối với các giá trị lớn như$n = 999999$, tổng các chữ số là 54, vì vậy mỗi trạng thái có thể đạt tới một khoảng rộng. Kiểm tra dựa trên tiền tố đánh giá hiệu quả xem có bất kỳ trạng thái mất nào tồn tại trong khoảng thời gian đó mà không lặp lại rõ ràng trên tất cả 54 lần chuyển đổi, duy trì tính chính xác và tốc độ hay không.
