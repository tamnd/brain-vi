---
title: "CF 102786J - \u041f\u0443\u0442\u0435\u0448\u0435\u0441\u0442\u0432\u0438\u0435 \u041aOR\u043e\u0432\u044c\u0435\u0432\u0430"
description: "Tôi sẽ cung cấp bài xã luận như một tài liệu hoàn chỉnh. Chỉnh sửa Chúng tôi có một mạng lưới đường vô hướng. Mỗi ngôi nhà là một đỉnh, mỗi con đường là một cạnh và mỗi con đường đều có một nhãn số nguyên."
date: "2026-07-27T19:31:21+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102786
codeforces_index: "J"
codeforces_contest_name: "\u041e\u0442\u043a\u0440\u044b\u0442\u044b\u0439 \u0447\u0435\u043c\u043f\u0438\u043e\u043d\u0430\u0442 \u042f\u0440\u0413\u0423 \u0438\u043c. \u041f.\u0413. \u0414\u0435\u043c\u0438\u0434\u043e\u0432\u0430 Demidov Open IT Cup 2019"
rating: 0
weight: 102786
solve_time_s: 57
verified: true
draft: false
---

[CF 102786J - \u041f\u0443\u0442\u0435\u0448\u0435\u0441\u0442\u0432\u0438\u0435 \u041aOR\u043e\u0432\u044c\u0435\u0432\u0430](https://codeforces.com/problemset/problem/102786/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 57s 
**Đã xác minh:** có 

##Giải pháp 
Tôi sẽ cung cấp bài xã luận như một tài liệu hoàn chỉnh. 

Chỉnh sửa 

#Hiểu vấn đề 

Chúng tôi có một mạng lưới đường vô hướng. Mỗi ngôi nhà là một đỉnh, mỗi con đường là một cạnh và mỗi con đường đều có một nhãn số nguyên. Chúng ta cần đi từ ngôi nhà được chỉ định này đến ngôi nhà khác, nhưng chi phí của một tuyến đường không phải là tổng chiều dài của con đường. Chi phí là bitwise HOẶC của tất cả các giá trị đường được sử dụng trên tuyến đường đó. Nhiệm vụ là tìm giá trị nhỏ nhất có thể có trong số tất cả các tuyến đường hoặc báo cáo rằng hai ngôi nhà đã bị ngắt kết nối. 

Đầu vào mô tả một biểu đồ có tối đa 100000 đỉnh và 100000 cạnh. Trọng số có thể lớn tới 10^9, do đó chỉ có thể xuất hiện khoảng 30 vị trí nhị phân. Biểu đồ có kích thước này ngay lập tức loại trừ các thuật toán liệt kê các đường dẫn hoặc chạy tìm kiếm trên tất cả các kết hợp tuyến đường có thể có. Ngay cả các thuật toán đường đi ngắn nhất thông thường như Dijkstra cũng không áp dụng trực tiếp vì OR không có thuộc tính phụ mà chúng yêu cầu. 

Khó khăn chính là tuyến đường có nhiều cạnh có thể tốt hơn tuyến đường có ít cạnh hơn. Ví dụ: một đường dẫn có trọng số 8 và 1 có giá trị 9, trong khi một cạnh có trọng số 10 có giá trị 10. Số cạnh không liên quan, chỉ có tập hợp các bit xuất hiện trong tuyến đường mới quan trọng. 

Có một số trường hợp đặc biệt làm hỏng việc triển khai đơn giản. Nếu biểu đồ bị ngắt kết nối, câu trả lời phải là -1 thay vì một giá trị ban đầu lớn nào đó. Ví dụ:```
3 1
1 2 7
1 3
```Đầu ra đúng là:```
-1
```Một giải pháp bất cẩn khởi tạo câu trả lời về 0 hoặc chỉ kiểm tra các cạnh hiện có có thể trả về một giá trị không chính xác ngay cả khi không thể truy cập được nhà 3. 

Bẫy thứ hai là giả định rằng đường đi có số lượng đường nhỏ nhất là tối ưu. Coi như:```
3 3
1 2 6
2 3 1
1 3 7
1 3
```Đầu ra đúng là:```
7
```Đường đi thẳng cho 7 và tuyến đường hai đường cho 6 HOẶC 1, cũng là 7. Phương pháp dựa trên số cạnh sẽ không giải quyết được vấn đề tối ưu hóa thực tế. 

Một lỗi phổ biến khác là xử lý bit sai hướng. Vì giá trị nhị phân của bit cao chiếm ưu thế trong tất cả các bit thấp hơn nên việc loại bỏ bit cao chỉ có thể được thực hiện sau khi kiểm tra xem liệu kết nối có còn khả thi nếu không có nó hay không. Ví dụ: nếu một giải pháp vô tình giữ lại một bit có thể đã bị xóa thì không có biện pháp tối ưu hóa bit thấp hơn nào sau này có thể khắc phục được kết quả. 

# Phương pháp tiếp cận 

Cách tiếp cận bạo lực trực tiếp sẽ cố gắng khám phá tất cả các đường đi có thể có giữa hai đỉnh và tính OR của mọi đường đi. Điều này đúng vì mọi câu trả lời có thể đều xuất phát từ một số đường dẫn, nhưng số lượng đường dẫn trong đồ thị dày đặc có thể theo cấp số nhân. Ngay cả một biểu đồ có 100000 cạnh cũng có thể chứa một số lượng lớn các tuyến đường khác nhau, khiến cho việc liệt kê là không thể. 

Một ý tưởng có cấu trúc hơn một chút là sử dụng thuật toán đường đi ngắn nhất và lưu trữ giá trị OR tốt nhất được tìm thấy cho đến nay cho mọi đỉnh. Quá trình chuyển đổi từ một đỉnh sang lân cận sẽ là:```
new_value = current_value | edge_weight
```Vấn đề là số lượng giá trị OR khác nhau có thể đạt tới một đỉnh không bị giới hạn một cách tự nhiên bởi một đỉnh. Đối số Dijkstra thông thường phụ thuộc vào việc kết hợp độ dài đường dẫn với phép cộng, trong khi OR có thể làm cho trạng thái trông tệ hơn trước đó trở nên hữu ích sau này. Khung đường dẫn ngắn nhất thông thường không khai thác cấu trúc đặc biệt của các giá trị OR. 

Quan sát quan trọng là kết quả OR chỉ chứa các bit xuất hiện trên ít nhất một cạnh trong tuyến đã chọn. Thay vì cố gắng xây dựng tuyến đường một cách trực tiếp, chúng ta có thể xây dựng câu trả lời từng chút một. 

Giả sử hiện tại chúng ta có một tập hợp bit vẫn được phép xuất hiện trong câu trả lời. Nếu chúng ta loại bỏ một bit và hai đỉnh mục tiêu vẫn được kết nối chỉ bằng các cạnh vừa khít bên trong mặt nạ còn lại thì bit đó không cần thiết và có thể bị loại bỏ vĩnh viễn. Nếu loại bỏ nó sẽ ngắt kết nối biểu đồ thì mọi tuyến hợp lệ đều cần bit đó, vì vậy chúng ta phải giữ nó. 

Điều này biến vấn đề thành việc kiểm tra kết nối lặp đi lặp lại. Vì trọng số cạnh chỉ có khoảng 30 bit liên quan nên chúng ta chỉ cần khoảng 30 lần kiểm tra. Mỗi lần kiểm tra có thể được thực hiện bằng một cấu trúc tập hợp rời rạc. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ của số lượng đường dẫn | O(n + m) | Quá chậm | 
| Tối ưu | O(B(n + m) α(n)) trong đó B là số bit | O(n) | Đã chấp nhận | 

#Hướng dẫn thuật toán 

1. Tính toán mặt nạ chứa OR của mỗi trọng số cạnh trong biểu đồ. Bất kỳ tuyến đường hợp lệ nào cũng chỉ có thể chứa các bit tồn tại trong mặt nạ này, vì vậy đây là câu trả lời lớn nhất có thể. 
2. Xử lý các bit từ bit cao nhất xuống bit thấp nhất. Đối với bit hiện tại, tạm thời xóa nó khỏi mặt nạ và kiểm tra xem đỉnh bắt đầu vẫn có thể đến đích chỉ bằng cách sử dụng các cạnh có tất cả các bit được đặt trong mặt nạ nhỏ hơn này hay không. 
3. Trong quá trình kiểm tra khả năng kết nối, hãy xây dựng cấu trúc liên kết tập hợp rời rạc. Đối với mọi cạnh, hợp nhất các điểm cuối của nó nếu trọng lượng của nó thỏa mãn`(weight & mask) == weight`. Điều kiện này có nghĩa là cạnh không đưa vào bất kỳ bit bị cấm nào. 
4. Nếu hai đỉnh nằm trong cùng một thành phần sau khi tháo bit, hãy giữ lại bit đó. Nếu chúng bị ngắt kết nối, hãy khôi phục bit vì mọi tuyến đường có thể đều yêu cầu nó. 
5. Sau khi tất cả các bit đã được xử lý, mặt nạ còn lại là giá trị OR tối thiểu có thể. Nếu biểu đồ ban đầu không bao giờ cho phép kết nối thì mặt nạ cuối cùng không đủ để phân biệt trường hợp đó, vì vậy hãy thực hiện kiểm tra kết nối cuối cùng và xuất ra -1 khi không thể truy cập được đích. 

Lý do quá trình tham lam này là đúng là vì các số nhị phân được sắp xếp theo thứ tự từ điển theo bit khác nhau cao nhất của chúng. Khi cân nhắc một chút, việc loại bỏ nó luôn tốt hơn là giữ nó nếu một tuyến đường hợp lệ vẫn tồn tại mà không có nó. Mọi quyết định sau này chỉ ảnh hưởng đến các bit thấp hơn và không thể bù đắp cho việc giữ bit cao hơn một cách không cần thiết. Điều bất biến là sau khi xử lý tiền tố bit, mặt nạ hiện tại là tập hợp nhỏ nhất có thể của các bit cao đã được coi là vẫn cho phép tuyến đường hợp lệ. 

#Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())
    edges = []
    mask = 0

    for _ in range(m):
        u, v, w = map(int, input().split())
        u -= 1
        v -= 1
        edges.append((u, v, w))
        mask |= w

    a, b = map(int, input().split())
    a -= 1
    b -= 1

    def connected(cur_mask):
        parent = list(range(n))
        size = [1] * n

        def find(x):
            while parent[x] != x:
                parent[x] = parent[parent[x]]
                x = parent[x]
            return x

        def union(x, y):
            x = find(x)
            y = find(y)
            if x == y:
                return
            if size[x] < size[y]:
                x, y = y, x
            parent[y] = x
            size[x] += size[y]

        for u, v, w in edges:
            if (w & cur_mask) == w:
                union(u, v)

        return find(a) == find(b)

    if not connected(mask):
        print(-1)
        return

    for bit in range(30, -1, -1):
        candidate = mask & ~(1 << bit)
        if connected(candidate):
            mask = candidate

    print(mask)

if __name__ == "__main__":
    solve()
```Biến`mask`đại diện cho câu trả lời tốt nhất vẫn có thể sau khi loại bỏ các bit không cần thiết. Nó bắt đầu bằng OR của tất cả các cạnh vì không có tuyến đường nào có thể chứa một bit không xuất hiện ở bất kỳ đâu trong biểu đồ. 

các`connected`hàm thực hiện kiểm tra đồ thị lặp lại. Nó không cần phải xây dựng một biểu đồ mới. Thay vào đó, nó nối chính xác các cạnh có trọng số tương thích với mặt nạ hiện tại. biểu thức`(w & cur_mask) == w`kiểm tra xem mọi bit được sử dụng bởi cạnh đều được cho phép. 

Việc triển khai kết hợp tập hợp rời rạc sử dụng tính năng nén đường dẫn và kết hợp theo kích thước, giữ cho mọi kiểm tra kết nối luôn gần với thời gian tuyến tính. Trọng số tối đa là 10^9, vì vậy việc kiểm tra các bit từ 30 xuống 0 sẽ bao gồm mọi bit được đặt có thể. 

Việc kiểm tra kết nối ban đầu là cần thiết vì chỉ riêng mặt nạ cuối cùng không thể biểu thị sự khác biệt giữa biểu đồ không thể truy cập và biểu đồ có thể truy cập có câu trả lời bằng 0. Đường dẫn chỉ chứa các cạnh có trọng số bằng 0 là hợp lệ và phải trả về 0. 

# Ví dụ đã hoạt động 

Đối với mẫu đầu tiên:```
3 3
1 2 5
1 3 1
2 3 5
1 2
```Mặt nạ ban đầu là`5 | 1 | 5 = 5`. Các đỉnh mục tiêu đã được kết nối. 

| Bước | Mặt nạ hiện tại | Đã xóa bit | Kết nối | Quyết định | 
| --- | --- | --- | --- | --- | 
| Bắt đầu | 5 | không | đã kết nối | bắt đầu | 
| Kiểm tra bit 2 | 1 | 4 | ngắt kết nối | giữ chút | 
| Kiểm tra bit 0 | 4 | 1 | ngắt kết nối | giữ chút | 

Câu trả lời cuối cùng là 5. Các tuyến đường duy nhất có thể có giữa đỉnh 1 và 2 sử dụng đường có trọng số 5, do đó giá trị OR không thể nhỏ hơn. 

Đối với mẫu thứ hai:```
5 3
3 5 6
1 4 7
2 4 6
1 3
```Mặt nạ ban đầu là`6 | 7 | 6 = 7`. Đỉnh 1 và 3 bị ngắt kết nối. 

| Bước | Mặt nạ hiện tại | Kết nối | Kết quả | 
| --- | --- | --- | --- | 
| Kiểm tra ban đầu | 7 | ngắt kết nối | đầu ra -1 | 

Điều này chứng tỏ tại sao khả năng tiếp cận phải được kiểm tra riêng. Giảm thiểu bit không thể tạo ra một đường dẫn không tồn tại. 

# Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(31(n + m) α(n)) | Có nhiều nhất các kiểm tra 31 bit và mỗi kiểm tra sẽ tạo một DSU trên tất cả các cạnh | 
| Không gian | O(n + m) | Đồ thị và mảng DSU được lưu trữ | 

Các giới hạn cho phép khoảng vài triệu thao tác đơn giản. Thuật toán thực hiện tối đa 31 lần quét toàn bộ danh sách cạnh, tức là khoảng 3,1 triệu lần kiểm tra cạnh cho đầu vào tối đa và các hoạt động DSU có thời gian khấu hao gần như không đổi. 

# Trường hợp thử nghiệm```python
import sys
import io

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    result = sys.stdout.getvalue()
    sys.stdin = old_stdin
    sys.stdout = old_stdout
    return result

assert run("""3 3
1 2 5
1 3 1
2 3 5
1 2
""") == "5\n", "sample 1"

assert run("""5 3
3 5 6
1 4 7
2 4 6
1 3
""") == "-1\n", "sample 2"

assert run("""2 1
1 2 0
1 2
""") == "0\n", "zero edge"

assert run("""3 3
1 2 6
2 3 1
1 3 7
1 3
""") == "7\n", "equal route values"

assert run("""4 4
1 2 8
2 3 4
3 4 2
1 4 15
1 4
""") == "14\n", "higher bit removal"

assert run("""2 1
1 2 1073741823
1 2
""") == "1073741823\n", "large weight"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Hai đỉnh được nối với nhau bằng cạnh 0 | 0 | Xác nhận rằng số 0 là câu trả lời hợp lệ | 
| Nhiều đường dẫn có cùng giá trị OR | 7 | Xác nhận rằng số cạnh không quan trọng | 
| Trọng lượng cạnh lớn | 1073741823 | Xác nhận việc xử lý bit gần ranh giới số nguyên | 
| Các đỉnh không thể tiếp cận | -1 | Xác nhận xử lý ngắt kết nối | 

# Vỏ cạnh 

Trường hợp biểu đồ bị ngắt kết nối được xử lý trước khi bắt đầu loại bỏ bit. Đối với đầu vào:```
3 1
1 2 7
1 3
```Mặt nạ ban đầu là 7. Kiểm tra kết nối đầu tiên cho thấy đỉnh 3 không nằm trong cùng thành phần DSU với đỉnh 1, do đó thuật toán ngay lập tức trả về -1. Nó không bao giờ cố gắng giảm thiểu một tuyến đường không tồn tại. 

Thuật toán cũng xử lý chính xác các tuyến đường có cạnh có trọng số bằng 0. Vì:```
2 1
1 2 0
1 2
```Mặt nạ ban đầu bằng không. Việc kiểm tra kết nối chấp nhận cạnh duy nhất vì`(0 & 0) == 0`. Không có bit nào có thể bị loại bỏ và câu trả lời vẫn là 0. 

Một tuyến đường có nhiều cạnh có thể tốt hơn một cạnh trực tiếp và thuật toán không ưu tiên một trong hai. Vì:```
4 4
1 2 8
2 3 4
3 4 2
1 4 15
1 4
```Cạnh trực tiếp cho 15. Đường đi dài hơn cho`8 | 4 | 2 = 14`. Việc loại bỏ tham lam kiểm tra từng bit cao và phát hiện ra rằng bit 0 có thể biến mất trong khi các đỉnh vẫn được kết nối, để lại 14 là giá trị tối thiểu. 

Các trường hợp cạnh cuối cùng liên quan đến bit cao. Bởi vì thuật toán xử lý tất cả các bit có thể có từ 30 xuống 0, nên các trọng số như 1073741823 được xử lý mà không bị tràn hoặc thiếu các vị trí quan trọng. 

Bài xã luận này có thể được điều chỉnh để ngắn hơn, mang tính cạnh tranh hơn hoặc tập trung vào bằng chứng hơn nếu cần.
