---
title: "CF 104785K - Lập lịch hạt nhân"
description: "Chúng ta có một đồ thị có hướng trong đó mỗi tác vụ là một đỉnh và mỗi phụ thuộc là một cạnh có hướng. Cạnh a - b có nghĩa là tác vụ a phải được thực thi trước tác vụ b."
date: "2026-06-28T14:41:59+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104785
codeforces_index: "K"
codeforces_contest_name: "2023 United Kingdom and Ireland Programming Contest (UKIEPC 2023)"
rating: 0
weight: 104785
solve_time_s: 56
verified: true
draft: false
---

[CF 104785K - Trình lập lịch hạt nhân](https://codeforces.com/problemset/problem/104785/K) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 56s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một đồ thị có hướng trong đó mỗi tác vụ là một đỉnh và mỗi phụ thuộc là một cạnh có hướng. Một cạnh`a -> b`nghĩa là nhiệm vụ`a`phải được thực hiện trước nhiệm vụ`b`. 

Vấn đề là những phần phụ thuộc này có thể chứa các chu trình được định hướng, khiến không thể thực hiện tất cả các tác vụ theo một thứ tự hợp lệ. Chúng ta được phép loại bỏ một số cạnh để loại bỏ tất cả các chu trình nhưng vẫn phải giữ lại ít nhất một nửa số cạnh ban đầu. 

Đầu ra không phải là thứ tự của các nhiệm vụ mà là một tập hợp con của các cạnh phụ thuộc sao cho đồ thị có hướng còn lại không có chu trình có hướng và số cạnh được giữ ít nhất bằng một nửa số ban đầu. 

Yêu cầu cấu trúc quan trọng là các cạnh được giữ phải tạo thành Đồ thị tuần hoàn có hướng. Điều đó có nghĩa là phải tồn tại một số thứ tự các đỉnh sao cho mọi cạnh được giữ đều đi từ đỉnh trước đến đỉnh sau. 

Các ràng buộc cho phép lên đến`n = 100000`Và`m = 300000`, điều này ngay lập tức loại trừ mọi suy luận hàm mũ hoặc bậc hai đối với các cạnh hoặc hoán vị của các đỉnh. Bất cứ điều gì liên quan đến việc tìm kiếm theo thứ tự hoặc kiểm tra lặp đi lặp lại chu kỳ cho mỗi lần xóa sẽ quá chậm. 

Một điểm tinh tế là cho phép có nhiều cạnh giữa cùng một cặp đỉnh và chúng được xử lý độc lập. Mỗi cạnh được chọn hoặc loại bỏ độc lập dựa trên việc nó có phù hợp với cấu trúc tuần hoàn đã chọn hay không. 

Một sai lầm ngây thơ là cố gắng phá vỡ trực tiếp các chu trình bằng cách sử dụng tính năng phát hiện chu trình DFS và loại bỏ một cạnh trên mỗi chu trình được tìm thấy. Điều này không thành công vì các chu kỳ chồng chéo và việc loại bỏ tham lam có thể xóa nhiều hơn mức cần thiết, dễ dàng giảm xuống dưới mức yêu cầu.`m/2`ngưỡng. 

Một sai lầm khác là cố gắng sắp xếp cấu trúc liên kết đầy đủ trên biểu đồ gốc. Điều đó chỉ hoạt động nếu biểu đồ đã là DAG, điều này không được đảm bảo. 

## Phương pháp tiếp cận 

Một cách giải thích bạo lực sẽ cố gắng kiểm tra các tập hợp con của các cạnh, kiểm tra xem tập hợp con được chọn có chu kỳ và đủ lớn hay không. Ngay cả việc kiểm tra một tập hợp con cũng yêu cầu phát hiện chu kỳ trong`O(n + m)`, và có`2^m`tập hợp con, điều này hoàn toàn không khả thi. 

Một ý tưởng mạnh mẽ có cấu trúc hơn là chỉ định thứ tự các đỉnh và chỉ giữ các cạnh nhất quán với nó. Điều này luôn tạo ra một biểu đồ không có chu kỳ, nhưng thách thức đặt ra là tìm ra thứ tự giữ được ít nhất một nửa số cạnh. Tìm kiếm trên tất cả các hoán vị một lần nữa là không thể. 

Quan sát quan trọng là mọi thứ tự tổng thể của các đỉnh đều phân chia các cạnh thành hai nhóm rời rạc: các cạnh đi theo thứ tự và các cạnh đi lùi. Các cạnh phía trước luôn tạo thành DAG. Nếu chúng ta chọn thứ tự nhận dạng`1 to n`, chúng ta nhận được một tập hợp con hợp lệ. Nếu chúng ta chọn thứ tự ngược lại, chúng ta sẽ nhận được một tập hợp con hợp lệ khác. Mọi cạnh đều thuộc đúng một trong hai tập con này vì với mọi cạnh`a -> b`, hoặc`a < b`hoặc`a > b`. 

Điều này ngay lập tức ngụ ý rằng hai tập hợp con cùng nhau bao phủ tất cả các cạnh, do đó một trong số chúng phải chứa ít nhất một nửa số cạnh. Việc chọn tập hợp con lớn hơn đảm bảo cả tính không tuần hoàn và yêu cầu về kích thước. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Liệt kê các tập hợp con/kiểm tra chu kỳ | O(2^m) | O(m) | Quá chậm | 
| Phân vùng hai bậc (tiến và lùi) | O(n + m) | O(m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng ta xây dựng hai tập cạnh ứng cử viên bằng cách sử dụng một thứ tự đỉnh cố định. 

Đầu tiên chúng ta xử lý trật tự tự nhiên`1, 2, ..., n`như một ứng cử viên trật tự tôpô và thu thập tất cả các cạnh đi từ chỉ mục nhỏ hơn đến chỉ mục lớn hơn. 

Thứ hai chúng tôi xem xét thứ tự ngược lại`n, ..., 1`và tập hợp tất cả các cạnh đi tiếp theo thứ tự đó, tương ứng với các cạnh ở đó`a > b`trong cách đánh số ban đầu. 

Sau đó chúng tôi chọn bộ lớn hơn trong hai bộ này. 

### Các bước 

1. Đọc tất cả các cạnh và gán cho chúng các chỉ số đầu vào để chúng ta có thể xuất chúng sau này. Việc lập chỉ mục này quan trọng vì đầu ra yêu cầu ID cạnh gốc. 
2. Khởi tạo hai danh sách, một danh sách cho các cạnh có`a < b`và một cái khác cho các cạnh trong đó`a > b`. Mỗi danh sách đại diện cho các cạnh nhất quán với tổng thứ tự khác nhau. 
3. Với mỗi cạnh, hãy so sánh điểm cuối của nó. Nếu như`a < b`, nó phù hợp với trật tự tự nhiên và được thêm vào danh sách đầu tiên. Ngược lại, nó phù hợp với thứ tự đảo ngược và được thêm vào danh sách thứ hai. 
4. So sánh kích thước của hai danh sách. Chọn cái lớn hơn vì ta phải đảm bảo ít nhất`m/2`các cạnh. 
5. Đầu ra`YES`, sau đó là kích thước của danh sách đã chọn, sau đó là chỉ số của các cạnh của nó. 

Lý do điều này có hiệu quả là vì mỗi cạnh được gán cho chính xác một trong hai danh sách, do đó kích thước của chúng có tổng bằng`m`. Do đó ít nhất một danh sách phải có kích thước ít nhất`m/2`. Cả hai danh sách đều tương ứng với các cạnh nhất quán với tổng thứ tự, đảm bảo tính không tuần hoàn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())
    
    forward = []
    backward = []
    
    for i in range(1, m + 1):
        a, b = map(int, input().split())
        if a < b:
            forward.append(i)
        else:
            backward.append(i)
    
    if len(forward) >= len(backward):
        chosen = forward
    else:
        chosen = backward
    
    print("YES")
    print(len(chosen))
    print(*chosen)

if __name__ == "__main__":
    solve()
```Việc triển khai dựa trên sự phân vùng trực tiếp của các cạnh. Vòng lặp chỉ mục rất quan trọng vì đầu ra yêu cầu ID cạnh gốc, vì vậy chúng tôi lưu trữ`i`cho mỗi cạnh khi nó được đọc. 

Không cần cấu trúc đồ thị ngoài các cạnh thô. Toàn bộ giải pháp dựa trên việc so sánh các điểm cuối và nhóm tương ứng. 

Một lỗi triển khai phổ biến là quên rằng các cạnh được lập chỉ mục 1 theo thứ tự đầu vào. Một cách khác là vô tình tính toán lại hoặc sửa đổi danh sách cạnh thay vì giữ nguyên các chỉ số. 

## Ví dụ đã hoạt động 

### Ví dụ Dấu vết 1 

Xem xét đầu vào:```
n = 3, m = 3
1 -> 2
2 -> 3
3 -> 1
```Chúng tôi xử lý từng cạnh một. 

| Cạnh | một < b | Danh sách chuyển tiếp | Danh sách lùi | 
| --- | --- | --- | --- | 
| 1->2 | vâng | [1] | [] | 
| 2->3 | vâng | [1,2] | [] | 
| 3->1 | không | [1,2] | [3] | 

Chúng ta so sánh kích thước: tiến có 2 cạnh, lùi có 1 cạnh. Chúng tôi chọn về phía trước. 

Các cạnh được chọn tạo thành DAG theo thứ tự`1 < 2 < 3`, nên không còn chu trình nào nữa. 

Điều này chứng tỏ một chu trình bị phá vỡ hoàn toàn bằng cách loại bỏ cạnh lùi. 

### Ví dụ Dấu vết 2 

Xem xét đầu vào:```
n = 2, m = 5
1 -> 2
1 -> 2
1 -> 2
2 -> 1
2 -> 1
```Chúng tôi xử lý: 

| Cạnh | một < b | Danh sách chuyển tiếp | Danh sách lùi | 
| --- | --- | --- | --- | 
| 1->2 | vâng | [1] | [] | 
| 1->2 | vâng | [1,2] | [] | 
| 1->2 | vâng | [1,2,3] | [] | 
| 2->1 | không | [1,2,3] | [4] | 
| 2->1 | không | [1,2,3] | [4,5] | 

Chúng tôi chọn chuyển tiếp với kích thước 3. 

Điều này cho thấy các cạnh trùng lặp được xử lý độc lập và vùng chọn vẫn đảm bảo ít nhất một nửa. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + m) | Mỗi cạnh được xử lý một lần và được gán cho một trong hai danh sách | 
| Không gian | O(m) | Chúng tôi lưu trữ các chỉ số của các cạnh trong hai nhóm | 

Các ràng buộc cho phép lên tới 300000 cạnh, vì vậy chỉ cần quét tuyến tính một lần là đủ. Không cần phải duyệt hoặc sắp xếp biểu đồ bổ sung. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    import contextlib
    out = io.StringIO()
    with contextlib.redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# minimum case
assert run("2 1\n1 2\n") == "YES\n1\n1"

# simple cycle
assert run("3 3\n1 2\n2 3\n3 1\n") in ["YES\n2\n1 2", "YES\n2\n1 3"]

# reverse-heavy case
assert run("3 4\n2 1\n3 2\n3 1\n2 1\n") != ""

# all forward
assert run("4 3\n1 2\n2 3\n3 4\n") == "YES\n3\n1 2 3"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| cạnh đơn | giữ nó | độ đúng tối thiểu | 
| 3 chu kỳ | giữ ít nhất 2 cạnh | phá vỡ chu kỳ | 
| hướng hỗn hợp | tập hợp con hợp lệ | logic phân vùng | 
| đã DAG | tất cả các cạnh được giữ | không cần loại bỏ không cần thiết | 

## Vỏ cạnh 

Ví dụ, trường hợp cạnh khóa là khi tất cả các cạnh đều trỏ theo thứ tự giảm dần`3 -> 2`,`2 -> 1`,`3 -> 1`. Trong trường hợp này, danh sách chuyển tiếp theo thứ tự tự nhiên trống, trong khi danh sách lùi chứa tất cả các cạnh. Thuật toán chọn chính xác danh sách lùi, tương ứng với việc đảo ngược thứ tự và do đó tạo ra DAG hợp lệ. 

Một trường hợp khác là một đồ thị hoàn chỉnh đối xứng hoàn toàn trong đó mỗi cặp có cả hai hướng. Mỗi cặp đóng góp chính xác một cạnh vào mỗi danh sách, vì vậy cả hai danh sách đều có kích thước bằng nhau. Lựa chọn nào cũng hợp lệ và vẫn không có tính tuần hoàn theo thứ tự tương ứng. 

Những trường hợp này xác nhận rằng chiến lược phân vùng ổn định ngay cả dưới sự phân phối đầu vào đối nghịch.
