---
title: "CF 1748A - Hình vuông tối thượng"
description: "Chúng ta được cho một dãy các khối gỗ hình chữ nhật. Khối thứ i luôn có chiều cao 1 và chiều dài của nó tăng chậm theo i theo quy tắc ⌈i/2⌉."
date: "2026-06-09T15:27:30+07:00"
tags: ["codeforces", "competitive-programming", "math"]
categories: ["algorithms"]
codeforces_contest: 1748
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 833 (Div. 2)"
rating: 800
weight: 1748
solve_time_s: 200
verified: true
draft: false
---

[CF 1748A - Hình vuông tối thượng](https://codeforces.com/problemset/problem/1748/A) 

**Đánh giá:** 800 
**Thẻ:** toán 
**Thời gian giải:** 3 phút 20s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một dãy các khối gỗ hình chữ nhật. Khối thứ i luôn có chiều cao 1 và chiều dài của nó tăng chậm theo i theo quy tắc ⌈i/2⌉. Vì vậy, các khối có các cặp có chiều rộng bằng nhau: chỉ số 1 và 2 đều có chiều rộng 1, chỉ số 3 và 4 đều có chiều rộng 2, chỉ số 5 và 6 đều có chiều rộng 3, v.v. 

Chúng ta được phép chọn bất kỳ tập hợp con nào của các khối này và đặt chúng mà không cần xoay. Mỗi khối đóng góp một dải ngang có chiều cao 1 và chiều rộng cố định. Mục tiêu là tập hợp các dải này thành một hình vuông đặc có kích thước s × s để có số nguyên s tối đa có thể. 

Hạn chế chính là các khối không thể xoay được, vì vậy mỗi khối đóng góp chính xác một hình chữ nhật 1 x w. 

Kích thước đầu vào lớn: n có thể lên tới 10^9 và có tới 10^4 trường hợp thử nghiệm. Điều này ngay lập tức loại trừ bất kỳ cách tiếp cận nào lặp lại trên tất cả các khối hoặc mô phỏng việc xây dựng. Mọi lời giải đều chỉ phụ thuộc vào n và sử dụng suy luận dạng đóng. 

Trường hợp cạnh tinh tế xuất hiện khi n nhỏ. Với n = 1 hoặc n = 2, tất cả các khối đều có chiều rộng 1, do đó chỉ có thể có hình vuông 1 × 1. Một cách tiếp cận ngây thơ giả định sự tăng trưởng chiều rộng bắt đầu ngay lập tức từ 2 sẽ đánh giá quá cao câu trả lời một cách không chính xác. Một cạm bẫy khác là giả định rằng chúng ta cần sử dụng tất cả các khối, trong khi trên thực tế, chúng ta chỉ chọn một tập hợp con phù hợp với ràng buộc đóng gói hình vuông. 

## Phương pháp tiếp cận 

Nếu chúng ta thử phương pháp xây dựng trực tiếp, chúng ta sẽ mô phỏng các dãy nhà của hình vuông. Mỗi hàng yêu cầu chiều rộng s và chúng tôi sẽ tham lam chọn các khối có sẵn có tổng chiều rộng ít nhất là s. Đối với mỗi ứng cử viên, điều này sẽ yêu cầu quét tất cả các khối hoặc duy trì nhiều chiều rộng. 

Ngay cả việc kiểm tra một s cũng tốn O(n) và vì s có thể lớn tới khoảng √n, điều này dẫn đến khoảng O(n√n) hoặc tệ hơn cho mỗi trường hợp thử nghiệm, điều này là không thể đối với n lên đến 10^9. 

Quan sát quan trọng là độ rộng khối có cấu trúc cực kỳ cao. Mỗi chiều rộng số nguyên w xuất hiện chính xác hai lần và tất cả các chiều rộng ≤ k xuất hiện chính xác 2k lần trong tiền tố của các khối. 

Vì vậy, thay vì suy nghĩ theo từng khối riêng lẻ, chúng tôi tổng hợp theo chiều rộng. Với một n nhất định, chúng ta có thể tính xem chúng ta có bao nhiêu nhóm có chiều rộng đầy đủ. Nếu k = ⌊n/2⌋ thì chúng tôi bao gồm đầy đủ chiều rộng từ 1 đến k, mỗi chiều đóng góp hai khối. Nếu n lẻ thì chúng ta có thêm một khối có chiều rộng k+1. 

Điều này biến vấn đề thành một vấn đề đếm thuần túy: chúng ta có thể hình thành một hình vuông lớn đến mức nào bằng cách sử dụng nhiều tập hợp trong đó mỗi chiều rộng w xuất hiện tối đa hai lần (ngoại trừ một phần có thể là chiều rộng cuối cùng)? 

Bây giờ, hiểu biết sâu sắc tiếp theo là hình học: để xây dựng một hình vuông có cạnh s, chúng ta cần chọn các dải có tổng chiều cao ít nhất là s (luôn đúng vì chúng ta có thể chọn hầu hết s hàng) và quan trọng hơn là chúng ta cần xếp từng hàng theo chiều dài s. Điều này giúp giảm việc kiểm tra tổng số “dải chiều cao đơn vị” ngang mà chúng tôi có thể phân bổ cho mỗi cấp độ rộng. 

Một cách đơn giản hơn để thấy nó là đảo ngược quan điểm: thay vì tập hợp một hình vuông, hãy xét số s tối đa có thể sao cho chúng ta có thể gán cho mỗi hàng có tổng chiều rộng ít nhất là s. Vì tất cả các hàng đều có yêu cầu giống nhau nên chúng tôi thực sự đang hỏi liệu tổng “đóng góp diện tích” có sẵn từ chiều rộng có thể hỗ trợ s × s hay không. 

Tổng diện tích đóng góp của tất cả các khối là: 

tổng_{i=1..n} ⌈i/2⌉. 

Tổng này có thể được tính ở dạng đóng bằng cách ghép các chỉ số. Với mỗi k, chúng ta nhận được hai khối có chiều rộng k đóng góp tổng diện tích 2k, cho đến khi có thể là cặp không hoàn chỉnh cuối cùng. 

Như vậy: 

Nếu n = 2m thì tổng diện tích = 2(1 + 2 + ... + m) = m(m+1) 

Nếu n = 2m + 1 thì tổng diện tích = m(m+1) + (m+1) = (m+1)^2 

Vậy tổng diện tích là m(m+1) hoặc (m+1)^2. Vì chúng ta muốn hình vuông lớn nhất s × s phù hợp với diện tích, nên chúng ta nghi ngờ s xấp xỉ bằng √(diện tích). Tuy nhiên, chỉ điều này thôi thì chưa đủ vì cấu trúc có thể hạn chế việc đóng gói.

Quan sát quan trọng cuối cùng là cấu trúc này chặt chẽ: chiều rộng không giảm và đủ cân bằng để mọi việc đóng gói khả thi chỉ phụ thuộc vào tổng diện tích và cạnh hình vuông tối ưu trở thành chính xác: 

s = ⌊√(tổng n chiều rộng đầu tiên)⌋. 

Việc tính toán này làm giảm toàn bộ vấn đề thành một công thức số học có thời gian không đổi cho mỗi trường hợp kiểm thử. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Xây dựng lực lượng vũ phu | O(n√n) | O(n) | Quá chậm | 
| Tiền tố Tổng + Công thức căn bậc hai | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

### Ý tưởng chính 

Chúng tôi tính tổng chiều rộng được đóng góp bởi n khối đầu tiên, sau đó lấy số nguyên lớn nhất s sao cho tổng s2. 

### bước 

1. Chia n thành các cặp chỉ số bằng cách tính m = n // 2. 

Nhóm này gồm các khối (1,2), (3,4), ..., mỗi cặp có cùng chiều rộng. 
2. Tính tổng đóng góp từ các cặp đầy đủ: m(m+1). 

Điều này xuất phát từ 2(1 + 2 + ... + m), vì mỗi chiều rộng k xuất hiện hai lần. 
3. Nếu n là số lẻ, hãy cộng phần đóng góp của khối cuối cùng có chiều rộng m+1. 
4. Gọi tổng là tổng được tính. 
5. Câu trả lời là số nguyên lớn nhất s sao cho s² ≤ tổng, tức là sàn(sqrt(tổng)). 

### Tại sao nó hoạt động 

Cấu trúc đơn điệu về chiều rộng, nghĩa là chiều rộng lớn hơn chỉ xuất hiện sau chiều rộng nhỏ hơn và luôn có đủ số bội. Điều này ngăn chặn các hạn chế đóng gói bệnh lý ở những nơi có diện tích nhưng không thể sắp xếp thành hình vuông. Vì mỗi mức chiều rộng đều đóng góp đầy đủ các dải 1 chiều cao nên hệ số giới hạn trở thành tổng diện tích sẵn có và không có ràng buộc tổ hợp bổ sung nào làm giảm tính khả thi. Do đó cạnh bình phương cực đại được xác định chính xác bởi diện tích giới hạn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
import math

t = int(input())
for _ in range(t):
    n = int(input())
    m = n // 2
    total = m * (m + 1)
    if n % 2 == 1:
        total += (m + 1)
    print(int(math.isqrt(total)))
```Việc thực hiện trực tiếp theo công thức dẫn xuất. Căn bậc hai số nguyên được sử dụng để tránh các vấn đề về độ chính xác của dấu phẩy động. Việc tính tổng được thực hiện trong O(1) bằng cách sử dụng các công thức chuỗi số học. 

Cần phải lưu ý rằng phép nhân trung gian m(m+1) có thể lớn, do đó các số nguyên chính xác tùy ý của Python là phù hợp ở đây. 

## Ví dụ đã hoạt động 

### Ví dụ 1: n = 5 

| Bước | Giá trị của m | Đóng góp theo cặp | Khối bổ sung | Tổng cộng | 
| --- | --- | --- | --- | --- | 
| Tính toán | 2 | 2(1+2)=6 | chiều rộng 3 | 9 | 

Bây giờ chúng ta tính s = ⌊√9⌋ = 3. 

Điều này phù hợp với cấu trúc trong đó các khối có chiều rộng 1,1,2,2,3 có thể tạo thành hình vuông 3 × 3. 

### Ví dụ 2: n = 2 

| Bước | Giá trị của m | Đóng góp theo cặp | Khối bổ sung | Tổng cộng | 
| --- | --- | --- | --- | --- | 
| Tính toán | 1 | 2(1)=2 | không | 2 | 

Bây giờ s = ⌊√2⌋ = 1. 

Chúng ta không thể tạo thành hình vuông 2 × 2 vì tổng chiều rộng có sẵn là không đủ nên kết quả là 1. 

Những ví dụ này cho thấy cạnh hình vuông bị chi phối hoàn toàn bởi tổng chiều rộng tổng hợp chứ không phải bởi sự sắp xếp khối riêng lẻ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) cho mỗi trường hợp thử nghiệm | Chỉ các phép tính số học và một căn bậc hai | 
| Không gian | O(1) | Không có công trình phụ trợ | 

Giải pháp dễ dàng xử lý 10^4 trường hợp thử nghiệm vì mỗi trường hợp đều hoạt động liên tục. 

## Trường hợp thử nghiệm```python
import sys, io, math

def solve(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    t = int(input())
    out = []
    for _ in range(t):
        n = int(input())
        m = n // 2
        total = m * (m + 1)
        if n % 2 == 1:
            total += (m + 1)
        out.append(str(math.isqrt(total)))
    return "\n".join(out)

# provided samples
assert solve("3\n2\n5\n197654321\n") == "1\n3\n98827161"

# custom cases
assert solve("1\n1\n") == "1", "minimum n"
assert solve("1\n2\n") == "1", "small even case"
assert solve("1\n6\n") == str(math.isqrt(3*4)), "even structured case"
assert solve("1\n7\n") == str(math.isqrt(16)), "odd boundary case"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| n = 1 | 1 | trường hợp cạnh tối thiểu | 
| n = 2 | 1 | trường hợp chẵn nhỏ nhất | 
| n = 6 | giá trị dựa trên sqrt | tính nhất quán của công thức ghép đôi | 
| n = 7 | giá trị dựa trên sqrt | xử lý độ dài lẻ | 

## Vỏ cạnh 

Với n = 1, chúng ta chỉ có một khối có chiều rộng 1. Công thức cho m = 0, tổng = 1 và câu trả lời là 1. Thuật toán xử lý điều này một cách chính xác vì phép cộng trường hợp lẻ đóng góp vào một khối. 

Với n = 2, cả hai khối đều có chiều rộng 1. Chúng ta nhận được m = 1, tổng = 2 và căn bậc hai là 1. Mặc dù có hai khối tồn tại nhưng chúng không thể tạo thành cạnh 2 vì tổng diện tích không đủ. 

Đối với n lẻ lớn, giả sử n = 10^9, thuật toán sẽ tính m = 5×10^8 và đánh giá an toàn biểu thức bậc hai trong số nguyên Python. Cấu trúc đơn điệu đảm bảo không xảy ra lỗi tràn hoặc lỗi đóng gói cấu trúc và căn bậc hai vẫn chính xác.
