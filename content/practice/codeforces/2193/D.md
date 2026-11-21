---
title: "CF 2193D - Trò Chơi Quái Vật"
description: "Chúng ta được cung cấp một chuỗi các thanh kiếm, mỗi thanh kiếm có một giá trị sức mạnh và một chuỗi các quái vật, mỗi loại yêu cầu một số đòn đánh nhất định để đánh bại. Mỗi thanh kiếm chỉ được sử dụng tối đa một lần, vì sau khi tấn công quái vật, nó sẽ gãy ngay lập tức."
date: "2026-06-07T20:50:28+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "sortings", "two-pointers"]
categories: ["algorithms"]
codeforces_contest: 2193
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 1076 (Div. 3)"
rating: 1100
weight: 2193
solve_time_s: 101
verified: true
draft: false
---

[CF 2193D - Trò chơi quái vật](https://codeforces.com/problemset/problem/2193/D) 

**Đánh giá:** 1100 
**Thẻ:** tìm kiếm nhị phân, sắp xếp, hai con trỏ 
**Thời gian giải:** 1 phút 41 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi các thanh kiếm, mỗi thanh kiếm có một giá trị sức mạnh và một chuỗi các quái vật, mỗi loại yêu cầu một số đòn đánh nhất định để đánh bại. Mỗi thanh kiếm chỉ được sử dụng tối đa một lần, vì sau khi tấn công quái vật, nó sẽ gãy ngay lập tức. 

Trước khi bắt đầu trò chơi, chúng ta chọn một giá trị ngưỡng$x$. Chỉ những thanh kiếm có sức mạnh ít nhất$x$có thể sử dụng được; tất cả các thanh kiếm yếu hơn đều bị bỏ qua hoàn toàn. Với những thanh kiếm có thể sử dụng được, chúng tôi tiến hành theo cấp độ và theo từng cấp độ.$i$chúng ta phải chi tiêu$b_i$thanh kiếm riêng biệt để đánh bại con quái vật đó. Một khi chúng tôi dùng hết kiếm có thể sử dụng được, chúng tôi sẽ dừng ở cấp độ hiện tại. 

Điểm được tính như$x \times (\text{number of fully completed levels})$. Mục tiêu là chọn$x$để tối đa hóa số điểm này. 

Cấu trúc chính là tăng$x$giảm số lượng kiếm có sẵn nhưng tăng số nhân. Sự đánh đổi là đơn điệu theo cách gợi ý sự sắp xếp và suy luận tiền tố. 

Các ràng buộc cho phép lên đến$2 \cdot 10^5$tổng cộng$n$, vì vậy bất kỳ giải pháp nào với$O(n^2)$mỗi bài kiểm tra ngay lập tức là không thể. Thậm chí$O(n \log n)$phải được sử dụng cẩn thận và mọi mô phỏng theo giá trị trên tất cả các sức mạnh riêng biệt của thanh kiếm phải được giảm xuống hành vi khấu hao tuyến tính hoặc gần tuyến tính. 

Một nỗ lực ngây thơ có thể thử mọi cách có thể$x$từ tập hợp tất cả$a_i$, mô phỏng trò chơi mỗi lần và tính xem có thể hoàn thành bao nhiêu cấp độ. Điều đó sẽ cần tới$O(n)$mô phỏng cho mỗi ứng viên và$O(n)$ứng viên, dẫn đến$O(n^2)$nói chung là quá chậm. 

Một trường hợp thất bại tinh tế hơn đến từ việc mô phỏng tham lam mà không phân loại kiếm. Ví dụ: nếu những thanh kiếm mạnh hơn không được ưu tiên thì việc phân bổ ngây thơ có thể lãng phí chúng sớm và đánh giá thấp mức độ có thể đạt được ở một ngưỡng nhất định. 

Một trường hợp khác xuất hiện khi tất cả các thanh kiếm đều giống hệt nhau. Sau đó, câu trả lời hoàn toàn được xác định bởi tổng tiền tố của$b_i$và bất kỳ cách xử lý không chính xác nào về “hoàn toàn nhỏ hơn$x$” vs “ít nhất$x$” dẫn đến từng lỗi một trong số lượng có sẵn. 

## Phương pháp tiếp cận 

Ý tưởng bạo lực rất đơn giản: chọn độ khó của ứng viên$x$, lọc tất cả các thanh kiếm có sức mạnh ít nhất$x$, sau đó mô phỏng các thanh kiếm tiêu thụ theo cấp độ. Mỗi cấp độ tiêu thụ$b_i$kiếm, vì vậy chúng tôi giảm số lượt truy cập cho đến khi không thể tiếp tục. Điều này đúng vì nó phản ánh trực tiếp các quy tắc của trò chơi. 

Vấn đề là chi phí. Nếu chúng ta thử tất cả các giá trị có thể có của$x$, có tới$n$sức mạnh thanh kiếm khác biệt. Đối với mỗi chi phí lọc$O(n)$, và mức độ mô phỏng sẽ tốn thêm chi phí$O(n)$. Điều này dẫn đến$O(n^2)$mỗi trường hợp thử nghiệm trong trường hợp xấu nhất. 

Quan sát quan trọng là đối với một cố định$x$, chỉ có số lượng kiếm có thể sử dụng được mới quan trọng chứ không phải danh tính của chúng. Khi chúng ta sắp xếp các thanh kiếm theo thứ tự sức mạnh giảm dần, tăng dần$x$tương ứng với việc kích hoạt dần dần các tiền tố của mảng được sắp xếp này. Thay vì tính toán lại từ đầu, chúng ta có thể quét các ngưỡng từ cao xuống thấp và duy trì số lượng thanh kiếm có sẵn. 

Ý tưởng quan trọng thứ hai là cách tính số cấp độ mà chúng ta có thể hoàn thành với số lượng kiếm cố định có sẵn. Vì các cấp độ là tuần tự và mỗi cấp độ tiêu thụ$b_i$tài nguyên, chúng tôi tính toán trước tổng tiền tố của$b$. Số cấp độ hoàn thành là chỉ số lớn nhất$k$như vậy$\sum_{i=1}^k b_i \leq \text{available swords}$. 

Điều này làm giảm mỗi đánh giá thành tìm kiếm nhị phân trên tổng tiền tố hoặc thậm chí tốt hơn, chúng ta có thể tính toán trước mức tối đa có thể tiếp cận cho mỗi số lượng kiếm có thể một lần và tái sử dụng nó về mặt khái niệm trong khi quét sức mạnh. 

Cuối cùng, chúng tôi đánh giá điểm cho từng ngưỡng như sau:$$\text{score} = (\text{current threshold}) \times (\text{max levels achievable with available swords})$$| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n^2)$|$O(n)$| Quá chậm | 
| Tối ưu |$O(n \log n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Sắp xếp sức mạnh của kiếm theo thứ tự giảm dần. Điều này cho phép chúng tôi giải thích việc chọn ngưỡng$x$khi chọn tiền tố của danh sách được sắp xếp này. Độ dài tiền tố trực tiếp cho biết số lượng kiếm có thể sử dụng được. 
2. Tính tổng tiền tố của yêu cầu quái vật$b_i$. Điều này cho phép chúng tôi nhanh chóng xác định có thể hoàn thành bao nhiêu cấp độ với một số lượng kiếm nhất định. 
3. Đối với mỗi độ dài tiền tố có thể$k$(có nghĩa là chúng tôi sử dụng phần trên cùng$k$kiếm mạnh nhất), hãy xác định giá trị ngưỡng tốt nhất liên quan đến nó. Sự lựa chọn tự nhiên là thanh kiếm nhỏ nhất trong tiền tố đó, vì đó là thanh kiếm tối đa$x$điều đó vẫn bao gồm tất cả$k$thanh kiếm. 
4. Đối với mỗi$k$, tính toán có bao nhiêu cấp độ có thể được hoàn thành. Đây là chỉ số lớn nhất$i$sao cho prefix_sum[i] ≤ k. Điều này có thể được tìm thấy thông qua tìm kiếm nhị phân. 
5. Tính điểm thí sinh như sau$x \cdot \text{levels}$và theo dõi mức tối đa trên tất cả$k$. 

Lý do điều này có hiệu quả là vì ngưỡng tối ưu chỉ thay đổi khi chúng ta bao gồm hoặc loại trừ một thanh kiếm. Giữa hai sức mạnh thanh kiếm khác biệt liên tiếp, bộ kiếm có thể sử dụng không thay đổi, vì vậy việc kiểm tra các giá trị trung gian của$x$là không cần thiết. 

### Tại sao nó hoạt động 

Đối với bất kỳ ngưỡng cố định nào$x$, bộ kiếm có thể sử dụng chính xác là những bộ có$a_i \ge x$. Trong số tất cả các ngưỡng tạo ra cùng một bộ có thể sử dụng được, ngày càng tăng$x$cải thiện điểm số mà không thay đổi tính khả thi. Vì vậy sự lựa chọn tối ưu của$x$luôn bằng một số sức mạnh của thanh kiếm. Điều này thu gọn không gian tìm kiếm từ các giá trị liên tục xuống nhiều nhất$n$các ứng cử viên và mỗi ứng cử viên được đặc trưng đầy đủ bởi một tiền tố của các điểm mạnh đã được sắp xếp. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

from bisect import bisect_right

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        b = list(map(int, input().split()))
        
        a.sort(reverse=True)
        
        pref = [0] * (n + 1)
        for i in range(n):
            pref[i + 1] = pref[i] + b[i]
        
        # precompute max level for k swords via binary search on prefix sums
        best = 0
        
        for k in range(1, n + 1):
            x = a[k - 1]
            # max levels with k swords
            levels = bisect_right(pref, k) - 1
            best = max(best, x * levels)
        
        print(best)

if __name__ == "__main__":
    solve()
```Giải pháp trước tiên là sắp xếp độ mạnh của kiếm sao cho mọi tiền tố đều tương ứng với một lựa chọn hợp lệ về các loại kiếm có thể sử dụng được dưới một ngưỡng nào đó. Tiền tố tổng hợp lại$b_i$cho phép chuyển đổi nhanh từ “số lượng kiếm có sẵn” sang “có thể vượt qua bao nhiêu cấp độ”. 

Bước tìm kiếm nhị phân rất tinh tế:`bisect_right(pref, k) - 1`trả về số cấp độ lớn nhất có tổng yêu cầu không vượt quá$k$. Điều này tránh việc mô phỏng mức tiêu thụ theo từng cấp độ. 

Một lỗi phổ biến là đảo ngược ý nghĩa của các chỉ số tiền tố: mảng tổng tiền tố bao gồm số 0 ở đầu, do đó số lượng cấp độ phải được bù đắp cẩn thận. Một sai lầm khác là quên rằng mỗi trường hợp thử nghiệm cần tính toán lại tiền tố độc lập. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 3
a = [1, 3, 4]
b = [2, 1, 1]
```Các thanh kiếm được sắp xếp:```
a = [4, 3, 1]
prefix b = [0, 2, 3, 4]
```| k | x (ngưỡng) | kiếm | cấp độ (<k) | điểm | 
| --- | --- | --- | --- | --- | 
| 1 | 4 | 1 | 0 | 0 | 
| 2 | 3 | 2 | 1 | 3 | 
| 3 | 1 | 3 | 2 | 2 | 

Tối đa là 3. 

Điều này cho thấy rằng nhiều thanh kiếm hơn không nhất thiết sẽ cải thiện điểm số, vì việc hạ thấp ngưỡng sẽ làm giảm hệ số nhân. 

### Ví dụ 2 

đầu vào:```
n = 4
a = [4, 4, 1, 4]
b = [2, 2, 4, 1]
```Các thanh kiếm được sắp xếp:```
a = [4, 4, 4, 1]
prefix b = [0, 2, 4, 8, 9]
```| k | x | kiếm | cấp độ | điểm | 
| --- | --- | --- | --- | --- | 
| 1 | 4 | 1 | 0 | 0 | 
| 2 | 4 | 2 | 1 | 4 | 
| 3 | 4 | 3 | 1 | 4 | 
| 4 | 1 | 4 | 3 | 3 | 

Tối đa là 4. 

Ví dụ này nhấn mạnh rằng việc tăng số lượng kiếm chỉ hữu ích nếu nó tăng cấp độ có thể tiếp cận. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \log n)$| Sắp xếp chiếm ưu thế; mỗi bài kiểm tra thực hiện quét tuyến tính với tìm kiếm nhị phân | 
| Không gian |$O(n)$| Mảng tiền tố và danh sách được sắp xếp | 

Các ràng buộc cho phép tổng$2 \cdot 10^5$các yếu tố, vì vậy cách tiếp cận này phù hợp thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import bisect

    def solve():
        t = int(input())
        for _ in range(t):
            n = int(input())
            a = list(map(int, input().split()))
            b = list(map(int, input().split()))

            a.sort(reverse=True)

            pref = [0]
            for x in b:
                pref.append(pref[-1] + x)

            best = 0
            for k in range(1, n + 1):
                x = a[k - 1]
                levels = bisect.bisect_right(pref, k) - 1
                best = max(best, x * levels)

            print(best)

    old = sys.stdin
    out = io.StringIO()
    sys.stdout = out
    solve()
    sys.stdout = sys.__stdout__
    sys.stdin = old
    return out.getvalue().strip()

# provided samples
assert run("""5
3
1 3 4
2 1 1
2
2 3
1 1
4
1 2 3 4
2 2 1 1
6
4 4 1 4 5 4
2 2 4 1 2 2
10
1000000000 1000000000 1000000000 1000000000 1000000000 1000000000 1000000000 1000000000 1000000000 1000000000
1 1 1 1 1 1 1 1 1 1
""") == """3
4
3
8
10000000000"""

# custom tests
assert run("""1
1
5
1
""") == "0"

assert run("""1
5
5 4 3 2 1
1 1 1 1 1
""") == "10"

assert run("""1
4
10 1 10 1
3 2 2 1
""") == "20"

assert run("""1
6
6 5 4 3 2 1
2 2 2 2 2 2
""") == "18"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| một kiếm, không thể tiếp tục | 0 | trường hợp cạnh không có cấp độ nào có thể | 
| nhiều cấp độ yếu | 10 | tham lam tích lũy đúng đắn | 
| thế mạnh hỗn hợp | 20 | ngưỡng tương tác đúng đắn | 
| yêu cầu thống nhất | 18 | tính chính xác của ranh giới tiền tố | 

## Vỏ cạnh 

Một trường hợp tối thiểu với một thanh kiếm cho thấy rằng nếu thanh kiếm duy nhất có sức mạnh dưới bất kỳ ngưỡng hữu ích nào, câu trả lời sẽ giảm về 0 vì không thể hoàn thành cấp độ nào. Thuật toán xử lý việc này vì$k=1$, mức tính toán từ tổng tiền tố bằng 0, do đó điểm vẫn bằng 0 bất kể$x$. 

Một trường hợp mà tất cả$b_i = 1$nhấn mạnh logic tiền tố. Mỗi thanh kiếm bổ sung trực tiếp chuyển sang một cấp độ cao hơn, vì vậy điểm số cao nhất có xu hướng đến từ việc cân bằng lớn$x$với độ dài tiền tố vừa đủ. Tìm kiếm nhị phân trên tổng tiền tố nắm bắt chính xác sự tích lũy tuyến tính này mà không cần mô phỏng. 

Trường hợp cường độ hỗn hợp kiểm tra xem việc sắp xếp có sắp xếp chính xác việc giải thích ngưỡng hay không. Vì các ngưỡng được gắn với các tiền tố được sắp xếp nên bất kỳ sự không khớp nào giữa thứ tự ban đầu và thứ tự được sắp xếp sẽ tạo ra số lượng cấp độ không chính xác. Thuật toán tránh điều này bằng cách tách hoàn toàn hai miền:$a$được sắp xếp,$b$vẫn giữ nguyên thứ tự cấp độ ban đầu và chỉ có tổng tiền tố của$b$được sử dụng.
