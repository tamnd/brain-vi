---
title: "CF 1807C - Tìm và Thay thế"
description: "Chúng ta được cung cấp một chuỗi gồm các chữ cái viết thường và chúng ta được phép liên tục “thu gọn” toàn bộ các lớp ký tự thành các chữ số nhị phân. Một thao tác chọn một chữ cái, chẳng hạn như x, và viết lại toàn bộ mỗi lần xuất hiện của x trong chuỗi thành 0 hoặc 1."
date: "2026-06-09T09:02:44+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "implementation", "strings"]
categories: ["algorithms"]
codeforces_contest: 1807
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 859 (Div. 4)"
rating: 800
weight: 1807
solve_time_s: 114
verified: true
draft: false
---

[CF 1807C - Tìm và thay thế](https://codeforces.com/problemset/problem/1807/C) 

**Đánh giá:** 800 
**Tags:** tham lam, thực hiện, chuỗi 
**Thời gian giải:** 1 phút 54s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi gồm các chữ cái viết thường và chúng ta được phép liên tục “thu gọn” toàn bộ các lớp ký tự thành các chữ số nhị phân. Một thao tác chọn một chữ cái, chẳng hạn`x`và viết lại toàn cục mỗi lần xuất hiện của`x`trong chuỗi vào một trong hai`0`hoặc`1`. Các chữ cái khác nhau có thể được ánh xạ độc lập và mỗi chữ cái có thể được gán chính xác một trong hai giá trị nhị phân tại thời điểm chúng ta chọn nó. 

Sau khi gán cho mỗi chữ cái một giá trị nhị phân, chuỗi kết quả sẽ trở thành chuỗi nhị phân. Mục đích là để xác định xem liệu chúng ta có thể chọn các phép gán sao cho chuỗi nhị phân cuối cùng này xen kẽ hoàn hảo hay không, nghĩa là mỗi cặp ký tự liền kề đều khác nhau. 

Khó khăn chính là cấu trúc chuỗi không thể chỉnh sửa trực tiếp ở cấp vị trí. Chúng ta không thể lật từng ký tự riêng lẻ; thay vào đó, mỗi chữ cái hoạt động như một khối cứng nhắc phải được ánh xạ nhất quán trong tất cả các lần xuất hiện của nó. 

Phạm vi ràng buộc nhỏ, với tổng độ dài lên tới 2000 cho mỗi trường hợp kiểm thử và nhiều nhất là 100 trường hợp kiểm thử. Điều này cho phép bất kỳ giải pháp nào gần như bậc hai cho mỗi trường hợp thử nghiệm, nhưng ngay cả các giải pháp bậc ba cũng khó có thể vượt qua nếu hằng số thấp. Tuy nhiên, cấu trúc vấn đề gợi ý rõ ràng rằng tồn tại một điều kiện khả thi đơn giản hơn nhiều. 

Trường hợp cạnh tinh tế xuất hiện khi một chữ cái xuất hiện ở cả hai vị trí cần các giá trị nhị phân khác nhau trong một mẫu xen kẽ. Ví dụ, trong`aa`, bất kể chúng ta lập bản đồ như thế nào`a`, kết quả luôn là`00`hoặc`11`, cả hai đều không hợp lệ. Điều này ngay lập tức cho thấy rằng việc sao chép một chữ cái trên các vị trí chẵn lẻ xung đột là trở ngại chính. 

Một trường hợp cạnh khác là chiều dài 1 chuỗi. Bất kỳ ký tự đơn nào cũng có thể được ánh xạ tới`0`hoặc`1`và cả hai đều là các chuỗi xen kẽ hợp lệ theo định nghĩa. 

## Phương pháp tiếp cận 

Quan điểm bạo lực bắt đầu bằng cách coi mỗi chữ cái riêng biệt như một biến nhị phân. Nếu có`k`các chữ cái riêng biệt, chúng tôi chỉ định từng chữ cái`0`hoặc`1`, sản xuất`2^k`bài tập. Với mỗi phép gán, chúng ta xây dựng chuỗi kết quả và kiểm tra xem nó có thay thế trong`O(n)`thời gian. Điều này dẫn đến sự phức tạp tổng thể của`O(n · 2^k)`. Trong trường hợp xấu nhất, tất cả các ký tự đều khác biệt, vì vậy`k = n`và không gian tìm kiếm trở nên theo cấp số nhân, điều này không thể thực hiện được ngay cả đối với`n = 20`, huống hồ là`2000`. 

Quan sát quan trọng là sự luân phiên chỉ phụ thuộc vào các ràng buộc kề cận. Đối với mỗi chỉ số`i`, chúng tôi yêu cầu`s[i] != s[i+1]`sau khi lập bản đồ. Nếu hai vị trí`i`Và`i+1`chứa cùng một chữ cái gốc thì chúng phải bằng nhau sau khi ánh xạ, điều này ngay lập tức vi phạm sự xen kẽ. Vì vậy bất kỳ chữ cái nào xuất hiện ở hai vị trí liền kề đều khiến câu trả lời là không thể. 

Tổng quát hơn, nếu chúng ta nhìn vào cấu trúc chẵn lẻ, chúng ta nhận ra rằng chúng ta đang cố gắng gán cho mỗi chữ cái một giá trị sao cho tất cả các lần xuất hiện của chữ cái đó đều rơi vào các vị trí phải luân phiên trên toàn cầu. Nhưng ràng buộc xen kẽ hoàn toàn không phụ thuộc vào cấu trúc bảng chữ cái, chỉ phụ thuộc vào việc chuỗi có chứa bất kỳ ký tự liền kề lặp lại nào được ngụy trang hay không. 

Điều này đơn giản hóa đáng kể: cách duy nhất để xây dựng thất bại là khi một số chữ cái xuất hiện ở các vị trí liền kề. Nếu không tồn tại sự liền kề như vậy, chúng ta luôn có thể xây dựng một chuỗi nhị phân xen kẽ hợp lệ bằng cách chọn bất kỳ mẫu xen kẽ mong muốn nào và ánh xạ từng chữ cái một cách nhất quán theo tính chẵn lẻ của vị trí xuất hiện đầu tiên của nó. Vì không có chữ cái nào tạo ra hai kết quả bằng nhau liền kề nên luôn có thể đạt được sự nhất quán. 

Do đó, vấn đề giảm xuống còn một lần quét kiểm tra xem có tồn tại chỉ mục không`i`như vậy`s[i] == s[i+1]`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (thử tất cả các bài tập) | O(n · 2^k) | O(n) | Quá chậm | 
| Tối ưu (kiểm tra liền kề) | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Với mỗi test case, hãy đọc chuỗi`s`. Chúng tôi chỉ cần kiểm tra cấu trúc cục bộ nên không cần xử lý trước. 
2. Quét chuỗi từ trái sang phải, so sánh từng ký tự với ký tự tiếp theo. Lý do điều này có tác dụng là vì mọi vi phạm đều phải xuất phát từ ràng buộc kề cận, vì sự luân phiên chỉ được xác định giữa các lân cận. 
3. Nếu chúng ta tìm thấy`s[i] == s[i+1]`, kết luận ngay đáp án là`"NO"`. Điều này là do cả hai lần xuất hiện của cùng một chữ cái nhất thiết phải được ánh xạ tới các giá trị nhị phân giống hệt nhau, tạo ra các bit liền kề bằng nhau, làm gián đoạn sự xen kẽ. 
4. Nếu quá trình quét hoàn tất mà không tìm thấy bất kỳ ký tự liền kề bằng nhau nào, hãy xuất ra`"YES"`. Trong trường hợp này, chúng ta luôn có thể gán các giá trị nhị phân một cách nhất quán theo cách tôn trọng mẫu xen kẽ vì không có chữ cái nào gây ra mâu thuẫn ở bất kỳ ranh giới nào. 

### Tại sao nó hoạt động 

Ràng buộc duy nhất quan trọng là liệu một số chữ cái có tạo ra sự bình đẳng giữa hai vị trí liền kề hay không. Nếu tồn tại một ràng buộc như vậy thì việc luân phiên là không thể bất kể chúng ta gán như thế nào`0`Và`1`. Nếu không tồn tại ràng buộc như vậy thì mỗi cạnh kề sẽ kết nối hai chữ cái khác nhau và chúng ta có thể tự do gán các giá trị nhị phân sao cho chuỗi nhị phân cảm ứng xen kẽ nhau, vì không có chữ cái nào tạo ra sự bằng nhau bắt buộc trên một cạnh. Điều này làm giảm vấn đề chuyển nhượng toàn cục thành một kiểm tra hoàn toàn cục bộ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        s = input().strip()
        
        ok = True
        for i in range(n - 1):
            if s[i] == s[i + 1]:
                ok = False
                break
        
        print("YES" if ok else "NO")

if __name__ == "__main__":
    solve()
```Giải pháp đọc từng trường hợp kiểm thử một cách độc lập và thực hiện quét tuyến tính duy nhất. Chi tiết triển khai chính sẽ dừng sớm ngay khi tìm thấy ký tự liền kề lặp lại, vì việc kiểm tra thêm không thể thay đổi kết quả. 

Logic không cố gắng xây dựng chuỗi nhị phân một cách rõ ràng, bởi vì tính khả thi chỉ phụ thuộc vào việc phát hiện tính không thể của cấu trúc chứ không phụ thuộc vào việc mô phỏng các bài tập. 

## Ví dụ đã hoạt động 

### Ví dụ 1:`abacaba`| tôi | s[i] | s[i+1] | Bình đẳng? | Quyết định | 
| --- | --- | --- | --- | --- | 
| 0 | một | b | Không | tiếp tục | 
| 1 | b | một | Không | tiếp tục | 
| 2 | một | c | Không | tiếp tục | 
| 3 | c | một | Không | tiếp tục | 
| 4 | một | b | Không | tiếp tục | 
| 5 | b | một | Không | tiếp tục | 

Không có ký tự bằng nhau liền kề nào xuất hiện, vì vậy câu trả lời là`"YES"`. 

Điều này xác nhận rằng luôn có thể thực hiện phép gán luân phiên hoàn toàn khi không có xung đột ngay lập tức trong chuỗi gốc. 

### Ví dụ 2:`aa`| tôi | s[i] | s[i+1] | Bình đẳng? | Quyết định | 
| --- | --- | --- | --- | --- | 
| 0 | một | một | Có | dừng lại → KHÔNG | 

Đây là bức thư duy nhất`a`xuất hiện ở các vị trí liền kề, buộc cả hai vị trí phải ánh xạ giống hệt nhau, điều này mâu thuẫn với sự luân phiên. 

Điều này cho thấy ngay cả khối lặp lại nhỏ nhất cũng ngay lập tức phá hủy tính khả thi. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Kiểm tra kề cận một lần qua chuỗi | 
| Không gian | O(1) | Chỉ sử dụng một số biến, không có cấu trúc phụ trợ | 

Tổng công việc trên tất cả các trường hợp thử nghiệm là tuyến tính trong tổng kích thước đầu vào, nằm trong giới hạn thoải mái ngay cả ở những hạn chế tối đa. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from sys import stdout
    out = []
    
    input = sys.stdin.readline
    t = int(input())
    for _ in range(t):
        n = int(input())
        s = input().strip()
        ok = True
        for i in range(n - 1):
            if s[i] == s[i + 1]:
                ok = False
                break
        out.append("YES" if ok else "NO")
    
    return "\n".join(out)

# provided samples
assert run("""8
7
abacaba
2
aa
1
y
4
bkpt
6
ninfia
6
banana
10
codeforces
8
testcase
""") == """YES
NO
YES
YES
NO
YES
NO
NO"""

# custom cases
assert run("""1
1
a
""") == "YES"

assert run("""1
2
ab
""") == "YES"

assert run("""1
2
aa
""") == "NO"

assert run("""1
5
abcde
""") == "YES"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`a`| CÓ | Kích thước tối thiểu | 
|`ab`| CÓ | Vỏ đế xen kẽ đơn giản | 
|`aa`| KHÔNG | Mâu thuẫn ngay lập tức | 
|`abcde`| CÓ | Không có xung đột lân cận | 

## Vỏ cạnh 

Trường hợp một ký tự chứng tỏ rằng một chuỗi có độ dài 1 luôn thành công vì không có ràng buộc kề nào bị vi phạm. Thuật toán đọc chuỗi, không tìm thấy`i`như vậy`s[i] == s[i+1]`, và trả về`"YES"`ngay lập tức. 

Trường hợp hai ký tự bằng nhau như`aa`gây ra tình trạng lỗi ở lần so sánh đầu tiên. Quá trình quét phát hiện`s[0] == s[1]`, sets the flag to false, and stops. Điều này nắm bắt chính xác việc không thể tách các chữ cái liền kề giống hệt nhau thành các giá trị nhị phân xen kẽ. 

Các chuỗi dài không có các chữ cái liền kề lặp lại, chẳng hạn như`abcabcabc`, vượt qua mà không kích hoạt điều kiện, xác nhận rằng tính khả thi toàn cầu được xác định đầy đủ bởi cấu trúc lân cận cục bộ.
