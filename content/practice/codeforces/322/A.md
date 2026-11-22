---
title: "CF 322A - Ciel và Khiêu vũ"
description: "Nhiệm vụ mô tả một phòng khiêu vũ trong đó mỗi buổi biểu diễn đều có sự tham gia của chính xác một chàng trai và đúng một cô gái. Những người tham gia bắt đầu mà không có kinh nghiệm trước đó và trong mỗi điệu nhảy, ít nhất một trong hai người phải nhảy lần đầu tiên."
date: "2026-06-06T02:39:09+07:00"
tags: ["codeforces", "competitive-programming", "greedy"]
categories: ["algorithms"]
codeforces_contest: 322
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 190 (Div. 2)"
rating: 1000
weight: 322
solve_time_s: 63
verified: true
draft: false
---

[CF 322A - Ciel và khiêu vũ](https://codeforces.com/problemset/problem/322/A) 

**Đánh giá:** 1000 
**Thẻ:** tham lam 
**Thời gian giải:** 1m 3s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Nhiệm vụ mô tả một phòng khiêu vũ trong đó mỗi buổi biểu diễn đều có sự tham gia của chính xác một chàng trai và đúng một cô gái. Những người tham gia bắt đầu mà không có kinh nghiệm trước đó và trong mỗi điệu nhảy, ít nhất một trong hai người phải nhảy lần đầu tiên. Nói cách khác, mỗi cặp phải bao gồm ít nhất một người tham gia chưa từng xuất hiện trong bất kỳ điệu nhảy nào trước đó. 

Chúng ta được cung cấp số lượng nam và nữ, và chúng ta cần xây dựng một chuỗi các điệu nhảy hợp lệ kéo dài nhất có thể đồng thời tôn trọng ràng buộc này. Mỗi điệu nhảy không độc lập, vì một khi một người đã tham gia, họ có thể không còn đáp ứng điều kiện “lần đầu tiên” trong các cặp sau này, vì vậy những lựa chọn trước đó sẽ ảnh hưởng trực tiếp đến những gì còn có thể thực hiện được sau này. 

Giới hạn rất nhỏ, cả hai đều có số lượng lên tới 100. Điều này ngay lập tức loại trừ mọi nhu cầu tối ưu hóa mạnh mẽ hoặc tìm kiếm trên các tập hợp con. Ngay cả cấu trúc O(n²) hoặc O(nm) cũng không đáng kể về mặt thời gian, vì vậy khó khăn thực sự không phải là hiệu quả mà là việc tìm ra cấu trúc tránh vô tình chặn các bước di chuyển hợp lệ trong tương lai. 

Một cách tiếp cận ngây thơ là liên tục chọn bất kỳ cặp hợp lệ nào và tiếp tục tham lam. Chế độ thất bại tinh tế xuất hiện khi cả hai người tham gia trong cặp được chọn đều đã được sử dụng trước đó. Ví dụ: nếu chúng ta ghép hai người đã sử dụng rồi, quy tắc sẽ bị vi phạm ngay lập tức và công trình bị phá vỡ. Một chế độ thất bại khác là làm cạn kiệt sớm một nhóm người tham gia “mới” trong khi khiến bên kia không được sử dụng đúng mức, điều này làm giảm tổng số bước nhảy có thể đạt được. 

Khó khăn chính là đảm bảo rằng chúng tôi không bao giờ “lãng phí” cơ hội giới thiệu người tham gia mới quá sớm theo cách ngăn cản việc ghép đôi hợp lệ sau này. 

## Phương pháp tiếp cận 

Chiến lược bạo lực sẽ mô phỏng tất cả các chuỗi hợp lệ có thể có. Ở mỗi bước, chúng ta có thể thử mọi cặp chưa sử dụng hoặc đã sử dụng trước đó mà vẫn đáp ứng quy tắc và lặp lại. Mặc dù về nguyên tắc là đúng, nhưng điều này nhanh chóng trở thành cấp số nhân vì mỗi trạng thái phân nhánh qua nhiều cặp có thể có. Ngay cả với n và m khoảng 100, số lượng trình tự sẽ bùng nổ, vì mỗi quyết định thay đổi mà những người tham gia vẫn là “mới” và tính hợp lệ của các chuyển đổi trong tương lai. 

Cái nhìn sâu sắc về cấu trúc là mỗi điệu nhảy đều có ít nhất một người tham gia chưa được sử dụng trước đó. Điều này có nghĩa là số lượng điệu nhảy được kiểm soát chặt chẽ bởi số lượng người mới mà chúng tôi có thể giới thiệu. Khi cả hai nhóm đã tiêu thụ một phần, chiến lược tốt nhất là tiếp tục giới thiệu chính xác một người tham gia mới cho mỗi điệu nhảy càng lâu càng tốt. Bất kỳ sai lệch nào khi giới thiệu sớm hai người tham gia mới đều không giúp ích được gì, vì nó làm giảm lượng đóng góp “mới” có sẵn trong tương lai. 

Điều này dẫn đến một công trình mà trước tiên chúng tôi “neo” một bé trai và một bé gái lại với nhau. Sau đó, chúng tôi ghép những chàng trai còn lại với một cô gái cố định, tiếp tục giới thiệu những chàng trai mới, sau đó ghép những cô gái còn lại với một chàng trai cố định, tiếp tục giới thiệu những cô gái mới. Điều này đảm bảo rằng mỗi bước sẽ giới thiệu chính xác một người tham gia mới sau lần ghép đôi đầu tiên, điều này là tối ưu. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | Hàm mũ | Hàm mũ | Quá chậm | 
| Tham lam mang tính xây dựng | O(n + m) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Chọn một nam và một nữ làm cặp neo ban đầu và sắp xếp cho họ khiêu vũ cùng nhau trước. Điều này đảm bảo rằng cả hai bên đều có ít nhất một người tham gia “đã qua sử dụng”, điều này mang lại sự linh hoạt cho việc xây dựng còn lại. 
2. Cố định cô gái đã chọn làm đối tác có thể tái sử dụng và ghép cô ấy với mọi chàng trai khác. Mỗi điệu nhảy này giới thiệu một chàng trai mới trong khi cô gái đã được sử dụng trước đó, vì vậy ràng buộc vẫn được thỏa mãn. 
3. Cố định chàng trai đã chọn làm đối tác có thể tái sử dụng và ghép anh ta với mọi cô gái còn lại ngoại trừ người đã được sử dụng ở bước đầu tiên. Mỗi điệu nhảy này giới thiệu một cô gái mới trong khi cậu bé đã được sử dụng, giữ cho quy tắc có hiệu lực. 
4. Xuất tất cả các cặp đã ghi theo thứ tự. 

Ý tưởng chính là sau lần ghép đôi đầu tiên, chúng tôi luân phiên bên nào đóng góp một người tham gia mới trong khi vẫn giữ một người tham gia “cũ” cố định ở phía bên kia. 

### Tại sao nó hoạt động 

Ở mỗi bước sau bước đầu tiên, có chính xác một người tham gia trong cặp chưa từng khiêu vũ trước đó. Điều này đảm bảo quy tắc được đáp ứng cho mỗi điệu nhảy. Việc xây dựng cũng đảm bảo rằng chúng tôi không bao giờ hết các cặp hợp lệ sớm, bởi vì mỗi cậu bé chưa sử dụng cuối cùng sẽ được ghép đôi một lần, và mọi cô gái không sử dụng cũng được ghép đôi một lần. Vì mỗi người tham gia mới được sử dụng chính xác một lần với tư cách là phần “mới” của một cặp đôi, nên không cấu hình nào có thể tạo ra nhiều điệu nhảy hơn mà không vi phạm ràng buộc. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())
    
    res = []
    
    # Step 1: anchor pair
    res.append((1, 1))
    
    # Step 2: use girl 1 with remaining boys
    for i in range(2, n + 1):
        res.append((i, 1))
    
    # Step 3: use boy 1 with remaining girls
    for j in range(2, m + 1):
        res.append((1, j))
    
    print(len(res))
    for a, b in res:
        print(a, b)

if __name__ == "__main__":
    solve()
```Giải pháp xây dựng lịch trình một cách rõ ràng hơn là tìm kiếm nó. Cặp đầu tiên khởi tạo cả hai người tham gia là “đã sử dụng”, điều này mở khóa khả năng tái sử dụng một trong hai bên một cách an toàn. Vòng lặp của các chàng trai giữ cho cô gái 1 cố định, đảm bảo mọi chàng trai mới đều đóng góp sự mới lạ cần thiết. Vòng lặp thứ hai phản ánh ý tưởng này với cậu bé 1 được cố định trong khi giới thiệu tất cả các cô gái còn lại. 

Thứ tự này rất quan trọng vì việc đảo ngược các vòng lặp không quan trọng đối với tính chính xác nhưng việc neo ban đầu phải xảy ra trước để đảm bảo tồn tại ít nhất một người tham gia được sử dụng lại trong các bước sau. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
2 1
```Chúng ta bắt đầu với một bé trai và một bé gái. 

| Bước | Cặp | Chàng Trai Mới | Cô Gái Mới | Hiệu lực | 
| --- | --- | --- | --- | --- | 
| 1 | (1,1) | vâng | vâng | ban đầu | 
| 2 | (2,1) | vâng | không | hợp lệ | 

Điều này tạo ra 2 điệu nhảy. Sau lần đầu tiên, bé trai 1 và bé gái 1 được sử dụng. Người thứ hai sử dụng cậu bé 2 làm người tham gia mới. 

### Ví dụ 2 

đầu vào:```
2 2
```| Bước | Cặp | Chàng Trai Mới | Cô Gái Mới | Hiệu lực | 
| --- | --- | --- | --- | --- | 
| 1 | (1,1) | vâng | vâng | ban đầu | 
| 2 | (2,1) | vâng | không | hợp lệ | 
| 3 | (1,2) | không | vâng | hợp lệ | 

Điều này mang lại 3 điệu nhảy. Mỗi bước giới thiệu chính xác một người tham gia mới sau lần ghép đôi đầu tiên. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + m) | Mỗi bé trai và bé gái được sử dụng tối đa một bước được xây dựng sau khi khởi tạo | 
| Không gian | O(1) | Chỉ có danh sách đầu ra được lưu trữ, không có cấu trúc phụ trợ tùy thuộc vào kích thước đầu vào | 

Các ràng buộc cho phép tối đa 100 người tham gia mỗi nhóm, do đó việc xây dựng tuyến tính là không đáng kể cả về thời gian và bộ nhớ. Kích thước đầu ra cũng được giới hạn bởi n + m, đủ nhỏ để in trực tiếp. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

def solve():
    n, m = map(int, input().split())
    res = [(1, 1)]
    for i in range(2, n + 1):
        res.append((i, 1))
    for j in range(2, m + 1):
        res.append((1, j))
    print(len(res))
    for a, b in res:
        print(a, b)

# provided sample
assert run("2 1\n") == "2\n1 1\n2 1"

# custom cases
assert run("1 1\n") == "1\n1 1", "minimum case"
assert run("1 5\n") == "5\n1 1\n1 2\n1 3\n1 4\n1 5", "single boy"
assert run("5 1\n") == "5\n1 1\n2 1\n3 1\n4 1\n5 1", "single girl"
assert run("3 3\n") == "5\n1 1\n2 1\n3 1\n1 2\n1 3", "balanced case"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1 | 1 cặp | cấu hình nhỏ nhất | 
| 1 5 | 5 đôi | chỉ có cô gái khác nhau | 
| 5 1 | 5 đôi | chỉ có con trai mới thay đổi | 
| 3 3 | 5 đôi | mở rộng hỗn hợp | 

## Vỏ cạnh 

Khi chỉ có một chàng trai hoặc chỉ một cô gái, thuật toán vẫn hoạt động vì việc ghép đôi đầu tiên ngay lập tức khiến người tham gia duy nhất có thể tái sử dụng và tất cả các điệu nhảy còn lại phải sử dụng lại người cố định đó đồng thời giới thiệu phía bên kia. Ví dụ, với đầu vào`1 4`, các cặp trình tự`(1,1), (1,2), (1,3), (1,4)`được sản xuất một cách tự nhiên và mỗi bước giới thiệu một cô gái mới, đáp ứng quy tắc. 

Khi cả n và m đều bằng 1 thì chỉ có thể nhảy một lần. Thuật toán tạo ra chính xác`(1,1)`và dừng lại vì không còn người tham gia nào để kéo dài chuỗi.
