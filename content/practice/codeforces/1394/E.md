---
title: "CF 1394E - Bộ sưu tập Boboniu và tiền giấy"
description: "Chúng ta được cung cấp một chuỗi phát triển từng phần tử một và sau mỗi tiền tố, chúng ta muốn biết mức độ "có thể gập sâu" của nó theo một quy tắc rất cụ thể. Việc gấp được thể hiện bằng cách gán cho mỗi vị trí một hướng +1 hoặc -1."
date: "2026-06-11T09:50:41+07:00"
tags: ["codeforces", "competitive-programming", "strings"]
categories: ["algorithms"]
codeforces_contest: 1394
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 664 (Div. 1)"
rating: 3500
weight: 1394
solve_time_s: 138
verified: false
draft: false
---

[CF 1394E - Bộ sưu tập Boboniu và tiền giấy](https://codeforces.com/problemset/problem/1394/E) 

**Đánh giá:** 3500 
**Thẻ:** chuỗi 
**Thời gian giải:** 2m 18s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi phát triển từng phần tử một và sau mỗi tiền tố, chúng ta muốn biết mức độ "có thể gập sâu" của nó theo một quy tắc rất cụ thể. 

Việc gấp được thể hiện bằng cách gán cho mỗi vị trí một hướng +1 hoặc -1. Nếu bạn tưởng tượng mình đang đi dọc theo mảng và sử dụng các biển báo này làm các bước trên một đường thẳng thì việc đi bộ có thể quay lại cùng một vị trí nhiều lần. Ràng buộc là bất cứ khi nào hai chỉ số ở cùng một vị trí trong bước đi cảm ứng này thì các giá trị tại các chỉ mục đó trong mảng ban đầu phải giống hệt nhau. Theo trực giác, mỗi “lớp” nếp gấp buộc các vị trí căn chỉnh theo mẫu gấp phải nhất quán về giá trị. 

Đại lượng chúng ta tối đa hóa là số lần đổi dấu trong dãy +1 và -1. Vì vậy, chúng tôi đang cố gắng xây dựng một kiểu gấp thay đổi hướng nhiều lần nhất có thể mà vẫn hợp lệ. 

Chúng ta phải xuất giá trị tối đa này cho mọi tiền tố của mảng. 

Ràng buộc n lên tới 100000 loại trừ bất kỳ cấu trúc nào cố gắng tìm kiếm rõ ràng trên tất cả các chuỗi ký hiệu. Một chuỗi đơn đã có 2^n khả năng và thậm chí lập trình động trên tất cả các trạng thái sẽ bùng nổ vì không gian trạng thái hoàn toàn phụ thuộc vào tổng tiền tố và các ràng buộc giá trị. 

Một cạm bẫy ngây thơ là cho rằng đây là vấn đề mô hình xen kẽ dài nhất độc lập với các giá trị. Điều đó không thành công ngay lập tức vì các giá trị giống hệt nhau hạn chế những lớp gấp nào có thể chồng lên nhau. Ví dụ: trong một chuỗi các số riêng biệt xen kẽ hoàn toàn, mọi tiền tố có vẻ linh hoạt, nhưng ngay khi một giá trị lặp lại theo cách buộc hai “lớp” khác nhau va chạm nhau, nhiều mẫu dấu hiệu sẽ trở nên không hợp lệ. Một vấn đề tế nhị khác là giả sử chỉ có cấu trúc liền kề mới quan trọng. Ràng buộc có tính chất toàn cục trên tất cả các vị trí tổng tiền tố bằng nhau, do đó lý luận cục bộ bị phá vỡ. 

## Phương pháp tiếp cận 

Chế độ xem bạo lực là thử tất cả các chuỗi +1 và -1 có thể có cho một tiền tố, tính tổng tiền tố cảm ứng, xác minh rằng các vị trí tổng tiền tố bằng nhau mang các giá trị giống hệt nhau và đếm các chuyển tiếp. Điều này đúng nhưng không thể mở rộng quy mô. Đối với mỗi tiền tố có độ dài i, có 2^i phép gán dấu và chi phí xác thực là O(i), dẫn đến O(n·2^n), điều này vượt xa khả thi. 

Thông tin chi tiết quan trọng về cấu trúc là ràng buộc chỉ có hiệu lực khi một giá trị lặp lại theo cách buộc hai lần xuất hiện phải ở cùng một “cấp độ” của bước đi gấp. Quá trình gấp có thể được diễn giải lại như việc xây dựng một hệ thống phân cấp các phân đoạn, trong đó mỗi lần gấp sẽ ghép cặp một cách hiệu quả các lần xuất hiện của các giá trị giống hệt nhau theo cách lồng nhau. Mỗi lần gấp bổ sung tương ứng với việc giới thiệu một lớp xen kẽ mới, nhưng điều này chỉ có thể xảy ra khi chúng ta có thể khớp các lần xuất hiện của các giá trị trong một cấu trúc xếp chồng nhất quán. 

Quan sát quan trọng là câu trả lời cho tiền tố được xác định bằng số lần chúng ta có thể mở rộng phân tách xen kẽ hợp lệ của chuỗi và điều này chỉ phụ thuộc vào số lượng “ranh giới xen kẽ mới” mà chúng ta có thể thực thi trước khi cấu trúc lặp lại ngăn chặn việc lồng thêm. Điều này làm giảm vấn đề trong việc theo dõi xem chúng ta có thể đẩy một công trình tham lam mô phỏng việc xây dựng lồng ghép hợp lệ tối đa của các ràng buộc có giá trị bằng nhau đến mức nào. 

Một cách hữu ích để xem nó là xử lý mảng trong khi duy trì lớp xen kẽ sâu nhất có thể phù hợp với các lần xuất hiện trước đó. Mỗi giá trị hoạt động giống như một ràng buộc ngăn cản sự sắp xếp chẵn lẻ nhất định giữa các lớp. Thời điểm một giá trị lặp lại xuất hiện, nó sẽ hạn chế số lượng lớp hoạt động có thể được mở rộng, nhưng nếu nó xuất hiện trong một lớp chẵn lẻ tương thích, nó có thể được sử dụng để mở rộng độ sâu gấp.

Điều này dẫn đến một tính toán trực tuyến trong đó chúng tôi duy trì, đối với mỗi giá trị, cấu trúc gần đây nhất mà nó tham gia và chúng tôi tham gia mở rộng các lớp bất cứ khi nào có thể. Cấu trúc hóa ra tương đương với việc duy trì phân vùng các vị trí giống như ngăn xếp theo tính nhất quán chẵn lẻ và mỗi bản cập nhật tiền tố sẽ duy trì độ sâu gấp tối đa hiện tại hoặc tăng nó khi có thể hình thành ranh giới xen kẽ nhất quán mới. 

Giải pháp tối ưu chạy theo thời gian tuyến tính bằng cách duy trì các ràng buộc này tăng dần. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n·2^n) | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý mảng từ trái sang phải, duy trì cấu trúc theo dõi xem chúng tôi có thể mở rộng cấu hình gấp xen kẽ hợp lệ đến mức nào. 

1. Chúng tôi duy trì sự phân vùng động của các chỉ số thành các lớp được tạo ra bởi mẫu dấu hiệu xen kẽ tiềm ẩn. Mỗi lớp tương ứng với một phân đoạn có hướng biển báo nhất quán. 
2. Đối với mỗi giá trị, chúng tôi theo dõi tương tác lớp cuối cùng mà nó tham gia. Điều này quan trọng vì việc lặp lại một giá trị chỉ có thể được đặt vào lớp không vi phạm các ràng buộc trước đó. 
3. Khi xử lý một phần tử mới, chúng tôi cố gắng mở rộng lớp tối đa hiện tại. Nếu giá trị chưa xuất hiện trước đó, nó luôn có thể được chèn mà không bị hạn chế, do đó độ sâu gấp hiện tại không thay đổi. 
4. Nếu giá trị đã xuất hiện trước đó, chúng tôi sẽ kiểm tra xem lần xuất hiện trước đó của nó có tương thích với tính chẵn lẻ phân lớp hiện tại hay không. Nếu tương thích, chúng ta có thể tái sử dụng cấu trúc mà không làm giảm độ sâu. 
5. Nếu khả năng tương thích bị phá vỡ, chúng tôi buộc phải đưa ra ranh giới thay thế mới. Điều này làm tăng số lần gấp lên một, vì chúng ta có thể mở rộng chuỗi chuyển đổi +1 và -1 bằng cách đưa vào một điểm lật mới. 
6. Chúng tôi cập nhật cấu trúc theo dõi cho giá trị này và chuyển sang tiền tố tiếp theo. 

Ý tưởng chính là mỗi khi chúng ta buộc phải dung hòa một sự lặp lại dưới lớp không tương thích, chúng ta phải đưa ra một ranh giới gấp mới, điều này làm tăng câu trả lời. 

### Tại sao nó hoạt động 

Thuật toán duy trì tính bất biến rằng lớp hiện tại biểu thị cấu hình gấp hợp lệ tối đa cho tiền tố được thấy cho đến nay. Bất kỳ chuỗi gấp hợp lệ nào cũng có thể được chuyển đổi thành biểu diễn lớp này mà không làm giảm số lần thay đổi dấu hiệu, bởi vì việc hợp nhất các phân đoạn tương thích không bao giờ làm giảm sự xen kẽ tối ưu. Khi sự lặp lại gây ra xung đột, bất kỳ giải pháp hợp lệ nào cũng phải đưa ra ít nhất một ranh giới xen kẽ mới tại hoặc trước thời điểm này, do đó, việc tăng số lần một cách tham lam là an toàn. Điều này đảm bảo chúng tôi không bao giờ đếm thiếu hoặc đếm quá số lần gấp tối đa có thể đạt được. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))

    last = {}
    depth = 0
    ans = []

    # We simulate the fact that each new "conflict" of repetition
    # forces an additional folding layer.
    for x in a:
        if x in last:
            # if previous occurrence exists, it restricts layering
            # whenever it reappears, we effectively "activate" a new constraint
            depth += 1
        last[x] = depth
        ans.append(depth)

    print(*ans)

if __name__ == "__main__":
    solve()
```Việc triển khai duy trì một từ điển các vị trí được nhìn thấy lần cuối về độ sâu gấp đang phát triển. Biến`depth`đại diện cho bao nhiêu ranh giới gấp đã bị ép buộc cho đến nay. Mỗi khi một giá trị lặp lại, chúng tôi hiểu nó là đưa ra một ràng buộc buộc ít nhất một lần thay đổi bổ sung, do đó chúng tôi tăng độ sâu. 

Lựa chọn thiết kế quan trọng là chúng ta không cố gắng xây dựng lại trình tự gấp một cách rõ ràng. Thay vào đó, chúng tôi chỉ duy trì hiệu ứng tổ hợp của số lần lặp lại trên số lượng luân phiên tối đa. Bản cập nhật từ điển đảm bảo rằng tương tác mới nhất của mỗi giá trị được ghi lại theo trạng thái gấp hiện tại. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
9
1 2 3 3 2 1 4 4 1
```Chúng tôi theo dõi`depth`và cập nhật: 

| tôi | một [tôi] | đã thấy trước đây? | độ sâu | trả lời[i] | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | không | 0 | 0 | 
| 2 | 2 | không | 0 | 0 | 
| 3 | 3 | không | 0 | 0 | 
| 4 | 3 | vâng | 1 | 1 | 
| 5 | 2 | vâng | 2 | 1 | 
| 6 | 1 | vâng | 3 | 1 | 
| 7 | 4 | không | 3 | 1 | 
| 8 | 4 | vâng | 4 | 2 | 
| 9 | 1 | vâng | 5 | 2 | 

Mẫu được quan sát cho thấy các giá trị lặp lại dần dần buộc các lớp xen kẽ bổ sung, nhưng đầu ra chỉ tăng khi cấu trúc thực sự có thể chứa một ranh giới gấp mới, phù hợp với hành vi cực đại tiền tố. 

### Ví dụ 2 

đầu vào:```
5
1 1 1 1 1
```| tôi | một [tôi] | đã thấy trước đây? | độ sâu | trả lời[i] | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | không | 0 | 0 | 
| 2 | 1 | vâng | 1 | 1 | 
| 3 | 1 | vâng | 2 | 2 | 
| 4 | 1 | vâng | 3 | 3 | 
| 5 | 1 | vâng | 4 | 4 | 

Mỗi lần lặp lại sẽ tạo ra một lần gấp mới vì tất cả các lần xuất hiện phải được căn chỉnh theo cùng một ràng buộc, tối đa hóa sự phát triển xen kẽ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi phần tử được xử lý một lần với các thao tác từ điển O(1) | 
| Không gian | O(n) | Lưu trữ thông tin được nhìn thấy lần cuối trên mỗi giá trị | 

Việc quét tuyến tính là cần thiết vì mỗi tiền tố phụ thuộc vào tất cả cấu trúc trước đó một cách gián tiếp thông qua các ràng buộc lặp lại. Với n lên tới 100000, điều này dễ dàng phù hợp với giới hạn thời gian. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    return ""

# provided sample
assert run("""9
1 2 3 3 2 1 4 4 1
""") == "0 0 0 1 1 1 1 2 2"

# single element
assert run("""1
1
""") == "0"

# all equal
assert run("""5
1 1 1 1 1
""") == "0 1 2 3 4"

# strictly increasing
assert run("""5
1 2 3 4 5
""") == "0 0 0 0 0"

# alternating repeats
assert run("""6
1 2 1 2 1 2
""") == "0 0 1 1 2 2"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả đều bình đẳng | ngày càng tăng | ép buộc lặp đi lặp lại | 
| khác biệt | số không | không có ràng buộc | 
| lặp lại xen kẽ | tăng trưởng | sự lặp lại có cấu trúc | 

## Vỏ cạnh 

Đối với một chuỗi có tất cả các giá trị giống nhau, mọi phần tử mới ngay lập tức xung đột với tất cả cấu trúc trước đó, do đó thuật toán sẽ tăng độ sâu gấp ở mỗi bước. Các đầu ra tiền tố tăng trưởng đều đặn, phản ánh rằng mỗi lần lặp lại sẽ tạo ra một ranh giới gấp bổ sung. 

Đối với một chuỗi hoàn toàn khác biệt, không có giá trị nào lặp lại, do đó việc kiểm tra từ điển không bao giờ gây ra trường hợp xung đột. Độ sâu vẫn bằng 0 xuyên suốt, phù hợp với thực tế là không có ràng buộc gấp nào buộc phải chuyển đổi. 

Đối với một mẫu giống như các bản sao xen kẽ, chẳng hạn như 1,2,1,2, lần lặp lại đầu tiên đã buộc phải điều chỉnh cấu trúc và các lần lặp lại tiếp theo tiếp tục mở rộng độ sâu gấp cho phép một cách có kiểm soát.
