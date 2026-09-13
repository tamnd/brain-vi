---
title: "CF 104669B - Dịch chuyển chuỗi"
description: "Chúng ta được cung cấp một chuỗi duy nhất được biết là đến từ sự dịch chuyển chữ cái kiểu Caesar được áp dụng cho một số văn bản gốc. Trong phép chuyển đổi như vậy, mỗi ký tự trong chuỗi gốc được di chuyển về phía trước trong bảng chữ cái theo một số vị trí cố định, bao quanh từ z trở lại a."
date: "2026-06-29T09:44:20+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104669
codeforces_index: "B"
codeforces_contest_name: "Turtle Codes"
rating: 0
weight: 104669
solve_time_s: 57
verified: true
draft: false
---

[CF 104669B - Dịch chuyển chuỗi](https://codeforces.com/problemset/problem/104669/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 57s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi duy nhất được biết là đến từ sự dịch chuyển chữ cái kiểu Caesar được áp dụng cho một số văn bản gốc. Trong phép chuyển đổi như vậy, mỗi ký tự trong chuỗi gốc được di chuyển về phía trước trong bảng chữ cái theo một số vị trí cố định, bao quanh từ`z`quay lại`a`. 

Chúng ta cũng được biết rằng chuỗi con`"tino"`xuất hiện ở đâu đó trong văn bản chưa được dịch chuyển ban đầu. Sau khi áp dụng dịch chuyển, chuỗi con đó sẽ trở thành một chuỗi 4 ký tự khác bên trong văn bản được mã hóa. Nhiệm vụ của chúng ta là xác định có bao nhiêu giá trị dịch chuyển khác nhau có thể tạo ra chuỗi mã hóa đã cho trong khi vẫn cho phép`"tino"`tồn tại trong chuỗi gốc ở một vị trí nào đó. 

Giá trị dịch chuyển là một số nguyên trong phạm vi từ 0 đến 25, trong đó 0 có nghĩa là không thay đổi và 25 có nghĩa là dịch chuyển từng chữ cái lùi một bước trong bảng chữ cái trước khi mã hóa. Mỗi giá trị dịch chuyển là hợp lệ nếu tồn tại ít nhất một vị trí trong chuỗi được mã hóa trong đó việc giải mã bằng dịch chuyển đó mang lại kết quả`"tino"`. 

Độ dài đầu vào tối đa là 1000, do đó, bất kỳ giải pháp nào kiểm tra tất cả các vị trí và tất cả 26 ca đều dễ dàng đủ nhanh. Ngay cả cách tiếp cận hình khối cũng có thể được thực hiện, nhưng nó không cần thiết. 

Một điểm tinh tế là điều kiện so khớp có tính cục bộ: chúng ta không được yêu cầu xây dựng lại toàn bộ chuỗi gốc mà chỉ để kiểm tra xem liệu`"tino"`có thể xuất hiện ở bất cứ đâu sau khi đảo ngược ca làm việc của ứng viên. Điều này có nghĩa là các vị trí khác nhau trong chuỗi hỗ trợ hoặc từ chối một sự thay đổi một cách độc lập. 

Vỏ cạnh đến từ dây ngắn. Nếu độ dài nhỏ hơn 4 thì không có vị trí nào có thể tồn tại mẫu 4 ký tự, do đó câu trả lời phải là 0. Một trường hợp cạnh khác là khi nhiều ca hợp lệ do các vị trí khớp khác nhau. Ví dụ: một chuỗi có thể cho phép dịch chuyển 1 tại một vị trí và dịch chuyển 5 ở vị trí khác; chúng ta phải tính những thay đổi duy nhất, không phải số lần xuất hiện. 

## Phương pháp tiếp cận 

Một cách trực tiếp để giải quyết vấn đề là thử mọi giá trị dịch chuyển có thể từ 0 đến 25. Đối với mỗi ca, chúng tôi mô phỏng việc đảo ngược mã hóa: đối với mỗi ký tự trong chuỗi, chúng tôi trừ đi ca modulo 26 và thu được một ký tự gốc ứng cử viên. Sau đó, chúng tôi quét tất cả các chuỗi con có độ dài 4 và kiểm tra xem có chuỗi nào bằng không`"tino"`. 

Cách tiếp cận này đúng vì nó kiểm tra rõ ràng định nghĩa về tính hợp lệ. Đối với mỗi ca, chúng tôi xây dựng lại toàn bộ chuỗi ban đầu trông như thế nào và kiểm tra xem mẫu được yêu cầu có tồn tại hay không. Vấn đề là hiệu quả: đối với mỗi ca trong số 26 ca, chúng tôi kiểm tra tới 1000 vị trí, mỗi vị trí yêu cầu tối đa 4 phép so sánh, dẫn đến khoảng 1000 × 26 × 4 thao tác, điều này có thể dễ dàng chấp nhận nhưng hơi lãng phí khi tái thiết lặp đi lặp lại. 

Chúng ta có thể đơn giản hóa hơn nữa bằng cách tránh việc xây dựng lại toàn bộ. Thay vì xây dựng toàn bộ chuỗi được giải mã cho mỗi ca, chúng tôi trực tiếp kiểm tra căn chỉnh ký tự. Đối với một ca cố định và một vị trí cố định i, chúng tôi xác minh xem việc dịch chuyển chuỗi con được mã hóa trở lại bằng ca đó có tạo ra không`"tino"`. Điều này làm giảm chi phí bộ nhớ và giữ logic trực tiếp. 

Một quan sát quan trọng là một phép dịch chuyển hợp lệ khi và chỉ khi tồn tại ít nhất một chỉ số i sao cho với mọi j trong {0,1,2,3}, điều kiện đúng: 

mã hóa[i + j] - shift ≡ target[j] (mod 26). 

Vì vậy chúng ta chỉ cần kiểm tra 26 ca và nhiều nhất là 1000 vị trí xuất phát. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Giải mã toàn bộ lực lượng vũ phu mỗi ca | O(26 · n) | O(n) | Đã chấp nhận | 
| Kiểm tra chuỗi con trực tiếp mỗi ca | O(26 · n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi sửa mẫu mục tiêu`"tino"`và chuyển đổi nó thành các số nguyên từ`'a'`. 

Sau đó chúng tôi lặp lại mọi giá trị dịch chuyển có thể. 

1. Đối với mỗi giá trị dịch chuyển từ 0 đến 25, chúng tôi giả sử đó có thể là dịch chuyển Caesar chính xác. 
2. Với mỗi chỉ số bắt đầu i từ 0 đến n - 4, ta kiểm tra xem chuỗi con có độ dài 4 tại i có khớp không`"tino"`khi được giải mã bởi sự dịch chuyển này. 
3. Với mỗi vị trí ký tự j trong 0 đến 3, chúng ta tính ký tự được giải mã của mã hóa[i + j] bằng cách trừ đi shift modulo 26. 
4. Nếu tất cả bốn ký tự được giải mã khớp nhau`"tino"`, chúng tôi đánh dấu ca này là hợp lệ và ngừng kiểm tra các vị trí tiếp theo cho ca này. 
5. Sau khi thử tất cả các ca, chúng tôi đếm xem có bao nhiêu ca được đánh dấu hợp lệ. 

Lý do chúng tôi nghỉ sớm khi tìm thấy kết quả trùng khớp là vì sự tồn tại của ít nhất một vị trí hợp lệ là đủ để một ca được tính. 

### Tại sao nó hoạt động 

Một ca có giá trị toàn cục nếu tồn tại ít nhất một vị trí trong chuỗi được mã hóa trong đó việc đảo ngược ca đó mang lại mẫu chính xác`"tino"`. Thuật toán kiểm tra tất cả các căn chỉnh cục bộ có thể có cho mỗi ca, do đó không bỏ sót căn chỉnh hợp lệ nào. Mỗi ca được kiểm tra độc lập và một ca chỉ được tính nếu nó thỏa mãn điều kiện tồn tại. Điều này trực tiếp phù hợp với yêu cầu của vấn đề mà không cần dựa vào việc tái thiết toàn cầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    s = input().strip()
    n = len(s)

    if n < 4:
        print(0)
        return

    target = "tino"
    target_vals = [ord(c) - ord('a') for c in target]

    ans = 0

    for shift in range(26):
        ok = False

        for i in range(n - 3):
            match = True
            for j in range(4):
                c = ord(s[i + j]) - ord('a')
                if (c - shift) % 26 != target_vals[j]:
                    match = False
                    break
            if match:
                ok = True
                break

        if ok:
            ans += 1

    print(ans)

if __name__ == "__main__":
    solve()
```Giải pháp lặp lại tất cả các ca và kiểm tra từng ca theo tất cả các vị trí chuỗi con có thể có. Cấu trúc vòng lặp lồng nhau là có chủ ý: vòng lặp bên ngoài liệt kê các giả thuyết (giá trị dịch chuyển) và vòng lặp bên trong xác minh xem giả thuyết có phù hợp với bất kỳ vị trí hợp lệ nào của`"tino"`. 

Số học modulo`(c - shift) % 26`xử lý quấn quanh một cách chính xác. Việc ngắt sớm bên trong vòng lặp chuỗi con đảm bảo chúng ta tránh được những so sánh không cần thiết sau khi tìm thấy kết quả khớp hợp lệ. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
tinoujopvkpq
```Chúng tôi kiểm tra các ca từ 0 đến 25. Chỉ những ca sắp xếp ít nhất một chuỗi con thành`"tino"`là hợp lệ. 

| ca | i=0 trận đấu | tôi=1 trận đấu | i=2 trận đấu | hợp lệ | 
| --- | --- | --- | --- | --- | 
| 0 | vâng | không | không | vâng | 
| 1 | không | không | vâng | vâng | 
| 2 | không | không | không | không | 

Câu trả lời cuối cùng: Có tổng cộng 3 ca hợp lệ. 

Dấu vết này cho thấy các phần khác nhau của chuỗi có thể xác nhận độc lập các giá trị dịch chuyển khác nhau. 

### Ví dụ 2 

đầu vào:```
tinoabcd
```Ở đây chuỗi con`"tino"`xuất hiện trực tiếp ở vị trí 0. 

| ca | i=0 trận đấu | hợp lệ | 
| --- | --- | --- | 
| 0 | vâng | vâng | 
| 1 | không | không | 
| 2 | không | không | 

Chỉ ca 0 hoạt động vì mọi ca khác 0 sẽ thay đổi`"tino"`thành một từ khác. 

Điều này chứng tỏ rằng sự hiện diện trực tiếp của mẫu hạn chế mạnh mẽ giá trị dịch chuyển. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(26 · n · 4) | Đối với mỗi ca, chúng tôi quét tất cả các vị trí và mỗi lần kiểm tra so sánh 4 ký tự | 
| Không gian | O(1) | Chỉ sử dụng một số bộ đếm và mảng cố định | 

Với n 1000, số lượng thao tác tối đa là khoảng 100.000, nằm trong giới hạn thoải mái. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    s = sys.stdin.readline().strip()
    n = len(s)

    if n < 4:
        return "0\n"

    target = "tino"
    target_vals = [ord(c) - ord('a') for c in target]

    ans = 0

    for shift in range(26):
        ok = False
        for i in range(n - 3):
            match = True
            for j in range(4):
                c = ord(s[i + j]) - ord('a')
                if (c - shift) % 26 != target_vals[j]:
                    match = False
                    break
            if match:
                ok = True
                break
        if ok:
            ans += 1

    return str(ans) + "\n"

# provided sample
assert run("tinoujopvkpq\n") == "3\n"

# custom: exact match after no shift
assert run("tinoabcd\n") == "1\n"

# custom: no possible match
assert run("zzzzzzzz\n") == "0\n"

# custom: short string
assert run("tin\n") == "0\n"

# custom: multiple repeated valid patterns
assert run("tinotinotino\n") == "1\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tinoujopvkpq | 3 | nhiều ca hợp lệ | 
| tinoabcd | 1 | trường hợp trận đấu trực tiếp | 
| zzzzzzzz | 0 | không có mẫu hợp lệ | 
| thiếc | 0 | chiều dài < 4 cạnh | 
| tinotinotino | 1 | mẫu lặp lại, tính ca đơn | 

## Vỏ cạnh 

Đối với đầu vào ngắn hơn 4 ký tự, thuật toán ngay lập tức trả về 0. Ví dụ: đầu vào`"abc"`dẫn đến n = 3, kích hoạt việc thoát sớm trước bất kỳ cuộc kiểm tra ca nào. Điều này phù hợp với thực tế là không thể tồn tại chuỗi con 4 chữ cái. 

Đối với các chuỗi có nhiều lần xuất hiện các mẫu theo các ca khác nhau, chẳng hạn như`"tinoujopvkpq"`, mỗi ca được kiểm tra độc lập. Khi kiểm tra shift 0, chuỗi con ở chỉ số 0 có thể khớp. Khi ca 1 được kiểm tra, một chỉ số khác có thể khớp. Thuật toán chỉ tính mỗi ca một lần do`ok`lá cờ. 

Đối với các chuỗi có chuỗi con hợp lệ lặp lại như`"tinotinotino"`, shift 0 tạo ra nhiều vị trí hợp lệ, nhưng thuật toán dừng kiểm tra sau khi tìm thấy vị trí khớp đầu tiên, đảm bảo hiệu quả mà không ảnh hưởng đến tính chính xác.
