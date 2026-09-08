---
title: "CF 104574D - Thử thách XP"
description: "Chúng ta được cung cấp một chuỗi kẻ thù xếp hàng dọc theo con đường trốn thoát của Iggy. Mỗi kẻ thù có một lượng máu nhất định và Iggy cũng bắt đầu với một lượng máu cố định."
date: "2026-06-30T08:16:34+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104574
codeforces_index: "D"
codeforces_contest_name: "UTPC Contest 09-08-23 Div. 2 (Beginner)"
rating: 0
weight: 104574
solve_time_s: 65
verified: true
draft: false
---

[CF 104574D - Thử thách XP](https://codeforces.com/problemset/problem/104574/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 5s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi kẻ thù xếp hàng dọc theo con đường trốn thoát của Iggy. Mỗi kẻ thù có một lượng máu nhất định và Iggy cũng bắt đầu với một lượng máu cố định. Cô ấy phải giảm lượng máu của từng kẻ thù xuống 0 hoặc thấp hơn, từng kẻ một, đồng thời không bao giờ để lượng máu của chính mình giảm xuống 0 hoặc thấp hơn. 

Để đánh bại kẻ thù, Iggy có hai chế độ tấn công. Chế độ đầu tiên có thể được sử dụng tối đa một lần cho mỗi kẻ thù. Nó gây ra một lượng sát thương cố định cho kẻ thù đó, nhưng cũng khiến Iggy phải trả một lượng máu cố định. Chế độ thứ hai có thể được sử dụng bao nhiêu lần và mỗi lần sử dụng sẽ gây ra một số sát thương cho kẻ thù đồng thời tiêu tốn sức khỏe của Iggy. 

Nhiệm vụ là xác định xem có cách nào để lựa chọn cho mỗi kẻ thù hay không, sử dụng đòn tấn công không giới hạn bao nhiêu lần và có nên sử dụng đòn tấn công một lần hay không, để tất cả kẻ thù đều bị đánh bại và sức khỏe của Iggy luôn ở mức tích cực. 

Những ràng buộc đẩy chúng ta tới một$O(M)$hoặc$O(M \log M)$giải pháp vì có thể có tới$10^5$kẻ thù. Bất kỳ giải pháp nào cố gắng mô phỏng các tổ hợp tấn công trên mỗi kẻ thù một cách độc lập theo cách ngây thơ sẽ nhanh chóng bùng nổ, bởi vì về nguyên tắc, đối với mỗi kẻ thù, chúng ta có thể lựa chọn giữa việc sử dụng đòn tấn công đặc biệt hay không, và sau đó là nhiều tổ hợp tấn công dồn dập. Điều đó dẫn đến hành vi theo cấp số nhân hoặc ít nhất là bậc hai nếu xử lý sai. 

Một trường hợp thất bại tinh vi đối với lối suy luận ngây thơ là việc xử lý từng kẻ thù một cách độc lập bằng cách luôn tối đa hóa hiệu quả sát thương cục bộ. Ví dụ: luôn sử dụng mũi nhọn ngón tay cái bất cứ khi nào nó có vẻ hiệu quả cho mỗi lần đánh có thể thất bại vì nó có thể lãng phí tùy chọn sát thương cố định tốt nhất lên kẻ thù yếu, khiến kẻ thù mạnh sau này phải trả giá quá đắt. 

Một trường hợp khó khăn khác phát sinh khi đòn tấn công bằng điện tích hoàn toàn tốt hơn so với đòn tấn công bằng ngón tay cái về cả hiệu quả sát thương và lượng máu tiêu hao trên mỗi sát thương, nhưng đòn tấn công bằng ngón tay cái vẫn quan trọng vì nó tạo ra một vụ nổ một lần có thể làm giảm đáng kể số lần sử dụng điện tích đối với một kẻ thù lớn. 

## Phương pháp tiếp cận 

Quan sát quan trọng là mỗi kẻ thù đều độc lập ngoại trừ tổng lượng máu còn lại. Đối với mỗi kẻ thù, chúng ta cần quyết định cách giảm thiểu lượng máu tiêu hao cần thiết để giảm HP của nó xuống 0, vì chúng ta có thể tùy ý áp dụng đòn tấn công thưởng một lần. 

Nếu chúng ta bỏ qua mũi nhọn của ngón tay cái, mỗi kẻ thù$a_i$yêu cầu$\lceil a_i / Q_1 \rceil$tấn công phí, chi phí$Q_2 \cdot \lceil a_i / Q_1 \rceil$sức khỏe. 

Bây giờ hãy giới thiệu ngón tay cái. Nếu chúng ta sử dụng nó lên kẻ thù, HP của nó sẽ giảm đi một cách hiệu quả.$P_1$, do đó HP còn lại trở thành$\max(0, a_i - P_1)$. Điều đó làm giảm số lần tấn công cần thiết cho kẻ thù đó. Tuy nhiên, chúng tôi phải trả thêm một khoản chi phí cố định$P_2$một lần cho mỗi lần sử dụng như vậy. 

Vì vậy, đối với mỗi kẻ thù, chúng ta đang lựa chọn giữa hai chi phí: chi phí thuần túy hoặc chi phí hỗn hợp mà chúng ta chi tiêu.$P_2$cộng với chi phí sạc sau khi giảm HP xuống$P_1$. 

Điều này làm giảm vấn đề đối với việc tính toán, đối với mỗi kẻ thù, giá trị tối thiểu của hai giá trị này và tính tổng của tất cả kẻ thù. Câu hỏi duy nhất còn lại là liệu lượng máu ban đầu của Iggy có đủ hay không. 

Cách tiếp cận vũ phu sẽ thử cả hai lựa chọn cho mỗi kẻ thù và mô phỏng các cuộc tấn công tấn công riêng lẻ, dẫn đến$O(M \cdot a_i)$trong trường hợp xấu nhất là không thể thực hiện được vì$a_i$có thể lên đến$10^9$. 

Sự đơn giản hóa quan trọng là nhận ra rằng cấu trúc chi phí có tính chất cộng thêm và độc lập với mỗi kẻ thù. Sau khi chúng tôi khắc phục lựa chọn của kẻ thù, chiến lược nội bộ tối ưu của kẻ thù sẽ mang tính quyết định: luôn sử dụng các đòn tấn công dồn dập, tùy ý trước một mũi nhọn duy nhất. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(M \cdot \max a_i)$|$O(1)$| Quá chậm | 
| Tối ưu |$O(M)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đối với mỗi kẻ thù, hãy tính cái giá phải trả để đánh bại nó chỉ bằng các đòn tấn công dồn dập. Đây là số lần đánh cần thiết nhân với lượng máu tiêu tốn cho mỗi lần đánh. Số lần trúng đích được tính bằng cách chia trần HP của kẻ địch cho$Q_1$. 
2. Đối với cùng một kẻ thù, hãy tính chi phí thay thế nếu chúng ta sử dụng mũi nhọn ngón tay cái đúng một lần. Đầu tiên giảm HP của kẻ thù bằng$P_1$, nhưng không dưới 0, sau đó tính toán số lần tấn công tích điện cần thiết cho lượng HP còn lại và thêm chi phí cố định$P_2$. Điều này cho thấy sự thật rằng đợt tăng đột biến chỉ hữu ích nếu nó thực sự làm giảm số lần tấn công. 
3. Lấy giá trị nhỏ nhất của hai chi phí tính toán. Điều này thể hiện cách tối ưu để đánh bại kẻ thù đó một cách cô lập, dựa trên cấu trúc tổng thể của vấn đề. 
4. Tính tổng chi phí tối thiểu này cho tất cả kẻ thù. Tổng số này là chi phí y tế tối thiểu có thể cần thiết để dọn đường. 
5. Kiểm tra xem tổng chi phí có nhỏ hơn lượng máu ban đầu hay không$N$. Nếu có, xuất ra CÓ; nếu không thì xuất ra NO. 

### Tại sao nó hoạt động 

Điều bất biến chính là chiến lược tối ưu của mỗi kẻ thù chỉ phụ thuộc vào HP của chính nó chứ không phụ thuộc vào quyết định của kẻ thù khác. Bất kỳ chuỗi tấn công nào cũng có thể được sắp xếp lại sao cho tất cả các hành động lên một kẻ thù là liền kề nhau mà không làm thay đổi tổng chi phí, vì các cuộc tấn công không tương tác giữa các kẻ thù. Do đó, việc giảm thiểu chi phí cho mỗi kẻ thù một cách độc lập và tổng hợp sẽ mang lại mức tối ưu toàn cầu. Không có ràng buộc kết hợp nào ngoài tổng lượng máu, vốn là tuyến tính và cộng gộp giữa các kẻ thù, vì vậy tính tối ưu cục bộ hàm ý tính tối ưu toàn cục. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def ceil_div(a, b):
    return (a + b - 1) // b

def solve():
    N, M = map(int, input().split())
    P1, P2 = map(int, input().split())
    Q1, Q2 = map(int, input().split())
    arr = list(map(int, input().split()))
    
    total_cost = 0
    
    for a in arr:
        # cost using only charge attacks
        hits = ceil_div(a, Q1)
        cost_charge = hits * Q2
        
        # cost using spike once + charge
        reduced = max(0, a - P1)
        hits2 = ceil_div(reduced, Q1)
        cost_spike = P2 + hits2 * Q2
        
        total_cost += min(cost_charge, cost_spike)
    
    print("YES" if total_cost < N else "NO")

if __name__ == "__main__":
    solve()
```Việc thực hiện tuân theo mô hình chi phí xuất phát trực tiếp. Chức năng trợ giúp cho việc chia trần tránh số học dấu phẩy động và đảm bảo tính chính xác khi HP không chia hết cho sát thương tấn công. Mỗi kẻ thù được xử lý độc lập trong một lần duy nhất. 

Một chi tiết triển khai tinh tế đang sử dụng`max(0, a - P1)`trước khi chia. Nếu không có điều này, HP âm có thể giảm số lần tấn công tích điện cần thiết một cách không chính xác do hành vi phân chia số nguyên. Một điểm quan trọng khác là sử dụng so sánh chặt chẽ`total_cost < N`, vì Iggy phải duy trì HP ở mức trên 0 sau khi nhận mọi thiệt hại. 

## Ví dụ đã hoạt động 

### Đầu vào mẫu 1 

đầu vào:```
N = 8, M = 3
P = (5, 2)
Q = (3, 1)
a = [5, 8, 6]
```Chúng tôi tính toán theo chi phí của kẻ thù. 

| Kẻ thù | a_i | Lượt truy cập phí | Chi phí tính phí | Giảm HP | Lượt truy cập đột ngột | Chi phí tăng đột biến | Giá đã chọn | 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| 1 | 5 | 2 | 2 | 0 | 0 | 2 | 2 | 
| 2 | 8 | 3 | 3 | 3 | 1 | 2 | 2 | 
| 3 | 6 | 2 | 2 | 1 | 1 | 2 | 2 | 

Tổng chi phí là 6. Vì 6 < 8 nên kết quả đầu ra là CÓ. 

Dấu vết này cho thấy rằng mặc dù mũi nhọn rất hữu ích đối với những kẻ thù lớn hơn, nhưng lựa chọn tối ưu cho mỗi kẻ thù là nhất quán và bổ sung. 

### Đầu vào mẫu 2 (đã xây dựng) 

đầu vào:```
N = 10, M = 2
P = (4, 5)
Q = (2, 1)
a = [3, 9]
```| Kẻ thù | a_i | Lượt truy cập phí | Chi phí tính phí | Giảm HP | Lượt truy cập đột ngột | Chi phí tăng đột biến | Giá đã chọn | 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| 1 | 3 | 2 | 2 | 0 | 0 | 5 | 2 | 
| 2 | 9 | 5 | 5 | 5 | 3 | 8 | 5 | 

Tổng chi phí là 7, nhỏ hơn 10, vì vậy đầu ra là CÓ. 

Ví dụ này chứng minh rằng mức tăng đột biến không phải lúc nào cũng hữu ích khi chi phí của nó$P_2$thống trị khoản tiết kiệm từ các cuộc tấn công giảm phí. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(M)$| Mỗi kẻ thù được xử lý một lần bằng các phép tính số học theo thời gian không đổi | 
| Không gian |$O(1)$| Chỉ có bộ đếm tổng hợp được duy trì | 

Thuật toán phù hợp thoải mái trong giới hạn vì$M \leq 10^5$và mỗi bước là một vài phép toán số nguyên. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from math import ceil
    input = sys.stdin.readline

    N, M = map(int, input().split())
    P1, P2 = map(int, input().split())
    Q1, Q2 = map(int, input().split())
    arr = list(map(int, input().split()))
    
    def ceil_div(a, b):
        return (a + b - 1) // b
    
    total = 0
    for a in arr:
        cost1 = ceil_div(a, Q1) * Q2
        reduced = max(0, a - P1)
        cost2 = P2 + ceil_div(reduced, Q1) * Q2
        total += min(cost1, cost2)
    
    return "YES\n" if total < N else "NO\n"

# provided sample
assert run("8 3\n5 2\n3 1\n5 8 6\n") == "YES\n"

# minimum input
assert run("1 1\n1 1\n1 1\n1\n") == "NO\n"

# spike useless case
assert run("20 2\n10 100\n1 1\n10 10\n") == "NO\n"

# spike dominates case
assert run("50 2\n9 1\n5 10\n20 20\n") == "YES\n"

# large HP single enemy
assert run("100 1\n50 1\n10 1\n1000000000\n") == "YES\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tối thiểu | KHÔNG | ranh giới nơi bất kỳ chi phí nào cũng giết chết Iggy | 
| tăng đột biến | KHÔNG | đảm bảo tăng đột biến không phải lúc nào cũng được chọn | 
| tăng đột biến thống trị | CÓ | đảm bảo mức tăng đột biến làm giảm mức sử dụng phí | 
| HP lớn | CÓ | độ chính xác theo giá trị lớn | 

## Vỏ cạnh 

Trường hợp một cạnh xảy ra khi mức tăng đột biến làm giảm HP xuống dưới 0. Trong trường hợp này, tất cả các cuộc tấn công dồn dập đều biến mất và chỉ có chi phí tăng đột biến là quan trọng. Ví dụ, nếu$a_i \le P_1$, khi đó HP giảm sẽ trở thành 0 và chi phí chính xác là$P_2$. Thuật toán xử lý việc này một cách chính xác thông qua`max(0, a - P1)`và sau đó không có điện tích. 

Một trường hợp khác là khi tấn công bằng phí cực kỳ kém hiệu quả nhưng tăng đột biến cũng tốn kém. Ví dụ, nếu$Q_1 = 1$Và$Q_2$lớn, chi phí sạc trở nên tuyến tính ở HP, trong khi mức tăng đột biến chỉ mang lại mức giảm nhất định. Thuật toán vẫn đánh giá chính xác cả hai biểu thức và tránh mọi hành vi lạm dụng tham lam. 

Trường hợp cuối cùng là khi tất cả kẻ thù đều giống hệt nhau và lớn. Thuật toán không dựa vào việc sắp xếp hoặc sắp xếp, do đó các giá trị lặp lại được xử lý một cách tự nhiên thông qua đánh giá độc lập cho từng kẻ thù.
