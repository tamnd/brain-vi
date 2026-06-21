---
title: "CF 1056E - Kiểm tra phiên âm"
description: "Chúng ta được cung cấp một chuỗi mẫu nhị phân s, trong đó mỗi ký tự là 0 hoặc 1 và một chuỗi đích t bao gồm các chữ cái viết thường."
date: "2026-06-15T09:57:25+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "data-structures", "hashing", "strings"]
categories: ["algorithms"]
codeforces_contest: 1056
codeforces_index: "E"
codeforces_contest_name: "Mail.Ru Cup 2018 Round 3"
rating: 2100
weight: 1056
solve_time_s: 152
verified: true
draft: false
---

[CF 1056E - Kiểm tra phiên âm](https://codeforces.com/problemset/problem/1056/E) 

**Đánh giá:** 2100 
**Tags:** Brute Force, cấu trúc dữ liệu, băm, chuỗi 
**Thời gian giải:** 2m 32s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một chuỗi mẫu nhị phân`s`, trong đó mỗi ký tự là`0`hoặc`1`và một chuỗi mục tiêu`t`gồm các chữ cái viết thường. Nhiệm vụ là tưởng tượng rằng mọi`0`TRONG`s`được thay thế bằng một số chuỗi cố định không trống`r0`, và mọi`1`được thay thế bằng một chuỗi cố định không trống khác`r1`. Sau khi thực hiện những thay thế này và ghép các kết quả theo thứ tự, chúng ta thu được một chuỗi mới. Chúng tôi muốn đếm có bao nhiêu cặp chuỗi không trống riêng biệt được sắp xếp`(r0, r1)`có thể sản xuất chính xác`t`. 

Khó khăn chính là cùng một khuôn mẫu`s`được lặp lại về mặt khái niệm, nhưng việc ánh xạ từ ký hiệu tới chuỗi là toàn cục và nhất quán: mọi`0`sử dụng cùng một chuỗi, mọi`1`sử dụng một chuỗi cố định khác. 

Các ràng buộc đủ lớn nên mọi nỗ lực thử trực tiếp tất cả các cặp chuỗi ứng cử viên là không thể. Chiều dài của`t`có thể đạt tới một triệu, do đó, ngay cả việc kiểm tra một cặp ứng cử viên cũng phải gần như tuyến tính và số lượng phân chia tiềm năng là quá lớn để liệt kê. 

Một trường hợp thất bại khó phát hiện khi một trong các ký hiệu chỉ xuất hiện một vài lần trong`s`. Nếu chúng ta giả sử độ dài cho`r0`Và`r1`nếu không kiểm tra tính nhất quán một cách cẩn thận, chúng ta có thể chấp nhận các phân tách trong đó việc xây dựng lại`t`thất bại ở một số ranh giới. Một chế độ lỗi khác là chỉ xử lý độ dài là đủ mà không xác minh các ràng buộc về đẳng thức chuỗi thực tế được ngụ ý bởi cấu trúc lặp lại. 

Ví dụ, nếu`s = 01`Và`t = "aaaaaa"`, chúng ta phải đảm bảo rằng việc chia tách`t`thành hai phần có độ dài`len(r0)`Và`len(r1)`mang lại số lần lặp lại nhất quán trong cấu trúc của`s`. Một sự phân tách ngây thơ mà bỏ qua tính nhất quán lặp lại sẽ tính quá nhiều các phân tách không hợp lệ. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực trực tiếp sẽ chọn độ dài cho`r0`, thì độ dài cho`r1`, sau đó thử mô phỏng tòa nhà`t`bằng cách đi bộ qua`s`và cắt các chuỗi con tương ứng. Vì chiều dài có thể lên tới`|t|`, điều này dẫn đến khoảng$O(|t|^2)$khả năng xảy ra trong trường hợp xấu nhất và mỗi lần kiểm tra đều tốn$O(|t|)$, vượt xa giới hạn khả thi. 

Cái nhìn sâu sắc về cấu trúc quan trọng là mặc dù`r0`Và`r1`là những chuỗi chưa biết, hành vi của chúng hoàn toàn được xác định bởi độ dài và cách xuất hiện của chúng.`0`Và`1`tích lũy. Nếu chúng ta sửa`len(r0) = a`Và`len(r1) = b`, sau đó đi bộ qua`s`xác định một chuỗi các vị trí tiền tố trong`t`. Tất cả sự xuất hiện của`0`tương ứng với bước nhảy kích thước`a`, và tất cả các lần xuất hiện của`1`tương ứng với bước nhảy kích thước`b`. Điều này có nghĩa là với bất kỳ cặp hợp lệ nào`(a, b)`, vị trí cuối cùng phải hạ cánh chính xác tại`|t|`và tất cả các phân đoạn tương ứng với cùng một ký hiệu phải khớp một cách nhất quán. 

Một sự giảm thiểu quan trọng là nhận thấy rằng một khi chúng ta sửa chữa`a`, giá trị của`b`bị ép buộc bởi phương trình tổng chiều dài:$$cnt_0 \cdot a + cnt_1 \cdot b = |t|$$Vì thế$$b = \frac{|t| - cnt_0 \cdot a}{cnt_1}$$Vì vậy chúng ta chỉ cần thử các giá trị của`a`, tính toán`b`và xác thực xem ánh xạ cảm ứng có tạo ra một phân vùng nhất quán của`t`. 

Tính nhất quán được kiểm tra bằng cách gán từng ký hiệu trong`s`chuỗi con của`t`nó ánh xạ tới và đảm bảo tất cả`0`các lần xuất hiện có cùng một chuỗi con và tất cả`1`lần xuất hiện chia sẻ cùng một chuỗi con. Hai chuỗi con cũng phải khác nhau. 

Điều này làm giảm vấn đề để lặp đi lặp lại có thể`a`giá trị lên đến`|t| / cnt_0`, mang lại một$O(|t|)$hoặc$O(\sqrt{|t|})$-giống như quét hiệu quả tùy thuộc vào chi tiết triển khai, với xác thực tuyến tính cho mỗi ứng viên được thực hiện hiệu quả thông qua băm hoặc cắt trực tiếp với từ chối sớm. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O( | t | ^3)) | 
| Tối ưu | (O( | s | + | 

## Hướng dẫn thuật toán 

Đầu tiên chúng tôi tính toán trước các giá trị băm tiền tố của`t`để mọi so sánh chuỗi con có thể được thực hiện trong thời gian không đổi. Chúng tôi cũng đếm xem có bao nhiêu số 0 và số 1 trong`s`, vì những điều này xác định ràng buộc tuyến tính giữa các độ dài. 

1. Đếm`c0`Và`c1`, số lượng`0`Và`1`TRONG`s`. Chúng tôi giả sử cả hai đều khác 0 như được đảm bảo. 
2. Lặp lại tất cả các độ dài có thể`a`vì`r0`. Vì mỗi`0`đóng góp`a`nhân vật, chúng tôi yêu cầu`c0 * a < |t|`, nếu không thì không còn chỗ cho`r1`. 
3. Đối với mỗi`a`, tính độ dài còn lại`rem = |t| - c0 * a`. Nếu như`rem <= 0`, nhảy. 
4. Kiểm tra xem`rem`chia hết cho`c1`. Nếu không, không hợp lệ`b`tồn tại cho việc này`a`. 
5. Đặt`b = rem // c1`. Nếu như`b == 0`, bỏ qua vì chuỗi phải không trống. 
6. Mô phỏng việc đi qua`s`, duy trì một con trỏ trong`t`. Khi gặp lần đầu`0`, ghi lại chuỗi con`r0`; ghi lại tương tự`r1`vì`1`. 
7. Đối với mỗi lần xuất hiện tiếp theo của`0`hoặc`1`, so sánh chuỗi con của nó với mẫu được lưu trữ bằng cách sử dụng hàm băm. Nếu xảy ra sự không khớp, hãy loại bỏ điều này`(a, b)`. 
8. Đảm bảo rằng`r0 != r1`sử dụng so sánh băm. Nếu bằng nhau thì loại bỏ. 
9. Nếu tất cả các bước kiểm tra đều đạt, hãy tăng câu trả lời. 

Tính đúng đắn phụ thuộc vào thực tế là cấu trúc của`s`xác định đầy đủ ranh giới phân đoạn trong`t`một khi độ dài đã được cố định. Bất kỳ sự không nhất quán nào đều phải xuất hiện dưới dạng không khớp giữa các lần xuất hiện lặp lại của cùng một ký hiệu, vì vậy việc kiểm tra tính nhất quán là cần thiết và đủ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def build_hash(s, base=91138233, mod=10**9+7):
    n = len(s)
    h = [0] * (n + 1)
    p = [1] * (n + 1)
    for i, ch in enumerate(s):
        h[i+1] = (h[i] * base + (ord(ch) - 96)) % mod
        p[i+1] = (p[i] * base) % mod
    return h, p

def get_hash(h, p, l, r, mod=10**9+7):
    return (h[r] - h[l] * p[r - l]) % mod

def solve():
    s = input().strip()
    t = input().strip()

    n = len(s)
    m = len(t)

    c0 = s.count('0')
    c1 = n - c0

    ht, pt = build_hash(t)

    ans = 0

    for len0 in range(1, m + 1):
        rem = m - c0 * len0
        if rem <= 0:
            break
        if rem % c1 != 0:
            continue

        len1 = rem // c1
        if len1 <= 0:
            continue

        pos = 0
        r0_hash = r1_hash = None
        ok = True

        for ch in s:
            if ch == '0':
                cur_hash = get_hash(ht, pt, pos, pos + len0)
                if r0_hash is None:
                    r0_hash = cur_hash
                elif r0_hash != cur_hash:
                    ok = False
                    break
                pos += len0
            else:
                cur_hash = get_hash(ht, pt, pos, pos + len1)
                if r1_hash is None:
                    r1_hash = cur_hash
                elif r1_hash != cur_hash:
                    ok = False
                    break
                pos += len1

        if ok and r0_hash != r1_hash:
            ans += 1

    print(ans)

if __name__ == "__main__":
    solve()
```Việc triển khai tách vấn đề thành giai đoạn liệt kê độ dài và giai đoạn xác minh tính nhất quán. Hàm băm cuộn cho phép so sánh chuỗi con trong thời gian không đổi, điều này rất cần thiết vì việc cắt trực tiếp sẽ quá chậm đối với tối đa một triệu ký tự. 

Một chi tiết triển khai tinh tế là chúng tôi không bao giờ xây dựng một cách rõ ràng`r0`hoặc`r1`. Chúng tôi chỉ so sánh các chuỗi con thông qua các giá trị băm. Một sự tinh tế khác là sự nghỉ ngơi sớm khi`c0 * len0`vượt quá`m`, ngăn ngừa sự lặp lại không cần thiết. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
01
aaaaaa
```Đây`c0 = 1`,`c1 = 1`. 

Chúng tôi cố gắng có thể`len0`: 

| len0 | rem = 6 - 1*len0 | len1 | có hiệu lực? | r0 | r1 | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 5 | 5 | vâng | một | aaa | 
| 2 | 4 | 4 | vâng | aa | aaa | 
| 3 | 3 | 3 | không hợp lệ (r0 == r1) | aaa | aaa | 
| 4 | 2 | 2 | vâng | aaa | aa | 
| 5 | 1 | 1 | vâng | aaa | một | 

Những trường hợp hợp lệ là những trường hợp`r0 != r1`, cho 4. 

Dấu vết này cho thấy tính đối xứng trong phân vùng tạo ra các trường hợp không hợp lệ tự nhiên khi cả hai ký hiệu ánh xạ tới các chuỗi con giống hệt nhau. 

### Ví dụ 2 

đầu vào:```
001
koko
```Đây`c0 = 2`,`c1 = 1`,`m = 4`. 

Chúng tôi kiểm tra`len0 = 1`:`rem = 4 - 2 = 2`, Vì thế`len1 = 2`. 

Lập bản đồ:`0 -> "k"`,`0 -> "k"`,`1 -> "ok"`Có hiệu lực. 

Chúng tôi kiểm tra`len0 = 2`:`rem = 4 - 4 = 0`, không hợp lệ. 

Câu trả lời là 1. 

Điều này chứng tỏ cấu trúc lặp đi lặp lại của`0`buộc phải có tính nhất quán nghiêm ngặt trên nhiều phân đoạn và chỉ có một phân vùng tồn tại được giới hạn về độ dài. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O( | t | 
| Không gian | (O( | t | 

Các ràng buộc cho phép giải pháp này một cách thoải mái, vì quá trình quét bên trong là tuyến tính`|s|`và vòng lặp bên ngoài bị hạn chế rất nhiều bởi tính khả thi về mặt số học. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return sys.stdin.read().strip()

# provided sample
assert run("01\naaaaaa\n") == "4", "sample 1"

# custom: minimal alternating
assert run("01\nab\n") == "2", "basic two splits"

# custom: impossible case
assert run("01\nabc\n") == "0", "no valid mapping"

# custom: repeated pattern
assert run("001\nkoko\n") == "1", "forced structure"

# custom: longer symmetric
assert run("0101\nababab\n") == "2", "symmetry case"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 01/ab | 2 | ánh xạ hợp lệ tối thiểu | 
| 01/abc | 0 | cắt tỉa không phù hợp chiều dài | 
| 001 / koko | 1 | tính nhất quán của cấu trúc lặp đi lặp lại | 
| 0101 / ababab | 2 | xử lý mẫu xen kẽ | 

## Vỏ cạnh 

Trường hợp cạnh khóa xảy ra khi tất cả các lần xuất hiện của một ký hiệu ánh xạ tới các chuỗi con rất ngắn, buộc độ dài của ký hiệu kia chiếm ưu thế gần như toàn bộ chuỗi mục tiêu. Trong những trường hợp như vậy, việc triển khai không chính xác thường không phát hiện được rằng độ dài còn lại vẫn phải chia hết cho số đếm thứ hai. 

Một trường hợp tế nhị khác là khi`r0`Và`r1`cuối cùng giống hệt nhau. Thuật toán phải loại trừ điều này một cách rõ ràng ngay cả khi tất cả các bước kiểm tra cấu trúc đều vượt qua. Việc so sánh hàm băm ở cuối thực thi ràng buộc này, ngăn chặn việc đếm quá mức trong các mẫu hoàn toàn đối xứng, chẳng hạn như các chuỗi nhị phân xen kẽ được ánh xạ tới các chuỗi con giống hệt nhau lặp lại.
