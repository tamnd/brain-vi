---
title: "CF 102420I - Tổng mức tối đa"
description: "Chúng ta có (n) vị trí trong một mảng, nhưng giá trị được gán cho các vị trí đó không cố định. Đối với mỗi lần thử, chúng tôi nhận được (n) giá trị và có thể hoán vị chúng theo cách chúng tôi muốn. Có (q) khoảng cố định trên mảng."
date: "2026-08-12T06:37:37+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102420
codeforces_index: "I"
codeforces_contest_name: "\u0418\u043d\u0442\u0435\u0440\u043d\u0435\u0442-\u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u044b, \u0421\u0435\u0437\u043e\u043d 2019-2020, \u0422\u0440\u0435\u0442\u044c\u044f \u043a\u043e\u043c\u0430\u043d\u0434\u043d\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430, \u0443\u0441\u043b\u043e\u0436\u043d\u0435\u043d\u043d\u0430\u044f \u043d\u043e\u043c\u0438\u043d\u0430\u0446\u0438\u044f"
rating: 0
weight: 102420
solve_time_s: 169
verified: true
draft: false
---

[CF 102420I - Tổng mức tối đa](https://codeforces.com/problemset/problem/102420/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 49s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có (n) vị trí trong một mảng, nhưng giá trị được gán cho các vị trí đó không cố định. Đối với mỗi lần thử, chúng tôi nhận được (n) giá trị và có thể hoán vị chúng theo cách chúng tôi muốn. 

Có (q) khoảng cố định trên mảng. Khi một hoán vị được chọn, mỗi khoảng sẽ đóng góp giá trị tối đa được đặt bên trong nó. Điểm của hoán vị là tổng của (q) cực đại đó. Chúng tôi cần số điểm tối đa có thể một cách độc lập cho mỗi lần thử. 

Ví dụ: nếu các khoảng là ([1,2]) và ([2,3]), việc đặt giá trị lớn nhất ở vị trí 2 đặc biệt hữu ích vì cùng một giá trị sẽ trở thành giá trị lớn nhất của cả hai khoảng. Vấn đề là xác định cách khai thác sự chồng chéo này giữa các khoảng thời gian. 

Các ràng buộc nhỏ về số lượng vị trí, với (n\le 30) và có tối đa 30 khoảng truy vấn. Điều này làm cho một thuật toán liên quan đến lượng công việc bậc hai hoặc thậm chí bậc ba cho mỗi lần thử trở nên hợp lý. Mặt khác, có thể có 1000 lần thử, do đó, bất kỳ số mũ nào trong (n), chẳng hạn như liệt kê tất cả (n!) hoán vị, là hoàn toàn không thể. Các giá trị có thể đạt tới (10^9) và câu trả lời cuối cùng có thể nằm trong khoảng (q\cdot10^9), do đó số nguyên 32 bit là không đủ trong các ngôn ngữ có số nguyên có chiều rộng cố định. 

Có một số trường hợp đặc biệt có thể bộc lộ việc triển khai không chính xác. 

Khi (n=1), không có hoán vị có ý nghĩa nào để chọn. Ví dụ: với (n=1,m=1,q=1), khoảng ([1,1]) và giá trị (42), câu trả lời là (42). Việc triển khai giả định có ít nhất hai vị trí có thể xử lý sai vòng lặp lựa chọn. 

Khoảng thời gian có thể chồng chéo hoàn toàn. Với (n=3), nếu hai khoảng là ([1,3]) và ([1,3]) và các giá trị là (1,5,2), thì câu trả lời là (10), vì giá trị (5) là giá trị lớn nhất của cả hai khoảng. Việc tính phạm vi bao phủ khoảng thời gian ban đầu của mỗi vị trí một lần mà không loại bỏ các khoảng thời gian được bao phủ sẽ dẫn đến những đóng góp sau này không chính xác. 

Một vị trí cũng có thể thuộc về không có khoảng thời gian hiện chưa được khám phá. Ví dụ: với (n=3), các khoảng ([1,1]) và ([3,3]), sau khi chọn vị trí 1 cho giá trị lớn nhất, vị trí 2 sẽ không còn khoảng nào. Việc gán một giá trị cho nó là vô hại nhưng việc triển khai không được coi nó như bao gồm một truy vấn. 

Giá trị bằng nhau là một trường hợp tế nhị khác. Nếu nhiều giá trị bằng nhau, thứ tự của chúng không ảnh hưởng đến điểm số. Ví dụ: với (n=3), một khoảng ([1,3]) và các giá trị (7,7,7), mọi hoán vị sẽ cho (7). Sự lựa chọn tham lam có thể phá vỡ mối quan hệ một cách tùy tiện. 

Cuối cùng, giá trị 0 là hợp lệ. Với (n=2), khoảng ([1,2]) và giá trị (0,0), câu trả lời là (0). Mã sử ​​dụng số 0 làm dấu hiệu cho thấy một vị trí hoặc khoảng chưa được xử lý có thể âm thầm nhầm lẫn giá trị hợp pháp với trạng thái chưa được khởi tạo. 

## Phương pháp tiếp cận 

Giải pháp bạo lực trực tiếp về mặt khái niệm rất đơn giản. Đối với mỗi hoán vị của giá trị (n), hãy đặt các giá trị theo hoán vị đó, quét tất cả các khoảng (q), tính giá trị lớn nhất bên trong mỗi khoảng và giữ điểm lớn nhất. Điều này đúng vì mọi thứ tự có thể đều được kiểm tra rõ ràng. 

Vấn đề là số lượng hoán vị. Có (n!) Trong số chúng và việc đánh giá một hoán vị có thể yêu cầu (O(qn)) hoạt động nếu mỗi khoảng được quét trực tiếp. Trong trường hợp xấu nhất, điều này mang lại (O(n!,qn)). Đối với (n=30), ngay cả số lượng hoán vị cũng là khoảng (2,65\cdot10^{32}), vì vậy cách tiếp cận này gần như không khả thi. 

Quan sát hữu ích đến từ việc xem xét các giá trị theo thứ tự giảm dần. 

Giả sử giá trị lớn nhất là (x). Khi chúng tôi quyết định đặt (x) ở đâu, mọi khoảng chứa vị trí đó sẽ nhận được (x) là mức tối đa cuối cùng của nó, miễn là khoảng đó chưa được xử lý bởi một giá trị thậm chí còn lớn hơn. Những khoảng thời gian như vậy có thể được loại bỏ khỏi việc xem xét. Sau đó chúng ta có thể đặt giá trị lớn thứ hai và lặp lại.

Bất cứ lúc nào, hãy gọi một khoảng thời gian hoạt động nếu không có giá trị nào lớn hơn giá trị hiện đang được đặt bên trong nó. Giá trị hiện tại đóng góp chính xác một lần cho mỗi khoảng thời gian hoạt động chứa vị trí đã chọn của nó. 

Điều này biến vấn đề thành một sự sắp xếp vị trí một cách tham lam. Đối với giá trị còn lại lớn nhất hiện tại, hãy chọn vị trí chưa sử dụng có trong số khoảng thời gian hoạt động lớn nhất. Sau khi chọn nó, hãy đánh dấu tất cả các khoảng đó là được bao phủ. 

Phần không rõ ràng đang chứng minh rằng sự lựa chọn tham lam này là tối ưu toàn cầu. Giả sử giá trị còn lại lớn nhất hiện tại là (x) và một giải pháp tối ưu đặt nó ở vị trí (r). Thuật toán tham lam của chúng tôi chọn vị trí (p), thuộc về ít nhất số khoảng hoạt động bằng (r). 

Gọi (y) là giá trị mà lời giải tối ưu đặt ở (p). Vì (x) là giá trị còn lại lớn nhất, (x\ge y). Hoán đổi (x) và (y), đặt (x) tại (p) và (y) tại (r). 

Khoảng hoạt động chứa cả hai vị trí không bị ảnh hưởng. Khoảng hoạt động chứa (p) nhưng không chứa (r) cải thiện từ (y) lên (x). Khoảng hoạt động chứa (r) nhưng không chứa (p) giảm từ (x) xuống (y). Do đó sự thay đổi trong tổng số điểm là 

[ 
(x-y)\left(c_p-c_r\right), 
] 

trong đó (c_p) và (c_r) là số khoảng hoạt động chứa (p) và (r). Cả hai thừa số đều không âm nên phép hoán đổi không bao giờ làm giảm đáp án. 

Do đó, luôn có một giải pháp tối ưu có giá trị còn lại lớn nhất được đặt ở vị trí có phạm vi hoạt động tối đa. Sau khi sửa vị trí đó, đối số tương tự sẽ được áp dụng cho các giá trị và khoảng còn lại. Điều này đưa ra thuật toán tham lam. 

Các khoảng có thể được biểu diễn bằng mặt nạ 30 bit vì (q\le30). Đối với mỗi vị trí, chúng tôi lưu trữ khoảng truy vấn nào chứa vị trí đó. Một cửa hàng bitmask khác có khoảng thời gian vẫn hoạt động. Số khoảng thời gian hoạt động chứa một vị trí khi đó chỉ là bitwise AND theo sau là số lượng dân số. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(n!,qn)) | (O(n+q)) | Quá chậm | 
| Tham lam | (O(n^2+n\log n)) mỗi lần thử | (O(n+q)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xây dựng mặt nạ bit cho mọi vị trí mảng. Bit (k) được đặt khi và chỉ khi khoảng truy vấn (k) chứa vị trí đó. Vì có tối đa 30 khoảng nên một số nguyên Python có thể biểu thị tất cả các khoảng đó. 
2. Đối với một lần thử, hãy sắp xếp các giá trị (n) của nó theo thứ tự giảm dần. Chúng tôi xử lý giá trị lớn nhất trước tiên vì nó sẽ xác định càng nhiều khoảng cực đại càng tốt. 
3. Ban đầu đánh dấu mỗi khoảng thời gian truy vấn là hoạt động. Khoảng thời gian hoạt động vẫn chưa nhận được giá trị lớn hơn giá trị hiện đang được xử lý. 
4. Đối với giá trị hiện tại, hãy kiểm tra mọi vị trí chưa sử dụng. Tính số khoảng thời gian hoạt động chứa vị trí đó bằng cách giao mặt nạ che phủ của nó với mặt nạ khoảng thời gian hoạt động. 
5. Chọn vị trí chưa sử dụng có số lượng lớn nhất. Nếu giá trị hiện tại là (x) và vị trí bao gồm (c) khoảng thời gian hoạt động, hãy thêm (x\cdot c) vào câu trả lời. Các khoảng (c) đó hiện có mức tối đa cuối cùng được cố định ở (x). 
6. Đánh dấu vị trí đã chọn là đã sử dụng và xóa mọi khoảng chứa vị trí đó khỏi mặt nạ hoạt động. Những khoảng thời gian đó sẽ không bao giờ cần phải xem xét lại vì tất cả các giá trị sau này đều không lớn hơn giá trị hiện tại. 
7. Tiếp tục với giá trị tiếp theo cho đến khi mọi vị trí đã được chỉ định. Các khoảng đã được bao phủ không đóng góp gì thêm, trong khi mọi khoảng không được bao phủ trước đó cuối cùng sẽ được bao phủ bởi vị trí được chọn đầu tiên bên trong nó. 

### Tại sao nó hoạt động 

Điều bất biến là sau khi xử lý các giá trị lớn nhất (k) đầu tiên, mọi khoảng đã bị xóa khỏi tập hoạt động đã có mức tối đa cuối cùng được cố định bởi một trong các giá trị (k) đó. Mỗi khoảng thời gian hoạt động chỉ chứa các vị trí không được sử dụng, do đó mức tối đa cuối cùng của nó phải đến từ một trong các giá trị còn lại.

Đối với giá trị lớn nhất tiếp theo (x), hãy xem xét cách sắp xếp tối ưu của các giá trị còn lại. Nếu (x) được đặt ở vị trí (r), trong khi thuật toán tham lam chọn vị trí (p), việc hoán đổi (x) với giá trị tại (p) sẽ thay đổi điểm bằng cách 

[ 
(x-y)(c_p-c_r)\ge0. 
] 

Yếu tố đầu tiên không âm vì (x) là giá trị còn lại lớn nhất và yếu tố thứ hai không âm vì vị trí tham lam có phạm vi hoạt động tối đa. Do đó, một sự sắp xếp tối ưu luôn có thể được chuyển thành một sự sắp xếp phù hợp với sự lựa chọn tham lam. 

Việc áp dụng lập luận trao đổi này nhiều lần chứng tỏ rằng mọi lựa chọn tham lam đều có thể được mở rộng thành một hoán vị hoàn chỉnh tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())
    q = int(input())

    intervals = []
    for _ in range(q):
        l, r = map(int, input().split())
        intervals.append((l - 1, r - 1))

    # cover[p] is a bitmask of all intervals containing position p.
    cover = [0] * n

    for k, (l, r) in enumerate(intervals):
        bit = 1 << k
        for p in range(l, r + 1):
            cover[p] |= bit

    full_mask = (1 << q) - 1

    answers = []

    for _ in range(m):
        values = list(map(int, input().split()))
        values.sort(reverse=True)

        active = full_mask
        used = [False] * n
        ans = 0

        for value in values:
            best_pos = -1
            best_count = -1

            for p in range(n):
                if used[p]:
                    continue

                count = (active & cover[p]).bit_count()

                if count > best_count:
                    best_count = count
                    best_pos = p

            ans += value * best_count
            used[best_pos] = True

            # Every active interval containing best_pos is now fixed.
            active &= ~cover[best_pos]

        answers.append(str(ans))

    sys.stdout.write("\n".join(answers))

if __name__ == "__main__":
    solve()
```Vòng tiền xử lý đầu tiên chuyển đổi mọi khoảng thời gian thành mặt nạ bao phủ vị trí. Ví dụ: nếu có ba khoảng và vị trí 2 thuộc về khoảng 0 và 2, mặt nạ của nó có các bit 0 và 2 được đặt. 

Đối với mỗi lần thử, việc sắp xếp các giá trị theo thứ tự ngược lại sẽ đưa ra chính xác trình tự mà đối số trao đổi tham lam yêu cầu. các`active`mặt nạ ban đầu chứa tất cả các khoảng (q). 

biểu hiện`(active & cover[p]).bit_count()`đếm chính xác các khoảng thời gian hoạt động chứa vị trí`p`. Bởi vì (q\le30), toàn bộ tập hợp các khoảng nằm trong một số nguyên. 

Sau khi chọn`best_pos`, biểu thức`active &= ~cover[best_pos]`loại bỏ tất cả các khoảng có chứa vị trí đó. Số nguyên Python có biểu diễn không giới hạn, do đó phần bù`~cover[best_pos]`có thể chứa vô số bit khái niệm một, nhưng ANDing nó với giá trị không âm`active`mặt nạ chỉ xóa các bit tương ứng với các khoảng được biểu thị. 

Vị trí được đánh dấu sử dụng trước khi chuyển sang giá trị tiếp theo. Điều này ngăn việc gán hai giá trị cho cùng một vị trí mảng. 

Số nguyên Python cũng xử lý trực tiếp câu trả lời có khả năng lớn. Trong ngôn ngữ có số nguyên 32 bit, câu trả lời xung quanh (30\cdot10^9) sẽ tràn, do đó cần có số nguyên 64 bit. 

Việc sử dụng`bit_count()`ở đây đặc biệt thuận tiện vì số khoảng truy vấn nhiều nhất là 30, làm cho trạng thái bao phủ phù hợp một cách tự nhiên với một số nguyên có kích thước bằng máy. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Mỗi khoảng truy vấn bao gồm chính xác một vị trí, do đó mỗi vị trí ban đầu bao gồm chính xác một khoảng hoạt động. Các giá trị được xử lý từ lớn nhất đến nhỏ nhất. 

Đối với trường hợp thử nghiệm đầu tiên, các giá trị là (2,1,1,1,1). 

| Giá trị | Vị trí được chọn | Khoảng thời gian hoạt động trước | Được bảo hiểm ngay bây giờ | Trả lời | 
| --- | --- | --- | --- | --- | 
| 2 | 5 | 5 | 1 | 2 | 
| 1 | 1 | 4 | 1 | 3 | 
| 1 | 2 | 3 | 1 | 4 | 
| 1 | 3 | 2 | 1 | 5 | 
| 1 | 4 | 1 | 1 | 6 | 

Giá trị đầu tiên có thể được đặt ở bất kỳ đâu vì tất cả các vị trí đều có phạm vi bao phủ như nhau. Khi một vị trí được chọn, khoảng thời gian đơn lẻ của nó sẽ bị xóa. Tỷ số cuối cùng là (2+1+1+1+1=6). 

Đối với những lần thử còn lại, quy trình tương tự sẽ gán mọi giá trị cho một khoảng đơn riêng biệt, do đó điểm chỉ đơn giản là tổng của tất cả các giá trị. Điều này tạo ra (7,8,9,10). 

### Mẫu 2 

Các truy vấn là ([1,2]) và ([2,3]). Sự chồng chéo của họ ở vị trí thứ 2 là phần quan trọng. 

Đối với mảng đầu tiên, các giá trị là (1,5,1). 

| Giá trị | Vị trí được chọn | Khoảng thời gian hoạt động trước | Được bảo hiểm ngay bây giờ | Trả lời | 
| --- | --- | --- | --- | --- | 
| 5 | 2 | 2 | 2 | 10 | 
| 1 | 1 | 0 | 0 | 10 | 
| 1 | 3 | 0 | 0 | 10 | 

Vị trí 2 thuộc về cả hai khoảng, vì vậy giá trị lớn nhất bao trùm cả hai khoảng cùng một lúc. Khi 5 được đặt ở đó, cả hai khoảng đều có mức tối đa cuối cùng bằng 5, cho ra (5+5=10). 

Đối với mảng thứ ba, các giá trị là (10,1,7). Vị trí 2 tương tự vẫn là vị trí tốt nhất cho giá trị lớn nhất, vì vậy 10 bao gồm cả hai khoảng. 

| Giá trị | Vị trí được chọn | Khoảng thời gian hoạt động trước | Được bảo hiểm ngay bây giờ | Trả lời | 
| --- | --- | --- | --- | --- | 
| 10 | 2 | 2 | 2 | 20 | 
| 7 | 1 | 0 | 0 | 20 | 
| 1 | 3 | 0 | 0 | 20 | 

Điều này cho ra (10+10=20), khớp với đầu ra mẫu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(m(n^2+n\log n)+nq)) | Mỗi lần thử sắp xếp (n) giá trị và thực hiện (n) vòng tham lam, mỗi lần kiểm tra tối đa (n) vị trí. | 
| Không gian | (O(n+q)) | Mặt nạ che phủ, giá trị hiện tại và trạng thái vị trí được sử dụng yêu cầu không gian tuyến tính. | 

Với (n,q\le30) và (m\le1000), phần tham lam chỉ thực hiện khoảng (1000\cdot30^2=900.000) kiểm tra vị trí và mỗi kiểm tra là một thao tác bit có thời gian không đổi. Việc sắp xếp chỉ đóng góp (1000\cdot O(30\log30)). Điều này nằm trong giới hạn của cuộc thi, trong đó liệt kê giới hạn thời gian 5 giây và 512 MB bộ nhớ cho vấn đề I. 

## Trường hợp thử nghiệm 

Khai thác thử nghiệm sau đây sử dụng lại dữ liệu đã gửi`solve()`chức năng. các`run`trình trợ giúp tạm thời thay thế đầu vào và đầu ra tiêu chuẩn, sau đó khôi phục chúng sau mỗi lần kiểm tra.```python
import sys
import io

def solve():
    n, m = map(int, input().split())
    q = int(input())

    intervals = []
    for _ in range(q):
        l, r = map(int, input().split())
        intervals.append((l - 1, r - 1))

    cover = [0] * n

    for k, (l, r) in enumerate(intervals):
        bit = 1 << k
        for p in range(l, r + 1):
            cover[p] |= bit

    full_mask = (1 << q) - 1

    answers = []

    for _ in range(m):
        values = list(map(int, input().split()))
        values.sort(reverse=True)

        active = full_mask
        used = [False] * n
        ans = 0

        for value in values:
            best_pos = -1
            best_count = -1

            for p in range(n):
                if used[p]:
                    continue

                count = (active & cover[p]).bit_count()

                if count > best_count:
                    best_count = count
                    best_pos = p

            ans += value * best_count
            used[best_pos] = True
            active &= ~cover[best_pos]

        answers.append(str(ans))

    sys.stdout.write("\n".join(answers))

input = sys.stdin.readline

def run(inp: str) -> str:
    global input

    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()
    input = sys.stdin.readline

    try:
        solve()
        return sys.stdout.getvalue()
    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout
        input = sys.stdin.readline

# Provided sample 1
assert run(
    """5 5
5
1 1
2 2
3 3
4 4
5 5
1 1 1 1 2
1 1 1 2 2
1 1 2 2 2
1 2 2 2 2
2 2 2 2 2
"""
) == "6\n7\n8\n9\n10", "sample 1"

# Provided sample 2
assert run(
    """3 4
2
1 2
2 3
1 1 1
1 5 1
10 1 7
4 2 0
"""
) == "2\n10\n20\n8", "sample 2"

# Minimum size
assert run(
    """1 1
1
1 1
42
"""
) == "42", "minimum size"

# All values equal
assert run(
    """4 1
3
1 3
2 4
1 4
7 7 7 7
"""
) == "21", "all equal values"

# Boundary intervals and overlapping intervals
assert run(
    """4 1
3
1 1
4 4
2 4
10 1 2 20
"""
) == "50", "boundary intervals"

# Maximum n and q, all intervals identical
intervals = "\n".join(["1 30"] * 30)
values = " ".join(str(x) for x in range(1, 31))

max_case = f"""30 1
30
{intervals}
{values}
"""

assert run(max_case) == "900", "maximum-size case"

# Large values, checks arithmetic beyond 32-bit range
assert run(
    """3 1
3
1 3
1 1
2 2
1000000000 1000000000 1000000000
"""
) == "3000000000", "large answer"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| (n=1), một khoảng, giá trị 42 | 42 | Kích thước tối thiểu và xử lý một vị trí | 
| Ba khoảng chồng chéo, tất cả các giá trị 7 | 21 | Giá trị bằng nhau và chồng chéo hoàn toàn | 
| Khoảng ([1,1]), ([4,4]), ([2,4]), giá trị (10,1,2,20) | 50 | Cả ranh giới mảng và vùng phủ sóng chồng chéo | 
| (n=q=30), tất cả các khoảng ([1,30]), giá trị (1\ldots30) | 900 | Hạn chế tối đa và phạm vi phủ sóng hoàn chỉnh lặp đi lặp lại | 
| Ba khoảng có giá trị (10^9) | 3000000000 | Câu trả lời lớn và số học số nguyên | 

## Vỏ cạnh 

Đối với trường hợp kích thước tối thiểu, đầu vào bao gồm (n=1), một truy vấn ([1,1]) và giá trị 42. Vị trí duy nhất có một khoảng hoạt động, do đó thuật toán tham lam chọn nó, thêm (42\cdot1), xóa khoảng và xuất ra 42. 

Đối với các khoảng chồng chéo hoàn toàn, giả sử (n=3), các khoảng là ([1,3]), ([2,3]) và ([1,2]) và mọi giá trị là 7. Mỗi vị trí ban đầu bao gồm hai hoặc ba khoảng, nhưng bất kỳ vị trí nào được chọn, mọi khoảng chứa nó sẽ bị xóa. Vì tất cả các giá trị đều bằng nhau nên đối số trao đổi cho phép bất kỳ sự ràng buộc nào và câu trả lời cuối cùng vẫn là ba đóng góp là 7, cụ thể là 21. 

Đối với khoảng đạt đến ranh giới bên phải, hãy xem xét đầu vào có các khoảng ([1,1]), ([4,4]) và ([2,4]) và các giá trị (10,1,2,20). Vị trí 4 bao gồm hai khoảng hoạt động, do đó 20 được đặt ở đó và đóng góp (40). Khoảng hoạt động duy nhất còn lại là ([1,1]), do đó 10 được đặt ở vị trí 1 và đóng góp 10 khác. Câu trả lời là 50. Điều này xác nhận rằng các điểm cuối của khoảng được xử lý một cách toàn diện. 

Đối với các giá trị bằng nhau, bằng chứng trao đổi sử dụng (x-y\ge0). Khi (x=y), việc hoán đổi chúng không làm thay đổi gì, do đó việc phá vỡ ràng buộc tùy ý là hợp lệ. Do đó, việc triển khai không cần bất kỳ xử lý đặc biệt nào đối với các giá trị trùng lặp. 

Đối với các giá trị bằng 0, hãy xem xét (n=2), một khoảng ([1,2]) và các giá trị (0,0). Thuật toán tham lam vẫn chọn một vị trí vì phạm vi hoạt động của nó là một, thêm (0\cdot1=0), xóa khoảng và nhận được câu trả lời đúng 0. Giá trị 0 không bao giờ được sử dụng làm trọng điểm, do đó không thể nhầm lẫn nó với trạng thái chưa được xử lý. 

Trường hợp kích thước tối đa có (n=q=30) và tất cả 30 khoảng bằng ([1,30]). Mỗi vị trí bao gồm tất cả 30 khoảng thời gian, vì vậy giá trị đầu tiên và lớn nhất ngay lập tức cố định mọi khoảng thời gian. Nếu các giá trị là (1,2,\ldots,30), thì giá trị lớn nhất 30 đóng góp (30\cdot30=900), trong khi mọi giá trị sau đó đóng góp bằng 0 vì không còn khoảng thời gian hoạt động nào. Kết quả là 900. Điều này chứng tỏ tại sao các khoảng bao phủ phải được loại bỏ sau mỗi lần lựa chọn tham lam.
