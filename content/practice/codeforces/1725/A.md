---
title: "CF 1725A - Tích lũy quân Domino"
description: "Lưới trong bài toán này không phải là tùy ý, nó hoàn toàn được xác định bởi các kích thước của nó. Mỗi ô chứa một số nguyên duy nhất và các số tăng dần theo hàng từ trái sang phải."
date: "2026-06-15T01:34:23+07:00"
tags: ["codeforces", "competitive-programming", "math"]
categories: ["algorithms"]
codeforces_contest: 1725
codeforces_index: "A"
codeforces_contest_name: "COMPFEST 14 - Preliminary Online Mirror (Unrated, ICPC Rules, Teams Preferred)"
rating: 800
weight: 1725
solve_time_s: 195
verified: true
draft: false
---

[CF 1725A - Tích lũy quân Domino](https://codeforces.com/problemset/problem/1725/A) 

**Đánh giá:** 800 
**Thẻ:** toán 
**Thời gian giải:** 3 phút 15s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Lưới trong bài toán này không phải là tùy ý, nó hoàn toàn được xác định bởi các kích thước của nó. Mỗi ô chứa một số nguyên duy nhất và các số tăng dần theo hàng từ trái sang phải. Hàng đầu tiên chứa M số nguyên đầu tiên, hàng thứ hai tiếp tục ngay sau đó, v.v. Vì vậy, lưới hoạt động giống như một mảng phẳng được viết theo thứ tự hàng lớn. 

Chúng ta được yêu cầu đếm các cặp ô liền kề thỏa mãn một điều kiện rất cụ thể. Một cặp được hình thành bởi hai ô có chung một cạnh, nghĩa là chúng ta chỉ xem xét các ô lân cận theo chiều ngang và chiều dọc. Một cặp như vậy được gọi là hợp lệ nếu chênh lệch tuyệt đối giữa các số bên trong hai ô đó chính xác là 1. Mỗi cặp ô không có thứ tự được tính một lần. 

Hạn chế chính về cấu trúc là các số liên tiếp theo một khuôn mẫu rất cứng nhắc. Điều này làm cho việc so sánh kề mang tính quyết định, vì chúng ta không bao giờ cần mô phỏng lưới mà chỉ lý giải về các mối quan hệ số học do vị trí hàng và cột gây ra. 

Các ràng buộc cho phép N và M lên tới 10^9, điều này ngay lập tức loại trừ mọi cách xây dựng lưới. Ngay cả một hàng có kích thước 10^9 cũng không thể được lưu trữ hoặc lặp lại một cách rõ ràng trong giới hạn thời gian. Bất kỳ giải pháp nào lặp lại trên các ô hoặc các cạnh riêng lẻ sẽ không thành công do lỗi tuyến tính hoặc bậc hai. Cách tiếp cận khả thi duy nhất là suy luận theo thuật ngữ số học O(1) hoặc O(log N). 

Trường hợp cạnh tinh tế xuất hiện khi N hoặc M bằng 1. Khi M = 1, không có kề cận ngang, chỉ có kề dọc. Khi N = 1, không có sự kề cận theo chiều dọc mà chỉ có sự kề cận theo chiều ngang. Việc triển khai ngây thơ giả định cả hai hướng tồn tại sẽ bị tính quá mức hoặc cố gắng lập chỉ mục không hợp lệ. Một trường hợp góc khác là sự chuyển đổi giữa các hàng: các giá trị bao bọc từ hàng i đến hàng i+1 và ranh giới này hoạt động khác với sự kề cận trong hàng. 

## Phương pháp tiếp cận 

Một giải pháp brute-force sẽ xây dựng lưới một cách rõ ràng và kiểm tra từng cặp ô liền kề. Mỗi ô có tối đa hai ô lân cận phía trước, do đó, điều này sẽ liên quan đến việc quét tất cả các ô N × M và thực hiện kiểm tra liên tục trên mỗi ô. Điều đó dẫn đến hoạt động O(NM), điều này là không thể vì NM có thể đạt tới 10^18. 

Quan sát quan trọng là tính kề là cục bộ và việc gán giá trị là tuyến tính. Trong một hàng, các cột liên tiếp khác nhau đúng 1 giá trị. Điều đó có nghĩa là mọi cạnh ngang sẽ tự động tạo thành một quân domino chặt chẽ hợp lệ. Có chính xác M - 1 cạnh như vậy trên mỗi hàng và có N hàng, do đó việc đóng góp theo chiều ngang là đơn giản. 

Các cạnh dọc tinh tế hơn. Di chuyển từ hàng i đến hàng i+1 trên cùng một cột sẽ làm tăng giá trị chính xác bằng M. Vì vậy, các hàng dọc dọc khác nhau M chứ không phải 1, nghĩa là các cạnh thẳng đứng không bao giờ đủ điều kiện trừ khi M = 1. Trong trường hợp đặc biệt đó, mỗi cột là một chuỗi các số nguyên liên tiếp và mọi cạnh dọc đều hợp lệ. 

Do đó, toàn bộ vấn đề quy về việc đếm các cạnh hợp lệ theo chiều ngang và cộng các cạnh dọc có điều kiện khi M = 1. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(NM) | O(1) | Quá chậm | 
| Tối ưu | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi chia việc đếm thành đóng góp theo chiều ngang và chiều dọc. 

1. Tính toán các quân domino chặt ngang bằng cách xem xét từng hàng độc lập. 

Trong một hàng có độ dài M, mọi cặp cột liền kề tạo thành các giá trị x và x+1, do đó tất cả các cạnh ngang M - 1 đều hợp lệ. Vì có N hàng nên điều này đóng góp N × (M - 1). 
2. Xử lý các vùng lân cận theo chiều dọc một cách riêng biệt bằng cách kiểm tra cấu trúc theo cột. 

Chênh lệch giá trị giữa một ô (i, j) và (i+1, j) luôn là M. Giá trị này chỉ bằng 1 khi M = 1, vì chỉ khi đó việc đánh số mới thu gọn thành một chuỗi duy nhất. 
3. Nếu M bằng 1 thì mỗi cột sẽ trở thành một dãy số nguyên liên tiếp theo chiều dọc.

Trong trường hợp đó, mỗi cột đóng góp N - 1 cạnh dọc hợp lệ và có M cột như vậy, do đó đóng góp dọc là M × (N - 1). 
4. Tổng hợp cả những đóng góp và đưa ra kết quả. 

Tại sao nó hoạt động 

Lưới xác định song ánh giữa tọa độ và số nguyên thông qua công thức tuyến tính. Liền kề theo chiều ngang bảo toàn các số nguyên liên tiếp vì việc xây dựng tăng thêm 1 dọc theo các hàng. Liền kề dọc chỉ bảo toàn các số nguyên liên tiếp khi sải hàng M bằng 1. Do cấu trúc kề dọc được phân tách hoàn toàn thành các thành phần ngang và dọc độc lập không có sự trùng lặp, nên việc đếm từng hướng riêng biệt sẽ đưa ra một phân vùng chính xác của tất cả các cạnh có thể có. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

N, M = map(int, input().split())

horizontal = N * (M - 1)
vertical = M * (N - 1) if M == 1 else 0

print(horizontal + vertical)
```Việc triển khai trực tiếp mã hóa sự phân tách có nguồn gốc trước đó. Các cạnh ngang được tính toán dựa trên thực tế là mỗi hàng đóng góp M - 1 cặp hợp lệ. Các cạnh dọc chỉ được thêm vào trong trường hợp suy biến trong đó M = 1, vì nếu không thì hiệu dọc luôn lớn hơn 1. 

Một lỗi phổ biến là cố gắng thêm đối xứng các cạnh dọc bằng cách sử dụng N × (M - 1), phản ánh công thức ngang. Điều đó không thành công vì độ kề dọc phụ thuộc vào bước đánh số chứ không phụ thuộc vào tính đối xứng của hình dạng lưới. Một cạm bẫy khác là quên rằng các cạnh thẳng đứng biến mất hoàn toàn khi M > 1. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3 4
```Đóng góp theo chiều ngang được tính trên mỗi hàng. Mỗi hàng có 3 cạnh ngang hợp lệ và có 3 hàng. 

Đóng góp theo chiều dọc bằng 0 vì M ≠ 1. 

| Bước | Ngang | Dọc | Tổng cộng | 
| --- | --- | --- | --- | 
| Ban đầu | 0 | 0 | 0 | 
| Sau ngang | 9 | 0 | 9 | 
| Sau dọc | 9 | 0 | 9 | 

Điều này xác nhận rằng chỉ các vùng lân cận trong hàng mới quan trọng trong lưới nhiều cột. 

### Ví dụ 2 

đầu vào:```
4 1
```Bây giờ lưới là một cột duy nhất nên các cạnh ngang không tồn tại. 

| Bước | Ngang | Dọc | Tổng cộng | 
| --- | --- | --- | --- | 
| Ban đầu | 0 | 0 | 0 | 
| Sau ngang | 0 | 0 | 0 | 
| Sau dọc | 3 | 3 | 3 | 

Mỗi cột tạo thành một chuỗi liên tiếp thuần túy, vì vậy mọi bước dọc đều hợp lệ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Chỉ các phép tính số học trên N và M | 
| Không gian | O(1) | Không sử dụng công trình phụ trợ | 

Giải pháp thực hiện một số phép tính số học không đổi bất kể kích thước đầu vào. Điều này dễ dàng phù hợp với các ràng buộc của N, M lên đến 10^9, vì không cần lặp lại trên lưới. 

## Trường hợp thử nghiệm```python
import sys, io

def solve(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline
    N, M = map(int, input().split())
    horizontal = N * (M - 1)
    vertical = M * (N - 1) if M == 1 else 0
    return str(horizontal + vertical)

# provided sample
assert solve("3 4\n") == "9"

# minimum case
assert solve("1 1\n") == "0"

# single row
assert solve("1 5\n") == "4"

# single column
assert solve("5 1\n") == "4"

# rectangular grid
assert solve("2 3\n") == "4"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1 | 0 | lưới nhỏ nhất | 
| 1 5 | 4 | chỉ có cạnh ngang | 
| 5 1 | 4 | chỉ các cạnh dọc | 
| 2 3 | 4 | tính nhất quán của cấu trúc hỗn hợp | 

## Vỏ cạnh 

Khi N = 1 và M = 1, lưới có một ô duy nhất nên không có cặp liền kề. Thuật toán trả về chiều ngang = 1 × 0 = 0 và bỏ qua chiều dọc vì điều kiện M ≠ 1 không thành công, cho kết quả 0 chính xác. 

Khi N = 1 và M > 1, lưới là một hàng. Tính toán theo chiều ngang mang lại M - 1, khớp với số cặp liền kề trên một dòng. Dọc vẫn bằng 0, tránh chính xác lý do liên hàng không hợp lệ. 

Khi M = 1 và N > 1, lưới sẽ trở thành một cột gồm các số nguyên liên tiếp. Chiều ngang bằng 0, trong khi chiều dọc trở thành 1 × (N - 1), nắm bắt chính xác cấu trúc chuỗi trong đó mỗi bước khác nhau chính xác 1.
