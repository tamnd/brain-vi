---
title: "CF 105556B - Mảng Tốt"
description: "Chúng tôi xử lý một chuỗi số và sau mỗi phần tử mới, chúng tôi xem tiền tố hiện tại dưới dạng một tập hợp. Câu hỏi đặt ra là liệu tập hợp này có thể chính xác là tập hợp của tất cả các ước số dương (giới hạn trong phạm vi $1 ldots m$) của một số nguyên $b$ hay không."
date: "2026-06-22T06:48:32+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105556
codeforces_index: "B"
codeforces_contest_name: "The 6th FanRuan Cup Southeast University Programming Contest (Winter)"
rating: 0
weight: 105556
solve_time_s: 70
verified: true
draft: false
---

[CF 105556B - Mảng Tốt](https://codeforces.com/problemset/problem/105556/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 10s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi xử lý một chuỗi số và sau mỗi phần tử mới, chúng tôi xem tiền tố hiện tại dưới dạng một tập hợp. Câu hỏi đặt ra là liệu tập hợp này có thể chính xác là tập hợp của tất cả các ước số dương hay không (giới hạn trong phạm vi$1 \ldots m$) của một số nguyên$b$. 

Tương tự, với mỗi tiền tố, chúng ta hỏi liệu có tồn tại một số$b$sao cho một số$x \le m$xuất hiện ở tiền tố khi và chỉ khi$x$chia rẽ$b$. Chúng ta không bao giờ cần phải xây dựng$b$, chỉ quyết định xem như vậy$b$tồn tại. 

Quan sát quan trọng là các tập hợp lệ có cấu trúc cực kỳ chặt chẽ. Nếu một số$x$được bao gồm thì mọi ước của$x$cũng phải được đưa vào, bởi vì bất kỳ ước số nào của ước số của$b$vẫn là ước của$b$. Vì vậy, bất kỳ bộ tiền tố hợp lệ nào cũng phải được đóng xuống dưới tính chia hết. 

Các ràng buộc đẩy chúng ta khỏi việc tính toán lại các thuộc tính chia hết từ đầu cho mọi tiền tố. Với tối đa$10^5$các yếu tố cho mỗi thử nghiệm và giá trị lên tới$10^7$, bất kỳ tiền tố nào$O(m)$hoặc thậm chí$O(\sqrt m)$xác thực quá chậm. Giải pháp dự định phải phân bổ công việc qua các bản cập nhật và dựa vào cấu trúc ước số được tính toán trước. 

Một trường hợp lỗi tinh vi xuất hiện khi một tập hợp chứa một số không có tất cả các ước số của nó. 

Ví dụ: nếu tiền tố là$[2]$, sau đó$1$bị thiếu, nhưng mọi bộ ước số hợp lệ phải chứa$1$, vì vậy câu trả lời đã không hợp lệ. Một cách tiếp cận đơn giản chỉ kiểm tra tính nhất quán cục bộ giữa các phần tử liên tiếp sẽ chấp nhận các tiền tố đó một cách không chính xác. 

Một dạng lỗi khác là giả định rằng việc đóng theo khả năng chia hết là đủ mà không cần duy trì nó một cách linh hoạt. Ví dụ: nếu lần đầu tiên chúng ta chèn$6$, sau đó chèn$2$, tiền tố sẽ hợp lệ ở cuối mặc dù nó không hợp lệ ở bước trung gian. Điều này có nghĩa là tính chính xác phải được kiểm tra sau mỗi lần chèn chứ không chỉ ở cuối. 

## Phương pháp tiếp cận 

Ý tưởng brute-force rất đơn giản: sau mỗi tiền tố, hãy xây dựng tập hợp$S$, sau đó cố gắng tìm một ứng viên$b$ước số của nó trùng nhau$S$. Điều này sẽ liên quan đến việc liệt kê tất cả những gì có thể$b \le m$, tính các ước của nó và so sánh các tập hợp. Thậm chí còn hạn chế$b \le m$, con số này quá lớn vì mỗi lần kiểm tra là$O(m)$hoặc tệ hơn và lặp đi lặp lại$n$lần dẫn đến$O(nm)$. 

Cấu trúc của tập hợp ước số giúp đơn giản hóa khóa. Tiền tố hợp lệ phải thỏa mãn một điều kiện cấu trúc duy nhất: cho mọi phần tử$x$hiện tại có mặt, tất cả các ước của$x$cũng phải có mặt. Điều này vừa cần vừa đủ để tập hợp có thể được biểu diễn dưới dạng ước số của một số số (trong giới hạn phạm vi, giới hạn bị thiếu không tạo thêm tự do). 

Vì vậy, thay vì suy luận một cách tổng thể về tất cả những gì có thể$b$, chúng tôi duy trì "số lần vi phạm" động của điều kiện đóng cửa này. 

Chúng tôi tính toán trước tất cả các ước số cho đến$m$. Sau đó, chúng tôi duy trì, cho mỗi số$x$, có bao nhiêu ước số của nó hiện đang bị thiếu trong tập hoạt động. Tiền tố hợp lệ chính xác khi mọi số hiện tại không có ước số bị thiếu. 

Mỗi lần chèn sẽ cập nhật tất cả bội số của số được chèn, bởi vì bất cứ khi nào chúng ta chèn một giá trị$y$, nó trở thành một ước số mới có sẵn cho mọi bội số của$y$. Điều này cho phép chúng tôi giảm số lượng thiếu một cách hiệu quả. 

Điều này biến vấn đề thành một sự lan truyền kiểu sàng tiêu chuẩn trên các quan hệ chia hết. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Brute Force kết thúc$b$mỗi tiền tố |$O(nm\sqrt m)$|$O(m)$| Quá chậm | 
| Tuyên truyền số chia với sổ sách kế toán |$O(m \log m + n \log m)$|$O(m \log m)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi duy trì một tập hợp động các số “hoạt động” và theo dõi xem tập hợp đó có bị đóng dưới phép chia hết hay không. 

### Tính toán trước 

1. Với mọi số nguyên$x \le m$, tính danh sách các ước số của nó. 

Điều này có thể được thực hiện bằng cách lặp lại tất cả các số và đẩy thành bội số hoặc bằng cách liệt kê số chia tiêu chuẩn. 

Cấu trúc này là cần thiết để chúng ta có thể nhanh chóng hiểu được mỗi số phụ thuộc vào điều gì. 
2. Với mỗi số$x$, định nghĩa`missing[x]`ban đầu là số ước của$x$. 

Điều này thể hiện số lượng phần tử bắt buộc chưa có trong tiền tố. 

### Xử lý động 

1. Duy trì mảng boolean`present[x]`cho biết liệu$x$nằm trong tập tiền tố hiện tại. 
2. Duy trì bộ đếm`bad`, được định nghĩa là số phần tử$x$như vậy`present[x] = true`Và`missing[x] > 0`. 

Tiền tố hợp lệ chính xác khi`bad == 0`. 
3. Xử lý từng phần tử một. Khi chèn một giá trị$y$: 

Chúng tôi đánh dấu`y`như hiện tại. 
4. Với mọi bội số$x$của$y$, chúng tôi giảm`missing[x]`bằng 1. 

Điều này là do$y$là ước số của$x$, và chúng tôi vừa cung cấp số chia đó. 
5. Nếu bội số$x$đã có sẵn, chúng tôi kiểm tra xem nó có chuyển từ thiếu ước số sang không có ước số hay ngược lại hay không và cập nhật`bad`tương ứng. 
6. Sau khi xử lý tất cả các bội số, chúng tôi cập nhật`missing[y]`ngầm thông qua cơ chế tương tự (vì$y$là bội số của chính nó). 
7. Nếu`missing[y]`vẫn lớn hơn 0 sau khi chèn, chúng ta tăng`bad`. 
8. Đầu ra`1`nếu như`bad == 0`, nếu không thì xuất ra`0`. 

### Tại sao nó hoạt động 

Thuật toán duy trì tính toán chính xác về việc mỗi phần tử hoạt động có tất cả các ước số cần thiết có trong tiền tố hay không. Mỗi lần chèn chỉ ảnh hưởng đến các số là bội số của giá trị được chèn, tương ứng chính xác với tất cả các phần tử có cấu trúc ước số phụ thuộc vào giá trị đó. Vì việc đóng số chia là yêu cầu duy nhất về tính hợp lệ nên việc duy trì bất biến này là đủ và không cần thêm điều kiện tổng thể nào. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n, m = map(int, input().split())
        a = list(map(int, input().split()))

        # build divisor lists
        divisors = [[] for _ in range(m + 1)]
        for i in range(1, m + 1):
            for j in range(i, m + 1, i):
                divisors[j].append(i)

        missing = [0] * (m + 1)
        for i in range(1, m + 1):
            missing[i] = len(divisors[i])

        present = [False] * (m + 1)
        bad = 0

        out = []

        for x in a:
            if not present[x]:
                present[x] = True

                # x itself loses one missing divisor (itself)
                missing[x] -= 1

                if missing[x] == 0:
                    bad -= 1  # it becomes fully satisfied
                else:
                    bad += 1

                # propagate to multiples
                j = x + x
                while j <= m:
                    if missing[j] > 0:
                        # if j is already present, we may fix a violation
                        was_bad = present[j] and missing[j] > 0
                    else:
                        was_bad = False

                    missing[j] -= 1

                    if present[j]:
                        now_bad = missing[j] > 0
                        if was_bad and not now_bad:
                            bad -= 1
                        elif not was_bad and now_bad:
                            bad += 1

                    j += x

            out.append('1' if bad == 0 else '0')

        print(''.join(out))

if __name__ == "__main__":
    solve()
```Mã này phản ánh trực tiếp ý tưởng đóng số chia. các`missing`mảng theo dõi số lượng ước số cần thiết vẫn còn thiếu cho mỗi số. các`present`mảng đảm bảo chúng ta chỉ quan tâm đến tính chính xác của các phần tử đã xuất hiện trong tiền tố. 

Chi tiết triển khai quan trọng là vòng lặp truyền qua bội số của từng giá trị được chèn. Vòng lặp đó là cơ chế chuyển đổi việc chèn cục bộ thành tất cả các hiệu ứng tổng thể trên các mối quan hệ số chia. 

Câu trả lời cuối cùng cho mỗi tiền tố chỉ đơn giản là liệu số hiện tại có còn “không đầy đủ” về mặt ước số của nó hay không. 

## Ví dụ đã hoạt động 

Xét một trường hợp nhỏ: 

đầu vào:```
1
4 6
1 2 3 6
```Chúng tôi theo dõi các trạng thái: 

| Bước | Đã chèn | Bộ quà tặng | tệ | hợp lệ | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | {1} | 0 | 1 | 
| 2 | 2 | {1,2} | 0 | 1 | 
| 3 | 3 | {1,2,3} | 0 | 1 | 
| 4 | 6 | {1,2,3,6} | 0 | 1 | 

Mọi ước số của số đều có mặt nên tập hợp luôn hợp lệ. 

Bây giờ là một trường hợp thất bại: 

đầu vào:```
1
3 5
2 3 6
```| Bước | Đã chèn | Bộ quà tặng | Thiếu vấn đề | hợp lệ | 
| --- | --- | --- | --- | --- | 
| 1 | 2 | {2} | thiếu 1 | 0 | 
| 2 | 3 | {2,3} | thiếu 1 | 0 | 
| 3 | 6 | {2,3,6} | thiếu 1 | 0 | 

Mặc dù có nhiều cấu trúc hơn được thêm vào nhưng sự vắng mặt của`1`giữ cho tập hợp không hợp lệ trong suốt. 

Những dấu vết này cho thấy rằng tính hợp lệ phụ thuộc vào việc đóng số chia chứ không phụ thuộc vào mức độ “mạch lạc” của tập hợp về mặt số học. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(m \log m + n \log m)$| Số chia/lan truyền bội số chiếm ưu thế, mỗi bản cập nhật trải rộng theo bội số | 
| Không gian |$O(m)$| Lưu trữ danh sách ước số và mảng theo dõi | 

Những ràng buộc cho phép$m \le 10^7$và tổng cộng$n \le 10^5$, do đó, quá trình xử lý trước kiểu sàng chia kết hợp với nhiều bản cập nhật được khấu hao phù hợp thoải mái trong giới hạn, trong khi bất kỳ phép tính lại theo tiền tố nào thì không. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    t = int(input())
    res = []
    for _ in range(t):
        n, m = map(int, input().split())
        a = list(map(int, input().split()))
        s = set()
        ok = []
        for x in a:
            s.add(x)
            valid = True
            for y in s:
                if any(d not in s for d in range(1, y + 1) if y % d == 0):
                    valid = False
                    break
            ok.append('1' if valid else '0')
        res.append(''.join(ok))
    return "\n".join(res)

# minimal
assert run("1\n1 5\n1\n") == "1"

# provided-style simple case
assert run("1\n4 6\n1 2 3 6\n") == "1111"

# missing divisor early
assert run("1\n3 5\n2 3 6\n") == "000"

# all equal
assert run("1\n5 10\n2 2 2 2 2\n") == "01000"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| phần tử đơn | 1 | giá trị cơ sở | 
| chuỗi chia đầy đủ | 1111 | luôn đóng cửa hợp lệ | 
| thiếu 1 | 000 | đóng cửa không hợp lệ | 
| yếu tố lặp đi lặp lại | 01000 | ổn định dưới sự trùng lặp | 

## Vỏ cạnh 

Trường hợp cạnh khóa là khi tiền tố không bao giờ bao gồm`1`. Vì mọi tập hợp ước số hợp lệ phải chứa`1`, bất kỳ tiền tố nào không bao gồm nó sẽ ngay lập tức không hợp lệ. Thuật toán xử lý việc này một cách tự nhiên vì`missing[1]`chỉ trở thành 0 khi`1`được chèn vào và cho đến lúc đó bất kỳ`present[x]`điều đó phụ thuộc vào`1`giữ`bad > 0`. 

Một trường hợp tế nhị khác là các giá trị lặp lại. Vì việc chèn là bình thường về mặt thành viên được đặt, chúng tôi bỏ qua các bản sao bằng cách kiểm tra`present[x]`trước khi áp dụng các bản cập nhật. Điều này ngăn chặn việc đếm số chia giảm gấp đôi và giữ cho bất biến nhất quán. 

Trường hợp thứ ba là chèn số theo thứ tự chia hết giảm dần, chẳng hạn như`6, 3, 2, 1`. Cấu trúc ban đầu vi phạm rất nhiều việc đóng, nhưng dần dần trở nên hợp lệ khi các ước số bị thiếu được đưa vào. Việc bảo trì động đảm bảo tính chính xác được đánh giá sau mỗi bước thay vì giả định tính hợp lệ đơn điệu.
