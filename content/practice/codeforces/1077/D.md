---
title: "CF 1077D - Cắt ra"
description: "Chúng ta được cho một tập hợp các số nguyên và chúng ta muốn xây dựng một mảng mẫu có độ dài cố định $k$. Khi chúng tôi chọn mẫu này, chúng tôi liên tục cố gắng “trích xuất” nó từ nhiều tập hợp ban đầu: mỗi lần trích xuất sử dụng một lần xuất hiện của mọi giá trị trong mẫu và sau mỗi…"
date: "2026-06-15T06:40:46+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "sortings"]
categories: ["algorithms"]
codeforces_contest: 1077
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 521 (Div. 3)"
rating: 1600
weight: 1077
solve_time_s: 140
verified: true
draft: false
---

[CF 1077D - Cắt bỏ](https://codeforces.com/problemset/problem/1077/D) 

**Đánh giá:** 1600 
**Thẻ:** tìm kiếm nhị phân, sắp xếp 
**Thời gian giải:** 2 phút 20s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp nhiều số nguyên và chúng tôi muốn xây dựng một mảng mẫu có độ dài cố định$k$. Khi chúng tôi chọn mẫu này, chúng tôi liên tục cố gắng "trích xuất" nó từ nhiều tập hợp ban đầu: mỗi lần trích xuất sử dụng một lần xuất hiện của mọi giá trị trong mẫu và sau mỗi lần trích xuất hoàn toàn, các phần tử đã sử dụng sẽ biến mất vĩnh viễn khỏi nhóm. 

Mục đích là chọn mẫu sao cho số lần trích xuất hoàn chỉnh càng lớn càng tốt. Chúng ta có thể tự do hoán vị mẫu và lặp lại các giá trị bên trong nó, nhưng mọi bản sao phải được hỗ trợ đầy đủ bởi các tần số có sẵn trong mảng ban đầu. 

Quan điểm chính là câu trả lời chỉ phụ thuộc vào số lần mỗi giá trị xuất hiện trong mẫu cuối cùng, vì thứ tự không liên quan đến tính khả thi. Nếu một giá trị xuất hiện$x$lần trong mẫu thì mỗi lần trích xuất hoàn toàn sẽ tiêu tốn$x$bản sao của giá trị đó từ mảng. 

Các ràng buộc cho phép lên đến$2 \cdot 10^5$nên bất kỳ giải pháp nào thử tất cả các mẫu ứng cử viên hoặc mô phỏng trích xuất cho nhiều lựa chọn sẽ quá chậm. Bất cứ điều gì bậc hai trong$n$hoặc phụ thuộc vào việc liệt kê tất cả các tập hợp con của giá trị sẽ bị loại trừ ngay lập tức. 

Trường hợp khó phát hiện khi tần số bị lệch cực kỳ lớn. Nếu một số xuất hiện rất thường xuyên và những số khác rất hiếm, thì việc lựa chọn tham lam ngây thơ có thể lạm dụng quá mức số thường xuyên trong mẫu, ngăn chặn khả năng cân bằng mẫu trên toàn bộ mẫu.$k$các vị trí. Một trường hợp phức tạp khác là khi$k$lớn so với số lượng phần tử riêng biệt, buộc các lần lặp lại phải được kiểm soát cẩn thận bằng các ràng buộc tần số thay vì trực giác. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ cố gắng xây dựng mọi tập hợp kích thước có thể$k$từ các giá trị riêng biệt trong$s$, sau đó mô phỏng số lần nó có thể được trích xuất. Ngay cả khi chúng ta hạn chế bản thân chỉ với những giá trị hiện diện trong$s$, số lượng mẫu có thể tăng theo cấp số nhân trong$k$và với mỗi ứng viên, chúng ta cần tính tỷ lệ tần số giới hạn, chi phí$O(n)$. Điều này nhanh chóng trở nên không khả thi. 

Quan sát trọng tâm là nếu chúng ta cố định một mẫu ứng cử viên thì số lần nó có thể được trích xuất được xác định hoàn toàn bằng tần số: nếu một giá trị$v$xuất hiện$c_v$lần trong mẫu và xuất hiện$f_v$lần trong mảng thì chúng ta có thể trích xuất mẫu nhiều nhất$\min_v \lfloor f_v / c_v \rfloor$lần. 

Điều này gợi ý một quan điểm ngược lại: thay vì chọn một mẫu và tính điểm của nó, chúng tôi quyết định tổng số “vị trí” mà mỗi giá trị sẽ chiếm trên tất cả các bản sao được trích xuất. Nếu chúng ta nhắm tới$x$bản sao thì chúng ta cần một mẫu có tổng nhu cầu trên mỗi giá trị không vượt quá tần số sẵn có. Nghĩa là, tổng mức sử dụng của từng giá trị trên tất cả các bản sao phải tối đa$f_v$, vì vậy mỗi bản sao có thể sử dụng tối đa$\lfloor f_v / x \rfloor$lần xuất hiện của giá trị$v$. 

Điều này dẫn đến một sự chuyển đổi khóa: đối với một số lượng bản sao cố định$x$, chúng ta có thể tham lam xây dựng mô hình tốt nhất có thể bằng cách chiếm tới$\lfloor f_v / x \rfloor$lần xuất hiện của mỗi giá trị, lên đến tổng kích thước$k$. Nếu chúng ta có thể đạt được kích thước$k$, sau đó$x$bản sao là khả thi. 

Bây giờ vấn đề trở thành tìm cực đại$x$. Vì tính khả thi là đơn điệu trong$x$(nếu chúng ta có thể trích xuất$x$bản sao, chúng tôi có thể trích xuất ít hơn), chúng tôi có thể tìm kiếm nhị phân$x$. Đối với mỗi ứng viên$x$, chúng tôi kiểm tra tính khả thi và xây dựng một mô hình một cách tham lam. 

Cuối cùng, khi đạt mức tối đa$x$đã biết, chúng tôi xuất ra bất kỳ mẫu hợp lệ nào được xây dựng theo ràng buộc đó. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | O(n) | Quá chậm | 
| Tìm kiếm nhị phân + xây dựng tham lam |$O(n \log n)$| O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đếm tần số của từng giá trị trong mảng. Điều này đưa ra hạn chế cung cấp tối đa cho mọi thành phần mẫu có thể có. 
2. Xác định hàm kiểm tra xem liệu chúng ta có thể xây dựng một mẫu hỗ trợ ít nhất$x$chiết xuất đầy đủ. Với mỗi giá trị$v$, tính xem nó có thể xuất hiện bao nhiêu lần trong một bản sao:$\lfloor f_v / x \rfloor$. Đây là mức cho phép tối đa cho mỗi bản sao mà không làm cạn kiệt giá trị trên$x$sự lặp lại. 
3. Tính tổng tất cả các khả năng trên mỗi giá trị này. Nếu tổng số nhỏ hơn$k$, thì ngay cả mẫu hào phóng nhất cũng không thể dài tới mức$k$, Vì thế$x$bản sao là không thể. 
4. Nếu khả thi, hãy xây dựng mẫu bằng cách lặp qua các giá trị và cộng từng giá trị lên tới$\lfloor f_v / x \rfloor$lần cho đến khi mẫu đạt chiều dài$k$. 
5. Sử dụng tìm kiếm nhị phân trên$x$giữa 1 và$n$, liên tục kiểm tra tính khả thi và điều chỉnh giới hạn. 
6. Sau khi tìm được phương án khả thi tối đa$x$, xây dựng lại mẫu cuối cùng bằng cách sử dụng cùng một cấu trúc tham lam. 

Lý do điều này có tác dụng là vì với một số lượng bản sao cố định$x$, mẫu tốt nhất có thể luôn thu được bằng cách bão hòa hoàn toàn từng giá trị đến giới hạn trên mỗi bản sao của nó. Bất kỳ sai lệch nào cũng sẽ làm giảm tổng kích thước hoặc lãng phí dung lượng tần số sẵn có. Tìm kiếm nhị phân đảm bảo chúng ta tìm được giá trị lớn nhất$x$vẫn cho phép xây dựng một mẫu kích thước có chiều dài đầy đủ$k$. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def can(x, freq, k):
    total = 0
    for v in freq:
        total += freq[v] // x
        if total >= k:
            return True
    return total >= k

def build(x, freq, k):
    res = []
    for v in freq:
        cnt = freq[v] // x
        take = min(cnt, k - len(res))
        res.extend([v] * take)
        if len(res) == k:
            break
    return res

def solve():
    n, k = map(int, input().split())
    arr = list(map(int, input().split()))

    freq = {}
    for v in arr:
        freq[v] = freq.get(v, 0) + 1

    lo, hi = 1, n
    best = 1

    while lo <= hi:
        mid = (lo + hi) // 2
        if can(mid, freq, k):
            best = mid
            lo = mid + 1
        else:
            hi = mid - 1

    ans = build(best, freq, k)
    print(*ans)

if __name__ == "__main__":
    solve()
```Việc thực hiện tách biệt việc kiểm tra tính khả thi khỏi việc xây dựng lại. các`can`hàm tính toán xem có thể đạt được số lượng bản sao nhất định hay không mà không cần xây dựng mẫu thực tế, điều này giúp tìm kiếm nhị phân hiệu quả. các`build`sau đó, hàm sẽ xây dựng lại mẫu một cách tham lam bằng cách sử dụng cùng một logic, đảm bảo tính nhất quán với thử nghiệm tính khả thi. 

Một chi tiết tinh tế là việc xây dựng lại phải tôn trọng chính xác các giới hạn trên mỗi giá trị được sử dụng trong`can`. Bất kỳ sự không phù hợp nào giữa hai điều này sẽ dẫn đến các mẫu không chính xác ngay cả khi thử nghiệm tính khả thi là chính xác. 

## Ví dụ đã hoạt động 

Hãy xem xét đầu vào:```
7 3
1 2 3 2 4 3 1
```Đầu tiên chúng ta tính tần số: 

| giá trị | tần số | 
| --- | --- | 
| 1 | 2 | 
| 2 | 2 | 
| 3 | 2 | 
| 4 | 1 | 

Chúng tôi kiểm tra tính khả thi. Vì$x = 2$, mỗi giá trị đóng góp$\lfloor f_v / 2 \rfloor$: 

| giá trị | đóng góp | 
| --- | --- | 
| 1 | 1 | 
| 2 | 1 | 
| 3 | 1 | 
| 4 | 0 | 

Tổng cộng là 3, đủ cho$k = 3$, vậy có thể có 2 bản sao. Vì$x = 3$, các đóng góp trở thành số 0 ngoại trừ không thể đạt được 3 yếu tố, vì vậy nó không thành công. Như vậy tốt nhất$x = 2$. 

Tái thiết chọn một trong 1, 2, 3, đưa ra`[1, 2, 3]`. 

Bây giờ hãy xem xét một trường hợp sai lệch:```
6 4
1 1 1 1 2 2
```Tần số: 

| giá trị | tần số | 
| --- | --- | 
| 1 | 4 | 
| 2 | 2 | 

Thử$x = 2$: đóng góp là 2 cho giá trị 1 và 1 cho giá trị 2, tổng cộng là 3, không đủ cho$k = 4$. Vì thế chỉ$x = 1$hoạt động. Chúng ta có thể lấy tối đa 4 số một và 2 số hai, nhưng chỉ cần 4 phần tử, vì vậy một mẫu hợp lệ là`[1, 1, 1, 2]`hoặc tương tự. 

Dấu vết này cho thấy thuật toán cân bằng một cách tự nhiên các phần tử thường xuyên và hiếm mà không cần lý giải rõ ràng về thứ tự. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \log n)$| Tìm kiếm nhị phân qua$x$, mỗi lượt quét bản đồ tần số | 
| Không gian |$O(n)$| Từ điển tần số và lưu trữ đầu ra | 

Sự phức tạp phù hợp thoải mái trong giới hạn vì$n \le 2 \cdot 10^5$và khoảng 18-20 lần kiểm tra tính khả thi được thực hiện. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n, k = map(int, input().split())
    arr = list(map(int, input().split()))

    freq = {}
    for v in arr:
        freq[v] = freq.get(v, 0) + 1

    def can(x):
        total = 0
        for v in freq:
            total += freq[v] // x
        return total >= k

    lo, hi = 1, n
    best = 1
    while lo <= hi:
        mid = (lo + hi) // 2
        if can(mid):
            best = mid
            lo = mid + 1
        else:
            hi = mid - 1

    res = []
    for v in freq:
        cnt = freq[v] // best
        res.extend([v] * min(cnt, k - len(res)))
        if len(res) == k:
            break

    return " ".join(map(str, res))

# provided sample
assert run("7 3\n1 2 3 2 4 3 1\n") in {"1 2 3", "1 3 2", "2 1 3"}

# all equal
assert len(run("5 2\n1 1 1 1 1\n").split()) == 2

# k = n
assert len(run("4 4\n1 2 3 4\n").split()) == 4

# skewed frequencies
assert len(run("6 3\n1 1 1 2 2 3\n").split()) == 3

# minimum case
assert run("1 1\n7\n") == "7"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả các giá trị bằng nhau | đầu ra cỡ k | độ ổn định lặp lại cao | 
| k = n | tái thiết toàn bộ | bão hòa ranh giới | 
| tần số lệch | độ dài hợp lệ k | hành vi cân bằng | 
| phần tử đơn | chính nó | trường hợp cạnh tối thiểu | 

## Vỏ cạnh 

Đối với đầu vào trong đó tất cả các phần tử đều giống hệt nhau, chẳng hạn như`5 3`với`[7 7 7 7 7]`, thuật toán tính toán rằng mỗi bản sao chỉ có thể chứa một giá trị và tìm kiếm nhị phân sẽ tìm thấy nhiều nhất giá trị đó$\lfloor 5/3 \rfloor = 1$có thể khai thác hoàn toàn. Việc tái thiết chỉ đơn giản là đưa ra ba số 7 bất kỳ, phù hợp với các ràng buộc về tính khả thi. 

Đối với những trường hợp$k = n$, thuật toán giảm một cách hiệu quả xuống việc chọn một tập hợp đầy đủ mà không có ràng buộc lặp lại và tìm kiếm nhị phân vẫn trả về số lượng bản sao tối đa hợp lệ, thường là 1. Việc xây dựng lại sẽ lấp đầy toàn bộ mảng lên đến$n$, thỏa mãn tính đúng đắn một cách tầm thường. 

Đối với các đầu vào mất cân bằng cao như`[1,1,1,1,2]`với kích thước lớn$k$, việc kiểm tra tính khả thi đảm bảo rằng nguyên tố hiếm không bị phân bổ quá mức, ngăn chặn các mô hình quá lạc quan sẽ thất bại trong quá trình trích xuất.
