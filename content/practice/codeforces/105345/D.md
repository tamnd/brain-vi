---
title: "CF 105345D - Cơn ác mộng ngày 24"
description: "Chúng tôi được cấp một dãy các tòa nhà, mỗi tòa nhà có một số học sinh. Freddy luôn bắt đầu từ tòa nhà đầu tiên và di chuyển hoàn toàn về bên phải. Khi đến thăm các tòa nhà theo thứ tự, anh ấy sẽ tích lũy số lượng học sinh mà anh ấy đã gặp cho đến nay."
date: "2026-06-23T05:48:41+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105345
codeforces_index: "D"
codeforces_contest_name: "UTPC Contest 09-13-24 Div. 1 (Advanced)"
rating: 0
weight: 105345
solve_time_s: 75
verified: false
draft: false
---

[CF 105345D - Cơn ác mộng ngày 24](https://codeforces.com/problemset/problem/105345/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 15s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cấp một dãy các tòa nhà, mỗi tòa nhà có một số học sinh. Freddy luôn bắt đầu từ tòa nhà đầu tiên và di chuyển hoàn toàn về bên phải. Khi đến thăm các tòa nhà theo thứ tự, anh ấy sẽ tích lũy số lượng học sinh mà anh ấy đã gặp cho đến nay. 

Đối với mỗi giá trị truy vấn, chúng ta được hỏi một câu hỏi hậu cần đơn giản: Freddy phải đi bao xa tính từ đầu để tổng số học sinh mà anh ta gặp ít nhất bằng giá trị truy vấn đó. Nếu ngay cả sau khi đã ghé thăm tất cả các tòa nhà mà tổng số vẫn nhỏ hơn truy vấn thì câu trả lời là không thể. 

Đối tượng chính là sự tích lũy tiền tố của mảng. Mọi truy vấn đều yêu cầu độ dài tiền tố nhỏ nhất có tổng đạt hoặc vượt mục tiêu. 

Các ràng buộc đẩy chúng ta tới quá trình tiền xử lý tuyến tính hoặc gần tuyến tính. Với tối đa 100000 tòa nhà và 100000 truy vấn, mọi giải pháp tính toán lại tổng cho mỗi truy vấn sẽ không thành công. Vòng lặp kép sẽ dẫn đến 10^10 thao tác trong trường hợp xấu nhất, vượt xa giới hạn 1 giây. 

Một trường hợp phức tạp xuất hiện khi các truy vấn yêu cầu không có sinh viên nào. Trong trường hợp đó, câu trả lời sẽ là không có tòa nhà nào, vì Freddy đã đáp ứng yêu cầu trước khi vào bất kỳ tòa nhà nào. Một trường hợp khác là khi các tòa nhà chứa số 0, có thể tạo ra những đoạn dài mà tổng tiền tố không thay đổi. Cách tiếp cận ngây thơ “dừng lại khi tổng số tiền tăng lên” sẽ bị phá vỡ ở đây vì tiến độ không gắn liền với việc tham quan các tòa nhà mới. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp xử lý từng truy vấn một cách độc lập. Đối với mục tiêu q nhất định, chúng tôi mô phỏng việc đi bộ từ tòa nhà đầu tiên và tiếp tục cộng số học sinh cho đến khi tổng đạt q. Điều này đúng vì nó tuân theo quy luật chuyển động chính xác. Tuy nhiên, mỗi truy vấn có thể yêu cầu quét hầu hết tất cả các tòa nhà. Với m truy vấn, giá trị này trở thành O(nm), quá chậm khi cả n và m đều lớn. 

Cấu trúc của vấn đề là đơn điệu. Khi chúng ta di chuyển sang phải, tổng tiền tố không bao giờ giảm. Tính đơn điệu này có nghĩa là một khi chúng ta tính toán trước các tổng tiền tố, mỗi truy vấn sẽ trở thành một vấn đề tìm kiếm trên một mảng được sắp xếp (không giảm). Thay vì mô phỏng lại mọi truy vấn, chúng ta có thể trả lời từng truy vấn bằng cách xác định tổng tiền tố đầu tiên đạt đến ngưỡng yêu cầu. 

Điều này làm giảm nhiệm vụ tìm kiếm nhị phân trên mảng tổng tiền tố. Mỗi truy vấn được trả lời theo thời gian logarit, sau một lần xử lý trước tuyến tính duy nhất. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(nm) | O(1) | Quá chậm | 
| Tổng tiền tố + Tìm kiếm nhị phân | O(n + m log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính tổng tiền tố của mảng xây dựng. Giá trị tại vị trí i biểu thị tổng số học sinh từ tòa nhà 1 đến tòa nhà i. Điều này biến vấn đề thành một mảng đơn điệu trong đó mỗi bước không bao giờ giảm. 
2. Đối với mỗi giá trị truy vấn q, hãy kiểm tra xem nó có vượt quá tổng tiền tố cuối cùng hay không. Nếu đúng như vậy thì không có tiền tố nào có thể đáp ứng yêu cầu nên câu trả lời là -1. Điều này tránh việc tìm kiếm không cần thiết. 
3. Nếu q bằng 0, trả về 0 ngay lập tức vì không cần tòa nhà nào để tiếp cận 0 học sinh. 
4. Mặt khác, thực hiện tìm kiếm nhị phân trên mảng tổng tiền tố để tìm chỉ số nhỏ nhất i sao cho tiền tố[i] lớn hơn hoặc bằng q. Điều này hoạt động vì mảng tiền tố được sắp xếp không giảm. 
5. Kết quả i + 1 nếu sử dụng chỉ số dựa trên 0 trong nội bộ, vì câu trả lời được xác định theo số lượng tòa nhà đã ghé thăm. 

### Tại sao nó hoạt động

Tính đúng đắn xuất phát từ tính đơn điệu của các tổng tiền tố. Khi tổng tiền tố đạt đến một giá trị nhất định thì tất cả các tiền tố sau này ít nhất cũng lớn bằng giá trị đó. Điều này đảm bảo rằng tập hợp các câu trả lời hợp lệ tạo thành một hậu tố chỉ số liền kề. Tìm kiếm nhị phân khai thác chính xác cấu trúc này bằng cách liên tục giảm một nửa không gian tìm kiếm trong khi vẫn giữ nguyên tính bất biến rằng bất kỳ câu trả lời hợp lệ nào cũng phải nằm ở bên phải của tất cả các vị trí có tổng không đủ và ở bên trái của tất cả các vị trí đã được xác nhận đủ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
import bisect

def solve():
    n, m = map(int, input().split())
    a = list(map(int, input().split()))
    q = list(map(int, input().split()))
    
    pref = []
    s = 0
    for x in a:
        s += x
        pref.append(s)
    
    total = pref[-1] if pref else 0
    
    out = []
    for x in q:
        if x == 0:
            out.append("0")
            continue
        if x > total:
            out.append("-1")
            continue
        idx = bisect.bisect_left(pref, x)
        out.append(str(idx + 1))
    
    print("\n".join(out))

if __name__ == "__main__":
    solve()
```Việc triển khai bắt đầu bằng việc xây dựng mảng tổng tiền tố trong một lần duyệt qua các tòa nhà. Điều này đảm bảo tiền xử lý O(n). 

Mỗi truy vấn được xử lý độc lập. Trường hợp cạnh x == 0 được trả về rõ ràng là 0 trước bất kỳ tìm kiếm nào, khớp với định nghĩa rằng không cần tòa nhà nào. 

Thao tác chính là`bisect_left`, tìm vị trí đầu tiên trong đó tổng tiền tố ít nhất là giá trị truy vấn. Việc thêm 1 sẽ chuyển đổi chỉ số dựa trên số 0 thành số lượng tòa nhà. 

Việc kiểm tra tổng sẽ ngăn chặn các tìm kiếm nhị phân không cần thiết khi truy vấn vượt quá tất cả học sinh có thể tiếp cận. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 7, a = [10, 8, 2, 4, 4, 8, 9]
queries = [10, 15, 25, 41, 100]
```Tổng tiền tố: 

| tôi | một [tôi] | tiền tố | 
| --- | --- | --- | 
| 1 | 10 | 10 | 
| 2 | 8 | 18 | 
| 3 | 2 | 20 | 
| 4 | 4 | 24 | 
| 5 | 4 | 28 | 
| 6 | 8 | 36 | 
| 7 | 9 | 45 | 

Dấu vết truy vấn: 

| q | mục tiêu tìm kiếm nhị phân | chỉ số kết quả | trả lời | 
| --- | --- | --- | --- | 
| 10 | đầu tiên ≥ 10 | 0 | 1 | 
| 15 | đầu tiên ≥ 15 | 1 | 2 | 
| 25 | đầu tiên ≥ 25 | 4 | 5 | 
| 41 | đầu tiên ≥ 41 | 6 | 7 | 
| 100 | vượt quá tổng số | không | -1 | 

Ví dụ này cho thấy cách tổng tiền tố hoạt động như một bảng tra cứu đơn điệu. Mỗi truy vấn tương ứng với việc chuyển trực tiếp đến tiền tố đầu tiên vượt qua ngưỡng. 

### Ví dụ 2 

đầu vào:```
a = [0, 0, 5, 0, 3]
queries = [0, 1, 5, 8]
```Tổng tiền tố: 

| tôi | tiền tố | 
| --- | --- | 
| 1 | 0 | 
| 2 | 0 | 
| 3 | 5 | 
| 4 | 5 | 
| 5 | 8 | 

Hành vi truy vấn: 

| q | lý luận | trả lời | 
| --- | --- | --- | 
| 0 | tiền tố trống là hợp lệ | 0 | 
| 1 | tiền tố đầu tiên ≥ 1 là thứ 3 | 3 | 
| 5 | tiền tố đầu tiên ≥ 5 là thứ 3 | 3 | 
| 8 | cần đầy đủ mảng | 5 | 

Điều này nhấn mạnh cách số 0 không phá vỡ tính chính xác. Mặc dù tiến trình bị đình trệ sớm, tổng tiền tố vẫn đơn điệu và tìm kiếm nhị phân vẫn tìm thấy ranh giới chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + m log n) | Một lượt xây dựng tổng tiền tố, mỗi truy vấn sử dụng tìm kiếm nhị phân | 
| Không gian | O(n) | Mảng tiền tố lưu trữ một giá trị cho mỗi tòa nhà | 

Các ràng buộc cho phép tổng số lên tới 200000 phần tử trên các mảng đầu vào và hành vi logarit trên mỗi truy vấn phù hợp thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import bisect

    def solve():
        n, m = map(int, input().split())
        a = list(map(int, input().split()))
        q = list(map(int, input().split()))
        
        pref = []
        s = 0
        for x in a:
            s += x
            pref.append(s)
        
        total = pref[-1] if pref else 0
        
        out = []
        for x in q:
            if x == 0:
                out.append("0")
            elif x > total:
                out.append("-1")
            else:
                idx = bisect.bisect_left(pref, x)
                out.append(str(idx + 1))
        
        return "\n".join(out)

    return solve()

# provided sample
assert run("""7 5
10 8 2 4 4 8 9
10 15 25 41 100
""") == """1
2
5
7
-1"""

# minimum size
assert run("""1 3
5
0 5 6
""") == """0
1
-1"""

# all zeros
assert run("""5 3
0 0 0 0 0
0 1 10
""") == """0
-1
-1"""

# increasing simple
assert run("""4 4
1 2 3 4
1 3 6 10
""") == """1
2
3
4"""

# boundary exact match
assert run("""3 2
2 2 2
6 7
""") == """3
-1"""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 phần tử | hành vi ranh giới trực tiếp | độ chính xác đầu vào nhỏ nhất | 
| tất cả số không | xử lý ứ đọng | tiền tố không tăng | 
| tăng đơn giản | trường hợp đơn điệu sạch sẽ | tính chính xác của tìm kiếm nhị phân | 
| ranh giới chính xác | xử lý bình đẳng | hành vi giới hạn dưới | 

## Vỏ cạnh 

Trường hợp không có truy vấn như đầu vào`[5, 2, 3]`với truy vấn`0`ngay lập tức trở lại`0`bởi vì điều kiện tổng tiền tố đã được thỏa mãn trước khi đến thăm bất kỳ tòa nhà nào. Thuật toán xử lý việc này trước khi xây dựng tìm kiếm, do đó không thực hiện lập chỉ mục. 

Một mảng hoàn toàn bằng 0 như`[0, 0, 0]`tạo ra tổng tiền tố`[0, 0, 0]`. Đối với một truy vấn như`1`, tìm kiếm nhị phân trả về chỉ mục đầu tiên, nhưng vì tổng vẫn bằng 0 nên việc kiểm tra trước`x > total`đầu ra chính xác`-1`. Điều này tránh hiểu sai các số 0 lặp đi lặp lại là sự tiến bộ. 

Một truy vấn bằng tổng, ví dụ như mảng`[3, 1, 2]`và truy vấn`6`, hạ cánh chính xác ở tiền tố cuối cùng. Tìm kiếm nhị phân trả về chỉ mục cuối cùng và câu trả lời là độ dài đầy đủ, vì không có tiền tố nào trước đó đạt được tổng số.
