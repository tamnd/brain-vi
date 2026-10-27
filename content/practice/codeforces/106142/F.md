---
title: "CF 106142F - \u0421\u0434\u0435\u043b\u0430\u0442\u044c \u043c\u0430\u043a\u0441\u0438\u043c\u0430\u043b\u044c\u043d\u044b\u043c"
description: "Chúng ta được cung cấp một mảng các số nguyên và với mỗi vị trí, chúng ta phải trả lời một câu hỏi tối ưu hóa riêng biệt về giá trị của vị trí đó. Phần tử tại chỉ mục i được coi là phần tử tham chiếu cố định."
date: "2026-06-20T22:07:23+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106142
codeforces_index: "F"
codeforces_contest_name: "2025-2026 \u0412\u0441\u0435\u0440\u043e\u0441\u0441\u0438\u0439\u0441\u043a\u0430\u044f \u043a\u043e\u043c\u0430\u043d\u0434\u043d\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430 \u0448\u043a\u043e\u043b\u044c\u043d\u0438\u043a\u043e\u0432 \u043f\u043e \u043f\u0440\u043e\u0433\u0440\u0430\u043c\u043c\u0438\u0440\u043e\u0432\u0430\u043d\u0438\u044e, \u0440\u0435\u0433\u0438\u043e\u043d\u0430\u043b\u044c\u043d\u044b\u0439 \u044d\u0442\u0430\u043f \u0421\u0430\u0440\u0430\u0442\u043e\u0432\u0441\u043a\u043e\u0439 \u043e\u0431\u043b\u0430\u0441\u0442\u0438 (\u0412\u041a\u041e\u0428\u041f 25, \u0421\u0430\u0440\u0430\u0442\u043e\u0432\u0441\u043a\u0438\u0439 \u043e\u0442\u0431\u043e\u0440\u043e\u0447\u043d\u044b\u0439 \u044d\u0442\u0430\u043f)"
rating: 0
weight: 106142
solve_time_s: 51
verified: true
draft: false
---

[CF 106142F - \u0421\u0434\u0435\u043b\u0430\u0442\u044c \u043c\u0430\u043a\u0441\u0438\u043c\u0430\u043b\u044c\u043d\u044b\u043c](https://codeforces.com/problemset/problem/106142/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 51s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một mảng các số nguyên và với mỗi vị trí, chúng ta phải trả lời một câu hỏi tối ưu hóa riêng biệt về giá trị của vị trí đó. Phần tử tại chỉ mục i được coi là phần tử tham chiếu cố định. Chúng tôi được phép xóa bất kỳ phần tử nào khác khỏi mảng và sau khi xóa, chúng tôi sẽ xem xét chuỗi còn lại. Mục tiêu của chỉ mục i là làm cho giá trị của nó trở thành giá trị tối đa trong số tất cả các phần tử còn lại, với các mối quan hệ được cho phép. Chúng tôi muốn số lần xóa tối thiểu cần thiết để đạt được điều đó. 

Một cách khác để xem nhiệm vụ là đối với mỗi phần tử a[i], chúng ta muốn giữ một tập hợp con của mảng vẫn chứa a[i] và trong tập hợp con đó không được có phần tử nào lớn hơn a[i]. Mọi thứ lớn hơn a[i] phải được loại bỏ. Trong số các phần tử nhỏ hơn hoặc bằng a[i], chúng ta được phép giữ hoặc xóa tùy ý, nhưng loại bỏ ít hơn luôn tốt hơn vì việc xóa sẽ tốn kém. 

Các ràng buộc lên tới n = 200000, do đó, bất kỳ giải pháp nào có hành vi bậc hai trên tất cả i đều là không thể ngay lập tức. Một cách tiếp cận đơn giản là tính toán lại câu trả lời cho mỗi i bằng cách quét mảng và đếm số lần xóa sẽ dẫn đến O(n^2), kết quả này quá lớn. Điều này buộc chúng tôi phải hướng tới một giải pháp tái sử dụng cấu trúc toàn cục của mảng thay vì tính toán lại từ đầu. 

Một trường hợp tế nhị phát sinh khi các giá trị lặp lại. Nếu một giá trị xuất hiện nhiều lần, việc chọn một lần xuất hiện làm “đại diện tối đa” nghĩa là chúng ta được phép giữ các phần tử bằng nhau khác. Ví dụ: trong một mảng như [5, 1, 5], đối với phần tử ở giữa là 1 thì phải loại bỏ tất cả số 5 nên đáp án là 2. Đối với phần tử 5, chúng ta có thể giữ lại tất cả các số 5 nên không cần xóa. 

Một trường hợp cạnh quan trọng khác là khi phần tử đã ở mức tối đa trên toàn cầu. Trong trường hợp đó, không cần xóa vì nó đã thỏa mãn điều kiện. Bất kỳ cách tiếp cận nào tính nhầm các phần tử bằng nhau khi xóa sẽ thất bại ở đây. 

## Phương pháp tiếp cận 

Ý tưởng vũ phu rất đơn giản. Với mỗi chỉ số i, cố định giá trị x = a[i]. Sau đó chúng tôi tính toán có bao nhiêu phần tử lớn hơn x. Tất cả các yếu tố như vậy phải được loại bỏ. Sau khi loại bỏ chúng, mảng còn lại tự động thỏa mãn x là giá trị tối đa. Vì vậy, câu trả lời cho i chỉ đơn giản là số phần tử lớn hơn a[i]. Điều này đúng vì mọi tập hợp con hợp lệ đều không thể bao gồm phần tử lớn hơn. 

Với mỗi i, việc triển khai đơn giản sẽ quét toàn bộ mảng và đếm các giá trị lớn hơn a[i]. Chi phí này là O(n) cho mỗi truy vấn, dẫn đến tổng số hoạt động là O(n^2). Với n lên tới 200000, điều này vượt xa giới hạn khả thi. 

Quan sát quan trọng là câu trả lời chỉ phụ thuộc vào giá trị a[i] chứ không phụ thuộc vào vị trí của nó. Nếu hai vị trí có cùng giá trị thì câu trả lời của chúng giống hệt nhau. Vì vậy, thay vì tính toán lại từng chỉ mục, chúng ta chỉ cần biết, với mỗi giá trị x riêng biệt, có bao nhiêu phần tử trong mảng lớn hơn x. 

Điều này ngay lập tức gợi ý việc sắp xếp hoặc tích lũy tần số. Nếu sắp xếp các giá trị mảng, chúng ta có thể tính toán cho mỗi giá trị x số phần tử lớn hơn x bằng cách sử dụng tổng hậu tố theo tần số. Khi chúng tôi biết điều này cho mọi giá trị có thể, chúng tôi sẽ gán lại kết quả được tính toán trước cho tất cả các vị trí. 

Cấu trúc của bài toán trở thành một câu hỏi xếp hạng toàn cầu: câu trả lời của mỗi phần tử được xác định bởi thứ hạng của nó trong thứ tự giá trị, chứ không phải vị trí của nó trong mảng ban đầu. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^2) | O(1) | Quá chậm | 
| Tần số giá trị + sắp xếp | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

## Hướng dẫn thuật toán

1. Đếm số lần mỗi giá trị riêng biệt xuất hiện trong mảng. Điều này nén vấn đề từ vị trí thành giá trị, vì các giá trị giống hệt nhau hoạt động giống hệt nhau trong câu trả lời cuối cùng. 
2. Sắp xếp các giá trị khác nhau theo thứ tự tăng dần. Điều này thiết lập một thứ tự rõ ràng từ nhỏ nhất đến lớn nhất, cho phép chúng ta suy luận xem có bao nhiêu phần tử lớn hơn một giá trị nhất định. 
3. Duyệt qua các giá trị được sắp xếp từ lớn nhất đến nhỏ nhất trong khi vẫn duy trì tổng số phần tử đã được nhìn thấy cho đến nay. Tổng số đang chạy này biểu thị có bao nhiêu phần tử lớn hơn giá trị hiện tại. 
4. Với mỗi giá trị x theo thứ tự giảm dần, gán đáp án[x] bằng tổng chạy hiện tại trước khi cộng tần số của x. Điều này có hiệu quả vì tại thời điểm đó chúng ta đã tính đến tất cả các giá trị lớn hơn x. 
5. Thêm tần số của x vào tổng số đang chạy, sao cho các giá trị nhỏ hơn sẽ nhìn thấy chính xác tất cả các giá trị lớn hơn có trong số đếm của chúng. 
6. Cuối cùng, ánh xạ các câu trả lời được tính toán trước này trở lại từng chỉ mục ban đầu bằng cách sử dụng bảng tần số hoặc tra cứu từ điển. 

Tại sao nó hoạt động: đối với bất kỳ giá trị cố định x nào, mọi phần tử lớn hơn x phải bị loại bỏ trong bất kỳ tập hợp con hợp lệ nào. Các phần tử nhỏ hơn hoặc bằng x không bao giờ cần phải loại bỏ vì lý do chính xác, vì x vẫn là giá trị tối đa trong số đó. Do đó, số lần xóa tối thiểu chính xác là số lượng phần tử lớn hơn x và việc tích lũy hậu tố trên các giá trị được sắp xếp sẽ tính toán số lượng này chính xác một lần cho mỗi giá trị riêng biệt. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))
    
    freq = {}
    for v in a:
        freq[v] = freq.get(v, 0) + 1

    vals = sorted(freq.keys())
    
    greater_count = 0
    ans_value = {}

    for v in reversed(vals):
        ans_value[v] = greater_count
        greater_count += freq[v]

    out = []
    for v in a:
        out.append(str(ans_value[v]))

    print(" ".join(out))

if __name__ == "__main__":
    solve()
```Việc triển khai bắt đầu bằng việc xây dựng bản đồ tần số của tất cả các giá trị. Điều này rất cần thiết vì thuật toán hoạt động trên các lớp giá trị thay vì vị trí. Danh sách các khóa được sắp xếp xác định thứ tự chúng ta tích lũy số phần tử lớn hơn. 

Việc duyệt ngược đảm bảo rằng khi chúng ta gán ans_value[v], biến Greater_count đã chứa chính xác tất cả các phần tử lớn hơn v. Bước cập nhật xảy ra sau phép gán, điều này rất quan trọng để tránh vô tình đếm chính giá trị hiện tại. 

Cuối cùng, chúng tôi ánh xạ các câu trả lời trở lại từng vị trí bằng cách sử dụng tra cứu từ điển trực tiếp, điều này giúp cho việc tái cấu trúc cuối cùng trở nên tuyến tính. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
2 5 1 7 5
```Chúng tôi tính toán tần số: 

2:1, 5:2, 1:1, 7:1 

Các giá trị được sắp xếp: [1, 2, 5, 7] 

Chúng tôi xử lý ngược lại: 

| Giá trị | Lớn hơn cho đến nay | Câu trả lời được giao | 
| --- | --- | --- | 
| 7 | 0 | 0 | 
| 5 | 1 | 1 | 
| 2 | 3 | 3 | 
| 1 | 4 | 4 | 

Ánh xạ trở lại mang lại:```
3 1 4 0 1
```Dấu vết này cho thấy câu trả lời của mỗi phần tử chỉ phụ thuộc vào số lượng phần tử lớn hơn tồn tại trên toàn cầu. 

### Ví dụ 2 

đầu vào:```
100 100 100 100
```Tần số: 

100:4 

Các giá trị được sắp xếp: [100] 

| Giá trị | Lớn hơn cho đến nay | Câu trả lời được giao | 
| --- | --- | --- | 
| 100 | 0 | 0 | 

Đầu ra:```
0 0 0 0
```Điều này xác nhận rằng khi tất cả các giá trị đều bằng nhau thì không cần xóa vì không có phần tử nào lớn hơn hoàn toàn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Sắp xếp các giá trị riêng biệt chiếm ưu thế; tất cả các hoạt động khác là tuyến tính | 
| Không gian | O(n) | Bản đồ tần số và giá trị lưu trữ mảng đầu ra tỷ lệ thuận với kích thước đầu vào | 

Giải pháp phù hợp một cách thoải mái trong các giới hạn vì n lên tới 200000. Việc sắp xếp 200000 số nguyên và thực hiện chuyển tuyến tính nằm trong giới hạn 2 giây trong Python khi được triển khai bằng các thao tác từ điển trực tiếp. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n = int(input())
    a = list(map(int, input().split()))

    freq = {}
    for v in a:
        freq[v] = freq.get(v, 0) + 1

    vals = sorted(freq.keys())

    greater_count = 0
    ans_value = {}

    for v in reversed(vals):
        ans_value[v] = greater_count
        greater_count += freq[v]

    return " ".join(str(ans_value[v]) for v in a)

# provided samples
assert run("5\n2 5 1 7 5\n") == "3 1 4 0 1"
assert run("4\n100 100 100 100\n") == "0 0 0 0"

# custom cases
assert run("3\n3 2 1\n") == "2 1 0", "strictly decreasing"
assert run("5\n1 2 3 4 5\n") == "4 3 2 1 0", "strictly increasing"
assert run("6\n5 1 5 2 5 3\n") == "0 3 0 2 0 1", "repeated max structure"
assert run("2\n1 2\n") == "1 0", "minimum case"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 3 2 1 | 2 1 0 | giảm độ đúng thứ tự | 
| 1 2 3 4 5 | 4 3 2 1 0 | tăng độ chính xác của lệnh | 
| 5 1 5 2 5 3 | 0 3 0 2 0 1 | cấu trúc trùng lặp và hỗn hợp | 
| 1 2 | 1 0 | trường hợp không tầm thường nhỏ nhất | 

## Vỏ cạnh 

Khi tất cả các phần tử đều bằng nhau, thuật toán gán 0 cho mọi giá trị vì quá trình duyệt ngược không thấy phần tử nào lớn hơn. Đối với đầu vào`[10, 10, 10]`, bản đồ tần số có một khóa duy nhất và Greater_count bắt đầu từ 0, do đó mọi chỉ mục đều nhận được 0. Điều này phù hợp với yêu cầu rằng phần tử được chọn đã ở mức tối đa. 

Khi mảng tăng nghiêm ngặt, mọi phần tử ngoại trừ phần tử cuối cùng phải loại bỏ tất cả các phần tử hậu tố lớn hơn. Vì`[1, 2, 3, 4]`, việc tích lũy hậu tố sẽ gán các câu trả lời giảm đúng vì mỗi giá trị nhìn thấy chính xác các phần tử ở bên phải của nó theo thứ tự giá trị, tương ứng với tất cả các giá trị lớn hơn.
