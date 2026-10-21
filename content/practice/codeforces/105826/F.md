---
title: "CF 105826F - \u041b\u044f\u0433\u0443\u0448\u043a\u0430 \u0438 \u0441\u0442\u0440\u043e\u043a\u0430"
description: "Bài toán mô tả một con ếch di chuyển dọc theo một sợi dây. Mỗi vị trí chứa một ký tự và các ký tự thuộc một trong ba loại: chữ thường, chữ hoa hoặc chữ số. Con ếch xuất phát ở vị trí đầu tiên và muốn đến vị trí cuối cùng."
date: "2026-06-25T14:58:20+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105826
codeforces_index: "F"
codeforces_contest_name: "\u041e\u0442\u0431\u043e\u0440 \u043d\u0430 \u0412\u041a\u041e\u0428\u041f.Junior 2025"
rating: 0
weight: 105826
solve_time_s: 29
verified: true
draft: false
---

[CF 105826F - \u041b\u044f\u0433\u0443\u0448\u043a\u0430 \u0438 \u0441\u0442\u0440\u043e\u043a\u0430](https://codeforces.com/problemset/problem/105826/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 29s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Bài toán mô tả một con ếch di chuyển dọc theo một sợi dây. Mỗi vị trí chứa một ký tự và các ký tự thuộc một trong ba loại: chữ thường, chữ hoa hoặc chữ số. Con ếch xuất phát ở vị trí đầu tiên và muốn đến vị trí cuối cùng. Một cú nhảy có thể đẩy con ếch về phía trước nhiều nhất`k`các vị trí, nhưng ếch chỉ được phép đáp xuống các vị trí có cùng đặc điểm với vị trí hiện tại của nó. 

Nhiệm vụ là tìm số lần nhảy tối thiểu cần thiết để về đích hoặc báo cáo rằng điều đó là không thể. 

Đầu vào cung cấp độ dài của chuỗi, độ dài bước nhảy tối đa và chính chuỗi đó. Đầu ra là số bước nhảy hợp lệ nhỏ nhất cần thiết để di chuyển từ ký tự đầu tiên đến ký tự cuối cùng. 

Độ dài của chuỗi có thể đạt tới 200.000. Điều đó ngay lập tức loại trừ các giải pháp liên tục thử mọi con đường có thể, bởi vì số lượng các con đường khả thi có thể tăng theo cấp số nhân. Ngay cả một cách tiếp cận kiểm tra từng cặp vị thế cũng có thể áp dụng được`n²`, tức là khoảng 40 tỷ thao tác ở giới hạn trên và vượt xa giới hạn thời gian thông thường cho phép. 

Điều quan trọng là con ếch chỉ di chuyển về bên phải. Không có chu kỳ, vì vậy chúng tôi có thể xử lý các vị trí theo thứ tự và chỉ giữ lại thông tin quan trọng cho các bước nhảy trong tương lai. 

Một số trường hợp rất dễ bỏ sót. 

Nếu ký tự đầu tiên và cuối cùng có danh mục khác nhau thì câu trả lời là không thể. Ví dụ:```
5 3
aB123
```Ký tự đầu tiên là chữ thường, ký tự cuối cùng là chữ số. Con ếch không bao giờ có thể thay đổi loại, vì vậy câu trả lời đúng là:```
-1
```Việc thực hiện bất cẩn có thể chỉ kiểm tra xem liệu một số vị trí đúng loại có tồn tại ở gần cuối hay không, nhưng điều đó là chưa đủ vì ếch phải bảo toàn danh mục của nó trong toàn bộ đường đi. 

Một trường hợp phức tạp khác là khi có đúng danh mục nhưng có khoảng cách lớn hơn`k`. 

Ví dụ:```
6 2
aaabca
```Ký tự đầu tiên và cuối cùng là chữ thường, nhưng vị trí chữ thường là:`1, 2, 3, 6`Bước nhảy từ vị trí 3 đến vị trí 6 có độ dài 3, quá lớn. Câu trả lời là:```
-1
```Việc triển khai chỉ tính số lượng vị trí hợp lệ tồn tại sẽ cho rằng một đường dẫn tồn tại một cách không chính xác. 

Trường hợp cạnh cuối cùng là khi mỗi bước nhảy hữu ích chính xác có độ dài tối đa cho phép. 

Ví dụ:```
7 3
aaaaaaa
```Con ếch có thể nhảy:`1 -> 4 -> 7`vậy câu trả lời là:```
2
```Sử dụng một cách nghiêm ngặt`< k`so sánh thay vì`<= k`sẽ thất bại ở đây. 

## Phương pháp tiếp cận 

Cách đơn giản để giải quyết vấn đề là mô phỏng tất cả các chuyển động có thể xảy ra. Từ mọi vị trí có thể tiếp cận, chúng tôi thử mọi vị trí tiếp theo trong khoảng cách`k`, kiểm tra xem ký tự của nó có cùng loại hay không và tiếp tục. Điều này đúng vì mọi con đường có thể đều được khám phá. 

Vấn đề là khối lượng công việc. Trong trường hợp xấu nhất, mọi vị trí đều có thể thử gần như`k`các vị trí tiếp theo. Vì cả hai`n`Và`k`có thể gần 200.000, điều này trở nên gần đúng`O(nk)`, nó quá lớn. 

Quan sát hữu ích xuất phát từ thực tế là tất cả các bước nhảy chỉ tiến về phía trước. Chúng ta không cần phải biết mọi con đường có thể. Chúng tôi chỉ cần câu trả lời tốt nhất cho vị trí có thể tiếp cận mới nhất của từng loại nhân vật. 

Giả sử chúng ta hiện đang ở một vị trí nào đó. Bất kỳ vị trí nào trong tương lai thuộc cùng loại đó đều có thể đạt được từ vị trí xa nhất có thể tiếp cận phía sau nó, vì điều đó mang lại khoảng cách nhảy lớn nhất còn lại. Nếu vị trí xa nhất có thể tiếp cận được ít nhất là`i - k`, thì có thể truy cập được vị trí hiện tại. 

Điều này biến vấn đề thành một lần quét duy nhất. Trong khi di chuyển từ trái sang phải, chúng tôi giữ vị trí có thể truy cập cuối cùng cho từng danh mục trong số ba danh mục. Bất cứ khi nào chúng tôi gặp một ký tự, chúng tôi sẽ kiểm tra xem vị trí có thể tiếp cận trước đó của danh mục đó có đủ gần hay không. Nếu đúng như vậy, chúng tôi sẽ cập nhật số lần nhảy cần thiết và đánh dấu vị trí này là có thể truy cập được. 

Ba loại có thể được nén thành ba trạng thái. Không cần bản đồ hoặc cấu trúc dữ liệu phức tạp vì các nhóm bảng chữ cái đã cố định. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(nk) | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Chuyển đổi mọi ký tự thành một trong ba mã định danh danh mục. Mỗi chữ cái viết thường, chữ in hoa và chữ số đều có trạng thái riêng. Điều này cho phép thuật toán so sánh các danh mục mà không quan tâm đến các ký tự thực tế. 
2. Bắt đầu ở vị trí đầu tiên. Nó luôn có thể truy cập được mà không cần nhảy, vì vậy hãy lưu trữ nó làm vị trí có thể truy cập mới nhất cho danh mục của nó. 
3. Quét chuỗi từ trái sang phải. Đối với mọi vị trí, hãy xem vị trí có thể tiếp cận gần đây nhất với cùng danh mục. 

Nếu vị trí đó nằm trong`k`ký tự phía sau cái hiện tại thì con ếch có thể nhảy vào đó. Vị trí hiện tại trở nên có thể truy cập được. 
4. Khi có thể truy cập được một vị trí, hãy cập nhật số lần nhảy cần thiết để đến đó. Số lần nhảy tối thiểu được tìm thấy bằng cách lấy câu trả lời của vị trí trước đó và thêm một câu trả lời. 

Các vị trí có thể truy cập được lưu trữ luôn là những vị trí xa nhất có thể cho danh mục của chúng. Vị trí có thể tiếp cận xa hơn ít nhất luôn hữu ích như vị trí gần hơn vì nó giúp ếch có nhiều khoảng cách hơn cho lần nhảy tiếp theo. 
5. Sau khi xử lý toàn bộ chuỗi, hãy kiểm tra xem vị trí cuối cùng có thể truy cập được hay không. Nếu không thì in`-1`. 

Tại sao nó hoạt động: 

Điều bất biến là sau khi xử lý bất kỳ tiền tố nào của chuỗi, đối với mỗi danh mục, chúng tôi lưu trữ vị trí xa nhất trong tiền tố đó mà ếch có thể tiếp cận, cùng với số lần nhảy tối thiểu cần thiết để đến đó. Khi chúng tôi xử lý một vị trí mới, mọi bước nhảy hợp lệ trước đó đều phải thuộc cùng một danh mục. Vị trí có thể tiếp cận xa nhất là vị trí duy nhất quan trọng vì nếu nó không thể tiếp cận được vị trí hiện tại thì không có vị trí nào trước đó có thể làm được. Nếu nó có thể đạt đến vị trí hiện tại, việc sử dụng nó sẽ mang lại số lần nhảy nhỏ nhất có thể. Điều này giữ cho bất biến đúng cho mọi vị trí và đảm bảo câu trả lời cuối cùng là tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, k = map(int, input().split())
    s = input().strip()

    def get_type(c):
        if 'a' <= c <= 'z':
            return 0
        if 'A' <= c <= 'Z':
            return 1
        return 2

    last = [-1, -1, -1]
    dist = [-1] * n

    dist[0] = 0
    last[get_type(s[0])] = 0

    for i in range(1, n):
        t = get_type(s[i])

        if last[t] != -1 and i - last[t] <= k:
            dist[i] = dist[last[t]] + 1
            last[t] = i

    print(dist[-1])

if __name__ == "__main__":
    solve()
```các`get_type`hàm giảm mọi ký tự xuống một trong ba trạng thái. Điều này tránh việc kiểm tra liên tục các phạm vi trong suốt thuật toán và giữ cho vòng lặp chính đơn giản. 

các`last`mảng lưu trữ vị trí có thể tiếp cận xa nhất cho mỗi danh mục. Chỉ cập nhật nó khi một vị trí có thể tiếp cận được là cần thiết, bởi vì một vị trí không thể tiếp cận được sẽ không thể giúp ích cho những lần nhảy trong tương lai. 

Mảng khoảng cách lưu trữ số lần nhảy tối thiểu cần thiết để đến mọi vị trí. Khi chúng tôi tìm thấy bước nhảy hợp lệ, câu trả lời hiện tại chính xác hơn câu trả lời của vị trí có thể truy cập trước đó một câu trả lời. 

Việc kiểm tra ranh giới sử dụng`<= k`bởi vì một bước nhảy có độ dài chính xác`k`được cho phép. Các vị trí được xử lý từ trái sang phải nên không cần phải di chuyển lùi hoặc xem lại trạng thái.
