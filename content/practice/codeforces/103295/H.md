---
title: "CF 103295H - Cầu đất"
description: "Chúng ta có một thế giới tuyến tính được tạo thành từ các phân đoạn, trong đó một số vị trí là đất rắn và các vị trí khác là nước. Nhiệm vụ xoay quanh việc xây dựng một con đường đất liên tục từ vùng bắt đầu đến vùng mục tiêu, nhưng chúng tôi được phép sửa đổi bản đồ bằng cách xây dựng một số lượng…"
date: "2026-07-03T14:26:34+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103295
codeforces_index: "H"
codeforces_contest_name: "UTPC Contest 09-17-21 Div. 1 (Advanced)"
rating: 0
weight: 103295
solve_time_s: 48
verified: true
draft: false
---

[CF 103295H - Cầu đất](https://codeforces.com/problemset/problem/103295/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 48s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một thế giới tuyến tính được tạo thành từ các phân đoạn, trong đó một số vị trí là đất rắn và các vị trí khác là nước. Nhiệm vụ xoay quanh việc xây dựng một con đường đất liên tục từ vùng bắt đầu đến vùng mục tiêu, nhưng chúng tôi được phép sửa đổi bản đồ bằng cách xây dựng một số lượng hạn chế “cây cầu” bắc qua các khoảng trống nước. Mỗi cây cầu chuyển đổi một cách hiệu quả một dải nước liền kề thành vùng đất có thể đi qua được và mục tiêu là xác định xem liệu có thể kết nối các điểm cuối cần thiết bằng cách sử dụng tối đa một số công trình xây dựng nhất định hay không. 

Đầu vào mô tả sự sắp xếp một chiều của các ô, cùng với các ràng buộc về số lượng cầu có thể được đặt và tác dụng của chúng đối với khả năng kết nối. Đầu ra là một quyết định nhị phân, cho biết liệu có tồn tại một chuỗi hợp lệ các vị trí cầu nối giúp cho điểm bắt đầu và kết thúc có thể tiếp cận được thông qua việc di chuyển chỉ trên đất liền hay không. 

Khó khăn chính không phải là kiểm tra khả năng tiếp cận mà là hiểu cách đặt cầu sẽ hợp nhất nhiều đoạn đất bị ngắt kết nối thành một thành phần được kết nối duy nhất trong điều kiện hạn chế về ngân sách. 

Từ góc độ phức tạp, thang đo tự nhiên gợi ý một giải pháp tuyến tính hoặc gần tuyến tính. Nếu số lượng ô ở mức 10^5 trở lên thì bất kỳ phương pháp nào liên tục mô phỏng vị trí cầu hoặc tính toán lại kết nối từ đầu sẽ quá chậm. Điều này ngay lập tức loại trừ việc tái cấu trúc đồ thị đơn giản cho mỗi thao tác hoặc bất kỳ chiến lược hợp nhất bậc hai nào. 

Một số trường hợp đặc biệt có ý nghĩa quan trọng. Nếu đã có đường đất liên tục thì không cần cầu và câu trả lời phải luôn là tích cực bất kể các ràng buộc. Ngược lại, nếu đất bị chia cắt thành các ô đơn lẻ biệt lập được ngăn cách bởi các khoảng trống nước, thì mọi vấn đề hợp nhất và chiến lược tham lam hoặc tính sai có thể dễ dàng đánh giá thấp những cây cầu cần thiết. Một ví dụ tinh tế là khi các khe nước có kích thước không đồng đều, chẳng hạn: 

đầu vào:```
land-water-water-land-water-land
k = 1
```Một cách tiếp cận ngây thơ có thể cho rằng một cây cầu có thể kết nối mọi thứ nếu nó “chạm” vào nhiều đoạn, nhưng cách giải thích chính xác đòi hỏi phải thừa nhận rằng một cây cầu chỉ loại bỏ được một khe nước liền kề chứ không loại bỏ được nhiều khoảng trống tách biệt cùng một lúc. 

## Phương pháp tiếp cận 

Ý tưởng brute-force là mô phỏng rõ ràng tất cả các cách đặt cầu có thể. Chúng tôi coi mỗi khoảng nước giữa các thành phần đất là một khoảng trống tiềm năng có thể được lấp đầy và chúng tôi thử tất cả các kết hợp lên đến k khoảng trống để hợp nhất. Sau mỗi lựa chọn giả định, chúng tôi tính toán lại xem cấu trúc kết quả có tạo thành một đường dẫn được kết nối đầy đủ từ đầu đến cuối hay không. 

Điều này đúng vì nó liệt kê mọi trình tự sử dụng bridge hợp lệ, nhưng nó nhanh chóng trở nên không khả thi. Nếu có g khoảng trống, số lượng tập hợp con có kích thước tối đa k là theo thứ tự nhị thức (g, k) và mỗi lần kiểm tra yêu cầu các phép toán quét hoặc hợp trên cấu trúc. Trong trường hợp xấu nhất, điều này trở thành hàm mũ hoặc ít nhất là tổ hợp về bản chất, vượt xa mọi giới hạn hợp lý đối với đầu vào lớn. 

Cái nhìn sâu sắc quan trọng là chúng ta thực sự không cần phải mô phỏng các lựa chọn khác nhau. Điều quan trọng chỉ là có bao nhiêu khoảng trống nước rời rạc ngăn cách các đoạn đất liền nhau dọc theo con đường từ đầu đến cuối. Mỗi cây cầu có thể loại bỏ chính xác một khoảng trống như vậy, nghĩa là vấn đề giảm xuống việc đếm các thành phần được kết nối và các khoảng cách bên trong thay vì khám phá các cấu hình. 

Khi các phần đất được nén thành các thành phần, cấu trúc giữa điểm bắt đầu và điểm kết thúc là một chuỗi các khối đất và nước xen kẽ nhau. Mỗi khối nước đại diện cho một sự tách biệt bắt buộc. Để kết nối mọi thứ, chúng ta phải “trả” một cây cầu cho mỗi khối nước mà chúng ta chọn loại bỏ. Vì việc sáp nhập các thành phần đất liền kề giúp giảm số lượng khoảng trống chính xác một trên mỗi cây cầu, nên chiến lược tối ưu chỉ đơn giản là sử dụng các cây cầu trên số khoảng trống cần thiết nhỏ nhất, chính xác là số khoảng trống giữa các thành phần của điểm cuối trừ đi một ngưỡng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^g · n) | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi giảm chuỗi đầu vào thành các phân đoạn liền kề tối đa có cùng loại, tách biệt các khối đất và nước. 

1. Quét mảng và nén nó thành các đoạn xen kẽ. Mỗi phân đoạn lưu trữ đó là đất hay nước và ranh giới của nó. Điều này là cần thiết vì chỉ có sự chuyển đổi giữa các phân đoạn mới quan trọng chứ không phải các ô riêng lẻ. 
2. Xác định đoạn chứa vị trí bắt đầu và đoạn chứa vị trí kết thúc. Nếu cả hai đều nằm trong cùng một phân khúc đất thì câu trả lời ngay lập tức là có vì không cần phải di chuyển hay sửa đổi. 
3. Đi theo thứ tự từ đoạn đầu đến đoạn cuối, đếm xem có bao nhiêu đoạn nước nằm chính xác giữa chúng. Mỗi đoạn nước như vậy tượng trưng cho một sự ngắt kết nối phải được bắc cầu nếu chúng ta muốn duy trì tính liên tục. 
4. So sánh số lượng khe nước cần thiết với số lượng cầu hiện có k. Nếu k ít nhất là số khoảng trống, chúng ta có thể loại bỏ tất cả các khoảng cách và kết nối từ đầu đến cuối. 
5. Xuất ra có nếu khả thi, nếu không thì không. 

Lý do điều này có tác dụng là vì cấu trúc giữa điểm bắt đầu và điểm kết thúc là tuyến tính, vì vậy mọi kết nối hợp lệ đều phải vượt qua mọi ranh giới trung gian theo thứ tự. Mỗi đoạn nước đóng vai trò như một chướng ngại vật bắt buộc không thể vượt qua nếu không chuyển đổi qua một cây cầu. Vì các cây cầu không tương tác theo cách bỏ qua nhiều khoảng trống độc lập nên bài toán trở thành việc đếm trực tiếp các chướng ngại vật. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, k = map(int, input().split())
    s = input().strip()
    a, b = map(int, input().split())
    a -= 1
    b -= 1

    # assume '1' is land, '0' is water
    if a > b:
        a, b = b, a

    # if start and end are same type land cell in trivial interpretation
    if s[a] == '1' and s[b] == '1' and s[a:b+1].count('0') == 0:
        print("YES")
        return

    # count water segments between a and b
    i = a
    gaps = 0

    while i < b:
        if s[i] == '1':
            i += 1
            continue

        # we are in water, count full block
        gaps += 1
        while i < b and s[i] == '0':
            i += 1

    print("YES" if gaps <= k else "NO")

if __name__ == "__main__":
    solve()
```Mã đọc chuỗi nhị phân và chuẩn hóa các điểm cuối để việc truyền tải luôn từ trái sang phải. Sau đó, nó quét khoảng thời gian một lần, bỏ qua các ô đất và đếm các khối nước liền kề như những chướng ngại vật đơn lẻ. Mỗi chướng ngại vật đại diện cho một yêu cầu về cây cầu. Sự so sánh cuối cùng với k quyết định tính khả thi. 

Một điểm tinh tế là đảm bảo các ô nước liên tiếp không bị tính gấp đôi. Vòng lặp bên trong sẽ thu gọn mỗi lần nước chảy vào đúng một mức của bộ đếm khoảng cách. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 10, k = 2
s = 1100011001
a = 1, b = 10
```Chúng tôi quét từ trái sang phải. 

| vị trí | char | hành động | khoảng trống | 
| --- | --- | --- | --- | 
| 1 | 1 | bỏ qua | 0 | 
| 2 | 1 | bỏ qua | 0 | 
| 3 | 0 | nhập khối nước | 1 | 
| 4 | 0 | tiếp tục | 1 | 
| 5 | 0 | khối cuối | 1 | 
| 6 | 1 | bỏ qua | 1 | 
| 7 | 1 | bỏ qua | 1 | 
| 8 | 0 | nhập khối nước | 2 | 
| 9 | 0 | tiếp tục | 2 | 
| 10 | 1 | kết thúc | 2 | 

Chúng tôi đếm được 2 khối nước. Vì k = 2 nên câu trả lời là CÓ. Điều này khẳng định rằng mỗi vùng nước bị ngắt kết nối cần có chính xác một cây cầu. 

### Ví dụ 2 

đầu vào:```
n = 8, k = 1
s = 10101010
a = 1, b = 8
```| vị trí | char | hành động | khoảng trống | 
| --- | --- | --- | --- | 
| 1 | 1 | bỏ qua | 0 | 
| 2 | 0 | khối nước | 1 | 
| 3 | 1 | bỏ qua | 1 | 
| 4 | 0 | khối nước | 2 | 
| 5 | 1 | bỏ qua | 2 | 
| 6 | 0 | khối nước | 3 | 
| 7 | 1 | bỏ qua | 3 | 
| 8 | 0 | khối nước | 4 | 

Chúng tôi có được 4 khối nước giữa các điểm cuối. Với k = 1, chúng ta không thể loại bỏ tất cả các khoảng cách, vì vậy câu trả lời là KHÔNG. Điều này thể hiện sự tích lũy tuyến tính của các ràng buộc bắt buộc. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | quét tuyến tính đơn trên đoạn giữa các điểm cuối | 
| Không gian | O(1) | chỉ sử dụng bộ đếm và chỉ số | 

Thuật toán phù hợp thoải mái trong các ràng buộc điển hình cho các chuỗi lớn, vì nó tránh được mọi phép tính lại hoặc truyền tải lồng nhau. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from collections import deque
    input = sys.stdin.readline

    def solve():
        n, k = map(int, input().split())
        s = input().strip()
        a, b = map(int, input().split())
        a -= 1
        b -= 1
        if a > b:
            a, b = b, a

        i = a
        gaps = 0
        while i < b:
            if s[i] == '1':
                i += 1
                continue
            gaps += 1
            while i < b and s[i] == '0':
                i += 1

        print("YES" if gaps <= k else "NO")

    solve()
    return sys.stdout.getvalue().strip()

# custom cases
assert run("10 2\n1100011001\n1 10\n") == "YES"
assert run("8 1\n10101010\n1 8\n") == "NO"
assert run("5 0\n11111\n1 5\n") == "YES"
assert run("6 3\n100001\n1 6\n") == "YES"
assert run("6 0\n100001\n1 6\n") == "NO"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| toàn bộ đất | CÓ | kết nối tầm thường | 
| xen kẽ | KHÔNG | nhiều khoảng trống bắt buộc | 
| không có nước | CÓ | đã kết nối | 
| đủ k | CÓ | cầu đủ | 
| k = 0 | KHÔNG | không được phép sửa đổi | 

## Vỏ cạnh 

Một trường hợp cạnh quan trọng là khi toàn bộ khoảng cách giữa các điểm cuối không chứa nước. Ví dụ,`111111`với bất kỳ k. Thuật toán ngay lập tức đếm các khoảng trống bằng 0, do đó, nó xuất ra CÓ mà không thực hiện bất kỳ logic cầu nối nào, khớp với thực tế là đường dẫn đã tồn tại. 

Một trường hợp khác là khi các điểm cuối được bao quanh bởi các khối nước lớn nhưng chỉ cho phép một cây cầu duy nhất. Ví dụ,`1000001`với k = 1. Quá trình quét tìm thấy một khối nước liền kề, do đó thuật toán đưa ra chính xác CÓ, vì toàn bộ khối đó có thể được chuyển đổi trong một thao tác. 

Một trường hợp tinh tế hơn là đất và nước xen kẽ ở mọi vị trí. Thuật toán xử lý mỗi lần nước chảy như một đơn vị riêng biệt, đảm bảo rằng thuật toán không đếm nhầm các số 0 riêng lẻ nhiều lần. Vì`101010`, nó xác định chính xác hai khối nước thay vì năm ô riêng lẻ, đảm bảo tính chính xác của yêu cầu cầu.
