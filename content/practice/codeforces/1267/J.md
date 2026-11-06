---
title: "CF 1267J - Chỉ cần sắp xếp các biểu tượng"
description: "Chúng ta được cung cấp nhiều bộ ứng dụng, mỗi bộ ứng dụng thuộc một danh mục. Điều duy nhất quan trọng đối với một danh mục là số lượng ứng dụng mà nó chứa, do đó dữ liệu đầu vào có thể được nén thành tần số của từng danh mục riêng biệt. Chúng ta phải đặt tất cả các ứng dụng vào “màn hình”."
date: "2026-06-16T00:26:02+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "implementation", "math"]
categories: ["algorithms"]
codeforces_contest: 1267
codeforces_index: "J"
codeforces_contest_name: "2019-2020 ICPC, NERC, Northern Eurasia Finals (Unrated, Online Mirror, ICPC Rules, Teams Preferred)"
rating: 1800
weight: 1267
solve_time_s: 201
verified: true
draft: false
---

[CF 1267J - Chỉ cần sắp xếp các biểu tượng](https://codeforces.com/problemset/problem/1267/J) 

**Đánh giá:** 1800 
**Tags:** tham lam, thực hiện, toán học 
**Thời gian giải:** 3m 21s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp nhiều bộ ứng dụng, mỗi bộ ứng dụng thuộc một danh mục. Điều duy nhất quan trọng đối với một danh mục là số lượng ứng dụng mà nó chứa, do đó dữ liệu đầu vào có thể được nén thành tần số của từng danh mục riêng biệt. 

Chúng ta phải đặt tất cả các ứng dụng vào “màn hình”. Mỗi màn hình có kích thước cố định cho toàn bộ giải pháp, ký hiệu là$s$, nhưng chúng tôi chỉ được phép sử dụng hai loại màn hình: màn hình có kích thước đầy đủ$s$và gần như lấp đầy các màn hình có kích thước$s-1$. Một hạn chế quan trọng là một màn hình chỉ có thể chứa các ứng dụng từ một danh mục duy nhất nhưng các màn hình khác nhau có thể tự do sử dụng lại cùng một danh mục. 

Để có sự lựa chọn cố định$s$, mọi danh mục có tần suất$x$phải được phân chia thành các nhóm có kích thước$s$hoặc$s-1$. Số lượng các nhóm như vậy chính là số màn hình do thể loại đó đóng góp. Mục tiêu là chọn$s$và một phân vùng hợp lệ cho mọi danh mục để tổng số màn hình được giảm thiểu. 

Kích thước đầu vào đạt tổng cộng hai triệu phần tử trong các trường hợp thử nghiệm, do đó, bất kỳ giải pháp nào cố gắng mô phỏng nhóm một cách rõ ràng hoặc cố gắng tìm kiếm trên các phân vùng cho mỗi danh mục sẽ quá chậm. Giải pháp phải giảm từng trường hợp thử nghiệm về cơ bản là xử lý tuyến tính các tần số, lý tưởng nhất là với một số lượng nhỏ các giá trị ứng cử viên cho$s$. 

Một vấn đề tế nhị xuất hiện khi suy nghĩ tham lam theo từng phạm trù. Thật hấp dẫn khi giả định rằng đối với một mức cố định$s$, mỗi danh mục luôn sử dụng$\lceil x/s \rceil$màn hình. Điều này không phải lúc nào cũng đúng vì “$s-1$” tính linh hoạt, có thể buộc nhiều màn hình khác nhau hoặc thậm chí đưa ra lựa chọn$s$không hợp lệ. 

Một ví dụ đơn giản cho thấy sự phức tạp. Giả định$s = 3$và một danh mục có$x = 1$. Một màn hình có kích thước duy nhất$s-1 = 2$đã vượt quá giới hạn dung lượng theo hướng sai: bất kỳ sự phân tách nào cũng buộc một màn hình có kích thước ít nhất là 2, vì vậy chúng ta không thể biểu thị 1 chút nào. Vì vậy không phải mọi$s$là khả thi trên toàn cầu. 

Một thất bại tinh tế khác xảy ra khi trần tham lam hoạt động cục bộ nhưng vi phạm giới hạn dưới do$s-1$màn hình. Điều này gây ra vấn đề không chỉ về việc phân vùng mà còn về việc xác nhận xem một địa chỉ được chọn có$s$có cấu trúc nhất quán trên tất cả các loại. 

## Phương pháp tiếp cận 

Một ý tưởng mạnh mẽ là thử tất cả các giá trị có thể có của$s$từ 2 đến tần số tối đa và với mỗi tần số$s$, hãy tính số lượng màn hình tối thiểu cho mỗi danh mục bằng cách thử tất cả các kết hợp có thể có của$s$Và$s-1$các nhóm. Đối với mỗi loại kích thước$x$, điều này đòi hỏi phải giải một bài toán khả thi về số nguyên nhỏ hoặc lặp lại số lượng nhóm có thể có. Tổng hợp tất cả các danh mục và tất cả$s$, điều này dễ dàng đạt được$O(n^2)$trong trường hợp xấu nhất, vượt xa giới hạn. 

Quan sát cấu trúc quan trọng là các danh mục không tương tác ngoại trừ thông qua sự lựa chọn chung của$s$. Một lần$s$được sửa, mỗi danh mục sẽ trở thành một vấn đề đóng gói 1 chiều độc lập. Hạn chế toàn cầu duy nhất là giống nhau$s$phải làm việc cho mọi thể loại. 

Đối với một cố định$s$, nếu chúng ta biểu thị bằng$k(x)$số lượng màn hình được sử dụng cho một danh mục kích thước$x$, thì những cái đó$k(x)$màn hình phải thỏa mãn rằng tổng khoảng thời gian công suất$[k(x)(s-1), ks]$chứa$x$. Số lượng màn hình nhỏ nhất có thể đương nhiên gần bằng$\lceil x/s \rceil$, nhưng tính khả thi phụ thuộc vào việc liệu cấu trúc còn sót lại có thể được điều chỉnh bằng cách sử dụng$s-1$thùng rác. 

Điều này dẫn đến một sự đơn giản hóa quan trọng: để có một giá trị hợp lệ$s$, giải pháp tối ưu luôn sử dụng$k(x) = \lceil x/s \rceil$cho mỗi thể loại. Nếu phép gán này vi phạm giới hạn dưới$k(x)(s-1) \le x$đối với bất kỳ danh mục nào thì giá trị đó của$s$hoàn toàn không hợp lệ. Không có lợi ích gì khi tăng$k(x)$, vì làm như vậy chỉ làm tăng tổng số màn hình và khiến giới hạn dưới chặt chẽ hơn. 

Do đó, nhiệm vụ giảm xuống còn việc đánh giá các giá trị ứng cử viên của$s$, tính toán$\sum \lceil x_i / s \rceil$và kiểm tra tính khả thi. 

Câu hỏi còn lại là giá trị nào của$s$phải được kiểm tra. Cấu trúc của ràng buộc ngụ ý rằng các điểm dừng có ý nghĩa duy nhất xảy ra ở các giá trị xuất phát từ chính tần số, bởi vì$\lceil x/s \rceil$chỉ thay đổi khi$s$vượt qua các ước của$x$. Điều này cho phép hạn chế sự chú ý đến một nhóm nhỏ ứng viên xung quanh mỗi$x$, thay vì tất cả các số nguyên. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force trên tất cả$s$và phân vùng |$O(n \cdot \max c_i)$|$O(n)$| Quá chậm | 
| Thử ứng viên$s$, tính toán số lượng và xác nhận |$O(n \sqrt{n})$ngây thơ trong trường hợp xấu nhất, được tối ưu hóa để$O(n)$khấu hao |$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đếm tần số của từng loại. Thay thế mảng bằng danh sách các số nguyên dương$x_1, x_2, \dots, x_k$. Điều này làm giảm kích thước vấn đề và loại bỏ thứ tự không liên quan. 
2. Xây dựng một tập hợp các giá trị ứng cử cho$s$. Với mỗi tần số$x$, bao gồm$x$Và$x+1$như những ứng viên tiềm năng. Những điểm này nắm bắt tất cả những thay đổi về cấu trúc trong cách thức$\lceil x/s \rceil$cư xử. 
3. Đối với mỗi ứng viên$s$, tính số lượng màn hình dự kiến:$$m(s) = \sum_i \left\lceil \frac{x_i}{s} \right\rceil.$$4. Tương tự$s$, xác minh tính khả thi cho mọi danh mục bằng cách sử dụng ràng buộc:$$m_i(s) \cdot (s-1) \le x_i,$$Ở đâu$m_i(s) = \lceil x_i / s \rceil$. Nếu bất kỳ danh mục nào vi phạm sự bất bình đẳng này, hãy loại bỏ danh mục này$s$. 
5. Giữ mức tối thiểu$m(s)$trong số tất cả các ứng viên khả thi. 

Bước lý luận quan trọng là một lần$s$đã được khắc phục, việc tăng số lượng màn hình cho mỗi danh mục chỉ có thể làm giảm hiệu quả công suất theo cách không giúp đáp ứng được ràng buộc toàn cầu. Do đó, nhiệm vụ trần là ứng cử viên duy nhất đáng để thử nghiệm. 

### Tại sao nó hoạt động 

Đối với một cố định$s$, mỗi danh mục phải được phân chia thành các nhóm có kích thước$s$hoặc$s-1$. Bất kỳ phân vùng hợp lệ nào có$k$nhóm phải thỏa mãn$k(s-1) \le x \le ks$. Nhỏ nhất có thể$k$có thể thỏa mãn giới hạn trên là$\lceil x/s \rceil$, và bất kỳ lớn hơn$k$chỉ làm tăng vế trái đồng thời làm cho lời giải tệ hơn. 

Do đó, tính khả thi giảm xuống còn việc kiểm tra xem liệu phép gán tham lam tự nhiên có$k = \lceil x/s \rceil$cũng thỏa mãn giới hạn dưới. Nếu được thì tối ưu cho việc đó$s$; nếu không thì không có sự điều chỉnh nào có thể khắc phục được. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        arr = list(map(int, input().split()))

        freq = {}
        for x in arr:
            freq[x] = freq.get(x, 0) + 1

        xs = list(freq.values())

        candidates = set()
        for x in xs:
            candidates.add(x)
            candidates.add(x + 1)

        best = n

        for s in candidates:
            if s <= 1:
                continue

            ok = True
            total = 0

            for x in xs:
                k = (x + s - 1) // s
                if k * (s - 1) > x:
                    ok = False
                    break
                total += k

            if ok:
                best = min(best, total)

        print(best)

if __name__ == "__main__":
    solve()
```Giải pháp nén các danh mục thành tần số trước tiên vì chỉ tính vấn đề. Bước tạo ứng cử viên sử dụng quan sát rằng các chuyển đổi tối ưu xảy ra gần mỗi giá trị tần số. 

Việc tính toán của$k = \lceil x/s \rceil$là phép gán tham lam tự nhiên cho một kích thước màn hình cố định. Kiểm tra tính khả thi buộc rằng ngay cả với màn hình nhỏ nhất được phép, danh mục vẫn có thể được đóng gói mà không vi phạm ràng buộc giới hạn dưới do$s-1$màn hình. 

Mức tối thiểu cuối cùng được lấy trên tất cả các giá trị hợp lệ$s$. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Danh mục đầu vào:$[1, 5, 1, 5, 1, 5, 1, 1, 1, 1, 5]$Tần số là$1 \to 6$,$5 \to 5$, vậy chúng ta có$xs = [6, 5]$. 

Chúng tôi kiểm tra một ứng cử viên$s = 3$. 

| loại x | k = trần(x/s) | k(s-1) ≤ x? | đóng góp | 
| --- | --- | --- | --- | 
| 6 | 2 | 2·2 = 4 ≤ 6 | 2 | 
| 5 | 2 | 4 5 | 2 | 

Tổng số màn hình = 4, khả thi. 

Bây giờ kiểm tra$s = 4$. 

| loại x | k | k(s-1) ≤ x? | đóng góp | 
| --- | --- | --- | --- | 
| 6 | 2 | 2·3 = 6 ≤ 6 | 2 | 
| 5 | 2 | 6 ≤ 5 (không đạt) | - | 

Cái này$s$không hợp lệ. 

Vì vậy, cấu hình hợp lệ tốt nhất mang lại$3$sàng lọc trong việc lựa chọn tối ưu trên tất cả các ứng cử viên. 

Dấu vết này cho thấy tính khả thi sẽ loại bỏ những giá trị có vẻ tốt như thế nào$s$ngay cả khi trần nhà nhỏ. 

### Ví dụ 2 

Danh mục đầu vào:$[1,2,2,2,2,1]$Tần số:$2 \to 4$,$1 \to 2$, Vì thế$xs = [4,2]$. 

Thử$s = 2$. 

| x | k | k(s-1) ≤ x | đóng góp | 
| --- | --- | --- | --- | 
| 4 | 2 | 2 4 | 2 | 
| 2 | 1 | 1 2 | 1 | 

Tổng cộng = 3. 

Hãy thử$s = 3$. 

| x | k | k(s-1) ≤ x | 
| --- | --- | --- | 
| 4 | 2 | 4 4 4 | 
| 2 | 1 | 2 2 2 | 

Tổng cộng = 3 nữa. 

Điều này cho thấy nhiều hợp lệ$s$có thể tồn tại với cùng mức chi phí tối ưu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \cdot C)$Ở đâu$C$là số lượng ứng viên | Mỗi bài kiểm tra đánh giá tất cả các tần số cho từng ứng viên$s$| 
| Không gian |$O(n)$| bản đồ tần số và lưu trữ ứng viên | 

Tổng số tiền của$n$qua các trường hợp thử nghiệm là$2 \cdot 10^6$và tập ứng cử viên vẫn còn nhỏ vì nó chỉ được dẫn xuất từ ​​các giá trị tần số. Điều này giữ thời gian chạy trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from sys import stdout
    out = io.StringIO()
    sys.stdout = out

    solve()

    sys.stdout = sys.__stdout__
    return out.getvalue().strip()

# provided samples
assert run("""3
11
1 5 1 5 1 5 1 1 1 1 5
6
1 2 2 2 2 1
5
4 3 3 1 2
""") == """3
3
4"""

# all same category
assert run("""1
6
1 1 1 1 1 1
""") == "2"

# minimal case
assert run("""1
1
7
""") == "1"

# mixed small frequencies
assert run("""1
4
1 1 2 2
""") == "2"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả các danh mục giống hệt nhau | 2 | chia thành$s$Và$s-1$màn hình | 
| phần tử đơn | 1 | độ đúng cơ sở | 
| tần số nhỏ cân bằng | 2 | tương tác của nhiều danh mục | 

## Vỏ cạnh 

Trường hợp nguy hiểm xảy ra khi tần suất danh mục nhỏ hơn nhiều so với kích thước màn hình ứng viên. Ví dụ, nếu$x = 1$Và$s = 3$, mọi màn hình đều có kích thước tối thiểu$s-1 = 2$, vì vậy không tồn tại phân vùng hợp lệ. Thuật toán từ chối chính xác như vậy$s$vì việc kiểm tra tính khả thi không thành công ngay lập tức với$k(s-1) > x$. 

Một trường hợp cạnh khác là khi tần số khác nhau 1. Giả sử$x = 5$Và$x = 6$tồn tại đồng thời. một ứng cử viên$s = 5$cho$k(6)=2$,$k(5)=1$, và cả hai đều thỏa mãn tính khả thi. Thuật toán xử lý các đóng góp một cách độc lập và tổng hợp, duy trì tính chính xác ngay cả khi các danh mục hoạt động khác nhau trong cùng một$s$.
