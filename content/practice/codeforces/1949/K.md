---
title: "CF 1949K - Tạo hình tam giác"
description: "Chúng ta được cấp một tập hợp các số nguyên dương và ba kích thước nhóm bắt buộc. Mỗi số phải thuộc chính xác một trong ba nhóm và mỗi nhóm phải chứa chính xác số phần tử được yêu cầu. Sau khi chia các số, chúng ta chỉ xét tổng của ba nhóm."
date: "2026-05-31T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "math"]
categories: ["algorithms"]
codeforces_contest: 1949
codeforces_index: "K"
codeforces_contest_name: "European Championship 2024 - Online Mirror (Unrated, ICPC Rules, Teams Preferred)"
rating: 2800
weight: 1949
solve_time_s: 149
verified: true
draft: false
---

[CF 1949K - Tạo tam giác](https://codeforces.com/problemset/problem/1949/K) 

**Đánh giá:** 2800 
**Tags:** thuật toán xây dựng, toán học 
**Thời gian giải:** 2m 29s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một tập hợp các số nguyên dương và ba kích thước nhóm bắt buộc. Mỗi số phải thuộc chính xác một trong ba nhóm và mỗi nhóm phải chứa chính xác số phần tử được yêu cầu. 

Sau khi chia các số, chúng ta chỉ xét tổng của ba nhóm. Nếu những khoản tiền đó là$s_a$,$s_b$, Và$s_c$, chúng phải tạo thành một tam giác không suy biến. Vì tất cả các giá trị đều dương nên điều kiện của tam giác giảm xuống một bất đẳng thức duy nhất:$$\max(s_a,s_b,s_c) < s_a+s_b+s_c-\max(s_a,s_b,s_c)$$Nói cách khác, không có cạnh nào có thể bằng ít nhất một nửa tổng chu vi. 

Tổng số phần tử trong tất cả các trường hợp thử nghiệm nhiều nhất là$200\,000$. Điều đó ngay lập tức loại trừ bất kỳ số mũ nào, bất kỳ tập hợp con DP nào và thậm chí$O(n^2)$mỗi trường hợp thử nghiệm. Chúng ta cần một cái gì đó gần gũi$O(n \log n)$, điều này gợi ý rõ ràng về việc sắp xếp cộng với cách xây dựng tham lam. 

Phần khó khăn là kích thước nhóm được cố định. Chúng ta không được tự do di chuyển các phần tử một cách tùy tiện khi nhóm đã đầy. 

Một sai lầm dễ mắc phải là nghĩ rằng chỉ có phần tử lớn nhất mới quan trọng. 

Coi như:```
n = 6
sizes = (2,2,2)
values = [3,1,1,1,1,1]
```Tổng số tiền là$8$, do đó tổng của mỗi nhóm phải nhỏ hơn$4$. Phần tử lớn nhất chỉ$3$, trông có vẻ vô hại. 

Nhưng mỗi nhóm phải chứa hai số. Nhóm duy nhất có thể chứa$3$có tổng$4$, và các nhóm còn lại có tổng$2$Và$2$. Các cạnh kết quả là$(4,2,2)$, đó là suy biến. Câu trả lời đúng là`NO`. 

Một trường hợp nguy hiểm khác là khi có lời giải nhưng các số lớn nhất không thể đơn giản được xếp lại với nhau.```
sizes = (1,2,2)
values = [2,2,2,1,1]
```Xếp hai số lớn nhất vào cùng một nhóm sẽ được tổng$(2,4,2)$, thất bại. 

Một phân vùng hợp lệ là:```
2
2 1
2 1
```với số tiền$(2,3,3)$. 

Thuật toán phải suy luận về các vị trí trong tương lai chứ không chỉ về số tiền hiện tại. 

## Phương pháp tiếp cận 

Giải pháp brute-force sẽ tìm mọi cách để chọn nhóm thứ nhất, sau đó tìm mọi cách để chọn nhóm thứ hai và xếp các số còn lại vào nhóm thứ ba. 

Ví dụ: nếu kích thước là$n_a,n_b,n_c$, số phân vùng là$$\binom{n}{n_a}\binom{n-n_a}{n_b}.$$Ngay cả đối với$n=40$, cái này đã lớn về mặt thiên văn rồi. Với$n$lên đến$200\,000$, vũ lực là hoàn toàn không thể. 

Quan sát quan trọng là giá trị của tam giác chỉ phụ thuộc vào tổng nhóm cuối cùng. 

Cho phép$$T = \sum x_i.$$Điều kiện tam giác tương đương với việc yêu cầu mọi tổng nhóm phải nhỏ hơn rất nhiều so với$T/2$. 

Bây giờ hãy tưởng tượng việc xử lý các số từ lớn nhất đến nhỏ nhất. 

Giả sử một nhóm hiện có tổng$S$và vẫn cần$k$nhiều phần tử hơn. Ngay cả trong kịch bản lạc quan nhất, những yếu tố tương lai đó ít nhất phải đóng góp vào tổng giá trị$k$số nhỏ nhất còn lại 

Điều đó đưa ra giới hạn dưới cho tổng nhóm cuối cùng. 

Nếu thậm chí giới hạn dưới đó đã đạt tới$T/2$, nhóm này không bao giờ có thể trở thành hợp lệ. 

Điều này biến vấn đề thành một cuộc kiểm tra tính khả thi đầy tham lam. Trong khi gán số lớn nhất còn lại, chúng tôi chỉ xếp nó vào nhóm mà tương lai tốt nhất có thể vẫn giữ tổng cuối cùng ở dưới$T/2$. 

Vì chỉ có ba nhóm nên việc kiểm tra tất cả các nhóm ứng cử viên cho từng phần tử là công việc liên tục sau khi sắp xếp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | Hàm mũ | Quá chậm | 
| Xây dựng tham lam tối ưu |$O(n \log n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính tổng số tiền$T$. 
2. Sắp xếp tất cả các số theo thứ tự tăng dần. 
3. Tính trước tổng tiền tố của mảng đã được sắp xếp. 

Tổng tiền tố cho phép chúng ta nhanh chóng có được tổng nhỏ nhất$k$những con số. 
4. Với mỗi nhóm, lưu trữ: 

- tổng hiện tại của nó, 
- còn bao nhiêu slot trống, 
- những con số thực tế được gán cho nó. 
5. Xử lý các số từ lớn nhất đến nhỏ nhất. 
6. Gọi số hiện tại là$x$. 
7. Hãy thử từng nhóm trong ba nhóm. 

Giả sử một nhóm hiện có tổng$S$và vẫn cần$r$các yếu tố bao gồm$x$. 

Sau khi đặt$x$, nhóm vẫn sẽ cần$r-1$nhiều phần tử hơn. 

Phần đóng góp bổ sung nhỏ nhất có thể có là tổng của phần đóng góp nhỏ nhất$r-1$số trong số tất cả các số vẫn chưa được xử lý. 

Vì chúng tôi xử lý từ lớn nhất đến nhỏ nhất nên những con số trong tương lai đó chính xác là những giá trị nhỏ hơn. Sử dụng nhỏ nhất toàn cầu$r-1$các giá trị đưa ra giới hạn dưới luôn an toàn. 
8. Xác định$$\text{best\_possible} = S + x + \text{sumSmallest}(r-1).$$Nếu như$$2 \cdot \text{best\_possible} < T,$$thì ngay cả khi hoàn thành lạc quan nhất thì nhóm này vẫn có thể là một cạnh tam giác hợp lệ. 

1. Địa điểm$x$vào nhóm đầu tiên thỏa mãn điều kiện đó. 
2. Nếu không có nhóm nào chấp nhận$x$, đầu ra`NO`. 
3. Nếu tất cả các số được gán thành công, xuất ra`YES`và ba nhóm. 

### Tại sao nó hoạt động 

Bất biến là: 

Đối với mỗi nhóm, vẫn còn ít nhất một cách để hoàn thành các vị trí còn lại trong khi vẫn giữ tổng số tiền cuối cùng ở mức dưới đây.$T/2$. 

Khi chúng ta đặt một số$x$, chúng tôi chỉ cho phép các phép gán bảo toàn bất biến này. 

số lượng$$S + x + \text{sumSmallest}(r-1)$$là số tiền cuối cùng tối thiểu mà nhóm có thể đạt được sau khi lấy$x$. Nếu thậm chí mức tối thiểu đó đã ít nhất$T/2$, sau đó đặt$x$ở đó khiến thành công là không thể. 

Ngược lại, nếu sự bất bình đẳng vẫn giữ nguyên thì nhóm vẫn còn chỗ để ở dưới mức$T/2$. 

Việc xử lý số từ lớn nhất đến nhỏ nhất là rất quan trọng. Số lượng lớn là khó đặt nhất. Nếu một số lớn không thể vừa với bất kỳ nhóm nào mà không vi phạm tính bất biến, thì không có số nhỏ hơn nào sau đó có thể khắc phục được tình trạng này. 

Khi thuật toán kết thúc, mọi nhóm vẫn thỏa mãn bất biến không còn vị trí nào. Giới hạn dưới trở thành tổng cuối cùng chính xác, vì vậy mọi tổng nhóm đều hoàn toàn thấp hơn$T/2$. Đó chính xác là điều kiện tam giác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())

    out = []

    for _ in range(t):
        n, na, nb, nc = map(int, input().split())
        a = list(map(int, input().split()))

        total = sum(a)

        a.sort()

        pref = [0] * (n + 1)
        for i in range(n):
            pref[i + 1] = pref[i] + a[i]

        groups = [[], [], []]
        sums = [0, 0, 0]
        rem = [na, nb, nc]

        ok = True

        for i in range(n - 1, -1, -1):
            x = a[i]

            placed = False

            for g in range(3):
                if rem[g] == 0:
                    continue

                need_after = rem[g] - 1

                min_final_sum = sums[g] + x + pref[need_after]

                if 2 * min_final_sum < total:
                    groups[g].append(x)
                    sums[g] += x
                    rem[g] -= 1
                    placed = True
                    break

            if not placed:
                ok = False
                break

        if not ok:
            out.append("NO")
            continue

        out.append("YES")
        out.append(" ".join(map(str, groups[0])))
        out.append(" ".join(map(str, groups[1])))
        out.append(" ".join(map(str, groups[2])))

    sys.stdout.write("\n".join(out))

solve()
```Bước đầu tiên là sắp xếp các giá trị. Mọi kiểm tra tính khả thi trong tương lai đều phụ thuộc vào việc biết con số nào vẫn nhỏ hơn con số hiện tại. 

Mảng tổng tiền tố là chi tiết triển khai giúp việc kiểm tra tham lam được nhanh chóng. Thay vì liên tục tính tổng nhỏ nhất$k$giá trị, chúng tôi có được số lượng đó trong$O(1)$. 

biểu hiện```
min_final_sum = sums[g] + x + pref[need_after]
```là cốt lõi của thuật toán. Nó đại diện cho tổng cuối cùng nhỏ nhất mà nhóm này có thể có nếu chúng ta đặt$x$vào đó ngay bây giờ. 

Sự bất bình đẳng nghiêm ngặt```
2 * min_final_sum < total
```phải luôn nghiêm ngặt. Thay thế nó bằng`<=`sẽ cho phép các tam giác suy biến trong đó một cạnh bằng tổng của hai cạnh còn lại. 

Một điểm tinh tế khác là chúng tôi lưu trữ các giá trị thực tế trực tiếp trong các nhóm. Tuyên bố yêu cầu các con số, không phải chỉ số của họ. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
6 2 2 2
1 1 1 1 1 1
```Tổng số tiền$T=6$. 

| Hiện tại x | Tổng nhóm trước | Các vị trí còn lại trước | Nhóm được chọn | 
| --- | --- | --- | --- | 
| 1 | (0,0,0) | (2,2,2) | A | 
| 1 | (1,0,0) | (1,2,2) | A | 
| 1 | (2,0,0) | (0,2,2) | B | 
| 1 | (2,1,0) | (0,1,2) | B | 
| 1 | (2,2,0) | (0,0,2) | C | 
| 1 | (2,2,1) | (0,0,1) | C | 

Tổng cuối cùng là$(2,2,2)$. 

Mỗi cạnh đều nhỏ hơn$3=T/2$, vậy tam giác đúng. 

Ví dụ này cho thấy trường hợp cân bằng đơn giản nhất trong đó tất cả các nhóm đều kết thúc bằng số tiền giống nhau. 

### Ví dụ 2 

đầu vào:```
6 2 2 2
1 1 1 1 1 3
```Tổng số tiền$T=8$. 

| Hiện tại x | Nhóm ứng viên | Số tiền cuối cùng tối thiểu có thể | 
| --- | --- | --- | 
| 3 | Nhóm nào | 4 | 
| 3 | Kiểm tra tình trạng |$2 \cdot 4 = 8$| 

Bất đẳng thức phải chặt chẽ:```
2 * 4 < 8
```đó là sai. 

Phần tử đầu tiên không thể được đặt ở bất cứ đâu. 

Thuật toán xuất ra ngay lập tức`NO`. 

Dấu vết này chứng tỏ tại sao mẫu này là không thể. Bất kỳ nhóm nào chứa$3$cuối cùng phải đạt được tổng$4$, bằng nửa chu vi. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \log n)$| Sắp xếp chiếm ưu thế trong thời gian chạy | 
| Không gian |$O(n)$| Nhóm được lưu trữ và tổng tiền tố | 

Tổng số phần tử trong tất cả các trường hợp thử nghiệm nhiều nhất là$200\,000$. Sắp xếp tất cả các yếu tố có giá xấp xỉ$200\,000 \log_2 200\,000$, dễ dàng phù hợp trong thời hạn. Việc sử dụng bộ nhớ là tuyến tính và thoải mái dưới mức$256$Giới hạn MB. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    import sys
    input = sys.stdin.readline

    t = int(input())
    out = []

    for _ in range(t):
        n, na, nb, nc = map(int, input().split())
        a = list(map(int, input().split()))

        total = sum(a)

        a.sort()

        pref = [0] * (n + 1)
        for i in range(n):
            pref[i + 1] = pref[i] + a[i]

        groups = [[], [], []]
        sums = [0, 0, 0]
        rem = [na, nb, nc]

        ok = True

        for i in range(n - 1, -1, -1):
            x = a[i]

            placed = False

            for g in range(3):
                if rem[g] == 0:
                    continue

                need_after = rem[g] - 1

                min_final_sum = sums[g] + x + pref[need_after]

                if 2 * min_final_sum < total:
                    groups[g].append(x)
                    sums[g] += x
                    rem[g] -= 1
                    placed = True
                    break

            if not placed:
                ok = False
                break

        if not ok:
            out.append("NO")
        else:
            out.append("YES")

    return "\n".join(out) + "\n"

# provided samples
assert run(
"""4
6 2 2 2
1 1 1 1 1 1
5 3 1 1
1 1 1 1 1
6 2 2 2
1 1 1 1 1 3
8 1 2 5
16 1 1 1 1 1 1 12
"""
) == "YES\nNO\nNO\nYES\n"

# minimum size
assert run(
"""1
3 1 1 1
1 1 1
"""
) == "YES\n"

# all equal values
assert run(
"""1
9 3 3 3
5 5 5 5 5 5 5 5 5
"""
) == "YES\n"

# degenerate boundary
assert run(
"""1
6 2 2 2
1 1 1 1 1 3
"""
) == "NO\n"

# uneven sizes but valid
assert run(
"""1
5 1 2 2
2 2 2 1 1
"""
) == "YES\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`3 1 1 1 / 1 1 1`| CÓ | Trường hợp pháp lý nhỏ nhất | 
| Chín giá trị bằng nhau | CÓ | Cấu trúc cân bằng đối xứng | 
|`1 1 1 1 1 3`với kích thước`(2,2,2)`| KHÔNG | Biên tam giác suy biến | 
|`2 2 2 1 1`với kích thước`(1,2,2)`| CÓ | Các giá trị lớn phải được tách riêng | 

## Vỏ cạnh 

Hãy xem xét:```
6 2 2 2
1 1 1 1 1 3
```Tổng số tiền là$8$. Bất kỳ nhóm nào chứa$3$cũng phải chứa thêm một phần tử nữa vì mọi kích thước nhóm đều$2$. Tổng cuối cùng nhỏ nhất có thể có của nó là$4$. 

Thuật toán tính toán:```
min_final_sum = 3 + 1 = 4
```và kiểm tra:```
2 * 4 < 8
```thất bại. 

Không nhóm nào có thể chấp nhận$3$, vậy đáp án đúng`NO`. 

Bây giờ hãy xem xét:```
5 1 2 2
2 2 2 1 1
```Tổng số tiền là$8$. 

đầu tiên$2$có thể đi vào kích thước-$1$nhóm vì tổng cuối cùng của nó vẫn còn$2$, an toàn bên dưới$4$. 

Hai người còn lại$2$các giá trị không thể được hợp nhất vào cùng một nhóm, bởi vì điều đó sẽ buộc tổng số trong tương lai là$4$. Quá trình kiểm tra tính khả thi sẽ từ chối vị trí đó và phân bổ chúng vào các nhóm khác nhau. 

Tổng cuối cùng trở thành:```
2, 3, 3
```thỏa mãn bất đẳng thức tam giác. 

Đây chính xác là trường hợp mà kẻ tham lam ngây thơ chỉ dựa trên số tiền hiện tại sẽ thất bại, trong khi kẻ tham lam dựa trên tính khả thi lại thành công. 

Giải pháp này dựa vào việc dự kiến ​​mức đóng góp tối thiểu có thể có trong tương lai, đó là lý do tại sao giải pháp này xử lý các trường hợp ranh giới này một cách chính xác.
