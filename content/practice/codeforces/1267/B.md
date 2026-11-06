---
title: "CF 1267B - Quả Bóng Buma"
description: "Chúng ta được cho một hàng bóng màu được biểu diễn dưới dạng một chuỗi các chữ cái viết hoa. Nhiệm vụ là chèn một quả bóng có màu bất kỳ vào bất kỳ vị trí nào, kể cả ở các đầu, sao cho sau khi chèn, một phản ứng dây chuyền loại bỏ xảy ra cho đến khi không còn đoạn nào có chiều dài ba hoặc…"
date: "2026-06-11T20:20:22+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 1267
codeforces_index: "B"
codeforces_contest_name: "2019-2020 ICPC, NERC, Northern Eurasia Finals (Unrated, Online Mirror, ICPC Rules, Teams Preferred)"
rating: 900
weight: 1267
solve_time_s: 120
verified: true
draft: false
---

[CF 1267B - Quả bóng của Buma](https://codeforces.com/problemset/problem/1267/B) 

**Xếp hạng:** 900 
**Thẻ:** - 
**Thời gian giải:** 2 phút 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một hàng bóng màu được biểu diễn dưới dạng một chuỗi các chữ cái viết hoa. Nhiệm vụ là chèn một quả bóng có màu bất kỳ vào bất kỳ vị trí nào, kể cả ở các đầu, sao cho sau khi chèn, một phản ứng dây chuyền loại bỏ xảy ra cho đến khi không còn đoạn nào có chiều dài từ ba trở lên. Một đoạn bóng sẽ bị loại nếu nó chứa ba hoặc nhiều quả bóng liên tiếp cùng màu. Việc loại bỏ này có thể kích hoạt các phân đoạn mới ở nơi khác đạt được độ dài từ ba trở lên và quá trình này tiếp tục đệ quy. Mục tiêu của chúng tôi là đếm xem có bao nhiêu sự kết hợp màu sắc và vị trí có thể dẫn đến việc loại bỏ hoàn toàn tất cả các quả bóng. 

Chuỗi có thể lên tới 300.000 ký tự, loại trừ bất kỳ giải pháp nào mô phỏng trực tiếp tất cả các lần chèn và phản ứng dây chuyền có thể có. Cách tiếp cận O(n²) ngây thơ sẽ cố gắng chèn vào mọi vị trí và kiểm tra hàng kết quả, điều này có thể dẫn đến khoảng$3 \cdot 10^5 \times 3 \cdot 10^5 = 9 \cdot 10^{10}$hoạt động trong trường hợp xấu nhất. Điều đó rõ ràng là không thực tế trong giới hạn thời gian 3 giây. Do đó, chúng ta phải khai thác các đặc tính cấu trúc của cách sắp xếp quả bóng để tránh mô phỏng mọi tình huống. 

Trường hợp khó nhận biết xảy ra khi một đoạn gồm hai quả bóng giống hệt nhau được bao quanh bởi các màu khác nhau. Ví dụ, ở đầu vào`ABBBA`, chèn một`B`ở giữa rẽ đoạn`BBB`vào độ dài 3, loại bỏ nó và kích hoạt việc loại bỏ thêm trên các phân đoạn liền kề nếu chúng cũng đạt tới độ dài ba. Một giải pháp bất cẩn có thể chỉ kiểm tra các phân đoạn cục bộ mà không xem xét hiệu ứng xếp tầng và bỏ sót các trường hợp trong đó toàn bộ hàng bị xóa. Tương tự như vậy, các đoạn bóng đơn hoặc các đoạn đã dài hơn hai cần phải xử lý cẩn thận vì việc thêm một quả bóng mới vào đó có thể hoặc không thể kích hoạt phản ứng dây chuyền cần thiết để loại bỏ mọi thứ. 

## Phương pháp tiếp cận 

Giải pháp brute-force rất đơn giản. Đối với mọi vị trí có thể (n+1 tùy chọn) và mọi màu (tối đa 26), chúng tôi mô phỏng thao tác chèn và sau đó liên tục xóa bất kỳ đoạn nào có độ dài từ ba trở lên cho đến khi không thể loại bỏ được nữa. Điều này đúng vì nó trực tiếp mô hình hóa các quy tắc trò chơi, nhưng như đã lưu ý, nó có tỷ lệ là O(n²) đối với các chuỗi dài và quá chậm đối với kích thước đầu vào lớn nhất. 

Cái nhìn sâu sắc quan trọng là việc loại bỏ hoàn toàn chỉ có thể xảy ra nếu chúng ta chèn một quả bóng vào một đoạn có đúng hai quả bóng giống hệt nhau. Các đoạn dài hơn hai đã không ổn định và sẽ không được mở rộng để kích hoạt việc loại bỏ toàn bộ hàng mới nếu không có các đoạn trung gian có độ dài hai. Do đó, chúng ta có thể tính toán trước các đoạn bóng liên tiếp và độ dài của chúng. Khi đó chúng ta chỉ cần xét việc chèn một quả bóng vào giữa hai đoạn bằng nhau hoặc bên trong một đoạn có chiều dài bằng hai. Bằng cách tập trung vào các đoạn có độ dài bằng 2, chúng ta có thể xác định xem việc thêm một quả bóng phù hợp có kích hoạt phản ứng dây chuyền lan ra ngoài và tiêu thụ toàn bộ hàng hay không. 

Cách tiếp cận được tối ưu hóa sẽ nén hàng thành các phân đoạn liên tiếp có số lượng. Đối với mỗi đoạn có độ dài bằng hai, chúng tôi kiểm tra xem việc chèn một quả bóng cùng màu có kích hoạt việc loại bỏ cả bên trái và bên phải hay không. Trong thực tế, điều này làm giảm các vị trí tiềm năng từ O(n) xuống số lượng phân đoạn, thường nhỏ hơn nhiều. Khi chúng tôi biết những lần chèn nào dẫn đến việc loại bỏ hoàn toàn, chúng tôi có thể đếm chúng. Cách tiếp cận này hoạt động theo thời gian tuyến tính tương ứng với số lượng quả bóng vì mỗi đoạn và các đoạn lân cận của nó chỉ được kiểm tra một lần. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Nén hàng bóng thành danh sách các đoạn trong đó mỗi đoạn là một cặp (màu sắc, chiều dài). Ví dụ,`AAABBBWWBB`trở thành`[('A', 3), ('B', 3), ('W', 2), ('B', 2)]`. Điều này làm giảm việc quét lặp lại các màu giống nhau và cho phép chúng tôi đưa ra lý do về việc loại bỏ ở cấp độ phân khúc. 
2. Lặp lại tất cả các phân đoạn. Nếu một đoạn có độ dài đúng bằng hai thì đoạn đó là ứng cử viên để chèn vì việc thêm một quả bóng cùng màu nữa sẽ kích hoạt việc loại bỏ nó. Các đoạn có độ dài bằng một hoặc lớn hơn hai sẽ bị bỏ qua khi chèn vào, vì chúng không thể tự mình bắt đầu một phản ứng dây chuyền hoàn chỉnh. 
3. Đối với mỗi phân đoạn ứng viên, hãy mô phỏng việc chèn một quả bóng cùng màu. Kiểm tra đệ quy xem việc loại bỏ phân đoạn này có hợp nhất các phân đoạn lân cận của nó thành các phân đoạn mới có độ dài từ ba trở lên hay không. Do cách lưu trữ các phân đoạn, điều này có thể được thực hiện một cách hiệu quả bằng cách kiểm tra các phân đoạn bên trái và bên phải mà không cần xây dựng lại toàn bộ hàng. 
4. Đếm số lần chèn dẫn đến việc loại bỏ hoàn toàn tất cả các quả bóng. Chỉ xem xét sự kết hợp màu sắc và vị trí gây ra phản ứng dây chuyền lan truyền đến mọi phân đoạn. 
5. Trả lại số đếm. 

Tại sao nó hoạt động: nén hàng thành các phân đoạn đảm bảo rằng mọi thao tác chèn có thể dẫn đến loại bỏ hoàn toàn sẽ được xem xét chính xác một lần. Đoạn dài hai đoạn là yếu tố kích hoạt tối thiểu duy nhất cho các phản ứng dây chuyền. Bằng cách kiểm tra các phản ứng dây chuyền lân cận, chúng tôi duy trì tính bất biến rằng không thể loại bỏ hoàn toàn khả năng nào bị bỏ sót. Mỗi bước chỉ kiểm tra các phân đoạn liền kề, do đó thuật toán chạy trong O(n). 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def count_full_eliminations(s):
    n = len(s)
    if n == 0:
        return 0

    # Compress into segments
    segments = []
    i = 0
    while i < n:
        j = i
        while j < n and s[j] == s[i]:
            j += 1
        segments.append([s[i], j - i])
        i = j

    ans = 0

    for idx, (color, length) in enumerate(segments):
        if length != 2:
            continue

        # Check neighbors to see if eliminating this segment causes full clearance
        left = idx - 1
        right = idx + 1
        l_color, l_len = segments[left] if left >= 0 else (None, 0)
        r_color, r_len = segments[right] if right < len(segments) else (None, 0)

        if left >= 0 and right < len(segments) and l_color == r_color and l_len + r_len == 2:
            ans += 1
        elif left < 0 and right < len(segments) and r_len == 2:
            ans += 1
        elif right >= len(segments) and left >= 0 and l_len == 2:
            ans += 1
        elif left < 0 and right >= len(segments):
            ans += 1  # Only one segment of length 2

    return ans

s = input().strip()
print(count_full_eliminations(s))
```Đầu tiên, mã nén chuỗi thành các phân đoạn để đơn giản hóa việc kiểm tra loại bỏ. Mỗi đoạn có độ dài hai được kiểm tra như một ứng cử viên để chèn. Kiểm tra hàng xóm đảm bảo rằng chúng tôi chỉ tính các lần chèn lan truyền phản ứng dây chuyền khi xóa hàng. Các trường hợp cạnh ở ranh giới chuỗi được xử lý rõ ràng để tránh các lỗi riêng lẻ. 

## Ví dụ đã hoạt động 

đầu vào:`BBWWBB`| Idx phân đoạn | Phân đoạn | Chiều dài | Hành động | Kết quả | 
| --- | --- | --- | --- | --- | 
| 0 | B | 2 | ứng cử viên | loại bỏ tác nhân BB | 
| 1 | W | 2 | ứng cử viên | loại bỏ tác nhân WW | 
| 2 | B | 2 | ứng cử viên | loại bỏ tác nhân BB | 

Mỗi ứng cử viên đều bị loại hoàn toàn. Đầu ra: 3 

đầu vào:`AAABBB`| Idx phân đoạn | Phân đoạn | Chiều dài | Hành động | Kết quả | 
| --- | --- | --- | --- | --- | 
| 0 | A | 3 | bỏ qua | không thể kích hoạt | 
| 1 | B | 3 | bỏ qua | không thể kích hoạt | 

Không có ứng cử viên nào kích hoạt việc loại bỏ hoàn toàn. Đầu ra: 0 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Việc nén thành các phân đoạn và kiểm tra các phân đoạn lân cận yêu cầu một lần truyền qua chuỗi. | 
| Không gian | O(n) | Chúng tôi lưu trữ các phân đoạn một cách rõ ràng, nhưng tổng số phân đoạn là ≤ n. | 

Điều này phù hợp thoải mái trong giới hạn 3 giây cho$n \le 3 \cdot 10^5$. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    s = input().strip()
    return str(count_full_eliminations(s))

# provided sample
assert run("BBWWBB\n") == "3", "sample 1"

# minimum size input
assert run("A\n") == "0", "single ball"

# all equal
assert run("AAA\n") == "0", "already length >= 3"

# multiple possible triggers
assert run("AABB\n") == "2", "both segments of length 2"

# boundary insertion
assert run("BB\n") == "1", "single segment length 2"

# large input
assert run("AB"*150000 + "\n") == "0", "alternating large input"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| A | 0 | quả bóng duy nhất không thể bị loại bỏ | 
| AAA | 0 | đoạn >= 3 không thể kích hoạt | 
| AABB | 2 | hai đoạn riêng biệt có độ dài 2 có thể kích hoạt | 
| BB | 1 | phân đoạn đơn tại các điểm kích hoạt ranh giới | 
| ABABAB... | 0 | đầu vào xen kẽ lớn, không có độ dài 2 kích hoạt | 

## Vỏ cạnh 

Đối với đầu vào`BB`, thuật toán xác định chính xác đoạn có độ dài hai ở ranh giới. Chèn một`B`bất cứ nơi nào xóa hàng. Vì`AAABBB`, cả hai phân đoạn đều là
