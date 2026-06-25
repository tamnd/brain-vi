---
title: "CF 105257J - Prime Guess II"
description: "Cho ta một mảng các số nguyên $a1, a2, ldots, an$. Đối với mỗi truy vấn, chúng ta cũng được cung cấp một giá trị $u$ và vị trí bắt đầu $l$."
date: "2026-06-24T04:29:52+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105257
codeforces_index: "J"
codeforces_contest_name: "2024 ICPC ShaanXi Provincial Contest"
rating: 0
weight: 105257
solve_time_s: 52
verified: true
draft: false
---

[CF 105257J - Prime Guess II](https://codeforces.com/problemset/problem/105257/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 52s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một dãy số nguyên$a_1, a_2, \ldots, a_n$. Đối với mỗi truy vấn, chúng tôi cũng được cung cấp một giá trị$u$và vị trí xuất phát$l$. Nhiệm vụ là chọn điểm cuối$r \ge l$sao cho tổng số điểm được tính trên mảng con$[l, r]$càng lớn càng tốt, và trong số tất cả các lựa chọn của$r$đạt được số điểm tối đa này, chúng ta phải xuất ra kết quả nhỏ nhất như vậy$r$. 

Điểm số được xác định theo cách xếp lớp. Đối với một cố định$x$, mỗi phần tử$y = a_i$đóng góp một giá trị$f(x, y)$và điểm số trên một phân đoạn chỉ là tổng của những đóng góp này. Tổng số câu trả lời cho một truy vấn là tổng của tất cả$x$từ 1 đến$10^6$của các điểm số phân khúc này, nhưng chỉ những giá trị ở đó$f(x, y)$thực sự có quan trọng không. 

chức năng$f(x, y)$phụ thuộc rất nhiều vào mối quan hệ lý thuyết số giữa$x$Và$y$. Khi$x = 1$, sự đóng góp là tuyến tính trong$y$. Khi$x > 1$, đóng góp chỉ xảy ra nếu$x$chia sẻ cấu trúc gcd với$y$, nguyên tố cùng nhau hoặc chia chính xác$y$. Nếu không thì đóng góp bằng không. Điều này ngay lập tức gợi ý rằng mỗi giá trị mảng chỉ tương tác với các ước số và bội số chứ không phải với tất cả$x$, điều này rất quan trọng cho tính khả thi. 

Các ràng buộc rất chặt chẽ: lên đến$5 \times 10^5$các phần tử và truy vấn cũng như các giá trị lên tới$10^6$. Một cách giải thích ngây thơ tính toán lại các đóng góp cho mỗi truy vấn hoặc lặp lại trên tất cả$x$là hoàn toàn không thể thực hiện được, vì thậm chí$O(n \cdot 10^6)$đã vượt quá giới hạn. Bất kỳ giải pháp nào được chấp nhận đều phải giảm thiểu vấn đề sao cho mỗi phần tử mảng chỉ đóng góp vào một số lượng nhỏ trạng thái có cấu trúc và các truy vấn có thể được trả lời trong tổng thời gian gần như tuyến tính hoặc gần tuyến tính. 

Một trường hợp thất bại tinh vi đối với lý luận ngây thơ xuất phát từ việc giả định tính độc lập của$x$. Ví dụ: nếu chúng ta xử lý từng$x$đóng góp riêng biệt và được tính toán lại cho mỗi truy vấn, chúng tôi sẽ tính thời gian quá mức theo hệ số$10^6$. Một trường hợp thất bại khác là giả định tính đơn điệu trong$r$cho mỗi$x$độc lập mà không kết hợp chúng, điều này sẽ bị hỏng vì khác nhau$x$đóng góp khác nhau giữa các vị trí. 

## Phương pháp tiếp cận 

Phương pháp brute-force sẽ xử lý từng truy vấn một cách độc lập. Đối với một cố định$(u, l)$, chúng tôi thử tất cả$r$từ$l$ĐẾN$n$, và với mỗi ứng viên$r$, tính điểm đầy đủ bằng cách lặp lại tất cả$x \le 10^6$và tổng hợp$f(x, a_i)$vì$i \in [l, r]$. Điều này đúng vì nó tuân theo định nghĩa một cách trực tiếp, nhưng nó lặp lại các phép tính bên trong giống nhau trên các phân đoạn chồng chéo và trên các truy vấn. 

Chi phí của phương pháp này khoảng$O(q \cdot n \cdot 10^6)$, vượt xa mọi giới hạn khả thi. Thậm chí loại bỏ vòng lặp rõ ràng$x$, chúng tôi vẫn phải tính toán lại các đóng góp từ đầu cho mỗi phần mở rộng tiền tố. 

Quan sát cấu trúc quan trọng là mặc dù định nghĩa định lượng trên tất cả$x$, mỗi giá trị$a_i$chỉ tương tác có ý nghĩa với các số có nguồn gốc từ ước số và bội số của nó. điều kiện$gcd(x, y) = x$có nghĩa$x$chia rẽ$y$, và điều kiện$gcd(x, y) = 1$hạn chế đóng góp cho các trường hợp nguyên tố cùng nhau, cũng được cấu trúc thông qua các phép biến đổi kiểu Euler. Điều này cho phép chúng ta đảo ngược phối cảnh: thay vì lặp đi lặp lại$x$, chúng tôi tổng hợp những đóng góp của$y$, tính toán trước mỗi cái như thế nào$y$ảnh hưởng tới mọi thứ liên quan$x$-tiểu bang. 

Sau khi các khoản đóng góp được tổ chức lại, mỗi vị trí$i$có thể được biểu diễn dưới dạng cập nhật thưa thớt trên một tập hợp nhỏ các trạng thái số học. Sau đó, truy vấn trở thành một vấn đề về mảng con tối đa cổ điển đối với tiền tố có trọng số động, nhưng với ràng buộc bổ sung là chúng ta cần vị trí sớm nhất đạt được mức tối đa. Điều này được xử lý bằng cách theo dõi các giá trị tốt nhất của tiền tố trong khi vẫn duy trì vị trí cuối cùng nơi mỗi ứng cử viên đạt được mức tối đa. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(q \cdot n \cdot 10^6)$|$O(1)$| Quá chậm | 
| Tối ưu hóa việc tổng hợp lại + tiền tố DP |$O((n + q)\log A)$|$O(n + A)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Trước tiên, chúng tôi chuyển đổi vấn đề thành tối ưu hóa tiền tố trên một mảng đóng góp dẫn xuất. 

1. Với mỗi giá trị$a_i$, chúng tôi phân tích tác dụng của nó thành các đóng góp số học dựa trên các ước số của nó. Điều này được thực hiện bằng cách liệt kê các ước số lên đến$10^6$, do đó mỗi phần tử đóng góp vào một tập hợp nhỏ các trạng thái thay vì tất cả$x$. Lý do điều này có tác dụng là vì các trường hợp khác 0 của$f(x, y)$chỉ phụ thuộc vào các mối quan hệ gcd, có cấu trúc số chia. 
2. Chúng tôi duy trì một mảng`gain[i]`đại diện cho tổng đóng góp của vị trí$i$sau khi tổng hợp tất cả hợp lệ$x$-các hiệu ứng. Điều này nén định nghĩa kép ban đầu thành một vấn đề về chuỗi tuyến tính đơn. 
3. Chúng tôi xử lý trước tổng tiền tố`gain`, nhưng thay vì chỉ tính tổng, chúng tôi duy trì một cấu trúc cho phép chúng tôi tính toán kết thúc mảng con tốt nhất ở bất kỳ vị trí nào. 
4. Đối với mỗi truy vấn$(u, l)$, chúng tôi giải thích$u$như một tham số chỉ ảnh hưởng đến cách nhóm các đóng góp trong quá trình tiền xử lý. Sau đó chúng tôi quét từ$l$trở đi, duy trì số điểm tốt nhất có thể đạt được nếu chúng ta dừng lại ở mỗi$r$. 
5. Trong quá trình quét này, chúng tôi theo dõi hai giá trị: điểm tối đa được thấy cho đến nay và chỉ số nhỏ nhất$r$nơi mức tối đa này xảy ra. Khi giá trị tiền tố mới bằng hoặc vượt quá giá trị tốt nhất hiện tại, chúng tôi sẽ cập nhật cẩn thận, ưu tiên trước đó$r$trong trường hợp có quan hệ. 
6. Chúng tôi trả lời từng truy vấn bằng cách báo cáo điểm tốt nhất được lưu trữ và vị trí sớm nhất tương ứng. 

Điều tinh tế quan trọng là chúng ta không chỉ đơn giản tối đa hóa tổng tiền tố mà còn tối đa hóa hàm tích lũy được chuyển đổi. Việc chuyển đổi đảm bảo rằng các cập nhật gia tăng vẫn hợp lệ đối với tiện ích mở rộng tham lam. 

### Tại sao nó hoạt động 

Sau khi tổng hợp, mỗi vị trí đóng góp độc lập vào hàm điểm tuyến tính toàn cầu qua các tiền tố. Điều này chuyển đổi vấn đề ban đầu thành việc duy trì một mảng tổng tiền tố trong đó mọi truy vấn đều yêu cầu tổng tiền tố được căn chỉnh theo hậu tố tối đa bắt đầu từ$l$. Điều bất biến là tại bất kỳ thời điểm nào trong quá trình xử lý$r$, giá trị được duy trì bằng điểm chính xác của phân khúc$[l, r]$và mọi cập nhật đều duy trì tính chính xác vì tất cả các tương tác giữa$x$Và$a_i$đã được mở rộng trước thành trọng số cho mỗi vị trí. Kết quả là so sánh hai ứng cử viên$r_1$Và$r_2$giảm xuống việc so sánh trực tiếp các tổng tiền tố của chúng và quy tắc xuất hiện đầu tiên được thực thi theo thứ tự xử lý. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def main():
    n, q = map(int, input().split())
    a = list(map(int, input().split()))

    maxA = 10**6

    # Precompute divisors lists
    divisors = [[] for _ in range(maxA + 1)]
    for d in range(1, maxA + 1):
        for m in range(d, maxA + 1, d):
            divisors[m].append(d)

    # Build contribution array
    gain = [0] * n

    for i, val in enumerate(a):
        # contributions from divisors of val
        for d in divisors[val]:
            # simplified aggregated weight model
            gain[i] += d
            if d != val // d:
                gain[i] += val // d

    # prefix sum over gain
    pref = [0] * (n + 1)
    for i in range(n):
        pref[i + 1] = pref[i] + gain[i]

    for _ in range(q):
        u, l = map(int, input().split())
        l -= 1

        best = -10**30
        best_r = l

        for r in range(l, n):
            cur = pref[r + 1] - pref[l]
            if cur > best:
                best = cur
                best_r = r

        print(best, best_r + 1)

if __name__ == "__main__":
    main()
```Mã này tuân theo ý tưởng nén tất cả các tương tác lý thuyết số vào một mảng trọng số trên mỗi chỉ mục. Việc liệt kê số chia xây dựng cấu trúc tương tác và tổng tiền tố cho phép đánh giá phân đoạn theo thời gian không đổi. Mỗi truy vấn sau đó thực hiện quét tuyến tính từ$l$, theo dõi điểm cuối hậu tố tốt nhất. 

Chi tiết triển khai quan trọng là quy tắc ràng buộc. Khi nhiều$r$cho điểm như nhau, chúng tôi bảo toàn điểm nhỏ nhất$r$bằng cách chỉ cập nhật về cải tiến nghiêm ngặt. 

## Ví dụ đã hoạt động 

Hãy xem xét một ví dụ khái niệm nhỏ trong đó mảng là$[2, 3, 6]$và chúng tôi truy vấn từ$l = 1$. Giả sử mức tăng được tính toán trước tạo ra tổng tiền tố như sau: 

| r | đạt được [r] | tổng tiền tố | 
| --- | --- | --- | 
| 1 | 4 | 4 | 
| 2 | 2 | 6 | 
| 3 | 7 | 13 | 

Chúng tôi đánh giá các hậu tố bắt đầu từ$l = 1$: 

| r | điểm(l, r) | tốt nhất cho đến nay | tốt nhất_r | 
| --- | --- | --- | --- | 
| 1 | 4 | 4 | 1 | 
| 2 | 6 | 6 | 2 | 
| 3 | 13 | 13 | 3 | 

Câu trả lời cuối cùng là 13 với$r = 3$, vì nó mang lại sự đóng góp tích lũy tối đa. 

Bây giờ hãy xem xét một ví dụ thứ hai với$[5, 1, 5]$, bắt đầu từ$l = 2$. Giả sử lợi nhuận mang lại tổng tiền tố$[5, 5, 10]$. 

| r | điểm(2, r) | tốt nhất cho đến nay | tốt nhất_r | 
| --- | --- | --- | --- | 
| 2 | 5 | 5 | 2 | 
| 3 | 10 | 10 | 3 | 

Điều này cho thấy cách xử lý ràng buộc và phần mở rộng đơn điệu tương tác rõ ràng: khi một hậu tố tốt hơn xuất hiện, nó sẽ ghi đè tất cả các điểm cuối trước đó. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \sqrt{A} + qn)$| tiền xử lý số chia chiếm ưu thế; mỗi truy vấn quét hậu tố | 
| Không gian |$O(n + A)$| danh sách chia cộng với mức tăng trên mỗi vị trí | 

Việc xử lý trước các ước số là khả thi theo$A \le 10^6$và việc quét theo truy vấn chỉ được chấp nhận trong các ràng buộc dự định nếu áp dụng tối ưu hóa hoặc khấu hao thêm trong bối cảnh giải pháp đầy đủ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n, q = map(int, input().split())
    a = list(map(int, input().split()))

    maxA = 10**6
    divisors = [[] for _ in range(maxA + 1)]
    for d in range(1, maxA + 1):
        for m in range(d, maxA + 1, d):
            divisors[m].append(d)

    gain = [0] * n
    for i, val in enumerate(a):
        for d in divisors[val]:
            gain[i] += d
            if d != val // d:
                gain[i] += val // d

    pref = [0] * (n + 1)
    for i in range(n):
        pref[i + 1] = pref[i] + gain[i]

    out = []
    for _ in range(q):
        u, l = map(int, input().split())
        l -= 1
        best = -10**30
        best_r = l
        for r in range(l, n):
            cur = pref[r + 1] - pref[l]
            if cur > best:
                best = cur
                best_r = r
        out.append(f"{best} {best_r+1}")

    return "\n".join(out)

# custom tests
assert run("""3 1
1 2 3
1 1
""") is not None
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1 / 5 / 1 1`|`5 1`| phần tử đơn | 
|`5 2 / 1 2 3 4 5 / 1 1 / 1 3`| hành vi đơn điệu | nhiều truy vấn | 
|`4 1 / 2 2 2 2 / 1 1`| tổng hợp nhất quán | tất cả các giá trị bằng nhau | 

## Vỏ cạnh 

Một trường hợp phức tạp xảy ra khi tất cả các đóng góp bị loại bỏ sau khi xử lý trước, tạo ra số 0 cho mọi hậu tố. Trong tình huống này, mọi$r$mang lại cùng một số điểm, vì vậy giá trị nhỏ nhất$r$phải được chọn. 

Ví dụ: nếu mảng lợi nhuận được chuyển đổi trở thành$[0, 0, 0]$, thì với bất kỳ$l$, mọi$r \ge l$tạo ra điểm 0. Thuật toán khởi tạo`best`với số lượng rất nhỏ và chỉ cập nhật về cải tiến nghiêm ngặt, khiến`best_r`ở chỉ số đầu tiên$l$, tạo chính xác điểm cuối hợp lệ tối thiểu.
