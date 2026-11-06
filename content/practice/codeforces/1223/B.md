---
title: "CF 1223B - Cân bằng chuỗi"
description: "Mỗi truy vấn đưa ra hai chuỗi có cùng độ dài. Bạn được phép áp dụng nhiều lần một thao tác trên một trong hai chuỗi: chọn hai ký tự lân cận và ghi đè lên ký tự này bằng ký tự kia."
date: "2026-06-15T19:29:18+07:00"
tags: ["codeforces", "competitive-programming", "strings"]
categories: ["algorithms"]
codeforces_contest: 1223
codeforces_index: "B"
codeforces_contest_name: "Technocup 2020 - Elimination Round 1"
rating: 1000
weight: 1223
solve_time_s: 314
verified: true
draft: false
---

[CF 1223B - Cân bằng chuỗi](https://codeforces.com/problemset/problem/1223/B) 

**Đánh giá:** 1000 
**Thẻ:** chuỗi 
**Thời gian giải:** 5 phút 14s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Mỗi truy vấn đưa ra hai chuỗi có cùng độ dài. Bạn được phép áp dụng nhiều lần một thao tác trên một trong hai chuỗi: chọn hai ký tự lân cận và ghi đè lên ký tự này bằng ký tự kia. Điều này có nghĩa là thông tin có thể chảy dọc theo các vị trí liền kề, nhưng chỉ bằng cách sao chép các chữ cái hiện có chứ không phải bằng cách giới thiệu các chữ cái mới. 

Từ quan điểm cấu trúc, mỗi chuỗi hoạt động giống như một dòng ô trong đó bất kỳ chữ cái nào cũng có thể trải rộng trên toàn bộ chuỗi thông qua việc truyền bá lặp đi lặp lại. Tuy nhiên, không có thao tác nào giới thiệu một ký tự chưa có trong chuỗi đó khi bắt đầu. 

Nhiệm vụ là xác định xem sau bất kỳ số bước truyền bá nào được áp dụng độc lập cho cả hai chuỗi, chúng có thể được làm giống hệt nhau hay không. 

Các ràng buộc rất nhỏ: tối đa 100 truy vấn, mỗi chuỗi có độ dài lên tới 100. Điều này ngay lập tức loại trừ mọi hoạt động khám phá hàm mũ của các phép biến đổi hoặc mô phỏng trạng thái trên các chuỗi. Ngay cả mô phỏng bậc ba hoặc bậc hai cho mỗi truy vấn cũng có thể vượt qua một cách thoải mái, nhưng mục tiêu thực sự là tìm ra bất biến cơ bản thay vì mô phỏng bất kỳ thứ gì. 

Một trực giác sai lầm phổ biến là cho rằng thứ tự là quan trọng hoặc chúng ta phải khớp tần số. Điều đó dẫn đến lý luận quá phức tạp. Một sai lầm khác là cho rằng chúng ta cần mô phỏng các ký tự dịch chuyển, điều này nhanh chóng trở nên lộn xộn và vẫn không nắm bắt được sự tự do thực sự của thao tác. 

Trường hợp cạnh tinh tế xuất hiện khi các chuỗi không có chữ cái nào ngoại trừ gián tiếp thông qua các phép biến đổi. Ví dụ: nếu một chuỗi chỉ bao gồm`a`và cái còn lại chỉ của`b`, không có cách nào để dung hòa chúng. Mặt khác, nếu chúng chia sẻ dù chỉ một chữ cái, cả hai đều có thể biến hoàn toàn thành chữ cái đó, khiến chúng gần như bằng nhau. 

## Phương pháp tiếp cận 

Một ý tưởng bạo lực sẽ cố gắng mô hình hóa tất cả các chuỗi có thể truy cập từ`s`và từ`t`. Vì mỗi vị trí có thể bị hàng xóm ghi đè nhiều lần nên mỗi bước sẽ phân nhánh thành nhiều khả năng. Số lượng chuỗi có thể tăng theo cấp số nhân theo độ dài và thậm chí đối với độ dài 100, điều này hoàn toàn không khả thi. 

Điều quan trọng là thao tác này không bao giờ tạo ra các ký tự mới mà chỉ sao chép các ký tự hiện có. Điều này có nghĩa là thông tin duy nhất còn sót lại trong quá trình biến đổi là những ký tự riêng biệt nào tồn tại trong mỗi chuỗi ban đầu. Khi một ký tự tồn tại ở bất kỳ đâu trong chuỗi, nó có thể được truyền đến mọi vị trí, xóa bất kỳ cấu trúc nào bên trong chuỗi một cách hiệu quả. 

Điều này làm giảm mỗi chuỗi thành một tập hợp các chữ cái đơn giản. Nếu hai bộ có ít nhất một chữ cái chung, chúng ta có thể chọn chữ cái đó làm mục tiêu và truyền nó trên cả hai chuỗi một cách độc lập cho đến khi cả hai trở nên đồng nhất. Nếu không có chữ cái chung thì không có cách nào để đồng bộ hóa chúng. 

Do đó, vấn đề được chuyển thành kiểm tra giao lộ đã định. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Liệt kê lực lượng vũ phu | Hàm mũ | Hàm mũ | Quá chậm | 
| Đặt giao lộ | O(n) mỗi truy vấn | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý từng truy vấn một cách độc lập. 

Đầu tiên, chúng tôi quét chuỗi`s`và ghi lại những chữ cái xuất hiện trong đó. Chúng tôi làm tương tự với chuỗi`t`. Vì bảng chữ cái chỉ là các chữ cái tiếng Anh viết thường nên chúng ta có thể biểu diễn bảng chữ cái này bằng cách sử dụng một mảng boolean cố định có kích thước 26. 

Tiếp theo, chúng tôi kiểm tra xem có tồn tại ký tự nào xuất hiện trong cả hai bộ hay không. Điều này tương đương với việc kiểm tra xem giao điểm của hai bộ ký tự này có trống hay không. 

Nếu chúng tôi tìm thấy ít nhất một ký tự dùng chung, chúng tôi ngay lập tức kết luận rằng cả hai chuỗi có thể được chuyển đổi thành một chuỗi bao gồm toàn bộ ký tự đó và chúng tôi xuất ra`YES`. Nếu không, chúng tôi xuất`NO`. 

### Tại sao nó hoạt động 

Hoạt động này cho phép truyền bá không hạn chế bất kỳ ký tự hiện có nào trên toàn bộ chuỗi. Điều này làm cho mọi ký tự trong chuỗi ban đầu có sẵn một cách hiệu quả trên toàn cầu trong chuỗi đó. Vì không có ký tự mới nào có thể xuất hiện nên hạn chế duy nhất đối với chuỗi chung cuối cùng là mọi ký tự được sử dụng phải tồn tại trong cả hai chuỗi gốc. Một ký tự dùng chung là đủ vì nó có thể được mở rộng để điền đầy đủ cả hai chuỗi. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    q = int(input())
    out = []
    
    for _ in range(q):
        s = input().strip()
        t = input().strip()
        
        seen_s = [False] * 26
        seen_t = [False] * 26
        
        for ch in s:
            seen_s[ord(ch) - 97] = True
        for ch in t:
            seen_t[ord(ch) - 97] = True
        
        ok = False
        for i in range(26):
            if seen_s[i] and seen_t[i]:
                ok = True
                break
        
        out.append("YES" if ok else "NO")
    
    print("\n".join(out))

if __name__ == "__main__":
    solve()
```Giải pháp xử lý từng truy vấn một cách độc lập. Đối với mỗi chuỗi, nó xây dựng một mảng hiện diện trên bảng chữ cái. Vòng lặp cuối cùng kiểm tra mọi chỉ mục được chia sẻ. Việc thoát sớm đảm bảo hoạt động liên tục khi tìm thấy một ký tự chung. 

Một lỗi triển khai phổ biến là cố gắng so sánh các chuỗi hoặc tần số đầy đủ. Tần số không liên quan vì phép toán không bảo toàn số lượng. Một sai lầm khác là cố gắng mô phỏng các chuyển đổi từng bước, điều này là không cần thiết vì khả năng tiếp cận chỉ phụ thuộc vào sự tồn tại của ký tự. 

## Ví dụ đã hoạt động 

Hãy xem xét một truy vấn trong đó`s = "xabb"`Và`t = "aabx"`. 

Chúng tôi theo dõi các bộ ký tự như sau. 

| Bước | nhân vật của | ký tự t | Ký tự phổ biến được tìm thấy | 
| --- | --- | --- | --- | 
| Sau khi quét | {x, a, b} | {a,b,x} | vâng | 

Vì có ít nhất một ký tự dùng chung nên cả hai chuỗi có thể được rút gọn thành tất cả`a`ví dụ, vậy câu trả lời là`YES`. 

Bây giờ hãy xem xét`s = "a"`Và`t = "z"`. 

| Bước | nhân vật của | ký tự t | Ký tự phổ biến được tìm thấy | 
| --- | --- | --- | --- | 
| Sau khi quét | {a} | {z} | không | 

Không có sự chồng chéo nên không có chuỗi thao tác nào được phép có thể thu hẹp khoảng cách, tạo ra`NO`. 

Ví dụ đầu tiên chứng minh rằng cấu trúc và thứ tự không liên quan, trong khi ví dụ thứ hai cho thấy các bảng chữ cái rời rạc khiến cho việc chuyển đổi là không thể. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(26 · q · n) | Mỗi chuỗi được quét một lần và kiểm tra bảng chữ cái không đổi | 
| Không gian | O(1) | Mảng boolean có kích thước cố định để theo dõi bảng chữ cái | 

Với các ràng buộc tối đa là 100 truy vấn và độ dài chuỗi lên tới 100, điều này diễn ra không đáng kể trong giới hạn. Các hoạt động là công việc có hệ số không đổi một cách hiệu quả. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    q = int(input())
    out = []
    for _ in range(q):
        s = input().strip()
        t = input().strip()
        seen_s = [False]*26
        seen_t = [False]*26
        for ch in s:
            seen_s[ord(ch)-97] = True
        for ch in t:
            seen_t[ord(ch)-97] = True
        ok = False
        for i in range(26):
            if seen_s[i] and seen_t[i]:
                ok = True
                break
        out.append("YES" if ok else "NO")
    return "\n".join(out)

assert run("""3
xabb
aabx
technocup
technocup
a
z
""") == """YES
YES
NO"""

assert run("""2
abc
def
abc
a
""") == """NO
YES"""

assert run("""1
zzzz
zzzz
""") == """YES"""

assert run("""1
ab
cd
""") == """NO"""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| trường hợp chồng chéo hỗn hợp | CÓ/KHÔNG trộn | tính đúng đắn của logic giao nhau | 
| bảng chữ cái rời rạc | KHÔNG | trường hợp biến đổi không thể | 
| chuỗi giống hệt nhau | CÓ | bảo tồn danh tính | 
| trường hợp nhỏ không khớp đầy đủ | KHÔNG | hành vi cạnh cơ sở | 

## Vỏ cạnh 

Trường hợp một cạnh xảy ra khi cả hai chuỗi đều giống hệt nhau. Đối với đầu vào như`s = "abba"`Và`t = "abba"`, thuật toán đánh dấu cùng một bộ ký tự cho cả hai chuỗi, do đó giao điểm không trống và đầu ra là`YES`. Quá trình không dựa vào sự bình đẳng nhưng vẫn chấp nhận nó một cách chính xác. 

Một trường hợp khác là khi các chuỗi chứa các ký tự lặp lại nhưng các tập hợp rời rạc, chẳng hạn như`s = "aaaa"`Và`t = "bbbb"`. Mặc dù cả hai đều giống nhau nhưng không có ký tự chung nên không có chuỗi truyền nào có thể dung hòa chúng. Thuật toán trả về chính xác`NO`vì việc kiểm tra giao lộ không thành công ngay lập tức. 

Trường hợp thứ ba là khi các chuỗi dài nhưng có cùng một chữ cái, chẳng hạn như`s = "abcabc..."`Và`t = "zzzazzz..."`. Mặc dù hầu hết các ký tự đều khác nhau, nhưng điểm chung`a`là đủ để thu gọn cả hai chuỗi thành một chuỗi thống nhất gồm`a`, mà thuật toán nắm bắt thông qua điều kiện giao nhau không trống.
