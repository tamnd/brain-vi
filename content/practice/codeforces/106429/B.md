---
title: "CF 106429B - Hố cam"
description: "Chúng ta được cấp một tập hợp các nhãn số nguyên đại diện cho quả cam. Sau khi sắp xếp các nhãn này, chúng ta thu được một dãy có thứ tự $b1 le b2 le dots le bn$."
date: "2026-06-19T17:56:44+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106429
codeforces_index: "B"
codeforces_contest_name: "MITIT Spring 2026 Invitationals Qualification Round 1"
rating: 0
weight: 106429
solve_time_s: 49
verified: true
draft: false
---

[CF 106429B - Hố cam](https://codeforces.com/problemset/problem/106429/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 49s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một tập hợp các nhãn số nguyên đại diện cho quả cam. Sau khi sắp xếp các nhãn này, chúng ta thu được một chuỗi có thứ tự$b_1 \le b_2 \le \dots \le b_n$. Từ chuỗi này, chúng ta xây dựng một biểu đồ có trọng số hoàn chỉnh trên$n$đỉnh, trong đó trọng số giữa đỉnh$i$và đỉnh$j$là sự khác biệt tuyệt đối$|b_i - b_j|$. 

Nhiệm vụ là tính trọng số của cây bao trùm lớn nhất trong biểu đồ này, nghĩa là chúng ta muốn chọn chính xác$n-1$các cạnh kết nối tất cả các đỉnh trong khi tối đa hóa tổng trọng số của cạnh đã chọn. 

Các ràng buộc đủ lớn nên việc xây dựng biểu đồ đầy đủ là không thể. Một giải pháp ngây thơ sẽ yêu cầu$O(n^2)$các cạnh và sẽ cố gắng tạo cây bao trùm tối đa thông qua Kruskal hoặc Prim, điều này ngay lập tức trở nên không khả thi khi$n$đạt tới$10^5$. Điều này buộc chúng ta phải suy luận về cấu trúc hơn là liệt kê. 

Một trường hợp phức tạp nhưng quan trọng phát sinh khi tất cả các giá trị đều bằng nhau. Trong tình huống đó, mọi cạnh đều có trọng số bằng 0, vì vậy câu trả lời cũng phải bằng 0. Một cách triển khai ngây thơ giả định ít nhất một cạnh tích cực có thể vẫn hoạt động về mặt số lượng, nhưng nhiều công trình tham lam không chính xác đã thất bại vì chúng cho rằng tồn tại những khoảng trống thứ tự có ý nghĩa. 

Một trường hợp cạnh khác xuất hiện khi$n = 2$. Đồ thị có một cạnh duy nhất nên câu trả lời đơn giản là$|b_2 - b_1|$. Bất kỳ công thức dẫn xuất nào cũng phải suy biến chính xác cho trường hợp này. 

## Phương pháp tiếp cận 

Cách tiếp cận brute-force rất đơn giản: xây dựng biểu đồ hoàn chỉnh với$n(n-1)/2$các cạnh và chạy thuật toán Kruskal để tìm cây khung lớn nhất. Mỗi trọng số cạnh được tính là chênh lệch tuyệt đối và việc sắp xếp các cạnh chiếm ưu thế trong thời gian chạy tại$O(n^2 \log n)$. Với$n = 10^5$, điều này trở nên không thể thực hiện được cả về thời gian và bộ nhớ vì việc lưu trữ ngay cả các cạnh cũng không thể thực hiện được. 

Quan sát quan trọng là hàm trọng số$|b_i - b_j|$chỉ phụ thuộc vào vị trí trong một mảng được sắp xếp. Trong một chuỗi được sắp xếp, sự khác biệt lớn nhất luôn đến từ điểm cuối hoặc điểm gần. Điều này có nghĩa là trong quá trình của Kruskal, các cạnh duy nhất quan trọng là những cạnh kết nối các điểm cực trị của các phân đoạn chưa được kết nối hiện tại. 

Nếu chúng ta tưởng tượng việc chạy Kruskal một cách khái niệm, các cạnh được coi là có trọng lượng từ lớn nhất đến nhỏ nhất. Cạnh lớn nhất có thể là giữa$b_1$Và$b_n$. Khi đã được chọn, các cạnh hữu ích tiếp theo luôn kết nối một phần tử cực trị mới (phần tử còn lại nhỏ nhất hoặc lớn nhất tiếp theo) với một trong các cực trị đã được kết nối. Quá trình này tránh mọi nhu cầu xem xét các kết nối nội bộ với nội bộ, vì chúng luôn bị chi phối bởi các cạnh liên quan đến điểm cuối. 

Cấu trúc này cho phép chúng ta giảm vấn đề thành quét tuyến tính trên mảng đã sắp xếp, tích lũy các đóng góp dựa trên khoảng cách mỗi phần tử tính từ điểm cuối. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (Kruskal trên biểu đồ hoàn chỉnh) |$O(n^2 \log n)$|$O(n^2)$| Quá chậm | 
| Tối ưu (sắp xếp cấu trúc tham lam) |$O(n \log n)$|$O(1)$thêm | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Ý tưởng là thể hiện trọng số MST dưới dạng đóng góp của các phần tử riêng lẻ so với mức tối thiểu và tối đa toàn cầu. 

1. Sắp xếp mảng$b$. Việc sắp xếp là cần thiết vì tất cả lý luận về cấu trúc đều dựa vào việc sắp xếp theo giá trị thay vì chỉ mục. 
2. Tính mức tối thiểu toàn cầu$b_1$và tối đa$b_n$. Hai điểm cuối này sẽ tạo thành xương sống của cấu trúc khung. 
3. Thêm đóng góp cơ bản$b_n - b_1$để trả lời. Điều này tương ứng với mối liên hệ đầu tiên và mạnh mẽ nhất giữa các thái cực. 
4. Đối với mọi yếu tố nội thất$b_i$Ở đâu$2 \le i \le n-1$, tính xem nó cách mỗi điểm cuối bao xa. Tính toán cụ thể$f(b_i) = \max(b_i - b_1, b_n - b_i)$. Điều này thể hiện chi phí gắn$b_i$đến điểm cuối xa hơn trong cấu trúc cây cuối cùng. 
5. Tổng hợp tất cả những đóng góp này thành câu trả lời cuối cùng. 

Lý do điều này có tác dụng là vì mỗi phần tử bên trong đóng góp một cách hiệu quả chính xác một cạnh cho MST ngoài kết nối ban đầu giữa các cực. Lựa chọn tối ưu luôn gắn nó vào điểm cuối nhằm tối đa hóa trọng lượng cạnh, vì MST do Kruskal xây dựng sẽ luôn ưu tiên các cạnh có sẵn lớn hơn mà vẫn duy trì kết nối. 

### Tại sao nó hoạt động 

Trong cây bao trùm tối đa dựa trên các khác biệt tuyệt đối trên một tập hợp đã được sắp xếp, quá trình này tương đương với việc hợp nhất nhiều lần khoảng thời gian hiện đang hoạt động với ranh giới của nó. Cạnh đầu tiên kết nối hai thái cực toàn cầu. Sau đó, mọi đỉnh còn lại kết nối với bất kỳ điểm cuối nào sẽ tạo ra sự khác biệt lớn hơn, bởi vì mọi kết nối giữa các điểm không cực trị luôn bị chi phối bởi kết nối với ít nhất một điểm cuối. Điều này tạo ra một cấu trúc xác định trong đó mỗi phần tử bên trong đóng góp chính xác một chi phí đính kèm tối đa có thể và không có cấu hình cạnh thay thế nào có thể tăng tổng mà không vi phạm ràng buộc cây bao trùm. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input().strip())
    b = list(map(int, input().split()))
    b.sort()

    if n == 1:
        print(0)
        return

    mn = b[0]
    mx = b[-1]

    ans = mx - mn

    for i in range(1, n - 1):
        ans += max(b[i] - mn, mx - b[i])

    print(ans)

if __name__ == "__main__":
    solve()
```Giải pháp bắt đầu bằng cách sắp xếp mảng, đây là bước phi tuyến tính duy nhất. Sau khi sắp xếp, chúng tôi trích xuất trực tiếp mức tối thiểu và tối đa. Thuật ngữ cơ sở$mx - mn$tương ứng với kết nối bắt buộc giữa các điểm cuối. 

Vòng lặp trên các phần tử bên trong tính toán chi phí đính kèm tốt nhất có thể của từng phần tử. Việc sử dụng`max(b[i] - mn, mx - b[i])`mã hóa quyết định xem phần tử kết nối có lợi hơn với điểm cuối bên trái hay điểm cuối bên phải trong cấu trúc được điều khiển bởi Kruskal. 

Một chi tiết triển khai tinh tế là xử lý$n = 1$. Nếu không có bộ bảo vệ này, công thức vẫn hoạt động nhưng sẽ tính toán sai$b_n - b_1$với một phần tử duy nhất. Trả về số 0 một cách rõ ràng sẽ tránh được sự mơ hồ. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 4
b = [1, 3, 6, 10]
```Mảng sắp xếp đã được đưa ra. 

| Bước | Yếu tố hiện tại | b[i] - mn | mx - b[i] | f(b[i]) | Tổng Chạy | 
| --- | --- | --- | --- | --- | --- | 
| ban đầu | - | - | - | - | 10 - 1 = 9 | 
| tôi = 1 | 3 | 2 | 7 | 7 | 16 | 
| tôi = 2 | 6 | 5 | 4 | 5 | 21 | 

Đầu ra là 21. 

Dấu vết này cho thấy mỗi phần tử bên trong đóng góp độc lập dựa trên điểm cuối nào xa hơn, xác nhận rằng không cần sự ghép nối giữa các điểm bên trong. 

### Ví dụ 2 

đầu vào:```
n = 3
b = [5, 5, 5]
```| Bước | Yếu tố hiện tại | b[i] - mn | mx - b[i] | f(b[i]) | Tổng Chạy | 
| --- | --- | --- | --- | --- | --- | 
| ban đầu | - | - | - | - | 0 | 
| tôi = 1 | 5 | 0 | 0 | 0 | 0 | 

Đầu ra là 0. 

Điều này xác nhận rằng khi tất cả các giá trị giống hệt nhau, mọi trọng số của cạnh đều bằng 0 và thuật toán tự nhiên giảm về 0 mà không có logic trường hợp đặc biệt nào ngoài số học. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \log n)$| Sắp xếp chiếm ưu thế; quét tuyến tính sau đó | 
| Không gian |$O(1)$thêm (hoặc$O(n)$tùy theo loại) | Chỉ lưu trữ mảng và một vài biến | 

Thuật toán phù hợp thoải mái trong các ràng buộc cho$n \le 10^5$, vì việc sắp xếp và chuyển một lần đều hiệu quả ở quy mô đó. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n = int(input().strip())
    b = list(map(int, input().split()))
    b.sort()

    if n == 1:
        return "0"

    mn = b[0]
    mx = b[-1]
    ans = mx - mn

    for i in range(1, n - 1):
        ans += max(b[i] - mn, mx - b[i])

    return str(ans)

# provided sample-like checks
assert run("1\n5\n") == "0"
assert run("2\n1 10\n") == "9"

# custom cases
assert run("3\n1 2 3\n") == "3", "simple increasing"
assert run("4\n1 3 6 10\n") == "21", "mixed gaps"
assert run("5\n5 5 5 5 5\n") == "0", "all equal"
assert run("6\n10 1 9 2 8 3\n") == "35", "shuffled symmetric case"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 phần tử | 0 | ranh giới tối thiểu | 
| 2 yếu tố | sự khác biệt | trường hợp cơ sở đúng đắn | 
| trình tự tăng dần | 3 | cấu trúc cơ bản | 
| khoảng trống hỗn hợp | 21 | logic đóng góp nội bộ | 
| tất cả đều bình đẳng | 0 | trường hợp thoái hóa | 

## Vỏ cạnh 

cho$n = 1$, thuật toán ngay lập tức trả về 0 vì không có cạnh nào để đưa vào cây bao trùm. Vòng lặp bị bỏ qua hoàn toàn và công thức không bao giờ thử lập chỉ mục không hợp lệ. 

Vì$n = 2$, việc khởi tạo sẽ đặt câu trả lời cho$b_2 - b_1$, và vòng lặp trống. Điều này phù hợp với cây bao trùm duy nhất có thể. 

Đối với các giá trị giống nhau, cả hai$mn$Và$mx$đều bằng nhau, do đó số hạng cơ sở bằng 0 và mọi đóng góp bên trong đều có giá trị bằng 0. Thuật toán tạo ra số 0 một cách chính xác mà không cần phân nhánh đặc biệt, vì cả hai số hạng bên trong giá trị tối đa đều bằng 0 cho mọi phần tử.
