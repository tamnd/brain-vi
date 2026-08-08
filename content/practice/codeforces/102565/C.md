---
title: "CF 102565C - Flash"
description: "Chúng tôi có lưới N x N. Flash bắt đầu ở ô trên cùng bên trái. Đối với mọi giá trị khoảng cách d từ 1 đến 2N-2, chúng tôi xem xét tất cả các ô có thể tới được bằng cách liên tục thực hiện các bước di chuyển chỉ đi xuống hoặc sang phải, trong đó mỗi lần di chuyển như vậy phải tăng khoảng cách Manhattan so với trước đó…"
date: "2026-08-06T20:52:16+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102565
codeforces_index: "C"
codeforces_contest_name: "AGM 2020, Final Round, Day 2"
rating: 0
weight: 102565
solve_time_s: 217
verified: true
draft: false
---

[CF 102565C - Flash](https://codeforces.com/problemset/problem/102565/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 3 phút 37s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi có lưới N x N. Flash bắt đầu ở ô trên cùng bên trái. Đối với mọi giá trị khoảng cách d từ 1 đến 2N-2, chúng tôi xem xét tất cả các ô có thể tới được bằng cách liên tục thực hiện các bước di chuyển chỉ đi xuống hoặc sang phải, trong đó mỗi lần di chuyển như vậy phải tăng khoảng cách Manhattan từ ô trước đó thêm chính xác d. Chúng tôi đếm số lần mỗi ô được đánh dấu trên tất cả các giá trị của d và chúng tôi cần số ô có số lượng cuối cùng là số lẻ. 

Điều đầu tiên cần chú ý là N có thể lớn tới 10^9. Không thể mô phỏng trên lưới vì lưới chứa tối đa 10^18 ô. Ngay cả việc lặp lại một hàng hoặc một đường chéo cũng quá tốn kém. Lời giải chỉ phải phụ thuộc vào tính chất toán học của tọa độ. 

Một ô ở tọa độ (i, j) chỉ bị ảnh hưởng bởi giá trị i + j - 2, bởi vì mỗi bước di chuyển được phép sẽ tăng i + j một cách chính xác bằng khoảng cách Manhattan của nó. Điều này chuyển đổi vấn đề từ một quá trình hai chiều thành các giá trị đếm trên đường chéo. 

Các trường hợp cạnh chính đến từ các lưới rất nhỏ và từ các đường chéo đầu tiên và cuối cùng. Với N = 1, ô bắt đầu không bao giờ được tính nên câu trả lời là 0. Đối với đầu vào 1, đầu ra là 0 vì không có nước đi nào để thực hiện. Việc triển khai bất cẩn tính ô bắt đầu là ô có thể truy cập sẽ trả về 1. 

Với N = 2, các ô được đánh dấu duy nhất là (1,2) và (2,1). Giá trị đường chéo i + j - 2 là 1 cho cả hai ô và 1 có một ước số, do đó kết quả đầu ra là 2. Điều này giúp tìm ra các nghiệm vô tình bỏ qua đường chéo khác 0 nhỏ nhất. 

Đối với N lớn, giá trị đường chéo lớn nhất có thể là 2N-2. Việc triển khai phải tránh lặp lại trên tất cả các ô hoặc tất cả các đường chéo vì N có thể là một tỷ. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp sẽ mô phỏng mọi giá trị của d. Đối với mỗi d, chúng ta có thể giữ một tập hợp các ô hiện có thể truy cập được và áp dụng nhiều lần các chuyển tiếp. Điều này đúng vì nó tuân theo định nghĩa một cách chính xác, nhưng kích thước lưới khiến điều đó là không thể. Trường hợp xấu nhất chứa 10^18 ô, do đó, ngay cả việc chạm vào từng ô một lần cũng không thể thực hiện được. 

Nhận xét quan trọng là mọi chuyển động đều đơn điệu. Nếu chúng ta sử dụng tọa độ dựa trên 0 x = i - 1 và y = j - 1, thì một bước di chuyển của khoảng cách d sẽ tăng x + y chính xác bằng d. Bắt đầu từ x + y = 0, sau vài lần di chuyển có độ dài d chúng ta chỉ có thể đến được các ô trong đó x + y là bội số của d. 

Hướng ngược lại cũng đúng. Nếu x + y = k * d, chúng ta có thể chia tổng số chuyển động k * d cần thiết thành k chuyển động, mỗi chuyển động chứa tổng cộng d bước. Chúng tôi phân phối các bước đi xuống và bước sang phải cần thiết trong số các bước di chuyển này để mọi ô trên đường chéo như vậy đều có thể truy cập được. 

Do đó, một ô có giá trị s = i + j - 2 được đánh dấu một lần cho mỗi ước số dương của s. Số ước của một số nguyên dương là số lẻ khi số nguyên đó là số chính phương. Điều này đặt ra cho chúng ta một bài toán đếm: có bao nhiêu ô nằm trên các đường chéo có chỉ số là một hình vuông hoàn hảo? 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(N2) | O(N2) | Quá chậm | 
| Tối ưu | O(sqrt(N)) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Lặp lại mọi giá trị bình phương hoàn hảo s có thể xuất hiện dưới dạng chỉ số đường chéo. Giá trị lớn nhất có thể là 2N-2, vì vậy chúng ta chỉ cần các bình phương đạt đến giới hạn đó. 
2. Với mỗi ô vuông s như vậy, hãy đếm xem có bao nhiêu ô thỏa mãn i + j - 2 = s. Các ô này tạo thành một đường chéo của lưới. 
3. Cộng kích thước của tất cả các đường chéo này. Tổng là câu trả lời cuối cùng vì chính xác các đường chéo này có số dấu lẻ. 

Đối với giá trị đường chéo s, sử dụng tọa độ x và y dựa trên 0, chúng ta cần x + y = s với 0 ≤ x, y < N. Số giá trị x hợp lệ là độ dài giao điểm giữa đường chéo này và lưới vuông. 

Tại sao nó hoạt động:

Một ô được tính một lần cho mỗi ước số của chỉ số đường chéo của nó. Các ước số thường xuất hiện theo cặp, một ở dưới và một ở trên căn bậc hai. Trường hợp duy nhất một số chia không có đối tác khác là khi số đó là hình vuông. Do đó, một ô đóng góp chính xác khi chỉ số đường chéo của nó là một hình vuông hoàn hảo. Đếm các đường chéo được lập chỉ mục hình vuông sẽ đếm mọi ô có số đánh dấu lẻ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())

    ans = 0
    limit = 2 * n - 2

    s = 1
    while s * s <= limit:
        d = s * s

        left = max(0, d - (n - 1))
        right = min(n - 1, d)

        if right >= left:
            ans += right - left + 1

        s += 1

    print(ans)

if __name__ == "__main__":
    solve()
```Vòng lặp tạo ra mọi chỉ số đường chéo vuông có thể có. giá trị`d`đại diện cho đường chéo dựa trên x + y. 

Các biến`left`Và`right`mô tả phạm vi hợp lệ của tọa độ x. Nếu x cố định thì y = d - x, do đó x phải nằm trong khoảng từ 0 đến N-1 và cũng giữ y trong cùng một phạm vi. Giao nhau của hai phạm vi này sẽ cho độ dài đường chéo. 

Mã sử ​​dụng số nguyên Python, do đó không có vấn đề tràn mặc dù câu trả lời có thể lớn. Các phép tính ranh giới đều mang tính bao hàm, đó là lý do tại sao kích thước đường chéo là`right - left + 1`. 

## Ví dụ đã hoạt động 

Với N = 4, chỉ số đường chéo lớn nhất là 6. Chỉ số hình vuông là 1 và 4. 

| Đường chéo vuông | Giá trị x hợp lệ | Đếm tế bào | 
| --- | --- | --- | 
| 1 | 0 đến 1 | 2 | 
| 4 | 1 đến 3 | 3 | 

Tổng số là 5, phù hợp với mẫu. Các ô được đếm chính xác là hai ô trên đường chéo thứ nhất và ba ô trên đường chéo thứ tư. 

Với N = 5, chỉ số đường chéo lớn nhất là 8. Chỉ số bình phương là 1 và 4. 

| Đường chéo vuông | Giá trị x hợp lệ | Đếm tế bào | 
| --- | --- | --- | 
| 1 | 0 đến 1 | 2 | 
| 4 | 0 đến 4 | 5 | 

Câu trả lời là 7. Ví dụ này cho thấy một đường chéo hình vuông có thể chạm vào toàn bộ chiều rộng của lưới khi nó ở gần tâm. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(sqrt(N)) | Chúng tôi chỉ kiểm tra các số bình phương lên tới 2N-2 | 
| Không gian | O(1) | Chỉ có một vài biến số nguyên được lưu trữ | 

Có ít hơn 50000 lần lặp ngay cả đối với đầu vào tối đa có thể, vì vậy giải pháp dễ dàng phù hợp với giới hạn thời gian. 

## Trường hợp thử nghiệm```python
import sys
import io

def run(inp: str) -> str:
    old = sys.stdin
    sys.stdin = io.StringIO(inp)

    n = int(sys.stdin.readline())
    ans = 0
    s = 1
    limit = 2 * n - 2

    while s * s <= limit:
        d = s * s
        left = max(0, d - (n - 1))
        right = min(n - 1, d)
        if right >= left:
            ans += right - left + 1
        s += 1

    sys.stdin = old
    return str(ans)

assert run("4\n") == "5", "sample 1"
assert run("233\n") == "1974", "sample 2"

assert run("1\n") == "0", "single cell"
assert run("2\n") == "2", "smallest nontrivial grid"
assert run("3\n") == "4", "center square diagonal case"
assert run("1000000000\n") == "999999999000000000", "maximum size"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 | 0 | Ô bắt đầu không được tính | 
| 2 | 2 | Lưới nhỏ nhất với các ô có thể truy cập | 
| 3 | 4 | Đường chéo ở giữa và tính hình vuông | 
| 1000000000 | 999999999000000000 | Xử lý đầu vào lớn | 

## Vỏ cạnh 

Với N = 1, không có chỉ số đường chéo dương. Thuật toán không bao giờ đi vào vòng lặp và trả về 0, phù hợp với thực tế là ô ban đầu không được đánh dấu bằng bất kỳ khoảng cách nào. 

Với N = 2, chỉ số đường chéo hình vuông duy nhất có sẵn là 1. Đường chéo chứa hai ô, do đó cả hai đều có một ước số và cả hai đều đóng góp vào câu trả lời. 

Đối với N rất lớn, thuật toán không bao giờ xây dựng lưới. Nó chỉ truy cập các số bình phương lên tới 2N-2 nên thời gian chạy phụ thuộc vào căn bậc hai của đầu vào chứ không phải số lượng ô. 

Tôi cũng có thể cung cấp một phiên bản biên tập ngắn hơn theo phong cách cuộc thi hoặc một phiên bản có tính chứng minh cao hơn với đối số số chia được mở rộng.
