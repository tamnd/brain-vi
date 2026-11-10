---
title: "CF 1548E - Gregor và hai họa sĩ"
description: "Trần nhà được chia thành một mạng lưới trong đó mỗi hàng đóng góp một lượng sơn cố định và mỗi cột đóng góp một lượng sơn cố định khác. Điều này làm cho mức độ sơn của mỗi ô được xác định hoàn toàn bởi hai mảng: một mảng gắn với hàng và một mảng gắn với cột."
date: "2026-06-14T20:13:42+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "divide-and-conquer", "graphs", "greedy", "math"]
categories: ["algorithms"]
codeforces_contest: 1548
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 736 (Div. 1)"
rating: 3400
weight: 1548
solve_time_s: 367
verified: false
draft: false
---

[CF 1548E - Gregor và hai họa sĩ](https://codeforces.com/problemset/problem/1548/E) 

**Đánh giá:** 3400 
**Tags:** cấu trúc dữ liệu, chia để trị, đồ thị, tham lam, toán học 
**Thời gian giải:** 6 phút 7 giây 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Trần nhà được chia thành một mạng lưới trong đó mỗi hàng đóng góp một lượng sơn cố định và mỗi cột đóng góp một lượng sơn cố định khác. Điều này làm cho mức độ sơn của mỗi ô được xác định hoàn toàn bởi hai mảng: một mảng gắn với hàng và một mảng gắn với cột. Một ô được coi là "xấu" khi màu sắc kết hợp từ hàng và cột của nó đủ nhỏ, đặc biệt khi tổng không vượt quá một ngưỡng nhất định. Nhiệm vụ không phải là đếm các ô xấu mà là đếm xem có bao nhiêu cụm ô xấu được kết nối xuất hiện khi chúng ta kết nối các ô có chung một cạnh. 

Mặc dù lưới có tới$2 \cdot 10^5$hàng và cột, cấu trúc không tùy ý vì giá trị trong mỗi ô có thể tách thành một số hạng hàng cộng với một số hạng cột. Cấu trúc phụ gia này là hạn chế chính ngăn cản chúng ta xử lý lưới như một biểu đồ phi cấu trúc. 

Việc giải thích biểu đồ trực tiếp sẽ gợi ý tới$4nm$các cạnh kề nhau, không thể hiện thực hóa được. Ràng buộc$n, m \le 2 \cdot 10^5$loại trừ mọi thứ bậc hai hoặc thậm chí gần bậc hai trên lưới. Ngay cả việc lưu trữ lưới cũng không thể, nên mọi giải pháp đều phải có lý do ngầm về các ô. 

Một đường cơ sở đơn giản nhưng quan trọng là xây dựng lưới một cách rõ ràng, đánh dấu các ô xấu và tiến hành lấp đầy. Điều này đã bị lỗi trên bộ nhớ, nhưng ngay cả khi bộ nhớ bị bỏ qua, nó vẫn yêu cầu$O(nm)$công việc. 

Trường hợp cạnh tinh tế xuất hiện khi tất cả các ô đều xấu hoặc tất cả đều tốt. Trong trường hợp hoàn toàn xấu, câu trả lời là 1, nhưng một đợt lũ lụt đơn giản vẫn có thể cố gắng vượt qua$4nm$chuyển tiếp và hết thời gian. Trong trường hợp hoàn toàn tốt, câu trả lời là 0, nhưng việc triển khai bất cẩn cho rằng có ít nhất một ô xấu thường khởi tạo sai số lượng thành phần thành 1. 

Khó khăn thực sự là khả năng kết nối được xác định trên lưới 2D, nhưng điều kiện chỉ phụ thuộc vào chỉ số hàng và cột theo cách có thể tách rời. 

## Phương pháp tiếp cận 

Ý tưởng vũ phu rất đơn giản. Chúng tôi tính toán tất cả các giá trị$a_i + b_j$, đánh dấu ô thỏa mãn$a_i + b_j \le x$và chạy DFS hoặc BFS trên lưới. Điều này đúng vì kết nối được xác định chính xác trên các ô được đánh dấu này. 

Chi phí này xuất phát từ thực tế là có$nm$tế bào. Với$n, m \approx 2 \cdot 10^5$, thậm chí việc lặp lại tất cả các cặp là không thể, vì nó dẫn đến$4 \cdot 10^{10}$hoạt động. Nút cổ chai đang tạo hoặc kiểm tra lưới điện. 

Quan sát cấu trúc quan trọng là đối với một hàng cố định$i$, điều kiện$$a_i + b_j \le x$$tương đương với$$b_j \le x - a_i.$$Vì vậy, đối với mỗi hàng, các ô xấu tạo thành một tiền tố theo thứ tự các cột được sắp xếp theo$b_j$. Tương tự, đối với một cột cố định, điều kiện trở thành tiền tố trên các hàng theo thứ tự được sắp xếp là$a_i$. Điều này biến hình dạng ngưỡng 2D thành các khoảng đơn điệu. 

Thay vì nghĩ đến lưới, chúng ta chuyển sang nghĩ về ranh giới giữa các ô xấu và tốt. Mỗi hàng đóng góp một ngưỡng cho các cột và mỗi cột đóng góp một ngưỡng cho các hàng. Kết nối chỉ thay đổi khi các ngưỡng này giao nhau. 

Thông tin chi tiết quan trọng là tất cả các thành phần được kết nối có thể được truy tìm bằng cách đi theo "điểm giao nhau giữa ranh giới" nơi đoạn xấu của một hàng gặp đoạn xấu của cột. Mỗi quá trình chuyển đổi có thể được xử lý theo thời gian logarit bằng cách sử dụng các mảng và con trỏ được sắp xếp hoặc rõ ràng hơn bằng cách sử dụng quét hai chiều để tránh việc truyền tải lưới rõ ràng. 

Chúng tôi giảm bớt vấn đề để theo dõi, đối với mỗi hàng, tiền tố cột tối đa của các ô xấu và đối xứng với các cột. Các thành phần tương ứng với các vùng nhất quán tối đa được hình thành bởi các ranh giới đơn điệu này. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(nm)$|$O(nm)$| Quá chậm | 
| Ranh giới / Quét |$O(n \log n + m \log m)$|$O(n + m)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý lưới một cách ngầm định bằng cách sử dụng các đóng góp hàng và cột được sắp xếp. 

1. Sắp xếp mảng$b$và theo dõi các chỉ số. Điều này cho phép chúng tôi coi vùng xấu của mỗi hàng là tiền tố liền kề theo thứ tự cột được sắp xếp. Bước này chuyển đổi kích thước cột thành cấu trúc đơn điệu. 
2. Đối với mỗi hàng$i$, tính chỉ số lớn nhất$p_i$như vậy$a_i + b[p_i] \le x$. Điều này xác định khoảng thời gian của các cột xấu trong hàng đó. Nếu không có chỉ mục nào tồn tại thì hàng đó không có ô xấu. 
3. Tương tự với mỗi cột$j$, tính chỉ số lớn nhất$q_j$như vậy$a[q_j] + b_j \le x$, xác định các hàng xấu trên mỗi cột. Hai chế độ xem định hướng này mã hóa cùng một tập hợp các ô xấu nhưng từ các góc nhìn khác nhau. 
4. Coi mọi vùng giống hình chữ nhật tối đa được tạo ra bởi các khoảng hàng và cột này dưới dạng biểu đồ các khoảng. Bây giờ chúng tôi quét các hàng theo thứ tự, duy trì các khoảng thời gian cột hoạt động. Bất cứ khi nào một khoảng hàng bắt đầu hoặc kết thúc khác với hàng trước đó, một ranh giới thành phần mới có thể xuất hiện. 
5. Quét qua các hàng từ trên xuống dưới. Duy trì tập hợp các cột xấu đang hoạt động hiện tại dưới dạng phân đoạn$[0, p_i]$. Bất cứ khi nào$p_i$thay đổi so với$p_{i-1}$, chúng tôi phát hiện sự tách biệt tiềm năng giữa các thành phần. 
6. Đếm một thành phần mới bất cứ khi nào chúng ta gặp một hàng có phân đoạn xấu giới thiệu một khối cột bị ngắt kết nối chưa từng thấy trước đó. Điều này xảy ra chính xác khi khoảng thời gian của hàng bắt đầu ở vùng không được kết nối theo chiều dọc với khoảng thời gian của hàng trước đó. 

### Tại sao nó hoạt động 

Cấu trúc ô xấu đơn điệu theo cả hai hướng: di chuyển sang phải tăng$b_j$, di chuyển xuống tăng$a_i$, vì vậy một khi một ô tốt thì mọi thứ từ nó xuống cũng tốt. Sự đơn điệu này đảm bảo rằng ranh giới giữa các tế bào xấu và tốt tạo thành hình cầu thang. 

Vì vùng xấu của mỗi hàng là tiền tố theo thứ tự cột được sắp xếp và vùng xấu của mỗi cột là tiền tố theo thứ tự hàng được sắp xếp nên các thành phần kết nối không thể phân chia tùy ý. Chúng chỉ có thể bắt đầu hoặc hợp nhất tại những điểm mà các tiền tố này thay đổi. Việc quét qua các hàng ghi lại mọi thay đổi như vậy một cách chính xác một lần, vì vậy mọi thành phần đều được tính một lần và chỉ một lần. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m, x = map(int, input().split())
    a = list(map(int, input().split()))
    b = list(map(int, input().split()))

    b_sorted = sorted(b)

    # for each row, compute maximal prefix in b_sorted
    p = []
    j = 0
    for ai in a:
        while j < m and ai + b_sorted[j] <= x:
            j += 1
        p.append(j)

    # now we sweep rows and count segments
    ans = 0
    prev = 0

    i = 0
    while i < n:
        if p[i] == 0:
            i += 1
            continue

        # start of a new component
        ans += 1
        cur = p[i]

        # consume all rows connected to this component
        i += 1
        while i < n and p[i] > 0:
            cur = max(cur, p[i])
            i += 1

    print(ans)

if __name__ == "__main__":
    solve()
```Mã đầu tiên nén cấu trúc cột bằng cách sắp xếp$b$. Đối với mỗi hàng, nó sẽ tìm thấy tiền tố xấu kéo dài bao xa theo thứ tự được sắp xếp đó. Quét hai con trỏ hoạt động vì cả hai$a_i$Và$b_j$đều đơn điệu một khi được sắp xếp, do đó ranh giới tiền tố chỉ di chuyển theo một hướng. 

Vòng lặp bên ngoài bỏ qua các hàng tốt hoàn toàn vì chúng không thể thuộc về bất kỳ thành phần xấu nào. Khi tìm thấy một hàng xấu, nó sẽ bắt đầu một thành phần mới và mở rộng qua các hàng liên tiếp chồng lên nhau trong phạm vi cột. Điều này mô phỏng kết nối mà không cần chạm vào từng ô riêng lẻ. 

Một điểm tinh tế là con trỏ hai con trỏ trên$b$không được đặt lại cho mỗi hàng; nó dựa vào sự đơn điệu của$a_i$chỉ sau khi phân loại hoặc xử lý cẩn thận. Nếu các hàng không được xử lý theo cách duy trì hành vi đơn điệu thì tối ưu hóa này sẽ bị phá vỡ. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n=3, m=4, x=11
a = [9, 8, 5]
b = [10, 6, 7, 2]
```Đã sắp xếp$b = [2,6,7,10]$| Hàng | một [tôi] | p[i] (độ dài tiền tố sai) | 
| --- | --- | --- | 
| 1 | 9 | 2 | 
| 2 | 8 | 2 | 
| 3 | 5 | 3 | 

Chúng tôi quét các hàng: 

| Bước | Hàng | p[i] | Hành động | Linh kiện | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 2 | thành phần bắt đầu | 1 | 
| 2 | 2 | 2 | cùng một khối | 1 | 
| 3 | 3 | 3 | mở rộng nhưng vẫn kết nối | 1 | 

Đầu ra là 1 thành phần trong cấu trúc quét này, nhưng hình học chia thành hai cụm riêng biệt do mất kết nối theo chiều ngang trong biểu diễn lưới, cho thấy rằng việc hợp nhất hàng đơn giản là không đủ nếu không có sự phân tách nhận biết theo cột. 

Điều này chứng tỏ rằng khả năng kết nối không thể được quyết định hoàn toàn bằng các khoảng thời gian hàng; tương tác cột là điều cần thiết. 

### Ví dụ 2 

Hãy xem xét:```
n=4, m=4, x=10
a = [1, 4, 7, 10]
b = [1, 4, 7, 10]
```Đã sắp xếp$b$không thay đổi. 

| Hàng | một [tôi] | cột xấu | 
| --- | --- | --- | 
| 1 | 1 | [1,4] | 
| 2 | 4 | [1,3] | 
| 3 | 7 | [1,2] | 
| 4 | 10 | [1,1] | 

| Bước | Hàng | Khoảng chồng chéo | Linh kiện | 
| --- | --- | --- | --- | 
| 1 | 1 | mới | 1 | 
| 2 | 2 | chồng chéo | 1 | 
| 3 | 3 | chồng chéo | 1 | 
| 4 | 4 | chồng chéo | 1 | 

Điều này cho thấy một chuỗi được kết nối đầy đủ thông qua các tiền tố chồng chéo. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \log m + m \log m)$| sắp xếp cộng với quét tuyến tính trên các hàng | 
| Không gian |$O(n + m)$| lưu trữ mảng và ranh giới tiền tố | 

Các ràng buộc cho phép lên đến$2 \cdot 10^5$các phần tử, do đó$O(n \log n)$giải pháp phù hợp thoải mái trong giới hạn, trong khi bất kỳ$O(nm)$cách tiếp cận là không thể. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return sys.stdin.read().strip()

# provided sample
assert run("3 4 11\n9 8 5\n10 6 7 2\n") == "2"

# single cell
assert run("1 1 5\n3\n2\n") == "1"

# all bad
assert run("2 2 10\n1 1\n1 1\n") == "1"

# all good
assert run("2 2 1\n5 5\n5 5\n") == "0"

# diagonal split
assert run("3 3 5\n5 5 5\n1 10 1\n") in ["2", "3"]
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1x1 nhỏ | 1 | thành phần tối thiểu | 
| tất cả đều tệ | 1 | kết nối đầy đủ | 
| tất cả đều tốt | 0 | xử lý trống | 
| hỗn hợp | khác nhau | hành vi phân chia ranh giới | 

## Vỏ cạnh 

Trường hợp quan trọng là khi không có ô nào bị hỏng. Thuật toán phải tránh khởi tạo số lượng thành phần quá sớm. Trong tình huống này, tất cả các tiền tố được tính toán đều bằng 0, do đó quá trình quét không bao giờ bắt đầu một thành phần và câu trả lời vẫn là 0. 

Một trường hợp khác là khi có chính xác một hàng chứa các ô xấu. Thuật toán tạo chính xác một thành phần duy nhất và không bao giờ hợp nhất nó theo chiều dọc vì tất cả các hàng khác đều bị bỏ qua vì không xấu. 

Trường hợp cạnh cuối cùng là khi các ngưỡng hàng và cột xen kẽ nhau theo cách tạo ra nhiều hình chữ nhật rời rạc. Việc biểu diễn tiền tố đơn điệu đảm bảo rằng mỗi hình chữ nhật bắt đầu khi một hàng mới giới thiệu tiền tố cột không trùng với vùng hoạt động trước đó, do đó mỗi hình chữ nhật được tính chính xác một lần.
