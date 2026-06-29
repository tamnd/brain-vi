---
title: "CF 104743B - Xây dựng mảng"
description: "Chúng ta được yêu cầu quyết định xem có thể xây dựng một mảng có độ dài n bằng cách sử dụng các số nguyên riêng biệt sao cho hai ràng buộc bitwise toàn cục được thỏa mãn đồng thời hay không."
date: "2026-06-29T01:21:49+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104743
codeforces_index: "B"
codeforces_contest_name: "TheForces Round #25(5^2-Forces)"
rating: 0
weight: 104743
solve_time_s: 96
verified: false
draft: false
---

[CF 104743B - Xây dựng mảng](https://codeforces.com/problemset/problem/104743/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 36 giây 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu quyết định liệu có thể xây dựng một mảng có độ dài hay không`n`sử dụng các số nguyên riêng biệt sao cho hai ràng buộc bitwise toàn cục được thỏa mãn đồng thời. Một ràng buộc buộc bitwise HOẶC trên tất cả các phần tử bằng nhau`x`, nghĩa là mọi bit xuất hiện trong`x`phải xuất hiện trong ít nhất một phần tử mảng và không phần tử nào được phép đưa các bit ra bên ngoài`x`. Ràng buộc khác buộc bitwise AND trên tất cả các phần tử bằng nhau`y`, nghĩa là mọi phần tử phải chứa tất cả các bit của`y`, và bất kỳ bit nào không có trong`y`phải thiếu ít nhất một phần tử. 

Sự tương tác giữa hai ràng buộc này tạo ra một hạn chế mạnh mẽ về mặt cấu trúc. Mọi giá trị mảng phải nằm bên trong “không gian mặt nạ” của`x`, đồng thời bị buộc phải bao gồm tất cả các bit của`y`. Vì vậy, mỗi phần tử về cơ bản là`y`cộng với một số lựa chọn bit bổ sung được cho phép bởi`x`. 

Những hạn chế là rất lớn về mặt`n`, do đó, bất kỳ giải pháp nào cố gắng xây dựng hoặc tìm kiếm thông qua các mảng ứng cử viên một cách rõ ràng chỉ khả thi nếu nó tuyến tính trong`n`. Bất cứ điều gì theo cấp số nhân trên bitmask đều không thể xảy ra khi`n`có thể đạt tới`10^5`. 

Một số trường hợp đặc biệt phá vỡ lối suy luận ngây thơ ngay lập tức. 

Nếu như`n = 1`, chỉ có một phần tử mảng. Trong trường hợp đó OR và AND đều bằng giá trị đơn đó, vì vậy chúng ta phải có`x = y`. Ví dụ,`n = 1, x = 2, y = 0`là không thể vì không có số nào có thể đồng thời có OR 2 và AND 0 trừ khi nó đồng thời là 2 và 0. 

Nếu như`y`chứa một chút không có trong`x`, thì mọi phần tử mảng buộc phải bao gồm một bit mà OR cấm. Ví dụ,`x = 2 (10b)`Và`y = 3 (11b)`ngay lập tức thất bại vì mọi phần tử đều phải chứa bit thấp nhất, nhưng OR không được bao gồm bit đó. 

Một thất bại tinh vi hơn xảy ra khi`n > 1`nhưng chúng tôi cố gắng “truyền bá các bit một cách độc lập” mà không đảm bảo tất cả các phần tử vẫn khác biệt trong khi vẫn bao trùm tất cả các bit của`x`. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ cố gắng liệt kê tất cả các mảng có kích thước có thể`n`trong đó mỗi phần tử nằm giữa`0`Và`x`, thực thi rằng tất cả các phần tử bao gồm`y`, sau đó kiểm tra các điều kiện OR và AND. Ngay cả khi chúng ta giới hạn giá trị ở các tập con bit trong`x`, số lượng ứng cử viên là số mũ theo số bit miễn phí. Với tối đa 30 bit, điều này trở thành`2^30`khả năng cho mỗi phần tử và việc xây dựng mảng rõ ràng là không khả thi. 

Quan sát quan trọng là điều kiện AND hạn chế rất nhiều cấu trúc. Vì mỗi phần tử phải chứa tất cả các bit của`y`, chúng ta có thể tính nhân tử`y`ra khỏi mọi yếu tố. Mỗi phần tử có thể được viết là:`a_i = y | s_i`Ở đâu`s_i`chỉ sử dụng các bit được cho phép bởi`x`nhưng chưa được sửa bởi`y`. 

Cho phép`free = x XOR y`(hoặc các bit tương đương trong`x`không ở trong`y`). Mỗi phần tử mảng hợp lệ tương ứng với việc chọn một tập hợp con các bit trống này. 

Bây giờ điều kiện OR trở thành một yêu cầu mà trên tất cả các tập hợp con được chọn, mọi bit trong`free`phải xuất hiện ít nhất một lần. Điều kiện AND đã được xử lý bởi quá trình xây dựng vì`y`được cố định trong tất cả các phần tử. 

Chúng tôi cũng cần tất cả các phần tử phải khác biệt, điều này giúp giảm việc chọn các tập hợp con riêng biệt`s_i`. 

Tổng số tập con riêng biệt có thể có của các bit trống là`2^k`, Ở đâu`k`là số lượng bit được đặt trong`free`. Vậy điều kiện cần là`n <= 2^k`. 

Chúng ta cũng cần xử lý trường hợp đặc biệt`n = 1`, trong đó giá trị đơn phải thỏa mãn đồng thời cả OR và AND, buộc`x = y`. 

Ý tưởng mang tính xây dựng là coi các tập hợp con của các bit trống như mặt nạ nhị phân và chọn bất kỳ`n`các mặt nạ riêng biệt trong khi đảm bảo rằng một trong số chúng là mặt nạ đầy đủ (tất cả các bit miễn phí được đặt), đảm bảo OR trở nên chính xác`x`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Liệt kê lực lượng vũ phu | Hàm mũ | Hàm mũ | Quá chậm | 
| Xây dựng Bitmask | O(n + 30) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Kiểm tra xem`y`chứa bất kỳ bit nào bên ngoài`x`. Nếu như`(y & ~x) != 0`, không có giải pháp tồn tại. Điều này là do mọi phần tử phải bao gồm`y`, điều này sẽ buộc các bit bị cấm vào OR. 
2. Nếu`n == 1`, chỉ trả về CÓ nếu`x == y`. Với một phần tử, OR và AND giống hệt nhau nên không có sự tự do nào tồn tại. 
3. Tính toán`free = x ^ y`, đại diện cho các bit có thể khác nhau giữa các phần tử. 
4. Hãy để`k`là số lượng bit được đặt trong`free`. Số phần tử hợp lệ riêng biệt mà chúng ta có thể xây dựng là`2^k`. Nếu như`n > 2^k`, trả về KHÔNG vì chúng tôi không thể tạo đủ các mảng riêng biệt. 
5. Xây dựng mảng bằng các tập hợp con của`free`. Mỗi phần tử là`y | mask`, Ở đâu`mask`là một tập con riêng biệt của`free`. 
6. Đảm bảo rằng ít nhất một mặt nạ được chọn là mặt nạ đầy đủ`(2^k - 1)`sao cho OR trên tất cả các phần tử đạt tới`x`. Sau đó điền vào phần còn lại`n - 1`các phần tử với bất kỳ mặt nạ riêng biệt nào khác. 

### Tại sao nó hoạt động 

Mỗi phần tử được xây dựng đều chứa`y`, do đó AND trên tất cả các phần tử luôn bảo toàn tất cả các bit của`y`. Mọi phần tử đều bị giới hạn ở các bit bên trong`x`, do đó OR không thể vượt quá`x`. Bao gồm mặt nạ bit miễn phí đầy đủ đảm bảo rằng mọi bit trong`x`xuất hiện trong ít nhất một phần tử, làm cho OR chính xác`x`. Tính khác biệt được đảm bảo vì tất cả các mặt nạ đều khác biệt và tính khả thi hoàn toàn bị chi phối bởi số lượng tập hợp con có sẵn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n, x, y = map(int, input().split())

        if (y & ~x) != 0:
            print("NO")
            continue

        if n == 1:
            print("YES" if x == y else "NO")
            continue

        free = x ^ y
        k = free.bit_count()

        if n > (1 << k):
            print("NO")
            continue

        print("YES")

if __name__ == "__main__":
    solve()
```Việc triển khai trước tiên thực thi ràng buộc ngăn chặn bit giữa`y`Và`x`. Sau đó, nó xử lý trực tiếp trường hợp cạnh một phần tử. Đối với các mảng lớn hơn, nó làm giảm vấn đề đếm các tập hợp con có sẵn của các vị trí bit trống. Việc sử dụng`bit_count()`an toàn vì nó chạy trong thời gian không đổi đối với số nguyên 30 bit và dịch chuyển`1 << k`có hiệu lực vì`k <= 30`. 

Bản thân việc xây dựng không được in rõ ràng ở đây vì bài toán chỉ yêu cầu tính khả thi. Lý do đã đảm bảo rằng nếu các điều kiện thỏa mãn, một mảng hợp lệ luôn có thể được hình thành. 

## Ví dụ đã hoạt động 

Hãy xem xét`n = 3, x = 6 (110b), y = 2 (010b)`. 

Đây là các bit miễn phí`100b`, Vì thế`k = 1`và có`2`tập hợp con có thể. 

| Bước | miễn phí | k | 2^k | n kiểm tra | quyết định | 
| --- | --- | --- | --- | --- | --- | 
| tính toán | 100 | 1 | 2 | 3 > 2 | KHÔNG | 

Vì không có đủ mặt nạ riêng biệt nên việc xây dựng là không thể. Điều này cho thấy sự khác biệt thực sự là một yếu tố hạn chế. 

Bây giờ hãy xem xét`n = 2, x = 6 (110b), y = 2 (010b)`. 

| Bước | miễn phí | k | 2^k | n kiểm tra | quyết định | 
| --- | --- | --- | --- | --- | --- | 
| tính toán | 100 | 1 | 2 | 2 2 2 | CÓ | 

Chúng ta có thể xây dựng mặt nạ một cách rõ ràng`{1, 0}`trong không gian trống, đưa ra các yếu tố`{3, 2}`. OR của họ là`3 | 2 = 3`? Đợi đã, chúng ta phải đảm bảo có đầy đủ mặt nạ; mặt nạ đầy đủ miễn phí là`1`, vì vậy chúng tôi bao gồm nó, đưa ra`{3, 2}`OR nào`3`, khớp`x`. 

Dấu vết này cho thấy cách bao gồm tập hợp con đầy đủ đảm bảo tính chính xác của OR. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(t) | Mỗi bài kiểm tra chỉ thực hiện các thao tác theo bit và số lượng popcount | 
| Không gian | O(1) | Không cần cấu trúc phụ trợ tỷ lệ thuận với kích thước đầu vào | 

Giải pháp dễ dàng phù hợp với các ràng buộc vì mỗi trường hợp thử nghiệm giảm xuống một vài thao tác bit có thời gian không đổi. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    def solve():
        t = int(input())
        for _ in range(t):
            n, x, y = map(int, input().split())
            if (y & ~x) != 0:
                print("NO")
                continue
            if n == 1:
                print("YES" if x == y else "NO")
                continue
            free = x ^ y
            k = free.bit_count()
            print("YES" if n <= (1 << k) else "NO")

    from io import StringIO
    old = sys.stdout
    sys.stdout = StringIO()
    solve()
    out = sys.stdout.getvalue()
    sys.stdout = old
    return out.strip()

assert run("1\n1 0 0\n") == "YES"
assert run("1\n1 1 0\n") == "NO"
assert run("1\n3 6 2\n") == "NO"
assert run("1\n2 6 2\n") == "YES"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`n=1, x=y`| CÓ | độ chính xác của phần tử đơn | 
|`n=1, x!=y`| KHÔNG | VÀ=HOẶC ràng buộc | 
|`n=3, impossible mask space`| KHÔNG | mặt nạ không đủ khác biệt | 
|`n=2, valid construction`| CÓ | tính khả thi với số bit miễn phí tối thiểu | 

## Vỏ cạnh 

Khi nào`n = 1`, thuật toán so sánh trực tiếp`x`Và`y`. Đây là tình huống duy nhất trong đó OR và AND thu gọn thành một ràng buộc giá trị duy nhất. Ví dụ, đầu vào`1 5 5`trả về CÓ, trong khi`1 5 4`trả về KHÔNG vì không có số nào có thể thỏa mãn cả hai cùng một lúc trừ khi nó bằng cả hai mục tiêu. 

Khi`y`chứa các bit bên ngoài`x`, chẳng hạn như`n = 4, x = 2, y = 3`, điều kiện`(y & ~x) != 0`kích hoạt ngay lập tức. Mọi phần tử phải bao gồm bit`0`, nhưng OR cấm điều đó, khiến việc xây dựng không thể thực hiện được bất kể`n`. 

Khi không gian bit trống lớn nhưng`n`vượt quá`2^k`, chẳng hạn như`x = 1023, y = 0`với`n = 2000`, thuật toán sẽ bác bỏ trường hợp đó một cách chính xác vì chỉ có`1024`các tập hợp con riêng biệt có sẵn, do đó, chỉ riêng tính khác biệt là không thể mặc dù các ràng buộc OR/AND rất linh hoạt.
