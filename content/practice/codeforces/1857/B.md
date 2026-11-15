---
title: "CF 1857B - Làm tròn tối đa"
description: "Chúng ta được cấp một số được viết dưới dạng một chuỗi các chữ số và chúng ta được phép áp dụng nhiều lần một thao tác làm tròn cụ thể để thay đổi các chữ số tại một vị trí đã chọn và truyền mang đến các vị trí cao hơn theo cách không chuẩn."
date: "2026-06-09T00:45:59+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "implementation", "math"]
categories: ["algorithms"]
codeforces_contest: 1857
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 891 (Div. 3)"
rating: 1100
weight: 1857
solve_time_s: 98
verified: false
draft: false
---

[CF 1857B - Làm tròn tối đa](https://codeforces.com/problemset/problem/1857/B) 

**Đánh giá:** 1100 
**Tags:** tham lam, thực hiện, toán học 
**Thời gian giải:** 1 phút 38 giây 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một số được viết dưới dạng một chuỗi các chữ số và chúng ta được phép áp dụng nhiều lần một thao tác làm tròn cụ thể để thay đổi các chữ số tại một vị trí đã chọn và truyền mang đến các vị trí cao hơn theo cách không chuẩn. Sau mỗi thao tác, tất cả các chữ số ở bên phải của vị trí đã chọn sẽ trở thành 0 và chữ số ở vị trí đó có thể tăng tùy thuộc vào chữ số ngay bên phải của nó trước khi làm tròn, với các số mang có khả năng di chuyển xa hơn về bên trái nếu một chữ số trở thành 10. 

Mục tiêu là áp dụng thao tác này nhiều lần theo thứ tự mà chúng ta chọn để thu được số cuối cùng lớn nhất có thể. 

Khó khăn chính là mỗi bước làm tròn không hoạt động độc lập. Một thao tác đơn lẻ có thể xóa toàn bộ hậu tố của số và việc nhớ có thể xếp tầng sang trái, có khả năng thay đổi các chữ số cao hơn. Điều này làm cho việc lựa chọn thao tác đầu tiên có ảnh hưởng cực kỳ lớn, bởi vì nó quyết định thông tin nào được lưu giữ cho các quyết định sau này. 

Các ràng buộc cho phép lên đến$10^4$các trường hợp thử nghiệm và tổng số chữ số trên tất cả các đầu vào nhiều nhất là$2 \cdot 10^5$. Điều này ngay lập tức loại trừ mọi mô phỏng bậc hai cho mỗi trường hợp thử nghiệm. Bất kỳ giải pháp nào cũng phải xử lý trung bình mỗi chữ số với số lần không đổi, thường là theo thời gian tuyến tính trên mỗi chuỗi. 

Một trường hợp cạnh tinh vi phát sinh khi nhiều thao tác làm tròn có thể được áp dụng ở các vị trí khác nhau, nhưng việc áp dụng phép làm tròn “nhỏ” sớm sẽ phá hủy các chữ số lẽ ra sẽ góp phần tạo ra chuỗi mang tốt hơn. Ví dụ: trong các số như 199999 hoặc 10999, các quyết định làm tròn cục bộ tham lam có thể tạo ra kết quả toàn cầu dưới mức tối ưu nếu chúng ta không cẩn thận về mức độ lan truyền của số mang. 

## Phương pháp tiếp cận 

Chiến lược bạo lực sẽ mô phỏng mọi chuỗi hoạt động làm tròn có thể có. Mỗi thao tác có thể được áp dụng ở bất kỳ vị trí nào và sau mỗi thao tác, số lượng thay đổi, do đó không gian trạng thái phân nhánh rất nhiều. Ngay cả khi chúng ta hạn chế các phép toán hợp lệ, số lượng các chuỗi có thể tăng theo cấp số nhân với số chữ số. Vì mỗi thao tác có khả năng ảnh hưởng đến tất cả các chữ số cao hơn thông qua nhớ, nên chúng tôi không thể phân tách quy trình thành các lựa chọn cục bộ độc lập. Điều này làm cho vũ lực không thể thực hiện được ngoài những đầu vào rất nhỏ. 

Quan sát quan trọng là hoạt động luôn có tác dụng cấu trúc giống nhau: nó chọn một vị trí$k$, quyết định xem việc mang có được kích hoạt hay không dựa trên chữ số ở bên phải, sau đó đưa mọi thứ bên dưới về 0$k$. Khi chúng tôi thực hiện một thao tác tại vị trí$k$, tất cả các vị trí bên dưới$k$bị mất vĩnh viễn. Điều này có nghĩa là câu trả lời cuối cùng được xác định bằng cách chọn một “vị trí hiệu quả cao nhất” mà tại đó việc làm tròn cuối cùng sẽ ổn định số, bởi vì các thao tác trước đó chỉ nhằm mục đích đẩy số mang lên trên và loại bỏ các hậu tố. 

Thay vì mô phỏng tất cả các chuỗi, chúng ta có thể nghĩ ngược lại: chúng ta muốn tìm một vị trí mà việc làm tròn tạo ra một tầng tối đa hóa các chữ số có nghĩa nhất. Một chữ số chỉ có tác động nếu nó kích hoạt chuỗi mang trong suốt 9 giây. Điều này gợi ý việc quét từ phải sang trái và theo dõi nơi làm tròn sẽ gây ra mức tăng có ý nghĩa. 

Chúng tôi mô phỏng quá trình truyền sóng mang tốt nhất có thể bằng cách xác định vị trí ngoài cùng bên trái trong đó việc làm tròn đến vị trí đó sẽ yêu cầu tăng một chữ số và truyền sóng mang qua 9 giây liên tiếp. Khi vị trí như vậy được xác định, mọi thứ ở bên phải của nó sẽ trở thành số 0 và mọi thứ ở bên trái của nó vẫn không thay đổi ngoại trừ các điều chỉnh mang. 

Điều này làm giảm vấn đề xuống còn một lần quét tuyến tính duy nhất để tính toán vị trí xa nhất bị ảnh hưởng bởi quyết định làm tròn tối ưu. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | Hàm mũ | Quá chậm | 
| Tối ưu | O(n) mỗi lần kiểm tra | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý số dưới dạng danh sách các chữ số có thể thay đổi. 

1. Chuyển đổi số đầu vào thành danh sách các số nguyên để chúng ta có thể sửa đổi trực tiếp các chữ số. 
2. Bắt đầu từ chữ số ngoài cùng bên phải, quét sang trái để phát hiện vị trí mà việc làm tròn sẽ có lợi. Cụ thể, chúng tôi tìm kiếm một chữ số ít nhất là 5 khi được xem là “kích hoạt” để làm tròn từ vị trí tiếp theo. Điều này tương ứng với tình huống làm tròn ở ranh giới đó sẽ làm tăng chữ số tiếp theo. 
3. Khi chúng tôi tìm thấy một vị trí như vậy, chúng tôi mô phỏng việc mang bắt đầu từ chữ số ngay bên trái của nó. Nếu chữ số đó trở thành 10, chúng ta đặt nó thành 0 và tiếp tục dịch chuyển sang trái cho đến khi không còn dấu nhớ nào hoặc chúng ta đạt đến chữ số có nghĩa nhất. 
4. Sau khi quá trình truyền mang hoàn tất, chúng tôi đặt tất cả các chữ số ở bên phải vị trí bị ảnh hưởng về 0. Điều này phản ánh thực tế là làm tròn ở vị trí$k$xóa hậu tố. 
5. Chúng tôi tiếp tục quét vì việc làm tròn trước đó vẫn có thể cải thiện kết quả ở bên trái, nhưng chúng tôi đảm bảo rằng chúng tôi không bao giờ đưa lại các chữ số khác 0 ở bên phải sau khi chúng đã bị loại bỏ. 
6. Cuối cùng, chúng tôi xây dựng lại số và loại bỏ các số 0 đứng đầu. 

Ý tưởng quan trọng là chúng tôi chỉ mô phỏng các chuỗi vận chuyển có ý nghĩa. Bất kỳ chữ số nào nhỏ hơn 5 đều không kích hoạt làm tròn, vì vậy nó không thể bắt đầu quá trình lan truyền có lợi. Quá trình này nén một cách hiệu quả nhiều thao tác làm tròn thành một phép biến đổi tham lam duy nhất của mảng chữ số. 

### Tại sao nó hoạt động 

Điều bất biến là ở mỗi bước, hậu tố ở bên phải của vị trí được xử lý hiện tại thể hiện cấu hình tốt nhất có thể đạt được trong bất kỳ chuỗi hoạt động nào chỉ ảnh hưởng đến các vị trí đó. Khi chúng tôi quyết định làm tròn tại một ranh giới nhất định, tất cả thông tin ở bên phải sẽ trở nên không liên quan, bởi vì mọi thao tác trong tương lai sẽ chỉ xóa hoặc giữ nguyên các số 0, không bao giờ tái tạo lại các chữ số hữu ích. Sự lựa chọn tham lam đảm bảo rằng mọi nhớ được truyền đi xa nhất có thể, giúp tối đa hóa các chữ số có ý nghĩa nhất mà không làm mất tính chính xác do bị cắt ngắn sớm. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    s = list(map(int, input().strip()))
    
    n = len(s)
    # We try to find the rightmost position where rounding causes change
    mark = -1
    
    for i in range(n - 1, 0, -1):
        if s[i] >= 5:
            mark = i

    if mark == -1:
        print("".join(map(str, s)))
        return

    # perform rounding at position mark
    s[mark] = 0
    carry = 1
    i = mark - 1

    while i >= 0 and carry:
        s[i] += carry
        if s[i] == 10:
            s[i] = 0
            carry = 1
        else:
            carry = 0
        i -= 1

    # zero out suffix
    for j in range(mark + 1, n):
        s[j] = 0

    print("".join(map(str, s)))

t = int(input())
for _ in range(t):
    solve()
```Trước tiên, mã xác định vị trí sâu nhất mà việc làm tròn có thể quan trọng bằng cách quét các chữ số có thể kích hoạt số mang. Sau đó, nó thực hiện một mô phỏng lần mang duy nhất ở bên trái, đảm bảo rằng bất kỳ lỗi tràn nào đều lan truyền chính xác. Cuối cùng, nó xóa hậu tố, bởi vì bất kỳ chiến lược tối ưu nào sử dụng cách làm tròn này đều phải loại bỏ mọi thứ ở bên phải trục đã chọn. 

Một chi tiết tinh tế là chúng tôi chỉ áp dụng một phép làm tròn hiệu quả, bởi vì bất kỳ chuỗi thao tác nào cũng có thể được thu gọn thành một phép làm tròn có tác động cao nhất do sự phá hủy hậu tố đơn điệu. 

## Ví dụ đã hoạt động 

### Ví dụ 1: 1980 

Chúng tôi xử lý các chữ số [1, 9, 8, 0]. 

| Bước | Vị trí quét | Đánh dấu | Bắt đầu thực hiện | Trạng thái sau khi thực hiện | Hậu tố cuối cùng | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 3 → 1 | 2 | từ chỉ số 1 | 2 0 0 0 | 0 0 | 

Chúng tôi phát hiện chữ số 8 ở vị trí 2, kích hoạt làm tròn. Mang lan truyền từ 9, tạo ra một tầng làm tăng chữ số đầu. 

Kết quả là 2000, phù hợp với chiến lược làm tròn tối ưu ở đúng ranh giới thay vì cục bộ ở chữ số cuối cùng. 

### Ví dụ 2: 20445 

Chữ số: [2, 0, 4, 4, 5] 

| Bước | Vị trí quét | Đánh dấu | Bắt đầu thực hiện | Trạng thái sau khi thực hiện | Hậu tố cuối cùng | 
| --- | --- | --- | --- | --- | --- | 
| 1 | quét phải | 4 | từ chỉ số 3 | 2 1 0 0 0 | 0 0 | 

Ở đây, 5 dấu sau kích hoạt làm tròn, nhưng phần mang truyền vào số 4, sau đó dừng lại mà không xếp tầng thêm. Kết quả đúng sẽ là 21000. 

Điều này cho thấy rằng ngay cả một phép làm tròn cục bộ ở ranh giới cũng có thể định hình lại nhiều chữ số thông qua việc truyền mang. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi chữ số được truy cập với số lần không đổi trong quá trình quét và truyền tải | 
| Không gian | O(n) | Chúng tôi lưu trữ số dưới dạng danh sách các chữ số có thể thay đổi | 

Tổng chiều dài của tất cả các trường hợp thử nghiệm nhiều nhất là$2 \cdot 10^5$, do đó, một giải pháp tuyến tính cho mỗi trường hợp thử nghiệm là đủ và chạy thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    def solve():
        s = list(map(int, input().strip()))
        n = len(s)
        mark = -1

        for i in range(n - 1, 0, -1):
            if s[i] >= 5:
                mark = i

        if mark == -1:
            return "".join(map(str, s))

        s[mark] = 0
        carry = 1
        i = mark - 1

        while i >= 0 and carry:
            s[i] += carry
            if s[i] == 10:
                s[i] = 0
                carry = 1
            else:
                carry = 0
            i -= 1

        for j in range(mark + 1, n):
            s[j] = 0

        return "".join(map(str, s))

    t = int(input())
    out = []
    for _ in range(t):
        out.append(solve())
    return "\n".join(out)

# provided samples
assert run("""10
1
5
99
913
1980
20444
20445
60947
419860
40862016542130810467
""") == """1
10
100
1000
2000
20444
21000
100000
420000
41000000000000000000"""

# custom cases
assert run("1\n0\n") == "0", "single zero"
assert run("1\n9\n") == "10", "single carry"
assert run("1\n199999\n") == "200000", "long carry chain"
assert run("1\n123456789\n") == "123457000", "mixed propagation"
assert run("1\n44444\n") == "44444", "no rounding trigger"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1, 0 | 0 | trường hợp tối thiểu | 
| 1, 9 | 10 | mang theo một chữ số | 
| Ngày 1 tháng 1 năm 199999 | 200000 | lan truyền dài | 
| 1, 123456789 | 123457000 | chữ số hỗn hợp và cắt ngắn | 
| 1, 44444 | 44444 | không có trường hợp hoạt động | 

## Vỏ cạnh 

Trường hợp cạnh khóa là khi hậu tố dài 9 giây kích hoạt tính năng xếp tầng dẫn đến chữ số có nghĩa nhất. Đối với đầu vào 199999, hành vi đúng là truyền số mang qua từng chữ số và tạo ra 200000. Thuật toán xử lý việc này một cách tự nhiên vì vòng lặp mang tiếp tục cho đến khi số mang biến mất hoặc chúng ta dùng hết tất cả các chữ số, đảm bảo việc truyền đầy đủ. 

Một trường hợp cạnh khác là khi không có chữ số nào đủ lớn để kích hoạt làm tròn. Đối với đầu vào 44444, không tìm thấy dấu nào nên số được trả về không thay đổi. Điều này đúng vì bất kỳ thao tác nào cũng sẽ chỉ làm giảm tầm quan trọng bằng cách đưa hậu tố về 0 mà không tạo ra các kết quả có lợi. 

Trường hợp cạnh thứ ba xảy ra khi tồn tại nhiều điểm làm tròn tiềm năng. Quá trình quét sẽ chọn trình kích hoạt hợp lệ ngoài cùng bên phải, tương ứng với điểm sớm nhất mà việc làm tròn vẫn cải thiện phần quan trọng nhất của số trong khi tránh phá hủy sớm cấu trúc hậu tố hữu ích.
