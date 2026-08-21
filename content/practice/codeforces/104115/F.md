---
title: "CF 104115F - \u041d\u043e \u0432\u044b \u043e\u0431\u043e \u043c\u043d\u0435 \u0441\u043b\u044b\u0448\u0430\u043b\u0438"
description: "Có một hàng rương $n$ được đánh số từ 1 đến $n$. Chính xác một rương $k$ chứa kho báu, trong khi tất cả các rương khác đều trống rỗng. Một tên cướp biển bắt đầu mở rương nhưng vẫn chưa phát hiện ra kho báu nằm ở đâu."
date: "2026-07-02T01:56:12+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104115
codeforces_index: "F"
codeforces_contest_name: "Voronezh State University - Sitronics contest, 2022"
rating: 0
weight: 104115
solve_time_s: 40
verified: true
draft: false
---

[CF 104115F - \u041d\u043e \u0432\u044b \u043e\u0431\u043e \u043c\u043d\u0435 \u0441\u043b\u044b\u0448\u0430\u043b\u0438](https://codeforces.com/problemset/problem/104115/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 40s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Có một hàng$n$rương được đánh số từ 1 đến$n$. Chính xác là một ngực$k$chứa kho báu, trong khi tất cả những thứ khác đều trống rỗng. Một tên cướp biển bắt đầu mở rương nhưng vẫn chưa phát hiện ra kho báu nằm ở đâu. 

Anh ta cam kết thực hiện một trong hai chiến lược cố định trước khi bắt đầu: hoặc anh ta mở rương từ trái sang phải, bắt đầu từ 1 và di chuyển lên trên, hoặc anh ta mở rương từ phải sang trái, bắt đầu từ$n$và di chuyển xuống dưới. Anh ta dừng lại ngay lập tức khi mở chiếc rương chứa kho báu. 

Chúng ta được yêu cầu tính xem anh ta sẽ mở bao nhiêu rương trong trường hợp xấu nhất, giả sử kho báu thực sự ở đúng vị trí.$k$, nhưng cướp biển chọn trước hướng nào tốt hơn trong hai hướng để giảm thiểu số lần sơ hở tối đa cần thiết. 

Ràng buộc$n \le 10^9$loại trừ mọi mô phỏng hoặc quét lặp trên mảng. Bất kỳ giải pháp nào cũng phải có thời gian không đổi, vì ngay cả việc quét tuyến tính trên$n$là không thể. 

Một tình huống phức tạp xảy ra khi kho báu ở gần một đầu. Ví dụ, nếu$n = 5, k = 4$, sau đó quét từ bên trái mất 4 lỗ, trong khi quét từ bên phải mất 2 lỗ (5 → 4). Một cách tiếp cận ngây thơ luôn cho rằng từ trái sang phải sẽ trả về 4 không chính xác mà không so sánh cả hai hướng. Câu trả lời đúng phụ thuộc vào độ gần với điểm cuối gần nhất. 

Một trường hợp cạnh khác là khi$k = 1$hoặc$k = n$. Trong cả hai trường hợp, một hướng sẽ tìm thấy kho báu ngay lập tức sau 1 bước, trong khi hướng còn lại sẽ tìm thấy kho báu.$n$các bước. Bất kỳ giải pháp nào tính trung bình hoặc kết hợp khoảng cách thay vì lấy mức tối thiểu sẽ thất bại ở đây. 

## Phương pháp tiếp cận 

Một mô phỏng trực tiếp sẽ cố gắng theo dõi quá trình của tên cướp biển theo đúng nghĩa đen. Nếu chúng ta xác định một hướng đi, chúng ta sẽ đếm xem có bao nhiêu rương được mở cho đến khi đạt được$k$. Từ bên trái, đây chính xác là$k$. Từ bên phải, đây là$n - k + 1$. Cách tiếp cận này đúng vì nó phản ánh chính xác định nghĩa quy trình, nhưng nó trở nên không phù hợp trong các ràng buộc lớn vì kích thước đầu vào rất lớn nhưng việc tính toán cho mỗi lần kiểm tra là không đáng kể. 

Quan sát quan trọng là chỉ có hai đường duyệt đơn điệu có thể xảy ra và mỗi đường tạo ra một chi phí xác định hoàn toàn dựa trên vị trí của$k$. Vấn đề giảm xuống còn việc chọn khoảng cách nhỏ hơn trong hai khoảng cách tuyến tính: khoảng cách từ ranh giới bên trái và khoảng cách từ ranh giới bên phải. Không có cấu trúc trung gian nào quan trọng vì không có sự phân nhánh hoặc tính ngẫu nhiên trong quá trình tìm kiếm. 

Vì vậy, giải pháp tối ưu chỉ đơn giản là tính toán cả chi phí và lấy mức tối thiểu. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(n) | O(1) | Quá chậm | 
| Công thức trực tiếp | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính xem có bao nhiêu rương sẽ được mở nếu cướp biển bắt đầu từ bên trái. Đây chính xác là$k$, vì anh ta mở 1, 2, ..., cho đến$k$. 
2. Tính xem có bao nhiêu rương sẽ được mở nếu cướp biển bắt đầu từ bên phải. Đây là$n - k + 1$, kể từ khi anh ấy mở$n, n-1, ..., k$. 
3. So sánh hai giá trị này và chọn giá trị nhỏ hơn, vì tên cướp biển sẽ chọn chiến lược giảm thiểu số lần sơ hở trong trường hợp xấu nhất khi biết về$k$. 

### Tại sao nó hoạt động 

Mỗi chiến lược là một quá trình truyền tải đơn điệu xác định không có bước bỏ qua. Chi phí chỉ phụ thuộc vào số bước cần thiết để đạt được chỉ mục$k$từ một điểm cuối cố định. Vì cả hai đường truyền đều tuyến tính và độc lập nên lựa chọn tối ưu luôn là khoảng cách nhỏ nhất tương ứng của chúng. Không có sự tương tác giữa các bước nên không có chiến lược thích ứng nào có thể cải thiện được hai phương án cố định này. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, k = map(int, input().split())
    left = k
    right = n - k + 1
    print(min(left, right))

if __name__ == "__main__":
    solve()
```Mã đọc hai số nguyên và tính toán trực tiếp hai chi phí tìm kiếm có thể. Giá trái tương ứng với các vị trí đếm từ 1 đến$k$, trong khi chi phí đúng tương ứng với việc đếm ngược từ$n$. Câu trả lời cuối cùng là mức tối thiểu của cả hai. 

Điều tinh tế duy nhất là hãy nhớ rằng khoảng cách bên phải bao gồm chính vị trí đó, do đó$+1$. Không có nó, số đếm sẽ bị giảm đi một bất cứ khi nào$k$chính xác là ở điểm cuối. 

## Ví dụ đã hoạt động 

### Ví dụ 1:$n = 5, k = 4$| Bước | Chi phí còn lại | Chi phí phù hợp | Quyết định | 
| --- | --- | --- | --- | 
| Ban đầu | 4 | 2 | So sánh | 
| Cuối cùng | 4 | 2 | Chọn 2 | 

Từ bên trái, các rương từ 1 đến 4 được mở. Từ bên phải, rương 5 và 4 được mở nên kho báu được tìm thấy sau 2 bước. Bảng xác nhận rằng khoảng cách gần với ranh giới bên phải chiếm ưu thế. 

### Ví dụ 2:$n = 10, k = 3$| Bước | Chi phí còn lại | Chi phí phù hợp | Quyết định | 
| --- | --- | --- | --- | 
| Ban đầu | 3 | 8 | So sánh | 
| Cuối cùng | 3 | 8 | Chọn 3 | 

Ở đây kho báu nằm gần phía bên trái hơn nên việc quét từ đầu là tối ưu. Điều này xác nhận quyết định giảm bớt việc so sánh khoảng cách với điểm cuối. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Chỉ các phép tính số học và một phép so sánh | 
| Không gian | O(1) | Không có cấu trúc dữ liệu phụ trợ | 

Các ràng buộc cho phép lên đến$10^9$, nhưng vì không có lần lặp nào phụ thuộc vào$n$, giải pháp sẽ chạy ngay lập tức ngay cả ở kích thước đầu vào tối đa. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n, k = map(int, input().split())
    left = k
    right = n - k + 1
    return str(min(left, right))

# provided samples
assert run("1 1") == "1"
assert run("5 4") == "2"
assert run("10 3") == "3"

# custom cases
assert run("5 1") == "1"   # boundary left
assert run("5 5") == "1"   # boundary right
assert run("6 3") == "3"   # symmetric-ish case
assert run("7 4") == "4"   # center case
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 5 1 | 1 | ranh giới bên trái thành công ngay lập tức | 
| 5 5 | 1 | đúng ranh giới thành công ngay lập tức | 
| 6 3 | 3 | cân bằng vị trí nhỏ-trung | 
| 7 4 | 4 | đối xứng vị trí trung tâm | 

## Vỏ cạnh 

Khi nào$k = 1$, chiến lược bên trái sẽ tìm thấy kho báu ngay lập tức trong một bước, trong khi chiến lược bên phải sẽ lấy$n$các bước. Thuật toán tính toán$left = 1$,$right = n$và trả về chính xác 1. 

Khi nào$k = n$, xảy ra tình huống đối xứng. Tính toán mang lại kết quả$left = n$,$right = 1$, và một lần nữa mức tối thiểu là 1. 

Khi nào$k$chính xác ở giữa, cả hai hướng đều hoạt động giống nhau nhưng khác nhau nhiều nhất một hướng do tính chẵn lẻ. Ví dụ,$n = 6, k = 3$cho$left = 3$,$right = 4$. Thuật toán chọn 3, phù hợp với thực tế là quá trình duyệt bên trái đạt đến nó trước. 

Những trường hợp này xác nhận rằng giải pháp giảm khoảng cách quy trình một cách chính xác đến điểm cuối mà không bỏ sót các hiệu ứng biên.
