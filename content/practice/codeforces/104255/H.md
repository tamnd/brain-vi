---
title: "CF 104255H - Sinh nhật"
description: "Chúng ta có một đa giác lồi có $n$ đỉnh. Hoạt động duy nhất được phép là vẽ một đường chéo giữa hai đỉnh hiện có. Mỗi đường chéo chia một vùng đa giác thành hai vùng nhỏ hơn."
date: "2026-07-01T21:53:57+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104255
codeforces_index: "H"
codeforces_contest_name: "BSUIR Open X. Reload. Students final"
rating: 0
weight: 104255
solve_time_s: 94
verified: false
draft: false
---

[CF 104255H - Sinh nhật](https://codeforces.com/problemset/problem/104255/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 34s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một đa giác lồi với$n$đỉnh. Hoạt động duy nhất được phép là vẽ một đường chéo giữa hai đỉnh hiện có. Mỗi đường chéo chia một vùng đa giác thành hai vùng nhỏ hơn. Mục tiêu là tiếp tục thêm các đường chéo cho đến khi mọi vùng kết quả đều là một hình tam giác và chúng tôi muốn giảm thiểu số lượng đường chéo được sử dụng. 

Một cách hữu ích để diễn đạt lại nhiệm vụ là chúng ta đang tam giác hóa một đa giác lồi. Mỗi lần cắt sẽ tăng số lượng mảnh và chúng tôi muốn đạt được sự phân tách hoàn toàn thành các hình tam giác với càng ít vết cắt càng tốt. 

Kích thước đầu vào nhỏ,$n \le 100$, do đó mọi suy luận bậc hai hoặc bậc ba về các đỉnh đều hoàn toàn an toàn. Điều này ngay lập tức gợi ý rằng câu trả lời có thể là một dạng đóng đã biết hoặc một thuộc tính tổ hợp đơn giản chứ không phải là một lập trình động trên các trạng thái phụ thuộc vào hình học. 

Một điểm tinh tế là “cắt theo đường chéo” ngụ ý mỗi đường cắt là một đường thẳng nối hai đỉnh và một đường chéo là thao tác duy nhất. Không có hạn chế nào về việc cắt các đường chéo, nhưng trong một đa giác lồi, các tam giác tối ưu không bao giờ yêu cầu các đường chéo. 

Không có trường hợp góc phức tạp nào ngoài đa giác hợp lệ nhỏ nhất. Vì$n = 4$, chúng ta có một hình tứ giác và một đường chéo tạo ra đúng hai hình tam giác. Đối với lớn hơn$n$, cấu trúc có tỷ lệ đều đặn. 

## Phương pháp tiếp cận 

Một cách tiếp cận trực tiếp nhưng quá nghĩa đen là mô phỏng quá trình cắt đa giác thành các hình tam giác bằng cách chọn liên tục các đường chéo. Ở mỗi bước, chúng ta có thể thử tất cả các đường chéo hợp lệ, áp dụng một đường chéo, giải đệ quy các đa giác con thu được và giảm thiểu số lần cắt. 

Quan điểm thô bạo này nhanh chóng trở thành một vấn đề về việc liệt kê các tam giác. Một đa giác lồi với$n$các đỉnh có nhiều tam giác theo số Catalan, chúng phát triển gần giống như$O(4^n / n^{3/2})$. Ngay cả đối với$n = 50$, điều này đã vượt xa mọi giới hạn tính toán, vì vậy việc tìm kiếm toàn diện là không thể. 

Quan sát quan trọng là chúng ta không được yêu cầu tối ưu hóa trên các tam giác khác nhau mà chỉ đếm xem cần bao nhiêu đường chéo trong bất kỳ tam giác hợp lệ nào. Điều này loại bỏ tất cả sự phức tạp của tổ hợp: mọi tam giác của một lồi$n$-gon sử dụng cùng số đường chéo. 

Để biết lý do tại sao, hãy xem xét phép tính tam giác tạo ra kết quả gì. Một lồi$n$-gon được chia thành các hình tam giác và mỗi hình tam giác có ba cạnh. Trong một tam giác, mỗi đường chéo được thêm vào đóng góp chính xác một cạnh trong và cấu trúc cuối cùng là một đồ thị phẳng trong đó tất cả các mặt đều là hình tam giác. 

Có một thực tế cấu trúc tiêu chuẩn: mọi tam giác lồi$n$-gon luôn tạo ra chính xác$n - 2$hình tam giác. Vì đa giác ban đầu có$n$các đỉnh, tam giác luôn chia nó thành$n - 2$các mặt hình tam giác. Mỗi đường chéo làm tăng số mặt lên đúng một. Bắt đầu từ 1 mặt (đa giác ban đầu), đạt tới$n - 2$khuôn mặt yêu cầu chính xác$n - 3$vết cắt. 

Điều này làm cho câu trả lời độc lập với hình học hoặc chiến lược. Bất kỳ chuỗi đường cắt chéo hợp lệ nào tạo thành tam giác đầy đủ cho đa giác đều phải sử dụng chính xác$n - 3$đường chéo. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Tìm kiếm tam giác Brute Force | Hàm mũ | O(n) | Quá chậm | 
| Công thức$n - 3$| O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số nguyên$n$, biểu thị số đỉnh của đa giác lồi. 
2. Nhận biết rằng mỗi đường cắt chéo sẽ chia một vùng thành hai, tăng số vùng lên đúng một. 
3. Bắt đầu từ một vùng (đa giác ban đầu). 
4. Phép tam giác đầy đủ tạo ra$n - 2$các vùng hình tam giác. 
5. Mỗi lần cắt sẽ tăng số vùng lên một, vì vậy số lần cắt chính xác là số lần chúng ta cần đi từ vùng 1 đến vùng$n - 2$các vùng. 
6. Tính đáp án dưới dạng$n - 3$và xuất nó. 

### Tại sao nó hoạt động 

Bất biến là mối quan hệ giữa các mặt và các vết cắt trong một phân khu phẳng của đa giác lồi. Ban đầu có một khuôn mặt. Mỗi lần chèn đường chéo sẽ chia chính xác một mặt hiện có thành hai, tăng số lượng mặt lên một. Vì mọi tam giác hợp lệ phải kết thúc bằng chính xác$n - 2$mặt tam giác, tổng số phần chia yêu cầu được cố định là$n - 3$, không phụ thuộc vào cách chọn đường chéo. Không có chuỗi chèn đường chéo hợp lệ nào có thể thay đổi số lượng này. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input().strip())
print(n - 3)
```Lời giải đọc số đỉnh và áp dụng trực tiếp công thức dẫn xuất. Không cần mô phỏng hình học hoặc duy trì bất kỳ cấu trúc nào vì số đếm cuối cùng chỉ phụ thuộc vào mối quan hệ bất biến giữa các đỉnh, hình tam giác và đường chéo trong bất kỳ tam giác nào. 

Chi tiết triển khai duy nhất cần cẩn thận là loại bỏ dòng đầu vào, vì đầu vào lập trình cạnh tranh thường bao gồm các dòng mới ở cuối. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5
```Một hình ngũ giác lồi phải được chia thành các hình tam giác. Thuật toán tính toán$5 - 3 = 2$. 

| Bước | n | Tính toán | Kết quả | 
| --- | --- | --- | --- | 
| Đọc đầu vào | 5 | - | - | 
| Áp dụng công thức | 5 | 5 - 3 | 2 | 

Điều này xác nhận rằng một hình ngũ giác cần có chính xác hai đường chéo để tạo thành tam giác đầy đủ. 

### Ví dụ 2 

đầu vào:```
8
```Đối với một hình bát giác, áp dụng bất biến tương tự. 

| Bước | n | Tính toán | Kết quả | 
| --- | --- | --- | --- | 
| Đọc đầu vào | 8 | - | - | 
| Áp dụng công thức | 8 | 8 - 3 | 5 | 

Đầu ra là 5, nghĩa là cần có 5 đường chéo để phân chia hình bát giác thành sáu hình tam giác. 

Dấu vết này cho thấy ngay cả khi đa giác phát triển, quá trình này chỉ phụ thuộc vào số đỉnh. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Chỉ một phép tính số học duy nhất sau khi đọc đầu vào | 
| Không gian | O(1) | Không có cấu trúc dữ liệu bổ sung nào được sử dụng | 

Ràng buộc đầu vào$n \le 100$vượt xa những gì giải pháp này yêu cầu. Việc tính toán là thời gian không đổi và phù hợp một cách tầm thường trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n = int(sys.stdin.readline().strip())
    return str(n - 3)

# provided samples
assert run("5\n") == "2", "sample 1"
assert run("8\n") == "5", "sample 2"

# custom cases
assert run("4\n") == "1", "minimum valid polygon"
assert run("6\n") == "3", "hexagon case"
assert run("100\n") == "97", "maximum n boundary"
assert run("7\n") == "4", "odd n sanity check"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 4 | 1 | đa giác nhỏ nhất (tứ giác) | 
| 6 | 3 | chia tỷ lệ đa giác lồi đơn giản | 
| 100 | 97 | độ đúng giới hạn trên | 
| 7 | 4 | tính nhất quán chung | 

## Vỏ cạnh 

cho$n = 4$, thuật toán tính toán$4 - 3 = 1$. Một hình tứ giác gần như đã có hình tam giác và cần có chính xác một đường chéo, phù hợp với thực tế hình học. 

Đối với lớn hơn$n$, chẳng hạn như$n = 5$, thuật toán trả về 2. Nếu chúng ta vẽ rõ ràng các đường chéo trong một hình ngũ giác, thì bất kỳ tam giác hợp lệ nào cũng tạo ra chính xác ba hình tam giác và do đó có chính xác hai đường chéo. Công thức khớp chính xác với cấu trúc này mà không phụ thuộc vào đường chéo nào được chọn. 

Đối với trường hợp tối đa$n = 100$, tính toán vẫn ổn định và trực tiếp. Không có sự tích lũy lỗi hoặc sự phụ thuộc trạng thái, vì công thức hoàn toàn là số học và bắt nguồn từ việc đếm khuôn mặt bất biến.
