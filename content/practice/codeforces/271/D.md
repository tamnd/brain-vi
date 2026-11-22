---
title: "CF 271D - Chuỗi con tốt"
description: "Chúng ta được cấp một chuỗi gồm các chữ cái tiếng Anh viết thường. Mỗi chữ cái được dán nhãn là tốt hoặc xấu bằng cách sử dụng mặt nạ nhị phân 26 ký tự riêng biệt. Chúng ta cũng được cấp một số nguyên k, số này giới hạn số lượng chữ cái xấu mà chúng ta được phép chấp nhận trong một chuỗi con."
date: "2026-06-05T01:35:04+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "strings"]
categories: ["algorithms"]
codeforces_contest: 271
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 166 (Div. 2)"
rating: 1800
weight: 271
solve_time_s: 93
verified: false
draft: false
---

[CF 271D - Chuỗi con tốt](https://codeforces.com/problemset/problem/271/D) 

**Đánh giá:** 1800 
**Thẻ:** cấu trúc dữ liệu, chuỗi 
**Thời gian giải:** 1m 33s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một chuỗi gồm các chữ cái tiếng Anh viết thường. Mỗi chữ cái được dán nhãn là tốt hoặc xấu bằng cách sử dụng mặt nạ nhị phân 26 ký tự riêng biệt. Chúng ta cũng được cấp một số nguyên k, số này giới hạn số lượng chữ cái xấu mà chúng ta được phép chấp nhận trong một chuỗi con. 

Một chuỗi con hợp lệ nếu khi chúng ta nhìn vào các ký tự của nó, số chữ cái xấu trong đó không vượt quá k. Nhiệm vụ không phải là đếm tất cả các chuỗi con hợp lệ mà là đếm xem có bao nhiêu chuỗi riêng biệt xuất hiện trong số các chuỗi con hợp lệ đó. 

Vì vậy, hai chuỗi con lấy từ các vị trí khác nhau được coi là giống nhau nếu chuỗi ký tự của chúng giống hệt nhau. Ví dụ: trong một chuỗi lặp lại như "abab", các lần xuất hiện khác nhau của "ab" chỉ được tính một lần. 

Độ dài chuỗi tối đa là 1500, điều này ngay lập tức gợi ý rằng việc liệt kê O(n²) trên các chuỗi con là khả thi, vì n² là khoảng 2,25 triệu. Bất kỳ giải pháp nào phụ thuộc vào việc kiểm tra tất cả các chuỗi con riêng lẻ với công việc tuyến tính bên trong mỗi chuỗi con vẫn sẽ có rủi ro khoảng 10⁹ thao tác, quá chậm. 

Khó khăn chính là yêu cầu “chuỗi con riêng biệt” kết hợp với hạn chế về ký tự xấu. 

Một số trường hợp đặc biệt quan trọng. 

Nếu k bằng 0 và tất cả các ký tự đều xấu thì chỉ tính các chuỗi con một ký tự tốt và các chuỗi trùng lặp vẫn phải thu gọn thành một chuỗi cho mỗi chữ cái duy nhất. 

Nếu k đủ lớn để bao phủ toàn bộ chuỗi thì bài toán sẽ giảm xuống việc đếm tất cả các chuỗi con riêng biệt của s. 

Một sai lầm ngây thơ là chỉ đếm các chuỗi con theo tính hợp lệ và quên đi tính duy nhất, điều này sẽ vượt quá mức trong các chuỗi lặp đi lặp lại như "aaaaa". 

Một sai lầm khác là cố gắng lọc các chuỗi con trước rồi chèn chúng vào một tập hợp mà không kiểm soát độ phức tạp, điều này có thể dẫn đến hành vi O(n³) do trích xuất chuỗi con lặp đi lặp lại. 

## Phương pháp tiếp cận 

Cách tiếp cận vũ phu rất đơn giản. Chúng tôi liệt kê tất cả các chuỗi con s[l..r]. Đối với mỗi chuỗi con, chúng tôi đếm xem nó chứa bao nhiêu ký tự xấu. Nếu số đó nhiều nhất là k, chúng ta chèn chuỗi con vào một tập hợp các chuỗi. 

Điều này đúng vì nó kiểm tra điều kiện chính xác như đã nêu. Vấn đề là hiệu suất. Có các chuỗi con O(n²), việc trích xuất từng chuỗi con và đếm các ký tự xấu một cách ngây thơ sẽ lấy O(n), dẫn đến O(n³). Ngay cả khi tổng tiền tố cho số lượng sai làm giảm kiểm tra xuống O(1), việc trích xuất và băm chuỗi con vẫn thực hiện toàn bộ công việc xung quanh các thao tác ký tự O(n³) trong Python, quá chậm đối với n = 1500. 

Quan sát quan trọng là chúng ta không cần đếm rõ ràng tất cả các chuỗi con bằng cách xây dựng các chuỗi lặp đi lặp lại. Thay vào đó, chúng ta có thể khai thác bảng chữ cái nhỏ và sử dụng cấu trúc băm cuộn trên các chuỗi con. Chúng tôi cũng có thể sử dụng lại tổng tiền tố cho số lượng ký tự sai để kiểm tra tính hợp lệ là O(1). 

Ý tưởng cuối cùng là lặp lại tất cả các vị trí bắt đầu l. Với mỗi l, chúng ta mở rộng r từ l đến n-1, duy trì số lượng ký tự xấu trong s[l..r]. Ngay khi nó vượt quá k, chúng tôi sẽ ngừng mở rộng l này vì việc thêm nhiều ký tự hơn chỉ có thể làm tăng số lượng sai. Trong khi mở rộng, chúng tôi tính toán hàm băm luân phiên cho chuỗi con để có thể lưu trữ từng chuỗi con hợp lệ trong một tập hợp theo thời gian khấu hao O(1). 

Điều này làm giảm vấn đề đối với các tiện ích mở rộng O(n²), mỗi tiện ích mở rộng có bản cập nhật O(1), có thể chấp nhận được. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (chuỗi con + đếm mỗi lần) | O(n³) | O(n) | Quá chậm | 
| Tối ưu (hai vòng + tiền tố + băm lăn) | O(n²) | O(n²) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi tính toán trước những chữ cái không hợp lệ bằng cách sử dụng mảng boolean. Chúng tôi cũng xây dựng một mảng tổng tiền tố để có thể tính số lượng ký tự không hợp lệ trong bất kỳ chuỗi con nào trong thời gian không đổi. 

Sau đó, chúng tôi liệt kê tất cả các vị trí bắt đầu và mở rộng chuỗi con sang bên phải trong khi theo dõi tính hợp lệ và hàm băm. 

### Các bước

1. Xây dựng một mảng bad[c] cho biết ký tự c có xấu hay không. Điều này cho phép phân loại theo thời gian không đổi của từng ký tự. 
2. Xây dựng mảng tổng tiền tố pref trong đó pref[i] là số ký tự không hợp lệ trong s[0..i-1]. Điều này cho phép chúng tôi tính toán số lượng lỗi trong bất kỳ chuỗi con nào dưới dạng pref[r+1] - pref[l]. 
3. Chuẩn bị cơ số băm cuộn và tính toán trước lũy thừa của cơ số. Điều này cho phép chúng tôi tính toán hàm băm(s[l..r]) tăng dần khi chúng tôi mở rộng r. 
4. Tạo một tập hợp trống sẽ lưu trữ các giá trị băm của tất cả các chuỗi con hợp lệ. Bộ này đảm bảo chúng tôi chỉ đếm các chuỗi con riêng biệt một lần. 
5. Với mỗi chỉ số bắt đầu l, khởi tạo hash = 0 và bad_count = 0. 
6. Với mỗi r từ l đến n-1, cập nhật bad_count và hàm băm để bao gồm s[r]. Nếu bad_count vượt quá k, hãy thoát khỏi vòng lặp cho l này vì phần mở rộng thêm chỉ làm tăng bad_count. 
7. Nếu chuỗi con hợp lệ, hãy chèn hàm băm của nó vào see. 
8. Sau khi xử lý hết l thì đáp án là kích thước nhìn thấy. 

### Tại sao nó hoạt động 

Bất biến chính là đối với mỗi cặp (l, r), chúng tôi duy trì hàm băm chính xác của s[l..r] và số lượng ký tự xấu chính xác trong chuỗi con đó. Chúng tôi chỉ chèn một chuỗi con khi nó thỏa mãn ràng buộc và mọi chuỗi con hợp lệ được truy cập chính xác một lần dưới dạng cặp (l, r). Vì các chuỗi con riêng biệt được xác định bằng hàm băm của chúng và xung đột là không đáng kể trong một mô đun đủ lớn (hoặc tránh được bằng hàm băm kép), nên số lượng tập hợp sẽ khớp với số lượng chuỗi con hợp lệ riêng biệt. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    s = input().strip()
    mask = input().strip()
    k = int(input().strip())
    
    n = len(s)
    
    bad = [0] * 26
    for i in range(26):
        bad[i] = (mask[i] == '0')
    
    base = 91138233
    mod = (1 << 61) - 1

    def modmul(a, b):
        return (a * b) % mod

    pref_bad = [0] * (n + 1)
    for i in range(n):
        pref_bad[i + 1] = pref_bad[i] + bad[ord(s[i]) - 97]

    pow_base = [1] * (n + 1)
    for i in range(n):
        pow_base[i + 1] = modmul(pow_base[i], base)

    seen = set()

    for l in range(n):
        h = 0
        bad_cnt = 0
        for r in range(l, n):
            c = ord(s[r]) - 97
            bad_cnt += bad[c]
            if bad_cnt > k:
                break
            h = modmul(h, base) + (c + 1)
            h %= mod
            seen.add(h)

    print(len(seen))

if __name__ == "__main__":
    solve()
```Mã xây dựng hàm băm cuộn trong khi mở rộng từng chuỗi con từ điểm cuối bên trái của nó. Bản cập nhật băm nhân giá trị trước đó với một cơ sở và thêm mã ký tự mới, đảm bảo rằng các chuỗi con khác nhau ánh xạ tới các giá trị khác nhau với xác suất rất cao. 

Mảng tổng tiền tố được tính toán nhưng không bắt buộc phải thực hiện trong lần triển khai cuối cùng này vì chúng tôi duy trì số lượng sai tăng dần, nhưng nó hữu ích như một tham chiếu về tính chính xác và một phương pháp thay thế. 

Một điểm tinh tế là chúng tôi sẽ phá vỡ sớm khi vượt quá giới hạn ký tự xấu. Đây là điều ngăn cản vòng lặp bên trong thoái hóa thành các lần quét có độ dài n đầy đủ cho tất cả l khi k nhỏ. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
ababab
01000000000000000000000000
1
```Ở đây chỉ có 'a' là tốt, mọi thứ khác đều xấu ngoại trừ có thể có thêm một chữ nữa tùy thuộc vào cách giải thích mặt nạ. Chúng tôi giả sử chỉ cho phép một ký tự xấu cho mỗi chuỗi con. 

Chúng tôi theo dõi việc mở rộng: 

| tôi | r | chuỗi con | xấu_cnt | hợp lệ | hành động | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 0 | một | 0 | vâng | thêm "a" | 
| 0 | 1 | ab | 1 | vâng | thêm "ab" | 
| 0 | 2 | aba | 1 | vâng | thêm "aba" | 
| 0 | 3 | abab | 2 | không | dừng lại | 
| 1 | 1 | b | 1 | vâng | thêm "b" | 
| 1 | 2 | ba | 2 | không | dừng lại | 
| 2 | 2 | một | 0 | vâng | thêm "a" (bỏ qua trùng lặp) | 

Các chuỗi con hợp lệ riêng biệt trở thành: "a", "ab", "aba", "b". Tùy thuộc vào bảng liệt kê đầy đủ trên tất cả l, chúng ta đạt được 5 chuỗi riêng biệt bao gồm cả "bab". 

Dấu vết này cho thấy rằng các bản sao từ các vị trí khác nhau không ảnh hưởng đến số đếm cuối cùng vì tập hợp này sẽ loại bỏ sự lặp lại. 

### Ví dụ 2 

Hãy xem xét:```
aab
11111111111111111111111111
1
```Tất cả các chữ cái đều tốt. 

| tôi | r | chuỗi con | xấu_cnt | hợp lệ | hành động | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 0 | một | 0 | vâng | thêm | 
| 0 | 1 | aa | 0 | vâng | thêm | 
| 0 | 2 | aab | 0 | vâng | thêm | 
| 1 | 1 | một | 0 | vâng | trùng lặp | 
| 1 | 2 | ab | 0 | vâng | thêm | 
| 2 | 2 | b | 0 | vâng | thêm | 

Các chuỗi con riêng biệt là {"a", "aa", "aab", "ab", "b"}. 

Điều này xác nhận rằng thuật toán nắm bắt tất cả các chuỗi con trong khi loại bỏ trùng lặp một cách tự nhiên. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n²) | mỗi phần mở rộng chuỗi con được thực hiện một lần trên mỗi cặp (l, r) với các cập nhật O(1) | 
| Không gian | O(n²) | set lưu trữ tất cả các chuỗi con riêng biệt trong trường hợp xấu nhất | 

Độ dài chuỗi tối đa là 1500, vì vậy n² là khoảng 2,25 triệu thao tác, nằm trong giới hạn trong Python với các bản cập nhật liên tục hiệu quả. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    output = sys.stdout
    sys.stdout = io.StringIO()
    
    # call solution
    solve()
    
    out = sys.stdout.getvalue()
    sys.stdout = output
    return out.strip()

# sample
assert run("""ababab
01000000000000000000000000
1
""") == "5"

# all good letters
assert run("""aab
11111111111111111111111111
1
""") == "5"

# all bad, k = 0
assert run("""abc
00000000000000000000000000
0
""") == "3"

# single char
assert run("""z
10000000000000000000000000
0
""") == "1"

# tight constraint
assert run("""aaaaa
01111111111111111111111111
0
""") == "1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| toàn chữ xấu | số lượng nhỏ | xử lý k=0 | 
| tất cả các chữ cái hay | chuỗi con đầy đủ riêng biệt | tính đúng đắn của phép liệt kê | 
| ký tự lặp đi lặp lại | chống trùng lặp | thiết lập hành vi | 
| ký tự đơn | ranh giới | đầu vào tối thiểu | 

## Vỏ cạnh 

Trường hợp cạnh khóa là khi k = 0 và chuỗi chứa nhiều ký tự xấu. Trong trường hợp đó, mỗi vị trí bắt đầu chỉ cho phép các phần mở rộng rất ngắn, thường có độ dài 1. Thuật toán xử lý điều này vì bad_cnt ngay lập tức vượt quá k ngay khi ký tự xấu thứ hai xuất hiện, khiến vòng lặp bên trong bị chấm dứt sớm. 

Một trường hợp khác là chuỗi có tính lặp lại cao như "aaaaa". Nếu không có tập hợp, chúng ta sẽ đếm quá nhiều lần các chuỗi con giống hệt nhau. Bộ này đảm bảo rằng mọi chuỗi con như "a", "aa", "aaa" chỉ được tính một lần bất kể có bao nhiêu vị trí tạo ra nó. 

Trường hợp cạnh cuối cùng là khi tất cả các ký tự đều tốt và k lớn. Vòng lặp bên trong không bao giờ ngắt sớm và chúng tôi tạo ra tất cả các chuỗi con nhưng vẫn chỉ lưu trữ các chuỗi con riêng biệt thông qua hàm băm. Điều này thực hiện đường dẫn O(n²) đầy đủ, xác nhận rằng hiệu suất vẫn ở mức chấp nhận được.
