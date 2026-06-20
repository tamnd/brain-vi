---
title: "CF 1036A - Chiều cao chức năng"
description: "Hình này là một đường đứt nét được tạo từ các điểm có tọa độ nguyên x từ 0 đến 2n. Ban đầu mọi điểm đều nằm trên trục x nên mọi tọa độ y đều bằng 0. Cách duy nhất để sửa đổi hình dạng là chọn một điểm có chỉ số lẻ và tăng chiều cao của nó thêm một đơn vị cho mỗi lần di chuyển."
date: "2026-06-16T19:03:00+07:00"
tags: ["codeforces", "competitive-programming", "math"]
categories: ["algorithms"]
codeforces_contest: 1036
codeforces_index: "A"
codeforces_contest_name: "Educational Codeforces Round 50 (Rated for Div. 2)"
rating: 1000
weight: 1036
solve_time_s: 360
verified: true
draft: false
---

[CF 1036A - Chiều cao chức năng](https://codeforces.com/problemset/problem/1036/A) 

**Đánh giá:** 1000 
**Thẻ:** toán 
**Thời gian giải:** 6 phút 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Hình này là một đường đứt nét được tạo từ các điểm có tọa độ nguyên x từ 0 đến 2n. Ban đầu mọi điểm đều nằm trên trục x nên mọi tọa độ y đều bằng 0. Cách duy nhất để sửa đổi hình dạng là chọn một điểm có chỉ số lẻ và tăng chiều cao của nó thêm một đơn vị cho mỗi lần di chuyển. Điểm được lập chỉ mục chẵn không bao giờ thay đổi. 

Khi độ cao được chỉ định, các điểm liên tiếp được nối bằng các đoạn thẳng, tạo thành chuỗi đa giác. “Diện tích của ô” là diện tích hình học giữa chuỗi này và trục x. “Chiều cao của biểu đồ” chỉ đơn giản là giá trị y tối đa trong số tất cả các điểm. 

Nhiệm vụ là chọn tăng mỗi điểm có chỉ số lẻ lên bao nhiêu lần sao cho tổng diện tích bao quanh trở thành chính xác k, đồng thời làm cho chiều cao tối đa càng nhỏ càng tốt. 

Các ràng buộc cho phép cả n và k lên tới 10^18. Điều đó ngay lập tức loại trừ bất kỳ cách tiếp cận nào cố gắng mô phỏng các bước tăng dần hoặc xây dựng cấu trúc một cách rõ ràng. Bất kỳ giải pháp nào cũng phải nén vấn đề thành một biểu thức dạng đóng hoặc tệ nhất là tính toán logarit hoặc tính toán theo thời gian không đổi. 

Trường hợp cạnh tinh vi phát sinh từ việc diễn giải hình học một cách chính xác. Một độc giả ngây thơ có thể nghĩ rằng diện tích phụ thuộc vào hình dạng của từng đoạn một cách phức tạp hoặc sự tương tác giữa các điểm lẻ lân cận là quan trọng. Ví dụ, người ta có thể nghi ngờ rằng việc tăng các điểm lẻ liền kề sẽ làm thay đổi sự đóng góp của phân khúc được chia sẻ theo một cách không hề tầm thường. Nếu điều đó là đúng thì ngay cả những đầu vào nhỏ cũng đòi hỏi sự tích lũy hình học cẩn thận. Tuy nhiên, trực giác này dẫn đến sự phức tạp quá mức và các công thức động không chính xác. 

Một cạm bẫy tiềm ẩn khác là giả định rằng việc phân bổ các thay đổi chiều cao không đồng đều có thể bằng cách nào đó làm giảm chiều cao tối đa hiệu quả hơn so với phân bố đồng đều. Điều này thường dẫn đến các chiến lược tham lam không chính xác như điền đầy đủ một điểm trước khi chuyển sang điểm tiếp theo, chiến lược này có thể có vẻ tối ưu cục bộ nhưng lại thất bại trên toàn cầu. 

## Phương pháp tiếp cận 

Ý tưởng brute-force là coi mỗi điểm có chỉ số lẻ là một biến và mô phỏng việc phân phối số gia đơn vị giữa chúng. Mỗi lần tăng sẽ cập nhật hình học và chúng tôi tính toán lại tổng diện tích sau mỗi lần gán. Điều này đòi hỏi phải khám phá tất cả các phân bố của k số gia trên n vị trí. Số lượng trạng thái là tổ hợp, về cơ bản là số thành phần yếu của k thành n phần, tăng dần theo thứ tự$\binom{k+n-1}{n-1}$. Ngay cả đối với các giá trị vừa phải, điều này hoàn toàn không khả thi. 

Sự đơn giản hóa chính xuất phát từ việc quan sát cách hình thành diện tích. Mỗi đoạn kết nối điểm có chiều cao bằng 0 với điểm lẻ hoặc ngược lại. Bởi vì các điểm có chỉ số chẵn luôn ở mức 0, nên mỗi “điểm va chạm” được tạo bởi một điểm có chỉ số lẻ sẽ tạo thành hai phần đóng góp hình tam giác giống hệt nhau, một phần đóng góp trên mỗi đoạn liền kề. Hai nửa này kết hợp thành một đóng góp tuyến tính duy nhất chỉ tỷ lệ với chiều cao của điểm lẻ đó. Điều này loại bỏ tất cả sự tương tác giữa các vị trí. 

Khi diện tích được coi là tổng thuần túy của các đóng góp độc lập, vấn đề sẽ trở thành tổ hợp thuần túy: chúng tôi đang phân phối k đơn vị giống hệt nhau trên n nhóm độc lập và chúng tôi muốn giảm thiểu giá trị nhóm tối đa. Đây là cấu trúc cân bằng tải cổ điển trong đó đạt được sự tối ưu bằng cách cân bằng càng nhiều càng tốt. 

Nếu chúng ta cố gắng giữ chiều cao tối đa ở H thì mỗi vị trí trong số n vị trí lẻ có thể đóng góp tối đa H vào tổng diện tích, do đó diện tích tối đa có thể đạt được là n·H. Điều này ngay lập tức đưa ra điều kiện khả thi cho H. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Phân phối vũ lực | Hàm mũ theo k | O(n) | Quá chậm | 
| Phân phối công bằng tối ưu | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Giải thích từng điểm có chỉ số lẻ như một biến độc lập$a_i$, đại diện cho số lần nó được tăng lên. Điều này biến bài toán hình học thành bài toán phân bổ trên n biến. 
2. Tính toán từng$a_i$đóng góp cho khu vực. Mỗi đơn vị tăng tại một điểm lẻ sẽ làm tăng tổng diện tích lên đúng một đơn vị, vì nó tạo thành hai nửa tam giác đối xứng trên các đoạn liền kề. Điều này thiết lập rằng tổng diện tích bằng$\sum a_i$. 
3. Viết lại bài toán dưới dạng phân phối k đơn vị giống nhau cho n biến$a_1, a_2, \dots, a_n$, trong đó mục tiêu là giảm thiểu$\max a_i$. 
4. Quan sát rằng với một chiều cao H tối đa cố định, tổng lớn nhất có thể xảy ra khi tất cả các biến bằng H, cho ra tổng n·H. Điều này có nghĩa là tính khả thi đòi hỏi$n \cdot H \ge k$. 
5. Chọn số nguyên H nhỏ nhất thỏa mãn bất đẳng thức này, vì mọi giá trị nhỏ hơn đều không thể chứa k và mọi giá trị lớn hơn đều không cần thiết. 
6. Kết luận rằng$H = \lceil k / n \rceil$. 

### Tại sao nó hoạt động 

Bất biến chính là diện tích chỉ phụ thuộc vào tập hợp các chiều cao chỉ số lẻ và chính xác là tổng của chúng. Vì không có sự ghép đôi giữa các biến nên mọi phép phân phối lại bảo toàn tổng sẽ không làm thay đổi diện tích. Do đó, việc giảm thiểu mức tối đa trong một khoản tiền cố định sẽ giúp cân bằng phân phối một cách đồng đều nhất có thể. Bất kỳ sai lệch nào so với tính đồng nhất chỉ làm tăng mức tối đa mà không tăng công suất vượt quá giới hạn tổng cộng như nhau. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, k = map(int, input().split())
print((k + n - 1) // n)
```Toàn bộ quá trình triển khai được rút gọn thành việc tính toán mức trần. Sự đơn giản hóa chính là hình học thu gọn thành một mô hình phụ gia, do đó không cần mô phỏng hoặc xây dựng cấu trúc. Chi tiết cẩn thận duy nhất là đảm bảo việc phân chia trần số nguyên được thực hiện chính xác; sử dụng`(k + n - 1) // n`tránh các vấn đề về dấu phẩy động và xử lý các giá trị lớn lên tới 10^18 một cách an toàn. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4 3
```Ta có n = 4 vị trí lẻ đóng góp độc lập. Chúng tôi phân phối 3 đơn vị cho 4 biến. 

| Bước | Trạng thái phân phối | Tối đa hiện tại | Tổng số tiền | 
| --- | --- | --- | --- | 
| 1 | [1,0,0,0] | 1 | 1 | 
| 2 | [1,1,0,0] | 1 | 2 | 
| 3 | [1,1,1,0] | 1 | 3 | 

Chiều cao tối đa cần thiết là 1 vì tất cả các đơn vị có thể được đặt mà không vượt quá 1 cho mỗi vị trí. Điều này phù hợp với công thức ceil(3/4) = 1. 

### Ví dụ 2 

đầu vào:```
4 12
```Bây giờ chúng tôi phân phối 12 đơn vị trên 4 vị trí. 

| Bước | Trạng thái phân phối | Tối đa hiện tại | Tổng số tiền | 
| --- | --- | --- | --- | 
| 1 | [3,3,3,3] | 3 | 12 | 

Ở đây sự phân bố được cân bằng hoàn hảo, do đó chiều cao tối đa là 3. Điều này khớp với ceil(12/4) = 3 và thể hiện trường hợp chặt chẽ trong đó tất cả các biến phải đạt cùng một mức. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Chỉ các phép tính số học được thực hiện | 
| Không gian | O(1) | Không có cấu trúc dữ liệu bổ sung nào được sử dụng | 

Lời giải là thời gian không đổi, điều này cần thiết vì cả n và k đều có thể lớn bằng 10^18. Bất kỳ cách tiếp cận lặp đi lặp lại hoặc dựa trên mô phỏng nào cũng sẽ không thể thực hiện được dưới những ràng buộc này. 

## Trường hợp thử nghiệm```python
import sys, io

def solve():
    import sys
    input = sys.stdin.readline
    n, k = map(int, input().split())
    print((k + n - 1) // n)

def run(inp: str) -> str:
    old_stdin = sys.stdin
    sys.stdin = io.StringIO(inp)
    old_stdout = sys.stdout
    sys.stdout = io.StringIO()
    solve()
    out = sys.stdout.getvalue()
    sys.stdin = old_stdin
    sys.stdout = old_stdout
    return out.strip()

# provided sample
assert run("4 3\n") == "1"

# minimum case
assert run("1 1\n") == "1"

# evenly divisible
assert run("5 10\n") == "2"

# large skew case
assert run("3 1000000000000000000\n") == str((10**18 + 2)//3)

# k smaller than n
assert run("10 3\n") == "1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1 | 1 | trường hợp không tầm thường nhỏ nhất | 
| 5 10 | 2 | phân phối đồng đều | 
| 3 10^18 | chia trần đúng cách | giá trị biên lớn | 
| 10 3 | 1 | k < n hành vi | 

## Vỏ cạnh 

Một sự hiểu lầm phổ biến là khi k nhỏ hơn n. Ví dụ, với đầu vào`n = 10, k = 3`, chiến lược tối ưu là đặt mỗi đơn vị ở một vị trí khác nhau, tạo ra độ cao như`[1,1,1,0,...]`, do đó chiều cao tối đa là 1. Công thức trả về đúng`ceil(3/10) = 1`, phù hợp với trực giác này. 

Một trường hợp cạnh khác là khi k chia hết cho n, chẳng hạn như`n = 5, k = 10`. Cấu hình tối ưu hoàn toàn đồng nhất, với mỗi điểm lẻ được đặt thành 2. Thuật toán trả về chính xác 2 và không có sự phân bố không đồng đều nào có thể giảm mức tối đa hơn nữa mà không vi phạm ràng buộc tổng. 

Cuối cùng, khi k cực lớn, chẳng hạn như 10^18, giải pháp vẫn hoạt động vì nó tránh xây dựng bất kỳ phân phối nào và chỉ dựa vào số học số nguyên, đảm bảo không có vấn đề tràn hoặc hiệu suất.
