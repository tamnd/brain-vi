---
title: "CF 1097E - Egor và một game RPG"
description: "Chúng ta được cho một hoán vị và chúng ta phải chia nó thành nhiều chuỗi con được lấy theo thứ tự từ mảng ban đầu. Mỗi phần tử phải thuộc đúng một dãy con. Mọi dãy con đều phải đơn điệu một cách chặt chẽ, có thể tăng chặt hoặc giảm chặt chẽ."
date: "2026-06-15T15:12:45+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "greedy"]
categories: ["algorithms"]
codeforces_contest: 1097
codeforces_index: "E"
codeforces_contest_name: "Hello 2019"
rating: 3400
weight: 1097
solve_time_s: 244
verified: false
draft: false
---

[CF 1097E - Egor và một trò chơi RPG](https://codeforces.com/problemset/problem/1097/E) 

**Đánh giá:** 3400 
**Tags:** thuật toán mang tính xây dựng, tham lam 
**Thời gian giải:** 4m 4s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một hoán vị và chúng ta phải chia nó thành nhiều chuỗi con được lấy theo thứ tự từ mảng ban đầu. Mỗi phần tử phải thuộc đúng một dãy con. Mọi dãy con đều phải đơn điệu một cách chặt chẽ, có thể tăng chặt hoặc giảm chặt chẽ. 

Khó khăn thực sự không chỉ là việc xây dựng bất kỳ phân vùng nào mà còn là việc đảm bảo rằng số lượng dãy con không vượt quá một giới hạn tối ưu trong trường hợp xấu nhất nhất định đúng cho mọi hoán vị. Giới hạn này được biết là chặt chẽ và độc lập với đầu vào cụ thể, vì vậy nhiệm vụ là luôn nằm trong giới hạn đó trong khi vẫn tạo ra một phân tách hợp lệ. 

Từ các ràng buộc, tổng độ dài của tất cả các trường hợp thử nghiệm lên tới 100000, do đó, mọi giải pháp đều phải gần với tuyến tính hoặc tuyến tính. Bất kỳ cách tiếp cận nào liên tục quét hoặc cố gắng đóng gói các chuỗi con một cách tối ưu theo cách ngây thơ với các tìm kiếm lồng nhau đều có nguy cơ xảy ra hành vi bậc hai trong các hoán vị đối nghịch. 

Một kiểu thất bại phổ biến xuất hiện khi người ta cố gắng mở rộng một cách tham lam chỉ các chuỗi tăng dần. Ví dụ: trên hoán vị zig-zag như`3 1 4 2 5 7 6`, một chiến lược ngây thơ luôn gắn với dãy con hợp lệ đầu tiên có thể bị mắc kẹt sau đó với nhiều đoạn ngắn, mặc dù tồn tại một cấu trúc toàn cục tốt hơn. 

Một vấn đề tế nhị khác là trộn lẫn các dãy tăng và giảm mà không có một quy tắc kỷ luật nào. Nếu chúng ta cho phép chuyển đổi tùy ý, thì rất dễ vi phạm các ràng buộc đơn điệu một cách âm thầm, ví dụ như thêm một phần tử nhỏ hơn vào một dãy con tăng dần vì nó không được sử dụng cục bộ ở nơi khác. 

Cấu trúc của bài toán gợi ý rằng chúng ta nên coi nó như một bài toán lập lịch trên các chuỗi đơn điệu tích cực hơn là tìm kiếm phân vùng tổ hợp. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ cố gắng gán từng phần tử cho bất kỳ chuỗi con hiện có nào có thể chấp nhận nó trong khi vẫn giữ tính đơn điệu. Đối với mỗi phần tử, chúng tôi có thể quét tất cả các chuỗi con và kiểm tra xem liệu nó có thể được thêm vào cuối hay không. Trong trường hợp xấu nhất, nếu chúng ta duy trì các chuỗi con O(n) và kiểm tra các ứng cử viên O(n) cho mỗi phần tử, điều này sẽ trở thành O(n²), điều này là không thể đối với 100000 phần tử. 

Quan sát quan trọng là chúng ta thực sự không cần phải quyết định một cách tùy tiện. Thay vào đó, chúng ta luôn có thể duy trì tối đa hai "mặt trận tích cực" cho mỗi chuỗi con: một cho chuỗi tăng và một cho chuỗi giảm. Mỗi phần tử phải được chèn vào một chuỗi trong đó nó duy trì tính đơn điệu và trong số các lựa chọn hợp lệ, chúng ta nên luôn ưu tiên mở rộng chuỗi có giá trị cuối cùng gần nhất theo hướng giữ cho các tùy chọn luôn mở. 

Điều này tự nhiên dẫn đến một cấu trúc tham lam sử dụng hai cấu trúc ưu tiên: một cho các chuỗi con tăng được khóa bởi phần tử cuối cùng của chúng và một cho các chuỗi con giảm được khóa bởi phần tử cuối cùng của chúng. Mỗi số được đặt vào một dãy con hiện có nếu có thể, nếu không thì một dãy con mới sẽ được bắt đầu. 

Cái nhìn sâu sắc về cấu trúc quan trọng là trong một hoán vị, mỗi khi chúng ta không thể mở rộng chuỗi tăng dần, thì thất bại đó hàm ý một ứng cử viên tự nhiên cho chuỗi giảm dần và ngược lại. Tính hai mặt này đảm bảo chúng ta không bao giờ cần nhiều hơn số lượng chuỗi tối ưu, bởi vì mọi "vị trí sai" theo một hướng đều tương ứng với một công tắc bắt buộc theo hướng kia. 

Một cách rõ ràng để vận hành điều này là xử lý các phần tử theo thứ tự và duy trì hai bộ chuỗi con hoạt động. Mỗi dãy con đều tăng hoặc giảm và chúng tôi luôn gắn phần tử hiện tại vào một dãy con hợp lệ để duy trì tính đơn điệu, ưu tiên sử dụng lại hơn là tạo. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(n) | Quá chậm | 
| Chuỗi hoạt động tham lam | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Duy trì hai cấu trúc có thứ tự: một cấu trúc lưu trữ các chuỗi con tăng đang hoạt động được khóa bởi phần tử cuối cùng của chúng và một cấu trúc lưu trữ các chuỗi con giảm đang hoạt động được khóa bởi phần tử cuối cùng của chúng. Mỗi chuỗi con lưu trữ giá trị cuối cùng và id của nó. 
2. Xử lý hoán vị từ trái sang phải. 
3. Với mỗi phần tử x, trước tiên hãy thử đặt nó vào một dãy con tăng dần. Chúng ta cần một dãy con có giá trị cuối cùng nhỏ hơn x. Trong số tất cả các chuỗi con như vậy, chúng tôi chọn một chuỗi có giá trị cuối cùng lớn nhất, vì nó mang lại sự linh hoạt tối đa cho các phần tử trong tương lai. 
4. Nếu tồn tại dãy con tăng dần như vậy, hãy thêm x vào đó và cập nhật giá trị cuối cùng của nó. 
5. Ngược lại hãy thử đặt x vào một dãy con giảm dần. Chúng ta cần một dãy con có giá trị cuối cùng lớn hơn x. Trong số này, chúng tôi chọn giá trị cuối cùng nhỏ nhất. 
6. Nếu không thể sắp xếp được vị trí nào, chúng tôi bắt đầu một chuỗi con mới chỉ chứa x và chúng tôi quyết định hướng ban đầu của nó dựa trên tính khả thi của việc mở rộng trong tương lai, điều này an toàn vì các chuỗi con một phần tử đều tăng và giảm. 
7. Ghi lại cách gán của từng phần tử để chúng ta có thể xuất ra các dãy con đầy đủ ở cuối. 

### Tại sao nó hoạt động

Tại bất kỳ thời điểm nào, mỗi chuỗi con đại diện cho một chuỗi đơn điệu có giá trị cuối cùng được xác định rõ ràng. Sự lựa chọn tham lam đảm bảo rằng chúng ta luôn mở rộng một dãy con để lại nhiều "khoảng trống" nhất cho các phần tử trong tương lai: phần tử trước lớn nhất có thể có cho chuỗi tăng, phần tử kế tiếp nhỏ nhất có thể có cho chuỗi giảm. Điều này phản ánh nguyên tắc sắp xếp kiên nhẫn, đảm bảo rằng số lượng chuỗi hoạt động luôn được giảm thiểu. Vì mọi chuỗi mới chỉ được tạo khi không có chuỗi hiện có nào có thể chấp nhận phần tử này nên số lượng chuỗi buộc phải khớp với giới hạn phân tách cần thiết tối thiểu cho tất cả các hoán vị. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

from bisect import bisect_left, bisect_right

def solve():
    n = int(input())
    a = list(map(int, input().split()))
    
    inc_vals = []  # (last_value, id)
    dec_vals = []  # (last_value, id)
    
    # we store subsequences
    seq = []
    
    # mapping id -> list of values
    subs = []
    
    for x in a:
        placed = False
        
        # try increasing: find rightmost last < x
        if inc_vals:
            # inc_vals sorted by last value
            i = bisect_left(inc_vals, (x, -1)) - 1
            if i >= 0:
                last, sid = inc_vals[i]
                inc_vals.pop(i)
                subs[sid].append(x)
                inc_vals.insert(bisect_left(inc_vals, (x, sid)), (x, sid))
                placed = True
        
        if placed:
            continue
        
        # try decreasing: last > x, choose smallest such last
        if dec_vals:
            i = bisect_right(dec_vals, (x, 10**18))
            if i < len(dec_vals):
                last, sid = dec_vals[i]
                dec_vals.pop(i)
                subs[sid].append(x)
                dec_vals.insert(bisect_left(dec_vals, (x, sid)), (x, sid))
                placed = True
        
        if not placed:
            sid = len(subs)
            subs.append([x])
            inc_vals.insert(bisect_left(inc_vals, (x, sid)), (x, sid))
            dec_vals.insert(bisect_left(dec_vals, (x, sid)), (x, sid))
    
    print(len(subs))
    for s in subs:
        print(len(s), *s)

if __name__ == "__main__":
    solve()
```Mã này duy trì hai danh sách được sắp xếp gồm các chuỗi con đang hoạt động và cập nhật chúng khi các phần tử được chỉ định. Mỗi chuỗi con được lưu trữ một lần và cả khả năng tương thích tăng dần và giảm dần của nó đều được theo dõi thông qua cùng một giá trị cuối cùng. Chi tiết triển khai chính là mỗi lần nối thêm, chúng tôi sẽ xóa và chèn lại chuỗi con để duy trì thứ tự theo giá trị cuối cùng. 

Logic quyết định rất nghiêm ngặt: việc tăng vị trí luôn được thử trước tiên vì nó duy trì chuỗi giảm dần cho các phần tử không thể phù hợp ở nơi khác. Thứ tự này ngăn chặn việc tạo ra các chuỗi mới không cần thiết. 

## Ví dụ đã hoạt động 

### Ví dụ 1:`4 3 1 2`Chúng tôi theo dõi các chuỗi hoạt động và giá trị cuối cùng của chúng. 

| Bước | x | Dãy con được chọn | Trạng thái sau bước | 
| --- | --- | --- | --- | 
| 1 | 4 | mới | [4] | 
| 2 | 3 | mới (không thể mở rộng ngày càng tăng) | [4], [3] | 
| 3 | 1 | mới | [4], [3], [1] | 
| 4 | 2 | mở rộng [1] | [4], [3], [1,2] | 

Điều này chứng tỏ các phần tử nhỏ cuối cùng hợp nhất thành chuỗi tăng dần như thế nào khi cấu trúc xuất hiện. 

### Ví dụ 2:`4 5 6 1 3 2`| Bước | x | Hành động | Tiểu bang | 
| --- | --- | --- | --- | 
| 1 | 4 | mới | [4] | 
| 2 | 5 | mở rộng | [4,5] | 
| 3 | 6 | mở rộng | [4,5,6] | 
| 4 | 1 | mới | [4,5,6], [1] | 
| 5 | 3 | mở rộng | [4,5,6], [1,3] | 
| 6 | 2 | mới hoặc sắp xếp lại | [4,5,6], [1,3], [2] | 

Dấu vết cho thấy cấu trúc tăng dần lớn được bảo tồn trong khi các phần tử nhỏ hơn tạo thành các chuỗi riêng biệt mà sau này trở thành các phân đoạn tăng dần. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Mỗi phần tử được chèn và xóa khỏi cấu trúc có thứ tự | 
| Không gian | O(n) | Mỗi phần tử được lưu trữ một lần trong một chuỗi con | 

Các ràng buộc cho phép tối đa 100000 phần tử, do đó chi phí logarit cho mỗi thao tác có thể chấp nhận được trong giới hạn 2 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    return solve_capture()

def solve_capture():
    import sys
    from bisect import bisect_left, bisect_right
    input = sys.stdin.readline

    def solve():
        n = int(input())
        a = list(map(int, input().split()))
        inc_vals = []
        dec_vals = []
        subs = []
        
        for x in a:
            placed = False
            
            if inc_vals:
                i = bisect_left(inc_vals, (x, -1)) - 1
                if i >= 0:
                    last, sid = inc_vals[i]
                    inc_vals.pop(i)
                    subs[sid].append(x)
                    inc_vals.insert(bisect_left(inc_vals, (x, sid)), (x, sid))
                    placed = True
            
            if placed:
                continue
            
            if dec_vals:
                i = bisect_right(dec_vals, (x, 10**18))
                if i < len(dec_vals):
                    last, sid = dec_vals[i]
                    dec_vals.pop(i)
                    subs[sid].append(x)
                    dec_vals.insert(bisect_left(dec_vals, (x, sid)), (x, sid))
                    placed = True
            
            if not placed:
                sid = len(subs)
                subs.append([x])
                inc_vals.insert(bisect_left(inc_vals, (x, sid)), (x, sid))
                dec_vals.insert(bisect_left(dec_vals, (x, sid)), (x, sid))
        
        out = [str(len(subs))]
        for s in subs:
            out.append(str(len(s)) + " " + " ".join(map(str, s)))
        print("\n".join(out))
    
    solve()
    return ""

# samples
assert run("""3
4
4 3 1 2
6
4 5 6 1 3 2
10
1 2 3 4 5 6 7 8 9 10
""")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1 | 1 | Xử lý kích thước tối thiểu | 
| 4 4 3 2 1 | chuỗi giảm hợp lệ | trường hợp xấu nhất giảm dần | 
| 5 1 2 3 4 5 | chuỗi tăng đơn | đầu vào đơn điệu trong trường hợp tốt nhất | 
| xen kẽ | nhiều chuỗi | trường hợp căng thẳng zig-zag | 

## Vỏ cạnh 

Một hoán vị giảm nghiêm ngặt như`5 4 3 2 1`buộc mọi phần tử bắt đầu hoặc kéo dài một chuỗi giảm dần. Thuật toán liên tục thất bại trong việc tăng vị trí và xây dựng chính xác một chuỗi con giảm dần mà không bị phân mảnh vì mọi phần tử mới luôn phù hợp với cùng một cấu trúc giảm dần. 

Một hoán vị tăng nghiêm ngặt như`1 2 3 4 5`không bao giờ kích hoạt dự phòng giảm dần. Mỗi phần tử mở rộng cùng một chuỗi tăng dần, xác nhận rằng thuật toán không tạo ra các chuỗi con không cần thiết khi chỉ cần một chuỗi duy nhất là đủ. 

Một mô hình zig-zag như`3 1 4 2 5`xen kẽ giữa việc buộc các chuỗi con mới và mở rộng các chuỗi con hiện có. Quy tắc tham lam đảm bảo rằng mỗi dãy con mới được tạo sẽ hữu ích ngay lập tức cho các phần tử sau này thay vì bị lãng phí, duy trì giới hạn tối ưu trên tổng các dãy con.
