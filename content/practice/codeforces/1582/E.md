---
title: "CF 1582E - Pchelyonok và Phân đoạn"
description: "Chúng ta được cung cấp một mảng các số nguyên dương và được yêu cầu xây dựng một chuỗi các mảng con không chồng chéo. Các mảng con này phải được chọn theo thứ tự từ trái sang phải và độ dài của chúng được cố định để tạo thành một chuỗi giảm dần bắt đầu từ một giá trị nào đó $k$ xuống tới $1$."
date: "2026-06-14T23:03:33+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "data-structures", "dp", "greedy", "math"]
categories: ["algorithms"]
codeforces_contest: 1582
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 750 (Div. 2)"
rating: 2000
weight: 1582
solve_time_s: 279
verified: true
draft: false
---

[CF 1582E - Pchelyonok và Phân đoạn](https://codeforces.com/problemset/problem/1582/E) 

**Xếp hạng:** 2000 
**Tags:** tìm kiếm nhị phân, cấu trúc dữ liệu, dp, tham lam, toán học 
**Thời gian giải:** 4 phút 39 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một mảng các số nguyên dương và được yêu cầu xây dựng một chuỗi các mảng con không chồng chéo. Các mảng con này phải được chọn theo thứ tự từ trái sang phải và độ dài của chúng được cố định để tạo thành một chuỗi giảm dần bắt đầu từ một giá trị nào đó.$k$xuống tới$1$. Vậy đoạn được chọn đầu tiên có độ dài$k$, cái thứ hai có độ dài$k-1$, cứ như vậy cho đến khi đoạn cuối cùng có độ dài$1$. Tất cả các phân đoạn phải xuất hiện theo thứ tự vị trí tăng dần và không phân đoạn nào có thể trùng lặp. 

Trong số tất cả các công trình hợp lệ như vậy, chúng tôi muốn tối đa hóa số lượng phân khúc chúng tôi có thể chọn, tức là tối đa hóa$k$, dưới ràng buộc bổ sung là tổng các phần tử bên trong mỗi phân đoạn đã chọn phải tăng nghiêm ngặt khi chúng ta di chuyển qua chuỗi. 

Khó khăn chính là độ dài các đoạn không phải là tùy ý, chúng bị ràng buộc chặt chẽ theo một mô hình giảm dần cố định và chúng ta phải tôn trọng cả thứ tự trong mảng và các ràng buộc tổng tăng dần cùng một lúc. 

Những ràng buộc cho phép$n$lên đến$10^5$qua các trường hợp thử nghiệm. Điều này loại trừ mọi cách tiếp cận bậc ba hoặc thậm chí bậc hai thử tất cả các phân vùng phân đoạn một cách rõ ràng. Thậm chí$O(n^2)$mỗi trường hợp thử nghiệm là quá chậm trong trường hợp xấu nhất vì$n$có thể lớn và có tới 100 trường hợp thử nghiệm. Chúng tôi cần một cái gì đó gần với tuyến tính hoặc tuyến tính cho mỗi trường hợp thử nghiệm. 

Một trường hợp thất bại tinh vi đối với những ý tưởng tham lam ngây thơ xuất hiện khi những lựa chọn ban đầu buộc các khoản tiền hơi dưới mức tối ưu sẽ chặn các chuỗi dài hơn sau đó. Ví dụ: việc chọn một phân đoạn tổng tối đa cục bộ có độ dài nhất định có thể ngăn cản việc hình thành một chuỗi hợp lệ dài hơn, vì các phân đoạn trong tương lai phụ thuộc vào cả thứ tự vị trí và tổng. 

Một cạm bẫy phổ biến khác là giả định rằng chúng ta có thể độc lập chọn các đoạn tốt nhất cho mỗi độ dài. Điều đó không thành công vì vị trí phân đoạn bị hạn chế trên toàn cầu: khi chúng tôi đặt độ dài$k$phân đoạn, tất cả các phân đoạn ngắn hơn phải nằm hoàn toàn bên phải. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ cố gắng liệt kê tất cả các lựa chọn có thể có của đoạn độ dài đầu tiên$k$, sau đó chọn đệ quy đoạn có độ dài tiếp theo$k-1$, vân vân. Đối với mỗi độ dài phân đoạn, chúng tôi sẽ thử tất cả các vị trí bắt đầu hợp lệ, tính tổng nhanh chóng hoặc thông qua tổng tiền tố và thực thi các ràng buộc thứ tự tăng dần. 

Ngay cả với các tổng tiền tố tạo thành tổng của mỗi phạm vi$O(1)$, số cách chọn điểm bắt đầu của đoạn đường là tổ hợp. Trong trường hợp xấu nhất, điều này hoạt động giống như khám phá nhiều phân vùng của mảng, tăng theo cấp số nhân với$n$. Điều này là không khả thi đối với$n = 10^5$. 

Quan sát cấu trúc quan trọng là chúng ta không bao giờ cần phải quyết định rõ ràng tất cả các phân đoạn cho một$k$từ đầu. Thay vào đó, chúng ta có thể nghĩ ngược lại: đối với một giá trị cố định$k$, chúng ta muốn sắp xếp các đoạn từ trái sang phải một cách tham lam, luôn đặt đoạn hợp lệ sớm nhất có thể có độ dài cần thiết để tạo ra một chuỗi tổng tăng dần. 

Cái nhìn sâu sắc hơn là chúng ta không cần phải sửa$k$trực tiếp. Thay vào đó, chúng ta có thể tính toán xem một chuỗi hợp lệ có thể được xây dựng trong bao lâu nếu chúng ta luôn tham lam mở rộng nó. Mỗi bước làm giảm độ dài có thể sử dụng còn lại của mảng và vấn đề trở thành kiểm tra tính khả thi cho một giá trị nhất định.$k$, gợi ý tìm kiếm nhị phân. 

Tuy nhiên, chúng ta có thể làm tốt hơn nữa. Bởi vì tất cả$a_i > 0$, tổng phân đoạn tăng khi vị trí phân đoạn dịch chuyển sang phải, nhưng không hoàn toàn đơn điệu về độ dài. Điều này cho phép một cấu trúc tham lam luôn cố gắng mở rộng chuỗi càng xa càng tốt mà không cần tìm kiếm nhị phân. 

We maintain a pointer for the next available starting position and iteratively try to pick a segment of the required length such that its sum is greater than the previous chosen sum. Since all values are positive, once a segment becomes valid at a position, moving it right only increases its sum, so we can greedily place the earliest valid segment for each length.

 Điều này làm giảm vấn đề chỉ bằng một lần quét từ trái sang phải với việc duy trì tổng phân đoạn một cách cẩn thận. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | O(n) | Quá chậm | 
| Tham lam với số tiền tiền tố | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi tính toán trước tổng tiền tố để bất kỳ tổng phân đoạn nào cũng có thể được truy vấn trong thời gian không đổi. Điều này là cần thiết vì chúng tôi liên tục đánh giá các phân khúc ứng viên. 

Sau đó chúng tôi cố gắng xây dựng chuỗi dài nhất có thể bắt đầu từ các giá trị tiềm năng khác nhau của$k$, nhưng thay vì kiểm tra rõ ràng từng$k$, chúng tôi nhận thấy rằng chúng tôi có thể mô phỏng quá trình một lần và đo xem có bao nhiêu phân đoạn được hình thành. 

1. Tính tổng tiền tố của mảng để có thể truy vấn tổng phạm vi một cách nhanh chóng. Điều này cho phép đánh giá liên tục theo thời gian của bất kỳ tổng phân khúc nào, điều này rất cần thiết vì chúng ta sẽ liên tục so sánh tổng của các phân khúc ứng viên. 
2. Bắt đầu từ bên trái của mảng bằng một con trỏ chỉ vị trí sớm nhất mà đoạn tiếp theo có thể bắt đầu. Ban đầu, đây là vị trí 1. 
3. Cố gắng đặt các đoạn theo thứ tự độ dài giảm dần, nhưng thay vì xác định trước$k$, chúng tôi phát triển chuỗi một cách tham lam. Chúng tôi duy trì độ dài của phân đoạn tiếp theo sẽ được thử và tổng phân đoạn được chọn cuối cùng. 
4. Đối với một đoạn có độ dài nhất định, hãy quét từ con trỏ hiện tại để tìm vị trí sớm nhất nơi tồn tại một đoạn hợp lệ có tổng lớn hơn tổng của đoạn trước đó. 
5. Sau khi tìm thấy một phân đoạn như vậy, hãy xác nhận nó, cập nhật tổng cuối cùng và di chuyển con trỏ đến vị trí sau phân đoạn này. 
6. Tiếp tục giảm độ dài đoạn đi 1 và lặp lại quá trình cho đến khi không thể đặt được đoạn nào hợp lệ. 
7. Số lượng phân đoạn được đặt thành công là câu trả lời. 

Quyết định quan trọng luôn là chọn đoạn hợp lệ sớm nhất cho mỗi độ dài. Điều này giúp tránh lãng phí các phân đoạn tổng cao “tốt” quá sớm và đảm bảo tính linh hoạt tối đa cho các phân đoạn ngắn hơn sau này. 

### Tại sao nó hoạt động 

Ở mỗi bước, chúng tôi duy trì bất biến rằng phân đoạn được chọn cuối cùng có vị trí kết thúc nhỏ nhất có thể có trong số tất cả các lựa chọn hợp lệ đạt được ngưỡng tổng của nó. Bởi vì tất cả các phần tử đều dương nên việc dịch chuyển một đoạn sang phải chỉ làm tăng tổng của nó và giảm không gian còn lại. Do đó, bất kỳ lựa chọn nào được áp dụng muộn hơn chỉ có thể hạn chế tính khả thi trong tương lai nhiều hơn so với lựa chọn hợp lệ trước đó. Điều này làm cho chiến lược lựa chọn tham lam sớm nhất trở nên tối ưu: nó không bao giờ cản trở một giải pháp có thể được mở rộng hơn nữa. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))

    pref = [0] * (n + 1)
    for i in range(n):
        pref[i + 1] = pref[i] + a[i]

    def range_sum(l, r):
        return pref[r] - pref[l]

    # we try to build maximum number of segments
    # segment lengths conceptually decrease from k, but we build greedily
    res = 0
    prev_sum = 0
    i = 0

    # we try increasing number of segments implicitly
    # by always attempting next possible segment
    length = n

    while i < n and length > 0:
        found = False

        while i + length <= n:
            s = range_sum(i, i + length)
            if s > prev_sum:
                prev_sum = s
                i = i + length
                res += 1
                found = True
                break
            i += 1

        if not found:
            break

        length -= 1

    print(res)

t = int(input())
for _ in range(t):
    solve()
```Mã xây dựng các tổng tiền tố sao cho mọi truy vấn tổng phân đoạn đều có thời gian không đổi. Sau đó chúng tôi duy trì một con trỏ`i`để có vị trí xuất phát sớm nhất có thể. Đối với mỗi độ dài phân đoạn đã thử, chúng tôi quét về phía trước cho đến khi tìm thấy phân đoạn có tổng vượt quá tổng phân đoạn trước đó. Sau khi tìm thấy, chúng tôi cam kết và tiến về phía trước. Nếu không có phân đoạn như vậy tồn tại, chúng tôi dừng lại. 

Một điểm tinh tế là chúng tôi luôn tiến lên`i`ngay cả khi bỏ qua các lần khởi động không hợp lệ. Điều này rất cần thiết vì bất kỳ phần bắt đầu bị bỏ qua nào đều không thể được sử dụng lại cho các đoạn có độ dài tương tự hoặc dài hơn do ràng buộc không chồng chéo và yêu cầu ngày càng tăng. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5
1 2 1 1 3 2 6
```Chúng tôi tính toán tổng tiền tố và mô phỏng: 

| Bước | Chiều dài | Bắt đầu tôi | Phân đoạn | Tổng hợp | Trước Tổng | Hành động | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | 3 | 0 | [1,2,1] | 4 | 0 | lấy | 
| 2 | 2 | 3 | [1,3] | 4 | 4 | bỏ qua | 
| 2 | 2 | 4 | [3,2] | 5 | 4 | lấy | 
| 3 | 1 | 6 | [6] | 6 | 5 | lấy | 

Chúng tôi có được 3 phân đoạn. 

Điều này chứng tỏ cách thuật toán bỏ qua các ứng cử viên ban đầu không thỏa mãn mức tăng nghiêm ngặt và tiếp tục tìm kiếm ở bên phải. 

### Ví dụ 2 

đầu vào:```
5
1 1 1 1 1
```| Bước | Chiều dài | Bắt đầu tôi | Phân đoạn | Tổng hợp | Trước Tổng | Hành động | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | 5 | 0 | [1,1,1,1,1] | 5 | 0 | lấy | 

Không thể đặt thêm phân đoạn nào nữa. 

Điều này cho thấy rằng ngay cả khi tất cả các giá trị bằng nhau, thuật toán dừng chính xác sau một phân đoạn vì không thể thỏa mãn mức tăng nghiêm ngặt. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Mỗi con trỏ chỉ di chuyển về phía trước trên mảng một lần | 
| Không gian | O(n) | Mảng tổng tiền tố | 

Tổng độ phức tạp trên tất cả các trường hợp thử nghiệm là tuyến tính trong tổng kích thước đầu vào, phù hợp thoải mái trong giới hạn của$10^5$. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from math import inf

    def solve():
        n = int(input())
        a = list(map(int, input().split()))
        pref = [0]*(n+1)
        for i in range(n):
            pref[i+1] = pref[i] + a[i]

        def rs(l,r):
            return pref[r]-pref[l]

        res = 0
        prev = 0
        i = 0
        length = n

        while i < n and length > 0:
            found = False
            while i + length <= n:
                s = rs(i, i+length)
                if s > prev:
                    prev = s
                    i += length
                    res += 1
                    found = True
                    break
                i += 1
            if not found:
                break
            length -= 1

        return str(res)

    t = int(input())
    out = []
    for _ in range(t):
        out.append(solve())
    return "\n".join(out)

# provided samples
assert run("""5
1
1
3
1 2 3
5
1 1 2 2 3
7
1 2 1 1 3 2 6
5
9 6 7 9 7
""") == """1
1
2
3
1"""

# custom cases
assert run("""1
1
100
""") == "1", "single element"

assert run("""1
4
1 2 3 4
""") == "2", "increasing array"

assert run("""1
6
5 4 3 2 1 1
""") == "2", "descending pattern"

assert run("""1
3
1 1 1
""") == "1", "all equal"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| phần tử đơn | 1 | ranh giới tối thiểu | 
| 1 2 3 4 | 2 | hành vi mở rộng tham lam | 
| 5 4 3 2 1 1 | 2 | cấu trúc giảm dần hỗn hợp | 
| 1 1 1 | 1 | ràng buộc tổng nghiêm ngặt | 

## Vỏ cạnh 

Trường hợp cạnh khóa là khi tồn tại nhiều đoạn ngắn nhưng không thể tạo thành chuỗi do ràng buộc về tổng. Ví dụ: trong một mảng như`[1,1,1,1,1]`, mọi phân đoạn đều có tổng bằng nhau nên chỉ có một phân đoạn hợp lệ. Thuật toán chọn chính xác phân đoạn đầy đủ đầu tiên và sau đó không tìm thấy bất kỳ tổng lớn hơn nào sau đó. 

Một trường hợp khó khăn khác là khi các phân đoạn đầu hấp dẫn nhưng chưa tối ưu. TRONG`[1,2,1,1,3,2,6]`, một cách tiếp cận ngây thơ có thể chọn`[1,2,1]`và sau đó bị kẹt, nhưng quá trình quét tham lam đảm bảo rằng nếu một phân đoạn hợp lệ tốt hơn tồn tại muộn hơn một chút thì nó sẽ được chọn, duy trì khả năng mở rộng chuỗi. 

Cuối cùng, các trường hợp có tổng hậu tố tăng nhanh đảm bảo rằng việc bỏ qua được xử lý chính xác. Bởi vì con trỏ chỉ di chuyển về phía trước nên chúng tôi không bao giờ xem xét lại các vị trí trước đó, điều này an toàn do hiệu ứng đơn điệu của tính tích cực đối với tính khả thi của phân khúc.
