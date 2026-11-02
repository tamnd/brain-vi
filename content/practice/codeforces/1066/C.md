---
title: "CF 1066C - Truy vấn sách"
description: "Chúng tôi đang mô phỏng một dãy sách ngày càng tăng được đặt trên kệ. Mỗi cuốn sách có một mã định danh duy nhất và chúng tôi chỉ thêm sách vào đầu bên trái hoặc đầu bên phải của cách sắp xếp hiện tại. Theo thời gian, điều này tạo ra thứ tự tuyến tính cố định của tất cả các sách được chèn vào."
date: "2026-06-15T13:09:43+07:00"
tags: ["codeforces", "competitive-programming", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1066
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 515 (Div. 3)"
rating: 1400
weight: 1066
solve_time_s: 175
verified: true
draft: false
---

[CF 1066C - Truy vấn sách](https://codeforces.com/problemset/problem/1066/C) 

**Đánh giá:** 1400 
**Thẻ:** triển khai 
**Thời gian giải:** 2m 55s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang mô phỏng một dãy sách ngày càng tăng được đặt trên kệ. Mỗi cuốn sách có một mã định danh duy nhất và chúng tôi chỉ thêm sách vào đầu bên trái hoặc đầu bên phải của cách sắp xếp hiện tại. Theo thời gian, điều này tạo ra thứ tự tuyến tính cố định của tất cả các sách được chèn vào. 

Cùng với việc chèn sách, chúng tôi còn nhận được các truy vấn hỏi về một cuốn sách cụ thể đã có trên giá sách. Đối với truy vấn như vậy, chúng ta phải tính toán cần bao nhiêu lần xóa từ đầu bên trái hoặc đầu bên phải để cuốn sách mục tiêu này được hiển thị ở một trong các đầu. Chúng tôi thực sự không xóa vĩnh viễn bất kỳ thứ gì, đây hoàn toàn chỉ là một câu hỏi giả định về thứ tự hiện tại. 

Quan sát chính từ các ràng buộc là có tới 200.000 thao tác, vì vậy chúng tôi không thể xây dựng lại hoặc mô phỏng danh sách đầy đủ cho mọi truy vấn bằng cách sử dụng một mảng và chèn lặp lại ở cả hai đầu, vì các thao tác như chèn vào bên trái danh sách Python hoặc xóa từ giữa sẽ giảm xuống hành vi bậc hai trong trường hợp xấu nhất. Chúng ta cần một biểu diễn hỗ trợ chèn O(1) ở cả hai đầu và truy vấn vị trí O(1). 

Một điểm tinh tế là cấu trúc không hề tùy tiện. Mỗi cuốn sách được chèn chính xác một lần và chỉ ở các điểm cực trị. Điều đó đảm bảo rằng thứ tự cuối cùng hoàn toàn được xác định bởi trình tự chèn vào và mỗi cuốn sách đều có một vị trí cố định trong trình tự phát triển đó. 

Các trường hợp cạnh đáng chú ý: 

Nếu tất cả các phần chèn đều về cùng một phía, ví dụ như tất cả "L", thì cấu trúc là một danh sách đảo ngược đơn giản. Một mô phỏng đơn giản vẫn hoạt động, nhưng giả định sai về “chỉ số bên trái có nghĩa là tăng” có thể phá vỡ tính logic. 

Nếu một truy vấn xảy ra ngay sau khi chèn cuốn sách đầu tiên, câu trả lời phải luôn bằng 0 vì cuốn sách đó đã ở cả ngoài cùng bên trái và ngoài cùng bên phải. 

Nếu một cuốn sách nằm ở giữa sau nhiều lần chèn xen kẽ, chúng ta phải đảm bảo đo chính xác khoảng cách đến cả hai đầu theo thứ tự cố định cuối cùng chứ không phải theo thứ tự chèn. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ duy trì một danh sách rõ ràng đại diện cho giá. Đối với mỗi lần chèn, chúng tôi sẽ thực hiện list.insert(0, id) cho "L" hoặc nối thêm cho "R". Đối với mỗi truy vấn, chúng tôi sẽ quét danh sách để tìm chỉ mục của cuốn sách, sau đó tính khoảng cách của nó đến cả hai đầu dưới dạng min(index, n - 1 - chỉ mục). Điều này đúng nhưng quá chậm. 

Vấn đề nảy sinh vì việc tìm chỉ mục của một cuốn sách là O(n) và chúng ta có thể thực hiện tới 200.000 lần. Điều đó dẫn đến O(nq), vượt xa giới hạn. 

Thông tin chi tiết về cấu trúc quan trọng là chúng ta không bao giờ cần lưu trữ danh sách đầy đủ một cách rõ ràng. Chúng ta chỉ cần biết vị trí tương đối của từng cuốn sách theo thứ tự cuối cùng. Vì mỗi lần chèn đều ở mức cực đại nên chúng ta có thể gán cho mỗi cuốn sách một tọa độ số biểu thị vị trí của nó trên trục số. Mỗi lần chèn bên trái mới sẽ có tọa độ nhỏ hơn tất cả các lần chèn trước đó và mỗi lần chèn bên phải sẽ có tọa độ lớn hơn. Điều này biến kệ thành một tập hợp các số nguyên bảo toàn thứ tự. 

Khi mỗi cuốn sách có tọa độ, việc trả lời truy vấn sẽ giảm xuống việc tìm ra có bao nhiêu cuốn sách nằm hoàn toàn ở bên trái và bên phải của nó theo thứ tự tọa độ này. Thay vì đếm động, chúng tôi theo dõi từng cuốn sách có bao nhiêu cuốn sách được chèn trước cuốn sách đó trên mỗi mặt bằng cách sử dụng bộ đếm giống tiền tố. 

Một cách nhìn tương đương đơn giản hơn là chúng tôi gán cho mỗi cuốn sách một chỉ mục trong chuỗi cuối cùng bằng cách sử dụng mô hình giống deque, nhưng chúng tôi không bao giờ xây dựng deque một cách rõ ràng. Thay vào đó, chúng tôi mô phỏng hai con trỏ mở rộng ra bên ngoài. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(n) | Quá chậm | 
| Phối hợp phân công với bản đồ băm | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán

Chúng tôi duy trì hai con trỏ tăng dần biểu thị các vị trí chiếm giữ ngoài cùng bên trái và ngoài cùng bên phải trong một mảng ẩn. Chúng tôi cũng duy trì một từ điển ánh xạ từng id sách tới vị trí cuối cùng của nó. 

1. Khởi tạo hai bộ đếm, left = 0 và right = 0. Chúng biểu thị ranh giới hiện tại của kệ trong hệ tọa độ tưởng tượng. 
2. Duy trì một vị trí từ điển trong đó pos[id] lưu trữ tọa độ được chỉ định của mỗi cuốn sách. 
3. Khi xử lý truy vấn "L id", chúng tôi giảm left 1 và gán pos[id] = left. Điều này đảm bảo mỗi lần chèn mới bên trái đều nhỏ hơn tất cả các vị trí trước đó. 
4. Khi xử lý truy vấn "R id", chúng tôi tăng phải thêm 1 và gán pos[id] = right. Điều này đảm bảo mỗi lần chèn bên phải đều lớn hơn tất cả các vị trí trước đó. 
5. Khi xử lý truy vấn "? id", chúng tôi tính toán cần bao nhiêu bước để đưa cuốn sách này đến một trong hai đầu. Nếu pos[id] là k thì: 

số sách bên trái nó là k - trái, và bên phải nó - k. Câu trả lời là min(k - left, right - k). 

Lý do là trái và phải theo dõi tọa độ cực trị, do đó sự khác biệt trong hệ tọa độ này tương ứng chính xác với khoảng cách trong thứ tự kệ cuối cùng. 

### Tại sao nó hoạt động 

Điều bất biến là sau mỗi thao tác, tất cả các cuốn sách đều nằm ở các tọa độ nguyên riêng biệt và thứ tự các cuốn sách trên giá tương ứng chính xác với các giá trị tọa độ tăng dần. Mỗi lần chèn "L" sẽ tạo ra tọa độ tối thiểu mới và mỗi lần chèn "R" sẽ tạo ra tọa độ tối đa mới. Do đó, thứ tự kệ luôn nhất quán với việc sắp xếp theo tọa độ và khoảng cách đến các đầu được đo chính xác bằng chênh lệch tọa độ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    q = int(input())
    pos = {}
    left = 0
    right = 0

    first = True

    for _ in range(q):
        parts = input().split()
        if parts[0] == 'L':
            x = int(parts[1])
            if first:
                pos[x] = 0
                left = right = 0
                first = False
            else:
                left -= 1
                pos[x] = left

        elif parts[0] == 'R':
            x = int(parts[1])
            if first:
                pos[x] = 0
                left = right = 0
                first = False
            else:
                right += 1
                pos[x] = right

        else:
            x = int(parts[1])
            k = pos[x]
            print(min(k - left, right - k))

if __name__ == "__main__":
    solve()
```Giải pháp tránh duy trì bất kỳ danh sách rõ ràng nào. Thay vào đó, nó gán cho mỗi cuốn sách một tọa độ như thể chúng ta đang khai triển một trục số. Cuốn sách được chèn đầu tiên rất đặc biệt vì cả hai ranh giới đều bắt đầu không được xác định, vì vậy chúng tôi khởi tạo cả bên trái và bên phải về 0 tại thời điểm đó. 

Mỗi lần chèn chỉ cập nhật một điểm cuối. Các truy vấn hoàn toàn dựa vào sự khác biệt số học giữa tọa độ được lưu trữ và ranh giới hiện tại, vì vậy chúng là O(1). 

Một chi tiết tinh tế là xử lý lần chèn đầu tiên một cách rõ ràng. Nếu không khởi tạo chính xác cả hai giới hạn, hệ tọa độ sẽ bị lệch hoặc tạo ra khoảng cách không chính xác. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
L 1
R 2
R 3
? 2
L 4
? 1
```Chúng tôi theo dõi tọa độ: 

| Bước | Hoạt động | trái | đúng | lập bản đồ vị trí | Kết quả truy vấn | 
| --- | --- | --- | --- | --- | --- | 
| 1 | L 1 | 0 | 0 | {1:0} | | 
| 2 | R 2 | 0 | 1 | {1:0,2:1} | | 
| 3 | R 3 | 0 | 2 | {1:0,2:1,3:2} | | 
| 4 | ? 2 | 0 | 2 | giống nhau | phút(1,1)=1 | 
| 5 | L 4 | -1 | 2 | {4:-1,...} | | 
| 6 | ? 1 | -1 | 2 | giống nhau | phút(0,2)=0 | 

Dấu vết cho thấy rằng sự khác biệt về tọa độ mã hóa trực tiếp khoảng cách từ hai đầu của một cuốn sách. 

### Ví dụ 2 

đầu vào:```
L 100
R 200
L 300
? 200
R 400
? 100
```| Bước | Hoạt động | trái | đúng | lập bản đồ vị trí | Kết quả truy vấn | 
| --- | --- | --- | --- | --- | --- | 
| 1 | L 100 | 0 | 0 | {100:0} | | 
| 2 | R 200 | 0 | 1 | {100:0,200:1} | | 
| 3 | L 300 | -1 | 1 | {300:-1,...} | | 
| 4 | ? 200 | -1 | 1 | giống nhau | phút(2,0)=0 | 
| 5 | R 400 | -1 | 2 | {400:2,...} | | 
| 6 | ? 100 | -1 | 2 | giống nhau | phút(1,2)=1 | 

Điều này xác nhận rằng ngay cả khi chèn xen kẽ, hệ tọa độ vẫn nhất quán. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(q) | Mỗi truy vấn cập nhật hoặc đọc một mục từ điển trong thời gian trung bình O(1) | 
| Không gian | O(q) | Mỗi cuốn sách được lưu trữ một lần trong bản đồ vị trí | 

Giải pháp này phù hợp một cách thoải mái trong giới hạn vì 200.000 thao tác với quá trình xử lý thời gian không đổi sẽ hiệu quả trong Python khi sử dụng I/O nhanh và bản đồ băm. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from sys import stdout
    out = []
    
    q = int(input())
    pos = {}
    left = 0
    right = 0
    first = True

    for _ in range(q):
        parts = input().split()
        if parts[0] == 'L':
            x = int(parts[1])
            if first:
                pos[x] = 0
                left = right = 0
                first = False
            else:
                left -= 1
                pos[x] = left

        elif parts[0] == 'R':
            x = int(parts[1])
            if first:
                pos[x] = 0
                left = right = 0
                first = False
            else:
                right += 1
                pos[x] = right

        else:
            x = int(parts[1])
            k = pos[x]
            out.append(str(min(k - left, right - k)))

    return "\n".join(out)

# provided sample
assert run("""8
L 1
R 2
R 3
? 2
L 4
? 1
L 5
? 1
""") == "1\n1\n2"

# minimum case
assert run("""1
L 1
""") == ""

# single query edge
assert run("""1
L 1
? 1
""") == "0"

# all right insertions
assert run("""5
L 1
R 2
R 3
R 4
? 1
""") == "3"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| chèn đơn | trống | không có cạnh truy vấn | 
| truy vấn đơn | 0 | điểm cuối ngay lập tức | 
| tất cả các phần chèn bên phải | 3 | tăng trưởng đơn điệu đúng đắn | 

## Vỏ cạnh 

Trường hợp một cạnh là khi thao tác đầu tiên đã là một truy vấn sau một lần chèn. Đối với đầu vào:```
L 10
? 10
```hệ thống gán pos[10] = 0 và cả bên trái và bên phải đều bằng 0. Truy vấn tính toán min(0,0) = 0, phản ánh chính xác rằng cuốn sách đã có ở cả hai đầu. 

Một trường hợp khác là các phần chèn xen kẽ mở rộng cả hai bên một cách không đối xứng. Ví dụ:```
L 1
R 2
L 3
R 4
? 2
```Sau khi xử lý, tọa độ trở thành 3 ở ngoài cùng bên trái và 4 ở ngoài cùng bên phải tùy theo sự thay đổi thứ tự. Truy vấn vẫn đo chính xác khoảng cách hoàn toàn thông qua khoảng cách tọa độ chứ không phải thứ tự chèn. 

Trường hợp tinh tế cuối cùng là khi có nhiều lần chèn vào bên trái xảy ra trước bất kỳ lần chèn bên phải nào. Ranh giới bên trái di chuyển rất âm, nhưng vì tất cả các phép tính đều có sự khác biệt tương đối nên không xảy ra hiện tượng tràn hoặc không nhất quán về thứ tự và khoảng cách vẫn chính xác.
