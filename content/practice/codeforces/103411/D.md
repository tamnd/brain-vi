---
title: "CF 103411D - \u0414\u041d\u041a-\u043f\u0430\u043b\u0438\u043d\u0434\u0440\u043e\u043c"
description: "Chúng ta có một chuỗi trên bảng chữ cái {A, C, G, T} đại diện cho chuỗi DNA. Mỗi ký tự có một phần bổ sung cố định: A cặp với T và C cặp với G."
date: "2026-07-03T10:56:38+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103411
codeforces_index: "D"
codeforces_contest_name: "2020-2021, ICPC, East Siberian Regional Contest"
rating: 0
weight: 103411
solve_time_s: 44
verified: true
draft: false
---

[CF 103411D - \u0414\u041d\u041a-\u043f\u0430\u043b\u0438\u043d\u0434\u0440\u043e\u043c](https://codeforces.com/problemset/problem/103411/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 44s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một chuỗi trên bảng chữ cái {A, C, G, T} đại diện cho chuỗi DNA. Mỗi ký tự có một phần bù cố định: A cặp với T và C cặp với G. Nếu chúng ta lấy một chuỗi DNA, đảo ngược nó và thay thế mọi ký tự bằng phần bù của nó, chúng ta thu được cái mà câu lệnh gọi là chuỗi bù ngược. 

Nhiệm vụ là kiểm tra xem chuỗi gốc có giống với phần bổ sung ngược của nó hay không. Nói cách khác, với mọi vị trí i từ bên trái, ký tự tại i phải khớp với phần bù của ký tự ở vị trí n − 1 − i. 

Kích thước đầu vào có thể lên tới 10^6 ký tự, điều này ngay lập tức loại trừ mọi thứ bậc hai. Bất kỳ giải pháp nào so sánh trực tiếp tất cả các cặp vị trí vẫn ổn nếu nó là tuyến tính, nhưng bất kỳ giải pháp nào thực hiện việc xây dựng chuỗi lặp lại hoặc đảo ngược lặp lại bên trong các vòng lặp sẽ vượt quá giới hạn thời gian. 

Một trường hợp thất bại phổ biến là vô tình chỉ kiểm tra tính palindromicity theo nghĩa thông thường mà không áp dụng quy tắc bổ sung. Ví dụ: chuỗi "ATAT" hợp lệ vì A ↔ T và việc đảo ngược sẽ bảo toàn mẫu sau khi ánh xạ, nhưng "AAAA" không thành công ngay cả khi đó là một bảng màu bình thường. Một lỗi nhỏ khác là xây dựng chuỗi bù ngược một cách rõ ràng; đây vẫn là O(n) nhưng có thể gây ra sự sao chép bộ nhớ hoặc chi phí không cần thiết nếu thực hiện bất cẩn trong một vòng lặp chặt chẽ. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là xây dựng chuỗi bù ngược một cách rõ ràng và so sánh nó với chuỗi gốc. Chúng tôi sẽ đảo ngược chuỗi, ánh xạ từng ký tự thông qua hàm bổ sung và sau đó kiểm tra sự bằng nhau. Điều này đúng và chạy theo thời gian tuyến tính, nhưng nó thực hiện phân bổ bộ nhớ bổ sung cho chuỗi được chuyển đổi và thực hiện việc xây dựng đầy đủ không cần thiết khi có thể phát hiện sớm sự không khớp. 

Chúng ta có thể cải thiện điều này bằng cách nhận thấy rằng chúng ta không bao giờ thực sự cần chuỗi chuyển đổi đầy đủ. Điều kiện hoàn toàn là theo cặp: vị trí i phải khớp với vị trí n − 1 − i sau khi bổ sung một bên. Điều này có nghĩa là chúng tôi có thể quét vào trong từ cả hai đầu và xác minh tình trạng một cách nhanh chóng. 

Thông tin chi tiết về cấu trúc quan trọng là vấn đề giảm xuống thành ràng buộc đối xứng trên các cặp chỉ số, vì vậy chúng ta chỉ cần thêm không gian O(1) và một lần truyền qua một nửa chuỗi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Xây dựng chuỗi bổ sung ngược | O(n) | O(n) | Đã chấp nhận | 
| So sánh hai con trỏ | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng ta xử lý chuỗi bằng hai con trỏ, một con trỏ bắt đầu ở đầu bên trái và một con trỏ ở đầu bên phải. Ở mỗi bước, chúng tôi kiểm tra xem ký tự bên trái có bằng phần bù của ký tự bên phải hay không. 

1. Khởi tạo hai chỉ số l = 0 và r = n − 1. Chúng biểu thị các vị trí đối xứng trong chuỗi phải thỏa mãn điều kiện bù ngược. 
2. Khi l ≤ r, so sánh s[l] với phần bù của s[r]. Nếu chúng khác nhau, chúng ta có thể kết luận ngay rằng chuỗi không phải là chuỗi DNA palindrome vì điều kiện xác định đã bị vi phạm ở cặp đối xứng này. 
3. Nếu chúng khớp nhau, hãy di chuyển cả hai con trỏ vào trong bằng cách đặt l += 1 và r -= 1. Điều này bảo toàn tính bất biến rằng tất cả các cặp đối xứng đã kiểm tra trước đó đều thỏa mãn điều kiện. 
4. Nếu tất cả các cặp đều hợp lệ cho đến khi các con trỏ giao nhau, thì chuỗi thỏa mãn tính đối xứng bù nghịch và chúng ta xuất ra CÓ. 

Ý tưởng quan trọng là mỗi ký tự tham gia vào chính xác một ràng buộc với vị trí được phản chiếu của nó, do đó không cần phải quay lui hoặc cấu trúc toàn cục. 

### Tại sao nó hoạt động

Tại bất kỳ thời điểm nào, thuật toán buộc tất cả các cặp được xử lý (i, n − 1 − i) thỏa mãn s[i] = phần bù(s[n − 1 − i]). Vì mỗi chỉ mục tham gia vào đúng một cặp như vậy nên việc bao gồm tất cả các cặp đảm bảo tính chính xác. Việc chấm dứt sớm khi không khớp là hợp lệ vì một ràng buộc bị vi phạm sẽ làm mất hiệu lực toàn bộ điều kiện. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

comp = {
    'A': 'T',
    'T': 'A',
    'C': 'G',
    'G': 'C'
}

n = int(input())
s = input().strip()

l, r = 0, n - 1

while l <= r:
    if comp[s[r]] != s[l]:
        print("NO")
        sys.exit(0)
    l += 1
    r -= 1

print("YES")
```Ánh xạ phần bù được tính toán trước trong từ điển để tra cứu O(1). Vòng lặp hai con trỏ kiểm tra các vị trí đối xứng mà không cần xây dựng bất kỳ chuỗi bổ sung nào. Việc chấm dứt thông qua`sys.exit(0)`đảm bảo chúng tôi dừng ngay sau khi phát hiện vi phạm, điều này là tối ưu trong trường hợp xấu nhất khi sự không phù hợp xảy ra sớm. 

## Ví dụ đã hoạt động 

### Ví dụ 1: ATAT 

đầu vào:```
n = 4
s = ATAT
```Chúng tôi theo dõi chuyển động của con trỏ: 

| tôi | r | s[l] | s[r] | phần bù(s[r]) | trận đấu | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 3 | A | T | A | vâng | 
| 1 | 2 | T | A | T | vâng | 

Sau các bước này, con trỏ sẽ giao nhau và tất cả các lần kiểm tra đều thành công. 

Điều này xác nhận rằng điều kiện đối xứng được thỏa mãn ở cả hai vị trí đối xứng. 

### Ví dụ 2: AAA 

đầu vào:```
n = 3
s = AAA
```| tôi | r | s[l] | s[r] | phần bù(s[r]) | trận đấu | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 2 | A | A | T | không | 

Ở lần so sánh đầu tiên, A không bằng T nên thuật toán dừng ngay và cho ra NO. 

Điều này chứng tỏ rằng ngay cả các dây palindromic theo nghĩa thông thường cũng thất bại nếu chúng vi phạm tính đối xứng bù. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi nhân vật được truy cập nhiều nhất một lần từ một trong hai đầu | 
| Không gian | O(1) | Chỉ sử dụng ánh xạ và con trỏ cố định | 

Quét tuyến tính vừa vặn thoải mái trong giới hạn tối đa 10^6 ký tự. Việc sử dụng bộ nhớ không đổi ngoài chính bộ lưu trữ đầu vào, điều này là không thể tránh khỏi. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    comp = {'A':'T','T':'A','C':'G','G':'C'}

    n = int(input())
    s = input().strip()

    l, r = 0, n - 1
    while l <= r:
        if comp[s[r]] != s[l]:
            return "NO"
        l += 1
        r -= 1
    return "YES"

# provided samples
assert run("4\nATAT\n") == "YES"
assert run("3\nAAA\n") == "NO"

# custom cases
assert run("1\nA\n") == "NO", "single char must mismatch unless self-complementary (none here)"
assert run("2\nAT\n") == "YES", "perfect complement pair"
assert run("6\nACGTAC\n") == "NO", "mixed structure breaks symmetry"
assert run("6\nACGTAC\n") == "NO"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 A | KHÔNG | trường hợp cạnh một ký tự | 
| 2 AT | YES | minimal valid pair |
 | 6 ACGTAC | KHÔNG | cấu trúc không palindromic | 

## Vỏ cạnh 

Đối với một ký tự đơn như "A", thuật toán so sánh s[0] với phần bù (s[0]). Vì phần bù (A) là T nên việc kiểm tra thất bại ngay lập tức, trả về NO một cách chính xác. Điều này cho thấy các chuỗi có độ dài 1 không bao giờ hợp lệ theo định nghĩa này. 

Đối với các chuỗi có độ dài chẵn như "ATAT", mỗi cặp được kiểm tra chính xác một lần và tất cả các ràng buộc đều được thỏa mãn, do đó, thuật toán tiếp tục mà không cần thoát sớm và trả về CÓ. 

Đối với các cặp đối xứng không khớp xuất hiện sớm trong chuỗi, chẳng hạn như "AAA", thuật toán sẽ chấm dứt ngay lập tức ở lần so sánh đầu tiên. Điều này xác nhận rằng việc thoát sớm không ảnh hưởng đến tính chính xác vì một ràng buộc bị vi phạm duy nhất cũng đủ để loại chuỗi.
