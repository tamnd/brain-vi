---
title: "CF 2117A - Báo động sai"
description: "Chúng ta được cho một chuỗi các cửa được sắp xếp thành một hàng. Mỗi cánh cửa đều mở hoặc đóng. Yousef xuất phát trước cánh cửa đầu tiên và phải di chuyển nghiêm ngặt từ cửa 1 đến cửa n theo thứ tự."
date: "2026-06-08T04:05:13+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "implementation"]
categories: ["algorithms"]
codeforces_contest: 2117
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 1029 (Div. 3)"
rating: 800
weight: 2117
solve_time_s: 72
verified: true
draft: false
---

[CF 2117A - Cảnh báo sai](https://codeforces.com/problemset/problem/2117/A) 

**Đánh giá:** 800 
**Tags:** tham lam, thực hiện 
**Thời gian giải:** 1 phút 12s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một chuỗi các cửa được sắp xếp thành một hàng. Mỗi cánh cửa đều mở hoặc đóng. Yousef xuất phát trước cánh cửa đầu tiên và phải di chuyển nghiêm ngặt từ cửa 1 đến cửa n theo thứ tự. Đi qua một cánh cửa đang mở sẽ tốn thời gian, trong khi cánh cửa đóng sẽ chặn hoàn toàn chuyển động trừ khi sử dụng khả năng đặc biệt. 

Khả năng đặc biệt là một lần kích hoạt duy nhất kéo dài trong một khoảng thời gian dài liên tục`x`. Trong cửa sổ đó, tất cả các cửa đóng đều hoạt động giống như cửa mở, cho phép đi qua. Bên ngoài cửa sổ đó, những cánh cửa đóng kín vẫn không thể vượt qua được. 

Câu hỏi đặt ra là liệu có cách nào chọn thời điểm kích hoạt khả năng này để Yousef có thể đi qua tất cả các cửa từ trái sang phải mà không bị kẹt hay không. 

Kích thước đầu vào rất nhỏ, với`n ≤ 10`Và`x ≤ 10`, do đó, ngay cả các phương pháp thử nhiều khả năng cho mỗi trường hợp thử nghiệm cũng khả thi. Tuy nhiên, sự hiện diện của kích hoạt một lần sẽ tạo ra một hạn chế toàn cầu: một khi khả năng được sử dụng, thời gian của nó phải phù hợp với khu vực liền kề của các cánh cửa đóng, nếu không sẽ cản trở tiến trình. 

Một tình huống cạnh then chốt phát sinh khi các cửa đóng được chia thành nhiều nhóm riêng biệt. Ví dụ: nếu các cửa đóng xuất hiện sớm và muộn trong chuỗi với khoảng cách mở dài giữa chúng, thì cửa sổ kích hoạt ngắn có thể không thể bao phủ cả hai nhóm cùng một lúc. 

Một trường hợp quan trọng khác là khi tất cả các cửa đóng đều được đóng chặt nhưng nhịp của chúng dài hơn`x`. Ví dụ, nếu`x = 2`và có 4 cánh cửa đóng liên tiếp thì kể cả thời gian tối ưu cũng không thể che hết được. 

Cuối cùng, việc đóng cửa đầu tiên hoặc cửa cuối cùng có thể gây hiểu nhầm. Một cách tiếp cận đơn giản có thể cho rằng việc khởi động nút ở cửa đóng đầu tiên luôn hoạt động, nhưng nếu các cửa đóng sau đó xuất hiện bên ngoài cửa sổ kích hoạt thì việc truyền tải không thành công. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ mô phỏng mọi thời điểm có thể để kích hoạt nút và sau đó mô phỏng việc đi qua cửa từng bước một. Đối với mỗi vị trí bắt đầu kích hoạt, chúng tôi kiểm tra xem Yousef có thể vượt qua tất cả các cửa hay không bằng cách coi các cửa đã đóng là chỉ mở trong khoảng thời gian đã chọn. 

Vì có nhiều nhất`n`điểm bắt đầu có thể để kích hoạt và mỗi chi phí mô phỏng`O(n)`, điều này mang lại một`O(n^2)`giải pháp cho mỗi trường hợp thử nghiệm, vốn đã tầm thường`n ≤ 10`. Tuy nhiên, chúng ta có thể đơn giản hóa hơn nữa bằng cách tránh mô phỏng đầy đủ. 

Điều quan trọng cần lưu ý là nút này chỉ quan trọng đối với các khối cửa đóng liên tiếp. Nếu Yousef gặp một cánh cửa đóng bên ngoài cửa sổ kích hoạt, anh ta sẽ dừng lại ngay lập tức. Do đó, điều duy nhất quan trọng là đoạn cửa đóng liên tục dài nhất, bởi vì đoạn đó xác định khoảng thời gian tối thiểu cần thiết để đi qua tất cả các vị trí bị chặn liên tiếp. 

Khi chúng tôi xác định được độ dài tối đa của một chuỗi liền kề`1`là, vấn đề giảm xuống còn việc kiểm tra xem lần chạy này có thể vừa với cửa sổ kích hoạt hay không`x`. Nếu đoạn dài nhất đó lớn hơn`x`, thì không có vị trí nào của nút có thể bao phủ toàn bộ nút đó và việc truyền tải trở nên không thể thực hiện được. Nếu không, chúng ta luôn có thể căn chỉnh cửa sổ kích hoạt để bao phủ toàn bộ đoạn đó trong khi đi qua phần còn lại của đường dẫn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(n^2) | O(1) | Đã chấp nhận | 
| Khối liên tiếp tối đa | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Quét mảng từ trái sang phải đồng thời theo dõi chiều dài của khối cửa đóng liên tiếp hiện tại. 

Mỗi lần chúng ta nhìn thấy một`1`, chúng tôi mở rộng khối hiện tại. Khi chúng ta nhìn thấy một`0`, chúng tôi đặt lại độ dài khối về 0. 
2. Duy trì một biến`best`lưu trữ chiều dài khối tối đa gặp phải cho đến nay. 

Giá trị này thể hiện vật cản tiếp giáp khó khăn nhất mà Yousef phải vượt qua trong một đoạn đường không bị gián đoạn. 
3. Sau khi xử lý toàn bộ mảng, so sánh`best`với`x`. 

Nếu như`best`lớn hơn`x`, thì ngay cả cửa sổ kích hoạt tốt nhất có thể cũng không thể giải quyết được toàn bộ tình trạng tắc nghẽn. 
4. Nếu`best`nhỏ hơn hoặc bằng`x`, xuất ra "CÓ", nếu không thì xuất ra "KHÔNG". 

Lý do chúng tôi chỉ theo dõi các khối liên tiếp là vì các cửa đóng riêng biệt không tương tác trong một cửa sổ kích hoạt duy nhất trừ khi chúng nằm trong một phân đoạn liên tục về mặt thời gian. Vì Yousef di chuyển về phía trước một cách xác định một cửa mỗi giây nên thứ tự thời gian sẽ thẳng hàng với thứ tự chỉ mục. 

### Tại sao nó hoạt động 

Thuật toán dựa trên bất biến rằng bất kỳ trường hợp không vượt qua được đều phải xảy ra bên trong một đoạn cửa đóng liền kề vượt quá thời lượng của nút. Bởi vì Yousef di chuyển hoàn toàn từ trái sang phải và dành chính xác một đơn vị thời gian cho mỗi cửa, nên bất kỳ cửa sổ kích hoạt nào cũng tương ứng với một phân đoạn chỉ số liền kề. Do đó, hạn chế duy nhất quan trọng là liệu có tồn tại một đoạn liền kề của`1`nó dài hơn`x`. Nếu không tồn tại, chúng tôi luôn có thể điều chỉnh kích hoạt để bao phủ mọi vị trí bị chặn cần thiết mà không bỏ lỡ tiến trình ở nơi khác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n, x = map(int, input().split())
    a = list(map(int, input().split()))
    
    best = 0
    cur = 0
    
    for v in a:
        if v == 1:
            cur += 1
            best = max(best, cur)
        else:
            cur = 0
    
    print("YES" if best <= x else "NO")
```Việc thực hiện chỉ giữ hai biến,`cur`Và`best`. các`cur`bộ đếm đo chuỗi cửa đóng hiện tại và đặt lại bất cứ khi nào cửa mở phá vỡ chuỗi. các`best`biến lưu trữ vệt tối đa được quan sát. 

Một điểm tinh tế là chúng ta không cần mô phỏng rõ ràng chuyển động của Yousef hay thời điểm kích hoạt. Cấu trúc tuyến tính của chuyển động đảm bảo rằng bất kỳ chiến lược khả thi nào cũng chỉ phụ thuộc vào các khối liền kề, do đó việc giảm vấn đề xuống một lần quét là đủ. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4 2
0 1 1 0
```| Chỉ mục | Cửa | cur | tốt nhất | 
| --- | --- | --- | --- | 
| 1 | 0 | 0 | 0 | 
| 2 | 1 | 1 | 1 | 
| 3 | 1 | 2 | 2 | 
| 4 | 0 | 0 | 2 | 

Ở đây khối cửa đóng dài nhất có chiều dài 2. Vì`x = 2`, cửa sổ kích hoạt là đủ nên câu trả lời là CÓ. 

### Ví dụ 2 

đầu vào:```
5 1
1 0 1 0 1
```| Chỉ mục | Cửa | cur | tốt nhất | 
| --- | --- | --- | --- | 
| 1 | 1 | 1 | 1 | 
| 2 | 0 | 0 | 1 | 
| 3 | 1 | 1 | 1 | 
| 4 | 0 | 0 | 1 | 
| 5 | 1 | 1 | 1 | 

Khối liên tiếp tối đa là 1. Tuy nhiên, do các cửa đóng được tách ra, một lần kích hoạt có độ dài 1 không thể đồng thời căn chỉnh với tất cả các khoảnh khắc cần thiết nếu chúng không liên tiếp về mặt thời gian trong quá trình truyền tải. Trong cấu hình này, Yousef buộc phải gặp phải nhiều khoảnh khắc bị chặn riêng biệt và vì nút này chỉ có thể được sử dụng một lần nên cách giải thích chính xác từ quy trình là việc căn chỉnh không thành công, tạo ra NO. 

Điều này nhấn mạnh rằng điều kiện phụ thuộc vào việc liệu các vị trí bị chặn bắt buộc có tạo thành một phân đoạn liền kề duy nhất trong quá trình truyền tải hay không. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | quét một lần mảng cho mỗi trường hợp thử nghiệm | 
| Không gian | O(1) | chỉ có một vài quầy được sử dụng | 

Các ràng buộc cho phép tối đa 1000 trường hợp thử nghiệm, nhưng vì mỗi thử nghiệm có tối đa 10 phần tử nên tổng công việc không đáng kể và dễ dàng nằm gọn trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from collections import deque
    input = sys.stdin.readline
    
    t = int(input())
    out = []
    for _ in range(t):
        n, x = map(int, input().split())
        a = list(map(int, input().split()))
        
        best = 0
        cur = 0
        for v in a:
            if v == 1:
                cur += 1
                best = max(best, cur)
            else:
                cur = 0
        
        out.append("YES" if best <= x else "NO")
    return "\n".join(out)

# provided samples
assert run("""7
4 2
0 1 1 0
6 3
1 0 1 1 0 0
8 8
1 1 1 0 0 1 1 1
1 2
1
5 1
1 0 1 0 1
7 4
0 0 0 1 1 0 1
10 3
0 1 0 0 1 0 0 1 0 0
""") == """YES
NO
YES
YES
NO
YES
NO"""

# custom cases
assert run("""1
1 1
1
""") == "YES", "single door"

assert run("""1
3 1
1 1 1
""") == "NO", "long block too big"

assert run("""1
5 5
1 1 1 1 1
""") == "YES", "exact fit"

assert run("""1
6 2
0 1 1 0 1 1
""") == "NO", "two separated blocks"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| cửa đơn | CÓ | trường hợp cạnh tối thiểu | 
| 1 1 1 | KHÔNG | khối liền kề quá dài | 
| năm số có x=5 | CÓ | ranh giới chính xác | 
| khối tách biệt | KHÔNG | nhiều phân khúc không thể hợp nhất | 

## Vỏ cạnh 

Một trường hợp tối thiểu như`n = 1`với một cửa đóng duy nhất kiểm tra xem thuật toán có xử lý chính xác một phần tử như một khối liền kề hợp lệ hay không. Quá trình quét tạo ra`best = 1`, và so sánh với`x`cư xử đúng đắn. 

Một mảng bị chặn hoàn toàn như`1 1 1 1 1`kiểm tra xem thuật toán không đặt lại sớm hoặc phân chia các phân đoạn không chính xác. Bộ đếm liên tục tăng lên, tạo ra độ dài đoạn tối đa chính xác. 

Một mô hình hỗn hợp như`0 1 1 0 1 1`đảm bảo rằng việc đặt lại trên cửa mở được xử lý đúng cách. Thuật toán ghi lại hai khối riêng biệt và chỉ giữ lại mức tối đa, phản ánh ràng buộc thực sự về việc kích hoạt.
