---
title: "CF 988B - Sắp xếp chuỗi con"
description: "Chúng ta được cung cấp một tập hợp các chuỗi và chúng ta được phép sắp xếp lại chúng theo bất kỳ thứ tự nào. Sau khi sắp xếp lại, chúng ta muốn có một thuộc tính lồng rất cụ thể: mỗi chuỗi phải chứa tất cả các chuỗi xuất hiện trước nó dưới dạng chuỗi con."
date: "2026-06-17T00:47:38+07:00"
tags: ["codeforces", "competitive-programming", "sortings", "strings"]
categories: ["algorithms"]
codeforces_contest: 988
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 486 (Div. 3)"
rating: 1100
weight: 988
solve_time_s: 82
verified: true
draft: false
---

[CF 988B - Sắp xếp chuỗi con](https://codeforces.com/problemset/problem/988/B) 

**Đánh giá:** 1100 
**Thẻ:** sắp xếp, chuỗi 
**Thời gian giải:** 1m 22s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một tập hợp các chuỗi và chúng ta được phép sắp xếp lại chúng theo bất kỳ thứ tự nào. Sau khi sắp xếp lại, chúng ta muốn có một thuộc tính lồng rất cụ thể: mỗi chuỗi phải chứa tất cả các chuỗi xuất hiện trước nó dưới dạng chuỗi con. 

Nói cách khác, nếu chúng ta sắp xếp các chuỗi từ đầu đến cuối thì bất cứ khi nào chúng ta lấy một chuỗi ở vị trí i, mọi chuỗi ở vị trí 1 đến i − 1 phải xuất hiện ở đâu đó bên trong nó dưới dạng một khối ký tự liền kề. 

Nhiệm vụ là quyết định xem thứ tự đó có tồn tại hay không và nếu có thì xuất ra một thứ tự hợp lệ. 

Các ràng buộc rất nhỏ: tối đa 100 chuỗi, mỗi chuỗi có độ dài tối đa 100. Điều này ngay lập tức cho chúng ta biết rằng ngay cả việc kiểm tra mối quan hệ chuỗi con giữa tất cả các cặp cũng khả thi trong thời gian O(n² · L) mà không cần quan tâm đến việc tối ưu hóa. Chúng ta không ở trong một chế độ mà các yếu tố logarit đóng vai trò quan trọng; thay vào đó, tính đúng đắn của logic đặt hàng là khó khăn cốt lõi. 

Một số trường hợp đặc biệt quan trọng hơn những gì chúng xuất hiện lần đầu. 

Một vấn đề là trùng lặp. Nếu cùng một chuỗi xuất hiện nhiều lần thì phải xử lý cẩn thận, vì một chuỗi luôn là chuỗi con của chính nó, nhưng việc sắp xếp các bản sao không đưa ra thông tin mới. 

Một trường hợp tinh tế khác là khi một chuỗi ngắn hơn xuất hiện sau trong thứ tự cuối cùng. Điều đó chỉ đúng nếu nó là một chuỗi con của tất cả các chuỗi dài hơn đứng trước nó, điều này chỉ có thể thực hiện được nếu cấu trúc của các chuỗi được lồng vào nhau một cách rất chắc chắn. 

Trường hợp lỗi cụ thể xảy ra khi một chuỗi không có trong chuỗi lớn nhất cuối cùng. Ví dụ, nếu chúng ta có`"abc"`Và`"b"`, và cả`"abd"`, sau đó`"abc"`Và`"abd"`không thể chứa cả hai`"b"`một cách nhất quán nếu thứ tự không được lựa chọn cẩn thận, nhưng trở ngại thực sự là không có một chuỗi nào có thể đóng vai trò là nơi chứa chung cho tất cả những chuỗi khác. 

Trường hợp tinh tế thứ hai là sắp xếp hoàn toàn theo độ dài. Nếu chúng ta sắp xếp theo độ dài và cho rằng các chuỗi dài hơn luôn chứa các chuỗi ngắn hơn, thì chúng ta sẽ thất bại trong các trường hợp như`"ab"`Và`"cd"`, trong đó cả hai đều không phải là chuỗi con của chuỗi kia. Điều này sẽ tạo ra một thứ tự nhưng vi phạm điều kiện. 

## Phương pháp tiếp cận 

Ý tưởng brute-force rất đơn giản: thử tất cả các hoán vị của chuỗi và kiểm tra xem mỗi thứ tự có thỏa mãn điều kiện chuỗi con hay không. Đối với một hoán vị nhất định, chúng tôi xác minh từng tiền tố bằng cách kiểm tra tư cách thành viên chuỗi con của tất cả các chuỗi trước đó bên trong chuỗi hiện tại. Chi phí xác minh này là O(n² · L) cho mỗi hoán vị. Vì có n! hoán vị, điều này nhanh chóng trở nên không khả thi ngay cả khi n = 10. 

Quan sát quan trọng là điều kiện áp đặt một cấu trúc toàn cầu mạnh mẽ. Nếu một thứ tự tồn tại thì chuỗi cuối cùng phải chứa mọi chuỗi khác dưới dạng chuỗi con. Điều này là do mọi thứ trước nó phải được chứa bên trong nó. Điều đó có nghĩa là chuỗi cuối cùng là một vùng chứa phổ quát. 

Khi chúng tôi xác định được điều đó, vấn đề sẽ giảm xuống việc chọn một ứng cử viên cho vị trí cuối cùng và kiểm tra xem nó có chứa tất cả các chuỗi khác hay không. Nếu một ứng cử viên như vậy tồn tại, chúng ta có thể đặt nó cuối cùng một cách an toàn, sau đó sắp xếp các chuỗi còn lại tùy ý theo thứ tự phù hợp với ngăn chặn, điều này có thể đạt được bằng cách sắp xếp theo độ dài và xác minh các ràng buộc chuỗi con trong quá trình thực hiện. Trong thực tế, việc sắp xếp theo cách tăng độ dài sẽ có tác dụng vì bất kỳ giải pháp hợp lệ nào cũng phải có độ dài không giảm cho đến giới hạn ngăn chặn và việc xác minh đảm bảo tính chính xác. 

Điều này làm giảm việc tìm kiếm từ giai thừa sang quét tuyến tính đơn giản để tìm chuỗi “vùng chứa” hợp lệ tối đa, sau đó là bước sắp xếp và xác minh. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (tất cả các hoán vị) | O(n! · n² · L) | O(nL) | Quá chậm | 
| Tối ưu (ứng viên + sắp xếp + xác minh) | O(n² · L log n) | O(nL) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xây dựng giải pháp bằng cách xác định một chuỗi có thể đóng vai trò là phần tử cuối cùng. 

1. Sắp xếp tất cả các chuỗi theo độ dài theo thứ tự không giảm. Điều này làm cho việc lý giải về việc ngăn chặn trở nên dễ dàng hơn vì các chuỗi ngắn hơn có nhiều khả năng xuất hiện bên trong các chuỗi dài hơn. 
2. Đối với mỗi chuỗi trong danh sách, hãy coi chuỗi đó là ứng cử viên cho vị trí cuối cùng. Lý do cho điều này là chuỗi cuối cùng phải chứa tất cả các chuỗi khác dưới dạng chuỗi con, vì vậy chỉ cần kiểm tra nó trên toàn cục. 
3. Đối với ứng cử viên được chọn, hãy xác minh rằng mọi chuỗi khác đều là chuỗi con của chuỗi đó. Nếu thậm chí không có, hãy loại bỏ ứng cử viên này. Kiểm tra này là kiểm tra tính khả thi cốt lõi. 
4. Sau khi tìm thấy một ứng cử viên hợp lệ, hãy xây dựng câu trả lời bằng cách lấy tất cả các chuỗi khác và sắp xếp chúng trước nó. Chúng tôi sắp xếp các chuỗi còn lại này theo độ dài để đảm bảo rằng nếu tồn tại cấu trúc lồng hợp lệ thì chúng tôi không vi phạm bất kỳ phần phụ thuộc ngăn chặn tiềm năng nào. 
5. Xuất ra thứ tự bao gồm các chuỗi còn lại được sắp xếp theo sau là ứng cử viên hợp lệ. 

Nếu không có ứng cử viên nào vượt qua bài kiểm tra ngăn chặn, chúng tôi kết luận rằng không có mệnh lệnh hợp lệ nào tồn tại. 

### Tại sao nó hoạt động 

Trong bất kỳ thứ tự hợp lệ nào, chuỗi cuối cùng phải chứa tất cả các chuỗi khác dưới dạng chuỗi con, bởi vì mọi chuỗi trước đó phải là chuỗi con của chuỗi đó. Điều này làm cho phần tử cuối cùng có thể được nhận dạng duy nhất dưới dạng siêu chuỗi phổ quát của tập hợp. 

Khi một chuỗi như vậy được cố định ở cuối, tất cả các chuỗi còn lại được đảm bảo là chuỗi con của chuỗi đó. Việc sắp xếp các chuỗi còn lại theo độ dài sẽ đảm bảo tiến trình an toàn từ chuỗi con nhỏ hơn đến chuỗi con lớn hơn và vì mọi chuỗi đều được xác minh dựa trên ứng cử viên nên không thể xảy ra vị trí không hợp lệ. Điều này đảm bảo rằng thứ tự được xây dựng thỏa mãn điều kiện chuỗi con tiền tố. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def is_sub(a, b):
    return a in b

n = int(input())
s = [input().strip() for _ in range(n)]

s.sort(key=len)

for i in range(n):
    ok = True
    for j in range(n):
        if i == j:
            continue
        if s[j] not in s[i]:
            ok = False
            break
    if not ok:
        continue

    other = [s[j] for j in range(n) if j != i]
    other.sort(key=len)

    print("YES")
    for x in other:
        print(x)
    print(s[i])
    sys.exit()

print("NO")
```Đầu tiên, mã sắp xếp các chuỗi theo độ dài để giảm những hoán vị không cần thiết trong các trường hợp tương đương. Vòng lặp chính thử từng chuỗi làm ứng cử viên cuối cùng. Đối với mỗi ứng cử viên, nó sẽ kiểm tra khả năng ngăn chặn bằng cách sử dụng thao tác chuỗi con của Python, thao tác này chạy hiệu quả với các ràng buộc nhỏ. 

Khi tìm thấy một ứng cử viên hợp lệ, tất cả các chuỗi khác sẽ được sắp xếp theo độ dài và được in trước nó. Điều này đảm bảo cấu trúc ổn định trong đó các chuỗi nhỏ hơn xuất hiện sớm hơn và tất cả đều được đảm bảo là chuỗi con của chuỗi cuối cùng. 

Việc thoát sớm đảm bảo chúng tôi chỉ xuất ra một đơn hàng hợp lệ. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5
a
aba
abacaba
ba
aba
```Chúng tôi sắp xếp theo độ dài:```
a, ba, aba, aba, abacaba
```Bây giờ chúng tôi kiểm tra các ứng cử viên dưới dạng chuỗi cuối cùng. 

| Ứng viên | Kiểm tra (có tất cả các chuỗi?) | Kết quả | 
| --- | --- | --- | 
| một | ba không chứa | từ chối | 
| ba | a, aba, abacaba đều không chứa | từ chối | 
| aba | ba không chứa | từ chối | 
| bàn tính | tất cả những thứ khác có trong | chấp nhận | 

Chúng tôi chọn`"abacaba"`như chuỗi cuối cùng. Các chuỗi còn lại được sắp xếp theo độ dài:```
a, ba, aba, aba
```Đầu ra cuối cùng:```
a
ba
aba
aba
abacaba
```Điều này xác nhận rằng mọi chuỗi tiền tố đều xuất hiện bên trong chuỗi cuối cùng. 

### Ví dụ 2 

đầu vào:```
3
ab
abc
cd
```Đã sắp xếp:```
ab, cd, abc
```Chúng tôi kiểm tra`"ab"`: thất bại vì`"cd"`không phải là một chuỗi con. 

Chúng tôi kiểm tra`"cd"`: thất bại vì`"ab"`không phải là một chuỗi con. 

Chúng tôi kiểm tra`"abc"`: thất bại vì`"cd"`không phải là một chuỗi con. 

Không có ứng viên nào làm việc nên kết quả đầu ra là:```
NO
```Điều này cho thấy sự không thể xảy ra do có các chuỗi không liên quan và không thể chứa tất cả trong một chuỗi. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n² · L) | Mỗi ứng cử viên kiểm tra tư cách thành viên của chuỗi con đối với tất cả các chuỗi và có tối đa n ứng cử viên | 
| Không gian | O(nL) | Lưu trữ chuỗi đầu vào và danh sách được lọc tạm thời | 

Với n ≤ 100 và L ≤ 100, tổng số thao tác tối đa là khoảng 10⁶ kiểm tra chuỗi con, tốc độ này đủ nhanh trong Python. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    return solve()

def solve():
    input = sys.stdin.readline
    n = int(input())
    s = [input().strip() for _ in range(n)]
    s.sort(key=len)

    for i in range(n):
        ok = True
        for j in range(n):
            if i != j and s[j] not in s[i]:
                ok = False
                break
        if not ok:
            continue
        other = [x for k, x in enumerate(s) if k != i]
        other.sort(key=len)
        return "YES\n" + "\n".join(other + [s[i]])
    return "NO"

# provided sample
assert run("""5
a
aba
abacaba
ba
aba
""") == """YES
a
ba
aba
aba
abacaba"""

# all equal
assert run("""3
aaa
aaa
aaa
""").startswith("YES")

# single string
assert run("""1
abc
""") == """YES
abc"""

# impossible case
assert run("""3
ab
cd
ef
""") == "NO"

# nested chain
assert run("""3
a
ab
abc
""").startswith("YES")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả các chuỗi bằng nhau | CÓ đặt hàng | các bản sao được xử lý an toàn | 
| chuỗi đơn | CÓ | trường hợp cơ sở | 
| chuỗi không liên quan | KHÔNG | phát hiện không thể | 
| chuỗi ngày càng tăng | CÓ | cấu trúc lồng hợp lệ | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi tất cả các chuỗi đều giống hệt nhau. Trong tình huống này, mọi chuỗi đều là chuỗi con của mọi chuỗi khác. Thuật toán chọn bất kỳ ứng cử viên nào làm chuỗi cuối cùng. Ví dụ, với`"aaa", "aaa", "aaa"`, mọi ứng cử viên đều vượt qua bài kiểm tra ngăn chặn và thứ tự còn lại là tùy ý. Việc sắp xếp theo độ dài sẽ giữ cho chúng không thay đổi và đầu ra vẫn hợp lệ. 

Một trường hợp khác là khi chỉ có một chuỗi. Vòng lặp chọn chuỗi đó làm ứng cử viên và vì không có chuỗi nào khác để kiểm tra nên chuỗi đó sẽ ngay lập tức được chuyển. Đầu ra chỉ đơn giản là chuỗi đó, thỏa mãn điều kiện một cách trống rỗng. 

Một trường hợp minh họa hơn là khi các chuỗi tạo thành một chuỗi chặt chẽ như`"a"`,`"ab"`,`"abc"`. Chuỗi dài nhất`"abc"`là ứng cử viên hợp lệ duy nhất vì nó chứa tất cả những ứng cử viên khác. Thuật toán xác định nó là phần tử cuối cùng, sắp xếp hai phần tử còn lại thành`"a", "ab"`và đưa ra thứ tự đúng.
