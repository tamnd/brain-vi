---
title: "CF 1204E - Natasha, Sasha và các tiền tố"
description: "Chúng ta đang xem xét tất cả các chuỗi có thể có độ dài $n+m$ được tạo thành từ chính xác $n$ đơn vị và $m$ đơn vị trừ. Mọi sự sắp xếp đều được xem xét một lần, vì vậy đây là vấn đề hoán vị nhiều tập hợp."
date: "2026-06-13T15:48:54+07:00"
tags: ["codeforces", "competitive-programming", "combinatorics", "dp", "math", "number-theory"]
categories: ["algorithms"]
codeforces_contest: 1204
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 581 (Div. 2)"
rating: 2300
weight: 1204
solve_time_s: 467
verified: false
draft: false
---

[CF 1204E - Natasha, Sasha và Tổng tiền tố](https://codeforces.com/problemset/problem/1204/E) 

**Đánh giá:** 2300 
**Tags:** tổ hợp, dp, toán, lý thuyết số 
**Thời gian giải:** 7 phút 47 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang xem xét tất cả các chuỗi có thể có độ dài$n+m$được làm chính xác$n$những cái và$m$điểm trừ. Mọi sự sắp xếp đều được xem xét một lần, vì vậy đây là vấn đề hoán vị nhiều tập hợp. Đối với mỗi chuỗi, chúng tôi quét các tiền tố từ trái sang phải, tính tổng hiện có của chúng và ghi lại tổng tiền tố lớn nhất đạt được nhưng chúng tôi giới hạn nó ở dưới 0 để các chuỗi không bao giờ vượt quá 0 sẽ đóng góp vào số 0. 

Nhiệm vụ không phải là đánh giá một chuỗi mà là tính tổng “chiều cao tiền tố tốt nhất” này qua mọi hoán vị hợp lệ của nhiều tập hợp. 

Việc đọc trực tiếp gợi ý rằng điều quan trọng là mức độ thường xuyên xảy ra các chuyến du ngoạn tích cực trong tất cả các bước đi có tổng tiền tố được hình thành bởi các hoán vị của$+1$Và$-1$. Từ$n,m \le 2000$, số hoán vị là$\binom{n+m}{n}$, rất lớn ngay cả ở các giá trị vừa phải. Bất kỳ cách tiếp cận nào liệt kê rõ ràng các hoán vị đều không thể thực hiện được ngay lập tức. 

Cấu trúc sâu hơn là mỗi chuỗi là một đường mạng bắt đầu từ 0, di chuyển lên trên$+1$và xuống cho$-1$. Tổng tiền tố tối đa là chiều cao tối đa mà đường dẫn đó đạt được nhưng không bao giờ dưới 0. 

Những trường hợp khó khăn phá vỡ lối suy nghĩ ngây thơ xuất hiện khi những bước đi tiêu cực chiếm ưu thế sớm. Ví dụ, khi$n=0$, mọi tổng tiền tố đều không dương, nên mọi đóng góp đều bằng 0. Một góc khác là khi$m=0$, trong đó mọi hoán vị đều là số một và câu trả lời đơn giản là$n$mỗi chuỗi, nhân với một cấu hình duy nhất. 

Một vấn đề tinh tế hơn là tổng tiền tố tối đa không bị ràng buộc với tổng cuối cùng$n-m$. Ngay cả các chuỗi kết thúc ở mức cao cũng có thể không bao giờ có đỉnh tiền tố lớn nếu các chuỗi đó bị trễ, do đó việc đếm chỉ dựa trên độ dịch chuyển cuối cùng sẽ không thành công. 

## Phương pháp tiếp cận 

Một giải pháp brute-force sẽ tạo ra mọi hoán vị của nhiều tập hợp và tính toán cực đại tiền tố trong$O(n+m)$mỗi trình tự. Vì có$\binom{n+m}{n}$trình tự, điều này dẫn đến khoảng$O((n+m)\binom{n+m}{n})$, phát nổ vượt quá mọi giới hạn đối với$n,m\le 2000$. 

Sự thay đổi cấu trúc quan trọng là ngừng suy nghĩ về các hoán vị và thay vào đó hãy nghĩ về thời điểm tổng tiền tố tối đa ít nhất trở thành một giá trị nhất định. Nếu chúng ta cố định chiều cao$h$, chúng ta có thể hỏi có bao nhiêu dãy đạt đến tổng tiền tố$\ge h$. Mỗi chuỗi đóng góp chiều cao tối đa của nó, do đó nó đóng góp$1$cho mỗi cấp độ nó quản lý để đạt được. Điều này biến vấn đề thành tổng hợp theo cấp độ chứ không phải theo trình tự. 

Vì vậy, thay vì tính tổng trực tiếp mức tối đa, chúng tôi tính mức đóng góp theo cấp độ. Đối với mỗi$h \ge 1$, chúng tôi đếm có bao nhiêu chuỗi có tiền tố đạt ít nhất$h$. Điều này tương đương với việc đếm các đường dẫn không bao giờ ở dưới mức nghiêm ngặt$h$trước thời điểm đánh đầu tiên của họ$h$. Đây là cách thiết lập nguyên tắc phản ánh cổ điển: chúng tôi đếm tất cả các chuỗi và trừ đi những chuỗi không bao giờ đạt đến mức$h$. 

Đối với một cố định$h$, chúng ta dịch chuyển hệ tọa độ để đạt được$h$tương ứng với việc đạt đến số 0 trong một bước đi được biến đổi và chúng tôi giảm vấn đề xuống việc đếm các đường đi nằm dưới một ranh giới. Điều này trở thành một vấn đề hạn chế tiền tố tổ hợp, có thể giải được bằng cách sử dụng các hệ số nhị thức và số lượng “loại phiếu bầu” tiêu chuẩn. 

Tổng hợp những đóng góp này trên tất cả những gì có thể$h$mang lại câu trả lời cuối cùng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O((n+m)\binom{n+m}{n})$|$O(n+m)$| Quá chậm | 
| Tối ưu |$O(nm)$|$O(nm)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi diễn giải lại câu trả lời bằng cách sử dụng danh tính$$f(a) = \sum_{h \ge 1} [\text{prefix sum reaches } h].$$Vì vậy, tổng câu trả lời sẽ trở thành tổng theo độ cao của số hoán vị hợp lệ đạt đến độ cao đó. 

1. Xác định$C = \binom{n+m}{n}$, tổng số trình tự. Đây là đường cơ sở mà từ đó tất cả các số lượng bị hạn chế được tính ra. 
2. Đối với chiều cao cố định$h$, xác định hàm$g(h)$là số chuỗi có tổng tiền tố không bao giờ đạt tới$h$. Đây chính xác là các chuỗi có tổng tiền tố tối đa nhiều nhất$h-1$. 
3. Chúng tôi tính toán$g(h)$sử dụng phép biến đổi kiểu lá phiếu. If a path never reaches$h$, sau đó dịch chuyển tất cả các tổng tiền tố xuống dưới$h$chuyển đổi vấn đề thành các đường đếm luôn ở dưới 0 sau một ca. Điều này trở nên tương đương với việc đi bộ bị hạn chế từ$(0,0)$không bao giờ vượt qua một đường ranh giới. 
4. Áp dụng nguyên lý phản xạ, số dãy đạt tới$h$là$$C - \binom{n+m}{n-h}$$bất cứ khi nào$n \ge h$, nếu không tất cả các chuỗi không đạt được$h$. 

Điều này xuất phát từ việc phản ánh điểm đánh đầu tiên ở cấp độ$h$, ánh xạ các đường dẫn không hợp lệ tới các đường dẫn không bị ràng buộc với số bước lên đã giảm. 
5. Mỗi chiều cao$h$đóng góp chính xác số lượng chuỗi đạt tới nó, vì vậy chúng tôi thêm:$$\sum_{h=1}^{n} \left(C - \binom{n+m}{n-h}\right)$$6. Chia số tiền:$$n \cdot C - \sum_{h=1}^{n} \binom{n+m}{n-h}$$Lập chỉ mục lại đơn giản hóa thuật ngữ thứ hai thành tiền tố tiêu chuẩn của các hệ số nhị thức. 
7. Tính toán trước các giai thừa và giai thừa nghịch đảo để đánh giá tất cả các hệ số nhị thức trong$O(1)$mỗi cái, sau đó tính tổng trong$O(n)$. 

### Tại sao nó hoạt động 

Mỗi chuỗi đóng góp chính xác một đơn vị cho mỗi cấp số nguyên mà nó đạt được. Đếm “mức đạt$h$" chuyển đổi hoạt động tối đa thành tổng các sự kiện chỉ báo. Nguyên tắc phản ánh đảm bảo rằng số lượng đường dẫn không đạt đến ngưỡng có thể được biểu thị dưới dạng một hệ số nhị thức dịch chuyển đơn. Vì mỗi đường dẫn được ánh xạ duy nhất dưới sự phản ánh ở lần vi phạm đầu tiên nên không xảy ra tình trạng đếm quá mức hoặc thiếu sót và việc phân tách theo độ cao vẫn chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 998244853

def solve():
    n, m = map(int, input().split())
    N = n + m

    if n == 0:
        print(0)
        return

    # factorials up to N
    fact = [1] * (N + 1)
    invfact = [1] * (N + 1)

    for i in range(1, N + 1):
        fact[i] = fact[i - 1] * i % MOD

    invfact[N] = pow(fact[N], MOD - 2, MOD)
    for i in range(N, 0, -1):
        invfact[i - 1] = invfact[i] * i % MOD

    def C(a, b):
        if b < 0 or b > a:
            return 0
        return fact[a] * invfact[b] % MOD * invfact[a - b] % MOD

    total
```
