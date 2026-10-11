---
title: "CF 105007A - Đi Tìm Bồ"
description: "Lưới mô tả một công viên dành cho chó được bố trí dưới dạng một hình chữ nhật gồm các ô. Mỗi ô trống, được đánh dấu bằng dấu chấm hoặc chứa một chữ số từ 1 đến 9 biểu thị chiều cao của con chó tại vị trí đó."
date: "2026-06-28T03:04:19+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105007
codeforces_index: "A"
codeforces_contest_name: "UTPC Contest 03-01-24 Div. 2 (Beginner)"
rating: 0
weight: 105007
solve_time_s: 86
verified: false
draft: false
---

[CF 105007A - Tìm Bo](https://codeforces.com/problemset/problem/105007/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 26s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Lưới mô tả một công viên dành cho chó được bố trí dưới dạng một hình chữ nhật gồm các ô. Mỗi ô trống, được đánh dấu bằng dấu chấm hoặc chứa một chữ số từ 1 đến 9 biểu thị chiều cao của con chó tại vị trí đó. Trong số tất cả các con chó trong công viên, có chính xác một con có chiều cao lớn nhất và nhiệm vụ là xác định vị trí của nó và báo cáo hàng và cột của nó bằng cách sử dụng chỉ mục dựa trên 1. 

Điều làm cho đầu vào nhiều hơn một danh sách đơn giản là dữ liệu có tính chất không gian. Chúng tôi không chỉ trích xuất giá trị tối đa mà còn theo dõi vị trí nó xuất hiện bên trong cấu trúc 2D. Do đó, đầu ra không phải là giá trị mà là tọa độ của ô chứa chữ số lớn nhất. 

Các ràng buộc rất nhỏ, với cả hai chiều đều lên tới 100. Điều đó có nghĩa là tồn tại tối đa 10.000 ô. Bất kỳ giải pháp nào quét từng ô với số lần không đổi đều đủ nhanh. Ngay cả việc quét toàn bộ lưới đơn giản cũng nằm trong giới hạn thoải mái, trong khi bất kỳ điều gì cố gắng thông minh hơn mức cần thiết sẽ không đạt được bất kỳ lợi thế hiệu suất có ý nghĩa nào. 

Một lỗi nhỏ thường xuất phát từ việc phân tích cú pháp đầu vào. Mỗi hàng là một chuỗi ký tự, không phải các giá trị được phân tách bằng dấu cách. Việc coi nó như một danh sách các số nguyên hoặc việc chia tách không chính xác có thể dịch chuyển các cột hoặc hợp nhất các hàng, tạo ra tọa độ không chính xác. Một vấn đề phổ biến khác là quên rằng câu trả lời phải dựa trên 1, vì các ngôn ngữ lập trình thường lập chỉ mục từ 0. 

Các trường hợp biên quan trọng ở đây chủ yếu là về cấu trúc hơn là thuật toán. Lưới có kích thước 1×1 là hợp lệ và phải trả về (1, 1). Một lưới trong đó tất cả các con chó có cùng chiều cao ngoại trừ một mức tối đa được đảm bảo là duy nhất, nhưng việc triển khai bất cẩn cập nhật câu trả lời về "lớn hơn hoặc bằng" thay vì lớn hơn hoàn toàn có thể ghi đè sai mức tối đa đầu tiên gặp phải. Một lưới có nhiều dấu chấm và một chữ số cũng kiểm tra xem liệu phân tích cú pháp có bỏ qua các ô không phải chó một cách chính xác hay không. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp nhất là quét từng ô trong lưới, kiểm tra xem nó có chứa chữ số hay không. Nếu đúng như vậy, chúng tôi sẽ so sánh nó với chiều cao tốt nhất được thấy cho đến nay và cập nhật giá trị và tọa độ tốt nhất được lưu trữ của chúng tôi khi giá trị lớn hơn xuất hiện. Điều này hiệu quả vì lưới nhỏ và mỗi ô được kiểm tra chính xác một lần. 

Về mặt khái niệm, một biến thể brute-force sẽ thử từng ô làm câu trả lời ứng cử viên và sau đó quét lại toàn bộ lưới để xác minh xem đó có phải là mức tối đa hay không. Điều đó sẽ liên quan đến việc so sánh từng cặp ô, dẫn đến các hoạt động khoảng O((NM)^2) trong trường hợp xấu nhất. Với N và M lên tới 100, đây là chi phí không cần thiết và có nguy cơ kém hiệu quả trong phiên bản hạn chế hơn của vấn đề. 

Quan sát chính là vấn đề chỉ yêu cầu mức tối đa toàn cục trên một tập hợp giá trị hữu hạn với tính năng theo dõi vị trí. Không có sự phụ thuộc giữa các ô, không có ràng buộc về thứ tự và không cần tiền xử lý. Điều này làm giảm nhiệm vụ thành một vấn đề tích lũy một lượt: duy trì kết quả tốt nhất cho đến nay trong khi duyệt qua lưới một lần. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Kiểm tra theo cặp Brute Force | O((NM)^2) | O(1) | Quá chậm | 
| Quét một lượt | O(NM) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Đọc kích thước N và M, sau đó đọc từng hàng lưới dưới dạng chuỗi. Điều này bảo toàn cấu trúc không gian sao cho mỗi ký tự tương ứng với chính xác một ô trong lưới. 
2. Khởi tạo một biến để lưu trữ chiều cao tối đa được tìm thấy cho đến nay. Đặt nó bên dưới chữ số nhỏ nhất có thể để bất kỳ con chó hợp lệ nào sẽ thay thế nó ngay lập tức. 
3. Khởi tạo hai biến để lưu trữ tọa độ của vị trí tốt nhất được tìm thấy cho đến nay. 
4. Lặp lại từng chỉ số hàng i từ 0 đến N−1. 
5. Bên trong mỗi hàng, lặp lại từng chỉ số cột j từ 0 đến M−1. 
6. Nếu ô hiện tại chứa một chữ số chứ không phải dấu chấm, hãy chuyển đổi nó thành số nguyên và so sánh nó với số lớn nhất hiện tại. Nếu nó lớn hơn, cập nhật giá trị lớn nhất và ghi lại (i, j) là vị trí mới tốt nhất. 
7. Sau khi quét tất cả các ô, xuất tọa độ đã lưu bằng cách sử dụng chỉ mục dựa trên 1. 

Lý do chúng tôi chỉ cập nhật khi thấy giá trị lớn hơn hoàn toàn gắn liền với việc đảm bảo rằng con chó cao nhất là duy nhất. Điều này đảm bảo chúng ta không bao giờ cần logic ràng buộc; lần xuất hiện đầu tiên của mức tối đa là đúng và vẫn ổn định. 

### Tại sao nó hoạt động 

Ở mỗi bước quét, thuật toán duy trì tính bất biến rằng vị trí được lưu trữ tương ứng với con chó cao nhất gặp trong phần lưới được xử lý cho đến nay. Vì mỗi ô được truy cập chính xác một lần và các phép so sánh đơn điệu về chiều cao, nên bất biến này đảm bảo rằng sau khi toàn bộ lưới đã được xử lý, vị trí được lưu trữ phải tương ứng với mức tối đa toàn cầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())
    
    best_val = -1
    best_i = 0
    best_j = 0

    for i in range(n):
        row = input().strip()
        for j in range(m):
            c = row[j]
            if c != '.':
                val = ord(c) - ord('0')
                if val > best_val:
                    best_val = val
                    best_i = i
                    best_j = j

    print(best_i + 1, best_j + 1)

if __name__ == "__main__":
    solve()
```Giải pháp dựa trên một lần truyền qua lưới. Sự chuyển đổi`ord(c) - ord('0')`tránh phân tích cú pháp số nguyên chậm hơn và an toàn vì đầu vào đảm bảo các số có một chữ số. Các tọa độ được lưu trữ ở dạng lập chỉ mục dựa trên 0 trong quá trình tính toán và chỉ được chuyển đổi thành dựa trên 1 tại thời điểm đầu ra, ngăn chặn các lỗi sai sót một trong quá trình cập nhật. 

Một lỗi triển khai phổ biến là cập nhật tọa độ trước khi kiểm tra xem giá trị có lớn hơn hay không hoặc sử dụng`>=`thay vì`>`. Điều đó sẽ ghi đè không chính xác các lần xuất hiện tối đa trước đó, điều này là không cần thiết và có thể làm phức tạp thêm lý luận về tính chính xác. Một vấn đề nhỏ khác là quên loại bỏ các ký tự dòng mới khỏi mỗi hàng, điều này có thể thay đổi việc lập chỉ mục hoặc đưa ra các ký tự không hợp lệ. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Hãy xem xét một lưới nhỏ:```
3 4
.2.1
..9.
.3.5
```| Ô (i, j) | Giá trị | Tốt nhất cho đến nay | Vị trí tốt nhất | 
| --- | --- | --- | --- | 
| (0,0) | . | - | (0,0) | 
| (0,1) | 2 | 2 | (0,1) | 
| (0,2) | . | 2 | (0,1) | 
| (0,3) | 1 | 2 | (0,1) | 
| (1,0) | . | 2 | (0,1) | 
| (1,1) | . | 2 | (0,1) | 
| (1,2) | 9 | 9 | (1,2) | 
| (1,3) | . | 9 | (1,2) | 
| (2,0) | . | 9 | (1,2) | 
| (2,1) | 3 | 9 | (1,2) | 
| (2,2) | . | 9 | (1,2) | 
| (2,3) | 5 | 9 | (1,2) | 

Quá trình quét kết thúc với giá trị tối đa 9 tại vị trí (1,2) trong lập chỉ mục dựa trên 0, trở thành (2,3) trong lập chỉ mục dựa trên 1. 

Dấu vết này xác nhận rằng thuật toán không phụ thuộc vào vị trí xuất hiện mức tối đa; nó chỉ theo dõi giá trị lớn nhất được nhìn thấy. 

### Ví dụ 2 

Trường hợp cạnh một hàng:```
1 5
.7.2.9
```| Ô (i, j) | Giá trị | Tốt nhất cho đến nay | Vị trí tốt nhất | 
| --- | --- | --- | --- | 
| (0,0) | . | - | (0,0) | 
| (0,1) | 7 | 7 | (0,1) | 
| (0,2) | . | 7 | (0,1) | 
| (0,3) | 2 | 7 | (0,1) | 
| (0,4) | . | 7 | (0,1) | 
| (0,5) | 9 | 9 | (0,5) | 

Giá trị tối đa thay đổi chính xác một lần khi gặp số 9, cho thấy quy tắc cập nhật xử lý chính xác giá trị cực đại xuất hiện muộn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(NM) | Mỗi ô được truy cập một lần và được xử lý trong thời gian không đổi | 
| Không gian | O(1) | Chỉ một số biến được sử dụng bất kể kích thước lưới | 

Kích thước lưới tối đa là 10.000 ô, do đó, một lần quét tuyến tính đơn lẻ là không đáng kể trong giới hạn 1 giây. Việc sử dụng bộ nhớ không đổi do không có cấu trúc bổ sung nào tỷ lệ thuận với kích thước đầu vào được lưu trữ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# simple sample
assert run("""3 4
.2.1
..9.
.3.5
""") == "2 3"

# single cell
assert run("""1 1
7
""") == "1 1"

# max at first position
assert run("""2 3
9..
.12
""") == "1 1"

# max at last position
assert run("""2 3
123
..9
""") == "2 3"

# all dots except one
assert run("""3 3
...
.5.
...
""") == "2 2"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Lưới 1×1 | 1 1 | trường hợp ranh giới tối thiểu | 
| ô đầu tiên tối đa | 1 1 | không cần ghi đè | 
| ô cuối cùng tối đa | 2 3 | cập nhật muộn đúng đắn | 
| lưới thưa thớt | 2 2 | phân tích các ô không có chữ số | 

## Vỏ cạnh 

Lưới 1×1 là cấu trúc đơn giản nhất có thể. Thuật toán khởi tạo giá trị tốt nhất bên dưới bất kỳ chữ số hợp lệ nào, do đó, ô đơn ngay lập tức được chọn và trả về dưới dạng (1, 1). Không có lần lặp nào có thể ghi đè kết quả này không chính xác. 

Một lưới trong đó mức tối đa xảy ra ở ô đầu tiên sẽ kiểm tra xem các so sánh sau này có thay thế sai một câu trả lời vốn đã tối ưu hay không. Bởi vì cập nhật chỉ xảy ra trên các giá trị lớn hơn, nên khi giá trị tối đa đầu tiên được lưu trữ, nó sẽ không thay đổi trừ khi xuất hiện giá trị lớn hơn, điều này không thể xảy ra khi nhập đúng. 

Lưới chứa hầu hết các dấu chấm đảm bảo rằng các ký tự không phải số sẽ được bỏ qua một cách an toàn. Quá trình quét chỉ cần bỏ qua các ô này, giữ nguyên giá trị tốt nhất cho đến khi gặp một chữ số.
