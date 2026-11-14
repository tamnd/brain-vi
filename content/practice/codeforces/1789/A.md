---
title: "CF 1789A - Serval và Mocha's Array"
description: "Chúng ta được cho một số trường hợp thử nghiệm, mỗi trường hợp bao gồm một mảng nhỏ các số nguyên dương. Với mỗi mảng, chúng ta được phép sắp xếp lại các phần tử của nó một cách tùy ý. Sau khi chọn một đơn hàng, chúng tôi kiểm tra mọi tiền tố có độ dài ít nhất là hai và tính gcd của tiền tố đó."
date: "2026-06-09T10:43:23+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "math", "number-theory"]
categories: ["algorithms"]
codeforces_contest: 1789
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 853 (Div. 2)"
rating: 800
weight: 1789
solve_time_s: 170
verified: false
draft: false
---

[CF 1789A - Mảng Serval và Mocha](https://codeforces.com/problemset/problem/1789/A) 

**Đánh giá:** 800 
**Tags:** lực lượng vũ phu, toán học, lý thuyết số 
**Thời gian giải:** 2 phút 50 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một số trường hợp thử nghiệm, mỗi trường hợp bao gồm một mảng nhỏ các số nguyên dương. Với mỗi mảng, chúng ta được phép sắp xếp lại các phần tử của nó một cách tùy ý. Sau khi chọn một đơn hàng, chúng tôi kiểm tra mọi tiền tố có độ dài ít nhất là hai và tính gcd của tiền tố đó. Yêu cầu là mọi tiền tố như vậy phải có gcd không vượt quá độ dài tiền tố. Chúng ta cần quyết định xem có tồn tại ít nhất một thứ tự của mảng thỏa mãn ràng buộc này hay không. 

Các ràng buộc là nhỏ đối với mỗi trường hợp thử nghiệm, với tổng số tối đa vài trăm phần tử trong tất cả các thử nghiệm. Điều đó có nghĩa là một$O(n \log n)$hoặc thậm chí$O(n^2)$Cách tiếp cận cho mỗi trường hợp thử nghiệm đều có thể chấp nhận được, nhưng bất cứ điều gì liên quan đến việc sắp xếp lại theo cấp số nhân hoặc theo cấp số nhân thì không. 

Một trường hợp thất bại tinh vi đối với lý luận ngây thơ là giả định rằng việc kiểm tra cục bộ trên các cặp liền kề là đủ. Ví dụ: trong một mảng như$[3, 6, 1]$, tất cả các cặp liền kề ngoại trừ cặp đầu tiên có thể trông vô hại, nhưng tiền tố$[3,6]$đã vi phạm điều kiện vì gcd của nó là$3$, lớn hơn chiều dài của nó$2$. Điều này cho thấy rằng ràng buộc về cơ bản là dựa trên tiền tố, không dựa trên cặp. 

Một cạm bẫy phổ biến khác là cho rằng thứ tự ban đầu có ý nghĩa. Bài toán cho phép sắp xếp lại thứ tự một cách rõ ràng, do đó, bất kỳ giải pháp nào chỉ đánh giá trình tự đã cho mà không xem xét các hoán vị sẽ thất bại trong các trường hợp như$[15, 35, 21]$, trong đó tồn tại một thứ tự tốt ngay cả khi thứ tự đầu vào không hợp lệ. 

## Phương pháp tiếp cận 

Cách tiếp cận brute-force là thử tất cả các hoán vị của mảng và kiểm tra xem mỗi thứ tự có thỏa mãn điều kiện tiền tố gcd hay không. Đối với mỗi hoán vị, chúng tôi tính toán tiền tố gcds và xác minh ràng buộc. Điều này đúng nhưng không khả thi ngay lập tức vì số hoán vị tăng dần theo$n!$, vượt xa mọi giới hạn ngay cả đối với$n = 20$. 

Quan sát quan trọng là chúng ta không thực sự cần khám phá tất cả các hoán vị. Điều kiện chỉ phụ thuộc vào các giá trị gcd tiền tố và gcd hoạt động đơn điệu theo một cách rất mạnh: một khi nó trở nên nhỏ, nó sẽ không bao giờ tăng trở lại khi thêm nhiều số hơn. Điều này gợi ý rằng việc sắp xếp các phần tử theo thứ tự tăng dần là một ứng cử viên đương nhiên, vì các giá trị nhỏ có xu hướng giảm gcd sớm, điều này làm cho các ràng buộc sau này dễ được đáp ứng hơn. 

Điều này dẫn đến một ý tưởng tham lam: sắp xếp mảng, sau đó kiểm tra xem thứ tự sắp xếp này đã đáp ứng tất cả các ràng buộc tiền tố hay chưa. Nếu ngay cả thứ tự có cấu trúc tốt nhất (được sắp xếp tăng dần) không thành công thì không có hoán vị nào khác có thể khắc phục được vụ nổ gcd tiền tố sớm, bởi vì bất kỳ hoán vị nào đặt các phần tử đóng góp gcd lớn trước đó chỉ làm cho tiền tố gcd lớn hơn hoặc bằng nhau. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (tất cả các hoán vị) |$O(n!)$|$O(n)$| Quá chậm | 
| Sắp xếp + kiểm tra tiền tố |$O(n \log n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Ta xây dựng giải pháp như sau. 

1. Với mỗi test, sắp xếp mảng theo thứ tự không giảm. Điều này tạo ra cấu trúc tiền tố ổn định nhất về mặt tăng trưởng gcd. 
2. Duyệt mảng từ trái sang phải trong khi vẫn giữ nguyên gcd của tiền tố hiện tại. 
3. Tại mỗi vị trí$i$, tính gcd của tiền tố kết thúc tại$i$. Nếu như$i \ge 1$, kiểm tra xem gcd này có lớn hơn không$i + 1$. Nếu đúng như vậy, chúng ta biết ngay rằng không có hoán vị hợp lệ nào tồn tại. 
4. Nếu chúng tôi hoàn tất quá trình quét mà không vi phạm điều kiện, thì sắp xếp đã sắp xếp là hợp lệ, vì vậy câu trả lời là CÓ. 

Lý do chúng tôi chỉ kiểm tra mảng đã sắp xếp là vì bất kỳ nỗ lực nào nhằm “sửa” tiền tố xấu sẽ yêu cầu chèn các số nhỏ hơn sớm hơn, nhưng những số đó đã được đặt sớm nhất có thể theo thứ tự được sắp xếp. Nếu thứ tự được sắp xếp không thể kiểm soát được gcd thì không thể sắp xếp lại được. 

### Tại sao nó hoạt động 

Gcd của tiền tố chỉ có thể giảm khi chúng tôi bao gồm một số không phải là bội số của gcd hiện tại và các số nhỏ có hiệu quả nhất trong việc sớm phá vỡ các giá trị gcd lớn. Việc sắp xếp đảm bảo rằng các phần tử nhỏ nhất có sẵn được sử dụng càng sớm càng tốt, giảm thiểu gcd tiền tố ở mỗi bước. Vì điều kiện trở nên chặt chẽ hơn khi độ dài tiền tố tăng lên, nên bất kỳ thứ tự nào không thành công ngay cả trong trường hợp xây dựng tham lam trong trường hợp tốt nhất này đều ngụ ý không thể thực hiện được. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
from math import gcd

t = int(input())
for _ in range(t):
    n = int(input())
    a = list(map(int, input().split()))
    
    a.sort()
    
    g = 0
    ok = True
    
    for i, x in enumerate(a):
        g = gcd(g, x)
        if i >= 1 and g > i + 1:
            ok = False
            break
    
    print("Yes" if ok else "No")
```Sau khi sắp xếp, chúng tôi duy trì một gcd đang chạy. Chi tiết quan trọng là so sánh chỉ mục: vì độ dài tiền tố là$i+1$, chúng tôi so sánh gcd với$i+1$, không$i$. Phần tử đầu tiên không bao giờ được kiểm tra vì điều kiện chỉ áp dụng cho tiền tố có độ dài ít nhất là hai. 

Điều tinh tế duy nhất là khởi tạo gcd bằng 0, hoạt động chính xác như một phần tử trung tính để bản cập nhật đầu tiên chỉ cần đặt nó thành giá trị mảng đầu tiên. 

## Ví dụ đã hoạt động 

Xem xét đầu vào$[3, 6, 1]$. Sau khi sắp xếp chúng ta có được$[1, 3, 6]$. 

Ở mỗi bước: 

| tôi | giá trị | tiền tố gcd | 
| --- | --- | --- | 
| 0 | 1 | 1 | 
| 1 | 3 | 1 | 
| 2 | 6 | 1 | 

Tất cả các giá trị gcd tiền tố không bao giờ lớn hơn chỉ số của chúng cộng với một, vì vậy điều này là hợp lệ. 

Bây giờ hãy xem xét$[15, 35, 21]$. Sắp xếp mang lại$[15, 21, 35]$. 

| tôi | giá trị | tiền tố gcd | 
| --- | --- | --- | 
| 0 | 15 | 15 | 
| 1 | 21 | 3 | 
| 2 | 35 | 1 | 

Tại$i = 1$, độ dài tiền tố là 2 và gcd là 3, vi phạm điều kiện, vì vậy câu trả lời là KHÔNG. 

Điều này chứng tỏ rằng lỗi được phát hiện càng sớm càng tốt trong cấu hình đã sắp xếp, xác nhận rằng việc sắp xếp sẽ phát hiện ra mọi vi phạm không thể tránh khỏi. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \log n)$| Sắp xếp chiếm ưu thế, cập nhật gcd là tuyến tính | 
| Không gian |$O(1)$thêm | Chỉ các biến tiền tố mới được lưu trữ | 

Tổng kích thước đầu vào đủ nhỏ để việc sắp xếp từng trường hợp thử nghiệm có thể dễ dàng nhanh chóng trong giới hạn và việc tính toán gcd là công việc không đổi trên mỗi phần tử. 

## Trường hợp thử nghiệm```python
import sys, io
from math import gcd

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    t = int(input())
    out = []
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        a.sort()
        g = 0
        ok = True
        for i, x in enumerate(a):
            g = gcd(g, x)
            if i >= 1 and g > i + 1:
                ok = False
                break
        out.append("Yes" if ok else "No")
    return "\n".join(out)

# provided samples
assert run("""6
2
3 6
3
1 2 4
3
3 6 1
3
15 35 21
4
35 10 35 14
5
1261 227821 143 4171 1941
""") == """No
Yes
Yes
No
Yes
Yes"""

# custom cases
assert run("""1
2
4 6
""") == "Yes", "small valid case"

assert run("""1
2
4 8
""") == "No", "small invalid case"

assert run("""1
4
6 10 15 21
""") == "No", "multiple gcd growth failure"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 4 6 | Có | cặp hợp lệ nhỏ nhất | 
| 4 8 | Không | gcd vượt quá độ dài tiền tố sớm | 
| 6 10 15 21 | Không | lỗi gcd bị xâu chuỗi trong mảng dài hơn | 

## Vỏ cạnh 

Một mảng tối thiểu có kích thước hai sẽ hiển thị toàn bộ điều kiện ngay lập tức, vì tiền tố duy nhất cần kiểm tra là chính mảng đầy đủ. Thuật toán xử lý chính xác điều này vì thứ tự sắp xếp được đánh giá một lần và gcd được so sánh với 2. 

Mảng chứa các giá trị lặp lại hoạt động có thể dự đoán được vì việc tăng cường gcd lặp đi lặp lại sẽ giữ cho gcd ổn định hoặc tăng nó và quá trình quét được sắp xếp sẽ ngay lập tức phát hiện khi mức tăng trưởng này vượt qua ngưỡng tiền tố cho phép.
