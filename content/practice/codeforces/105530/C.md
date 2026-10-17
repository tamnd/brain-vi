---
title: "CF 105530C - Đi bộ quá nhiều"
description: "Chúng ta có một tập hợp các điểm trên lưới 2D, trong đó mỗi điểm có thể được coi là một ô có tọa độ nguyên. Đối với mỗi điểm, chúng tôi muốn xác định khoảng cách từ điểm đó đến điểm xa nhất khác, nơi khoảng cách được đo bằng khoảng cách Manhattan."
date: "2026-06-23T22:58:24+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105530
codeforces_index: "C"
codeforces_contest_name: "Metropolitan University Inter University Programming Contest - Sylhet Division 2024"
rating: 0
weight: 105530
solve_time_s: 50
verified: true
draft: false
---

[CF 105530C - Đi bộ quá nhiều](https://codeforces.com/problemset/problem/105530/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 50s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một tập hợp các điểm trên lưới 2D, trong đó mỗi điểm có thể được coi là một ô có tọa độ nguyên. Đối với mỗi điểm, chúng tôi muốn xác định khoảng cách từ điểm đó đến điểm xa nhất khác, nơi khoảng cách được đo bằng khoảng cách Manhattan. Khoảng cách Manhattan giữa hai điểm là tổng chênh lệch tuyệt đối của tọa độ x và y của chúng. 

Nhiệm vụ là tính toán, cho mỗi điểm, khoảng cách tối đa của Manhattan đến bất kỳ điểm nào khác trong tập hợp và xuất ra các giá trị tối đa này. 

Cấu trúc ràng buộc là điều làm cho điều này trở nên thú vị. Nếu có tới n điểm, việc so sánh trực tiếp theo cặp sẽ bao hàm việc kiểm tra tất cả các cặp, dẫn đến hành vi bình phương n. Với n khoảng 200.000 hoặc thậm chí 2000 tùy theo phiên bản, n bình phương đã không khả thi vì nó sẽ liên quan đến hàng tỷ hoặc hàng nghìn tỷ phép tính. Điều đó ngay lập tức loại trừ bất kỳ giải pháp nào so sánh rõ ràng từng điểm với mọi điểm khác. 

Việc triển khai ngây thơ cũng có thể vấp phải các giá trị tuyệt đối. Ví dụ: cố gắng tối ưu hóa trực tiếp bằng cách sắp xếp các điểm theo x hoặc y một cách độc lập không giải quyết trực tiếp khoảng cách Manhattan, vì cả hai tọa độ đều tương tác. 

Trường hợp cạnh tinh tế phát sinh khi tất cả các điểm giống hệt nhau hoặc nằm trên một đường thẳng. Nếu tất cả các điểm đều giống nhau thì câu trả lời đúng cho mọi điểm là 0. Nếu tất cả các điểm nằm trên một đường chéo như (i, i), thì khoảng cách Manhattan hoàn toàn tỷ lệ thuận với chênh lệch chỉ số và các đơn giản hóa không chính xác xử lý x và y một cách độc lập có thể vẫn hoạt động trên các đầu vào có cấu trúc như vậy trong khi không thành công trong các trường hợp chung. 

Ví dụ: xét các điểm (0, 0), (1, 2), (3, 1). Một phép chiếu đơn giản lên x hoặc y không thể hiện được rằng điểm xa nhất phụ thuộc vào một biểu thức kết hợp, chứ không phải cực đại độc lập. 

## Phương pháp tiếp cận 

Khó khăn chính là giá trị tuyệt đối trong công thức khoảng cách Manhattan. Đối với hai điểm (x, y) và (a, b), khoảng cách là |x − a| + |y − b|. Phương pháp bạo lực trực tiếp tính toán giá trị này cho mỗi cặp và theo dõi mức cực đại trên mỗi điểm. Điều này đúng vì nó tuân theo định nghĩa một cách chính xác, nhưng nó tốn O(n²) tính toán khoảng cách, mỗi lần không đổi, quá chậm khi n lớn. 

Quan sát quan trọng là các biểu thức giá trị tuyệt đối có thể được mở rộng thành một tập hợp hữu hạn các dạng tuyến tính tùy thuộc vào việc lựa chọn dấu hiệu. Đối với một chênh lệch tuyệt đối duy nhất, |x − a| có thể được viết lại thành max(x − a, a − x). Mở rộng ý tưởng này, khoảng cách Manhattan trở thành giá trị lớn nhất trong bốn biểu thức tuyến tính, mỗi biểu thức tương ứng với một lựa chọn dấu hiệu cho hiệu x và y. Điều này chuyển đổi một số liệu phi tuyến tính thành một tập hợp các hàm tuyến tính. 

Một khi được viết lại dưới dạng này, vấn đề sẽ thay đổi về cơ bản. Thay vì so sánh trực tiếp từng cặp điểm, chúng ta chỉ cần đánh giá một số lượng nhỏ biểu thức tuyến tính cố định trên tất cả các điểm. Đối với mỗi biểu thức này, chúng ta có thể tính toán trước giá trị lớn nhất trên tất cả các điểm. Sau đó, đối với mỗi điểm, chúng tôi đánh giá mức đóng góp của nó theo từng biểu thức và kết hợp nó với cực đại toàn cục được tính toán trước. 

Điều này hoạt động vì mỗi trường hợp khoảng cách Manhattan tương ứng với chính xác một trong các cấu hình ký hiệu này và giá trị tối đa trên tất cả các điểm phải xuất hiện ở một trong các dạng tuyến tính này. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(1) | Quá chậm | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi chuyển đổi từng điểm thành các đóng góp theo các mẫu ký hiệu khác nhau, sau đó sử dụng cực trị tổng thể để trả lời các truy vấn trong thời gian không đổi cho mỗi điểm.

1. Với mỗi điểm (x, y), hãy tính bốn giá trị được chuyển đổi tương ứng với các tổ hợp dấu: x + y, x − y, −x + y và −x − y. Mỗi biểu thức thể hiện một cách mà các giá trị tuyệt đối có thể giải quyết khi được mở rộng. 
2. Đối với mỗi biểu thức trong số bốn biểu thức này, hãy quét tất cả các điểm và tính giá trị lớn nhất. Chúng thể hiện sự đóng góp điểm đối tác tốt nhất có thể theo từng cấu hình dấu hiệu. Bước này tách biệt cấu trúc toàn cục nên sau này chúng ta không cần so sánh từng cặp. 
3. Với mỗi điểm (x, y), hãy tính giá trị của từng biểu thức trong bốn biểu thức tại điểm đó. 
4. Đối với mỗi biểu thức, hãy tính toán câu trả lời của ứng viên bằng cách trừ giá trị lớn nhất của điểm đó khỏi giá trị tối đa được tính toán trước của biểu thức đó. Điều này mô phỏng việc ghép điểm với đối tác có dấu hiệu ngược lại tốt nhất có thể. 
5. Lấy điểm tối đa trong bốn ứng cử viên. Điều này mang lại khoảng cách Manhattan xa nhất cho điểm đó. 

Lý do phép trừ xuất hiện là vì mỗi dạng tuyến tính mã hóa khoảng cách một cách hiệu quả dưới dạng chênh lệch giữa hai giá trị điểm được chuyển đổi, do đó việc ghép nối sẽ giảm đến mức tối đa hóa sự khác biệt đó. 

### Tại sao nó hoạt động 

Khoảng cách Manhattan phân rã thành giá trị cực đại trên một tập hợp cố định các hàm tuyến tính rút ra từ các lựa chọn dấu độc lập trên hiệu x và y. Mỗi cặp điểm đóng góp khoảng cách của nó dưới đúng một trong các dạng này. Bằng cách tính toán trước giá trị tối đa của từng biểu mẫu trên toàn cầu, chúng tôi đảm bảo rằng đối với bất kỳ điểm cố định nào, đối tác tốt nhất có thể có trong mỗi biểu mẫu đều đã được biết đến. Lấy mức tối đa trên các biểu mẫu sẽ xây dựng lại khoảng cách tối đa thực sự vì mọi cặp hợp lệ đều được bao phủ bởi chính xác một trong các biểu mẫu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    pts = [tuple(map(int, input().split())) for _ in range(n)]

    INF = 10**18
    max1 = -INF  # x + y
    max2 = -INF  # x - y
    max3 = -INF  # -x + y
    max4 = -INF  # -x - y

    for x, y in pts:
        max1 = max(max1, x + y)
        max2 = max(max2, x - y)
        max3 = max(max3, -x + y)
        max4 = max(max4, -x - y)

    for x, y in pts:
        ans = 0
        ans = max(ans, max1 - (x + y))
        ans = max(ans, max2 - (x - y))
        ans = max(ans, max3 - (-x + y))
        ans = max(ans, max4 - (-x - y))
        print(ans)

if __name__ == "__main__":
    solve()
```Giải pháp đầu tiên tổng hợp cực trị tổng thể cho bốn hệ tọa độ được chuyển đổi. Mỗi cực trị này tương ứng với điểm cuối tốt nhất có thể có trong cấu hình dấu hiệu cố định. 

Ở bước thứ hai, mỗi điểm tính toán xem nó có thể kéo dài bao xa trong mỗi hệ thống được chuyển đổi bằng cách trừ đi phần đóng góp của chính nó. Phép trừ rất quan trọng vì nó chuyển đổi việc lựa chọn cặp tuyệt đối thành bài toán tối đa hóa sai phân. 

Một lỗi triển khai phổ biến là tính lại cực đại trong khi lặp lại các câu trả lời, điều này vô tình cho phép một điểm ghép nối với chính nó theo cách có thể làm sai lệch lý luận trong các trường hợp góc. Tính toán trước đảm bảo tính nhất quán trên tất cả các truy vấn. 

Một vấn đề vi tế khác là quên rằng bốn sự chuyển hóa là đủ; việc thêm nhiều hoặc ít biểu thức sẽ phá vỡ tính hoàn chỉnh của việc phân tách giá trị tuyệt đối. 

## Ví dụ đã hoạt động 

Xét các điểm (0, 0), (2, 3), (5, 1). 

Chúng tôi tính toán các giá trị được chuyển đổi: 

| Điểm | x+y | x−y | −x+y | −x−y | 
| --- | --- | --- | --- | --- | 
| (0,0) | 0 | 0 | 0 | 0 | 
| (2,3) | 5 | -1 | 1 | -5 | 
| (5,1) | 6 | 4 | -4 | -6 | 

Cực đại toàn cục là max1 = 6, max2 = 4, max3 = 1, max4 = 0. 

Bây giờ tính toán câu trả lời: 

| Điểm | max1-(x+y) | max2-(x−y) | max3-(-x+y) | max4-(-x−y) | Trả lời | 
| --- | --- | --- | --- | --- | --- | 
| (0,0) | 6 | 4 | 1 | 0 | 6 | 
| (2,3) | 1 | 5 | 0 | 5 | 5 | 
| (5,1) | 0 | 0 | 5 | 6 | 6 | 

Điều này xác nhận rằng mỗi điểm đều tìm thấy đối tác xa nhất của nó mà không cần liệt kê cặp rõ ràng. 

Ví dụ thứ hai có cấu trúc đối xứng: (1,1), (1,1), (1,1). Tất cả các giá trị được chuyển đổi đều giống hệt nhau, vì vậy mọi bản thân trừ tối đa đều bằng 0. Đầu ra chính xác trở thành tất cả các số 0, xác nhận tính chính xác của đầu vào suy biến. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Hai đường đi tuyến tính qua các điểm với công không đổi trên mỗi điểm | 
| Không gian | O(1) | Chỉ một số cực đại đang chạy được lưu trữ | 

Thuật toán phù hợp thoải mái trong các ràng buộc điển hình cho các tập tọa độ lớn vì nó tránh hoàn toàn việc tính toán khoảng cách theo cặp. Ngay cả với n lên tới 200.000, giải pháp chỉ thực hiện một số phép tính số học đơn giản cho mỗi điểm. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from sys import stdout
    out_lines = []

    n = int(sys.stdin.readline())
    pts = [tuple(map(int, sys.stdin.readline().split())) for _ in range(n)]

    INF = 10**18
    max1 = max(x + y for x, y in pts)
    max2 = max(x - y for x, y in pts)
    max3 = max(-x + y for x, y in pts)
    max4 = max(-x - y for x, y in pts)

    res = []
    for x, y in pts:
        ans = max(
            max1 - (x + y),
            max2 - (x - y),
            max3 - (-x + y),
            max4 - (-x - y),
        )
        res.append(str(ans))
    return "\n".join(res)

# sample-like
assert run("3\n0 0\n2 3\n5 1\n") == "6\n5\n6"

# single point
assert run("1\n10 10\n") == "0"

# all equal
assert run("3\n1 1\n1 1\n1 1\n") == "0\n0\n0"

# line structure
assert run("3\n0 0\n1 1\n2 2\n") == "4\n2\n4"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| điểm duy nhất | 0 | trường hợp cơ bản tầm thường | 
| mọi điểm bằng nhau | tất cả số không | xử lý trùng lặp | 
| đường chéo | khoảng cách đối xứng | độ chính xác hình học có cấu trúc | 

## Vỏ cạnh 

Đối với đầu vào một điểm như (10, 10), thuật toán tính toán tất cả các cực đại được chuyển đổi tương ứng là 20, 0, 0, -20. Khi trừ phần đóng góp của điểm đó, mọi biểu thức đều có giá trị bằng 0, tạo ra câu trả lời đúng. 

Đối với tất cả các điểm giống nhau, mọi giá trị được chuyển đổi đều giống hệt nhau, vì vậy mỗi giá trị tối đa bằng giá trị của từng điểm. Phép trừ hủy bỏ chính xác, đảm bảo khoảng cách bằng 0 và xác nhận không phát sinh vấn đề tự ghép nối. 

Đối với các điểm được sắp xếp trên đường chéo như (0,0), (1,1), (2,2), phép biến đổi giảm xuống thành các độ lệch tuyến tính đơn giản. Thuật toán vẫn nắm bắt chính xác mức chênh lệch tối đa vì mỗi cấu hình dấu hiệu đều thu gọn một cách nhất quán và cực đại tổng thể thể hiện chính xác các điểm cuối của đường.
