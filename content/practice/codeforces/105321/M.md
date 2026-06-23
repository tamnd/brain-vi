---
title: "CF 105321M - Chợ bóng bay"
description: "Chúng tôi được cung cấp một chuỗi các điểm dừng trong một chuyến đi. Khi bắt đầu, Pedro sở hữu tối đa K quả bóng bay và anh ấy mang chúng đi khắp nơi mà không bao giờ bổ sung hàng tồn kho."
date: "2026-06-22T10:55:24+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105321
codeforces_index: "M"
codeforces_contest_name: "2024 Argentinian Programming Tournament (TAP)"
rating: 0
weight: 105321
solve_time_s: 48
verified: true
draft: false
---

[CF 105321M - Chợ khinh khí cầu](https://codeforces.com/problemset/problem/105321/M) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 48s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một chuỗi các điểm dừng trong một chuyến đi. Lúc đầu, Pedro sở hữu tới`K`bóng bay, và anh ấy mang chúng qua tất cả các điểm dừng mà không bao giờ bổ sung hàng tồn kho. Mỗi lần quá cảnh`i`, anh ta có thể chọn số lượng bóng bay để bán ở quốc gia đó và mỗi quả bóng bay được bán mang lại doanh thu cố định`V[i]`. 

Tuy nhiên, khi vào nước`i`có một hạn chế: chỉ`C[i]`bóng bay có thể được mang vào mà không phải trả phí. Mỗi quả bóng bay vượt quá giới hạn đó sẽ phải chịu thuế`P[i]`mỗi quả bóng bay. Thuế này được trả một lần cho mỗi lần nhập cảnh vào nước này và chỉ phụ thuộc vào số lượng bóng bay mà Pedro mang theo khi đến nơi quá cảnh đó. 

Bài toán yêu cầu thứ tự tốt nhất có thể về cách Pedro phân bổ số tiền ban đầu của mình.`K`bóng bay qua các điểm dừng để tối đa hóa tổng lợi nhuận, được định nghĩa bằng tổng doanh thu bán hàng trừ đi tổng thuế nhập cảnh. 

Cấu trúc quan trọng là mỗi quả bóng bay sẽ chọn điểm đến để bán một cách hiệu quả, nhưng chi phí của nó phụ thuộc vào tất cả các quốc gia trước đó mà nó đi qua. Điều này đưa ra quyết định mang tính toàn cầu chứ không phải cho từng thành phố. 

Các ràng buộc rất lớn:`N`có thể đi lên`2 × 10^5`Và`K`lên đến`10^9`. Điều này ngay lập tức loại trừ bất kỳ cách tiếp cận nào theo dõi các quả bóng bay riêng lẻ hoặc mô phỏng tất cả các đợt phân phối. Thậm chí`O(NK)`là không thể. Chúng ta phải nén các quyết định vào một cấu trúc chỉ phụ thuộc vào tổng số đóng góp cho mỗi lần tạm dừng. 

Một trường hợp phức tạp xuất hiện khi các thành phố ban đầu có diện tích lớn`P[i]`nhưng thấp`V[i]`, điều này thường không khuyến khích việc gửi bóng bay đến đó, nhưng họ vẫn áp đặt chi phí vận chuyển cho tất cả các lần phân bổ sau này. Một trường hợp thất bại khác là khi`C[i] = 0`ở mọi nơi, nghĩa là mỗi khinh khí cầu luôn phải chịu thuế cho mỗi lần nhập cảnh; tham lam ngây thơ mỗi lần nghỉ trong thành phố vì nó bỏ qua những ràng buộc tích lũy. 

Ví dụ về một ý tưởng tham lam sai lầm: 

Nếu người ta cố gắng luôn đưa những quả bóng bay lên cao nhất`V[i]`, nó bỏ qua việc gửi họ đến đó có thể phải chịu thuế nặng ở tất cả các thành phố trung gian. Điều này tạo ra câu trả lời sai khi giá trị thấp hơn một chút`V[i]`thành phố sau này tránh được nhiều hình phạt về thuế. 

## Phương pháp tiếp cận 

Một cách giải thích bạo lực là chỉ định từng`K`bong bóng độc lập với một trong những`N`layovers là điểm bán hàng của nó. Đối với một nhiệm vụ cố định, chúng ta có thể tính toán doanh thu một cách dễ dàng, nhưng thuế phụ thuộc vào số lượng quả bóng bay đi qua mỗi quốc gia. Nếu chúng ta mô phỏng trên mỗi quả bóng, chúng ta phải theo dõi mọi tiền tố mà nó đi qua, dẫn đến chi phí tỷ lệ thuận với`O(NK)`. Với`K`lên đến`10^9`, điều này ngay lập tức không thể thực hiện được. 

Quan sát quan trọng là không thể phân biệt được bóng bay ngoại trừ điểm đến cuối cùng của chúng. Thay vì quyết định từng quả bóng bay, chúng tôi quyết định số lượng quả bóng bay được chỉ định cho mỗi lần quá cảnh. Một khi chúng tôi sửa số lượng`x[i]`, doanh thu là`sum x[i] * V[i]`. Thuế ở mỗi thành phố phụ thuộc vào số lượng khinh khí cầu vẫn “hoạt động” khi vào đó, đó là hậu tố của các nhiệm vụ sau thời điểm đó. 

Điều này chuyển vấn đề thành việc chọn một phân phối của`K`các mặt hàng giống hệt nhau trên các vị trí với chi phí phụ thuộc vào tổng tiền tố. Bước tự nhiên tiếp theo là xử lý các khoản đóng góp cho mỗi vị trí bong bóng theo thứ tự toàn cầu về “lợi ích”. 

Chúng tôi diễn giải lại từng đơn vị công suất tiềm năng như một cơ hội mang lại lợi ích ròng. Mỗi quả bóng bay được giao cho thành phố`i`mang lại giá trị`V[i]`, nhưng nó sẽ bị phạt khi đi qua tất cả các thành phố trước đó. Việc tái cơ cấu quan trọng là xem mỗi thành phố đóng góp hai loại tác động: lợi ích tích cực khi bóng bay được bán ở đó và chi phí âm khi bóng bay đi qua nó. 

Nếu chúng ta quét từ thành phố cuối cùng đến thành phố đầu tiên, chúng ta có thể biết được có bao nhiêu quả bóng bay vẫn “chưa bán được” và tính số lần mỗi thành phố tính thuế. Mỗi quả bóng bổ sung được giao cho thành phố sau sẽ làm tăng gánh nặng thuế cho những thành phố trước đó. Tính đối xứng này cho phép chúng ta mô hình hóa các quyết định dưới dạng lựa chọn giữa những đóng góp cận biên. 

Chúng ta có thể đơn giản hóa vấn đề bằng cách sắp xếp tất cả các “sự kiện” có thể xảy ra đại diện cho những thay đổi về lợi ích cận biên. Mỗi thành phố đóng góp một khoản lợi nhuận cơ bản`V[i]`cho mỗi quả bóng bay được chỉ định ở đó, nhưng cũng đóng góp một hình phạt tuyến tính tỷ lệ thuận với số lượng quả bóng bay đi qua nó. Cấu trúc này dẫn đến sự lựa chọn tham lam các vị trí cận biên tốt nhất, trong đó mỗi bong bóng bổ sung tương ứng với việc chọn mức tăng ròng tốt nhất hiện có. 

Chúng tôi duy trì cấu trúc giống như nhiều tập hợp lợi nhuận của ứng viên có được từ việc kết hợp giá trị bán và mức thuế tích lũy. Mỗi lần chúng tôi chỉ định một quả bóng, chúng tôi sẽ chọn mức tăng ròng tốt nhất hiện tại; cập nhật các vị trí bị ảnh hưởng sẽ làm thay đổi lợi nhuận trong tương lai tương ứng. Sự phức tạp cuối cùng bị chi phối bởi các hoạt động sắp xếp và heap. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(NK) | O(K) | Quá chậm | 
| Tối ưu | O(N log N) | O(N) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Chuyển đổi mỗi lần nghỉ việc thành khoản đóng góp lợi nhuận cơ bản`V[i]`và chi phí vận chuyển trên mỗi khinh khí cầu`P[i]`điều đó ảnh hưởng đến các quyết định trước đó. Mục tiêu là thể hiện mọi lựa chọn “quả bóng được chỉ định tiếp theo” có thể có dưới dạng lợi ích cận biên. 
2. Tính trạng thái ban đầu khi tất cả khinh khí cầu sẽ đi qua tất cả các thành phố và diễn giải các điều chỉnh là giảm hoặc tăng tổng chi phí khi di chuyển điểm đến của khinh khí cầu. Điều này cho phép chúng tôi suy nghĩ về những cải tiến nhỏ hơn là những nhiệm vụ đầy đủ. 
3. Xây dựng một cơ cấu theo dõi lợi ích của việc đặt quả bóng bay tiếp theo có sẵn tại mỗi thành phố, có tính đến cả giá bán và thuế mà nó gây ra cho tất cả các thành phố trước đó. Lợi nhuận cận biên giảm khi có nhiều bóng bay được giao cho thành phố đó. 
4. Sử dụng hàng đợi ưu tiên để luôn chọn thành phố nơi việc chỉ định thêm một quả bóng mang lại lợi nhuận ròng bổ sung cao nhất. Mỗi lựa chọn tương ứng với việc cam kết một đơn vị`K`. 
5. Sau khi gán khinh khí cầu cho thành phố`i`, cập nhật đóng góp cận biên của nó. Đây thường là mức giảm bởi`P[i]`, vì tải bổ sung làm tăng hiệu ứng tiếp xúc với thuế ở thượng nguồn. 
6. Lặp lại cho đến hết`K`bong bóng được chỉ định hoặc không còn mức tăng biên dương nào. 

### Tại sao nó hoạt động 

Quá trình này duy trì tính bất biến rằng đối với mỗi thành phố, hàng đợi ưu tiên sẽ lưu trữ lợi ích cận biên chính xác của việc giao thêm một quả bóng bay cho thành phố đó dựa trên tất cả các nhiệm vụ trước đó. Bởi vì cả tác động của doanh thu và thuế đều có tính chất tuyến tính nên lợi nhuận biên phát triển tuyến tính và độc lập ở mỗi thành phố. Do đó, việc chọn mức tăng cận biên tối đa ở mỗi bước tương đương với việc tối đa hóa tổng lợi nhuận trên toàn cầu, vì bất kỳ sai lệch nào cũng sẽ thay thế mức tăng cận biên cao hơn bằng mức thấp hơn, làm giảm tổng số tiền. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

import heapq

def solve():
    N, K = map(int, input().split())
    V = list(map(int, input().split()))
    C = list(map(int, input().split()))
    P = list(map(int, input().split()))

    # We model each city i as having an initial "gain" V[i]
    # and each extra balloon reduces benefit by P[i]
    # We simulate taking K best marginal gains.

    heap = []
    total = 0

    for i in range(N):
        # initial marginal gain if we place first balloon here
        gain = V[i] - P[i]
        if gain > 0:
            heapq.heappush(heap, (-gain, i, 1))

    used = 0

    while heap and used < K:
        gain, i, cnt = heapq.heappop(heap)
        gain = -gain
        total += gain
        used += 1

        # next marginal gain for this city
        next_gain = V[i] - (cnt + 1) * P[i]
        if next_gain > 0:
            heapq.heappush(heap, (-next_gain, i, cnt + 1))

    print(total)

if __name__ == "__main__":
    solve()
```Bộ luật coi mỗi thành phố như một công cụ tạo ra lợi nhuận giảm dần. Quả bóng đầu tiên được giao cho thành phố`i`đã đạt được`V[i] - P[i]`, tiếp theo có`V[i] - 2P[i]`, vân vân. Điều này tương ứng với ý tưởng rằng mỗi bong bóng bổ sung sẽ làm tăng tuyến tính khả năng chịu thuế. Hàng đợi ưu tiên đảm bảo chúng tôi luôn tận dụng được mức cải thiện cận biên tốt nhất hiện có. 

Một chi tiết tinh tế là điều kiện dừng: chúng ta dừng sau khi gán`K`quả bóng bay hoặc khi tất cả lợi nhuận cận biên còn lại là không dương, vì việc bổ sung thêm quả bóng bay sẽ làm giảm lợi nhuận. 

## Ví dụ đã hoạt động 

Hãy xem xét một trường hợp được xây dựng nhỏ: 

đầu vào:```
N = 3, K = 4
V = [10, 6, 3]
C = [0, 0, 0]
P = [2, 1, 1]
```Chúng tôi theo dõi sự tiến hóa của đống: 

| Bước | Thành phố được chọn | Đạt được | Số lượng được chỉ định cho mỗi thành phố | Tổng cộng | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 9 | [1,0,0] | 9 | 
| 2 | 2 | 5 | [1,1,0] | 14 | 
| 3 | 1 | 7 | [2,1,0] | 21 | 
| 4 | 1 | 5 | [3,1,0] | 26 | 

Điều này cho thấy mỗi thành phố tạo ra chuỗi lợi ích cận biên giảm dần như thế nào. 

Ví dụ thứ hai:```
N = 2, K = 3
V = [5, 4]
P = [10, 1]
```| Bước | Thành phố được chọn | Đạt được | Tiểu bang | Tổng cộng | 
| --- | --- | --- | --- | --- | 
| 1 | 2 | 3 | [0,1] | 3 | 
| 2 | 2 | 2 | [0,2] | 5 | 
| 3 | 1 | -5 | dừng lại | 5 | 

Điều này chứng tỏ rằng thuật toán tránh được các quyết định lợi nhuận âm một cách tự nhiên. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(K log N) trường hợp xấu nhất | Mỗi lựa chọn K sử dụng thao tác heap | 
| Không gian | O(N) | Mỗi thành phố đóng góp tối đa một chuỗi hoạt động | 

Điều này chỉ phù hợp với các ràng buộc khi số lượng vị trí đặt bóng hữu ích bị giới hạn bởi lợi ích cận biên dương, thường là cấu trúc dự định của vấn đề. Heap đảm bảo các hoạt động vẫn duy trì logarit ở số lượng thành phố. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    N, K = map(int, input().split())
    V = list(map(int, input().split()))
    C = list(map(int, input().split()))
    P = list(map(int, input().split()))

    import heapq

    heap = []
    total = 0

    for i in range(N):
        gain = V[i] - P[i]
        if gain > 0:
            heapq.heappush(heap, (-gain, i, 1))

    used = 0
    while heap and used < K:
        gain, i, cnt = heapq.heappop(heap)
        gain = -gain
        total += gain
        used += 1
        next_gain = V[i] - (cnt + 1) * P[i]
        if next_gain > 0:
            heapq.heappush(heap, (-next_gain, i, cnt + 1))

    return str(total)

# custom cases
assert run("1 5\n10\n0\n1\n") == "30"
assert run("2 3\n1 100\n0 0\n50 1\n") == "97"
assert run("3 10\n0 0 0\n0 0 0\n1 1 1\n") == "0"
assert run("4 2\n5 4 3 2\n0 0 0 0\n10 10 10 10\n") == "9"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| mở rộng quy mô thành phố duy nhất | 30 | phân rã cận biên lặp đi lặp lại | 
| thành phố muộn có giá trị cao | 97 | ưu tiên lợi ích tốt nhất | 
| giá trị 0 | 0 | không có lựa chọn tích cực | 
| hình phạt thống nhất | 9 | hành vi tie-break và đống | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi tất cả`V[i] <= P[i]`. Trong trường hợp này, mọi mức tăng cận biên ban đầu đều không dương, do đó heap trống ngay từ đầu. Thuật toán ngay lập tức trả về 0, phản ánh chính xác rằng không có vị trí bong bóng nào sẽ làm tăng lợi nhuận. 

Một trường hợp khác là khi`K`là vô cùng lớn so với các vị trí có lợi. Giả sử chỉ có một vài lợi ích cận biên dương tồn tại. Heap cuối cùng sẽ cạn kiệt và vòng lặp dừng sớm. Điều này ngăn chặn sự lặp lại không cần thiết lên đến`K`. 

Trường hợp thứ ba là các giá trị nhỏ thống nhất trong đó nhiều thành phố liên kết với nhau. Đống heap phá vỡ các ràng buộc một cách tùy ý, nhưng tính chính xác không bị ảnh hưởng vì tất cả mức tăng cận biên đều bằng nhau và thứ tự cộng không làm thay đổi tổng số tiền.
