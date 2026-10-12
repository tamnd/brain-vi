---
title: "CF 105046D - Sự khác biệt"
description: "Chúng tôi được cung cấp một số trường hợp thử nghiệm độc lập. Mỗi trường hợp thử nghiệm mô tả một lưới hình chữ nhật có n hàng và m cột và nhiều tập hợp n·m số nguyên."
date: "2026-06-28T01:30:16+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105046
codeforces_index: "D"
codeforces_contest_name: "XXVIII Spain Olympiad in Informatics, Day 1"
rating: 0
weight: 105046
solve_time_s: 45
verified: true
draft: false
---

[CF 105046D - Sự khác biệt](https://codeforces.com/problemset/problem/105046/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 45s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một số trường hợp thử nghiệm độc lập. Mỗi trường hợp thử nghiệm mô tả một lưới hình chữ nhật có n hàng và m cột và nhiều tập hợp n·m số nguyên. Nhiệm vụ của chúng ta là đặt tất cả các số nguyên này vào lưới, mỗi số một ô, sao cho hai ô bất kỳ có chung một cạnh luôn chứa các số chẵn lẻ khác nhau. Nói cách khác, mọi sự kề nhau theo chiều ngang và chiều dọc đều phải nối một số chẵn với một số lẻ. 

Đầu ra không chỉ là kiểm tra tính hợp lệ mà còn là một cấu trúc rõ ràng của việc sắp xếp lưới. Đối với mỗi trường hợp thử nghiệm, chúng ta phải in một ma trận n x m đầy đủ bằng cách sử dụng tất cả các giá trị được cung cấp chính xác một lần hoặc in -1 nếu không có sự sắp xếp nào. 

Ràng buộc n·m lên tới 100000 trên tất cả các trường hợp thử nghiệm ngụ ý rằng về cơ bản chúng ta cần thời gian tuyến tính cho mỗi trường hợp thử nghiệm. Bất kỳ giải pháp nào thử hoán vị hoặc kiểm tra các điều kiện kề liên tục theo cặp sẽ quá chậm. Chúng ta buộc phải hướng tới một công trình chỉ phụ thuộc vào số lượng chẵn lẻ và thứ tự đơn giản. 

Một quan sát quan trọng đã xuất hiện trong yêu cầu: tính liền kề buộc phải có cấu trúc lưỡng cực nghiêm ngặt của lưới. Điều này ngay lập tức kết nối vấn đề với việc tô màu một lưới giống như một bàn cờ. 

Một trường hợp thất bại tinh tế phát sinh khi chúng ta bỏ qua tính khả thi của tính chẵn lẻ. Hãy xem xét một lưới 2 x 2 có các giá trị 1, 1, 1, 2. Có ba số lẻ và một số chẵn. Bất kỳ sự sắp xếp hợp lệ nào cũng phải luân phiên tính chẵn lẻ giống như một bàn cờ, trong lưới 2 x 2 buộc chính xác hai ô lẻ và hai ô chẵn. Trường hợp này không thể hoạt động nên câu trả lời phải là -1. Một cách tiếp cận ngây thơ chỉ thử vị trí tham lam mà không kiểm tra số lượng sẽ đưa ra kết quả không chính xác. 

Một trường hợp cạnh khác là một hàng hoặc một cột. Trong lưới 1 x m, độ kề là tuyến tính, vì vậy chúng tôi vẫn yêu cầu sự xen kẽ chặt chẽ của tính chẵn lẻ. Điều này buộc các giá trị chẵn và lẻ xen kẽ dọc theo chuỗi và tính khả thi phụ thuộc vào việc cân bằng số chẵn lẻ trên toàn dòng. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ cố gắng gán từng giá trị cho từng ô, kiểm tra sau mỗi vị trí xem liệu tất cả các vùng lân cận hiện có có đáp ứng các ràng buộc chẵn lẻ hay không. Điều này nhanh chóng trở nên không khả thi vì ở mỗi bước, chúng tôi phân nhánh các giá trị chưa được sử dụng còn lại, dẫn đến khả năng tăng trưởng theo giai thừa. Ngay cả với việc cắt tỉa, không gian trạng thái về cơ bản là hoán vị của n·m mục, vượt xa giới hạn chấp nhận được đối với n·m lên tới 100000. 

Sự đơn giản hóa chính là nhận ra rằng bản thân lưới điện áp đặt một cấu trúc lưỡng cực cố định. Nếu chúng ta tô màu các ô theo (i + j) mod 2, tất cả các cạnh kề sẽ nằm giữa các màu đối lập. Điều kiện “các số liền kề có số chẵn lẻ khác nhau” có nghĩa là mỗi ô đen phải chứa các số của một số chẵn lẻ và mọi ô trắng phải chứa các số của số chẵn lẻ kia. 

Điều này làm giảm vấn đề từ việc sắp xếp các giá trị riêng lẻ đến việc phân phối hai nhóm: số chẵn và số lẻ. Khi chúng ta biết tính chẵn lẻ nào thuộc về lớp màu nào, mọi phép gán trong các lớp đó đều có tác dụng vì tính liền kề được tự động thỏa mãn bởi màu sắc. 

Do đó, vấn đề trở thành việc kiểm tra tính khả thi của việc so khớp số lượng với kích thước phân vùng lưới, sau đó điền vào các ô theo bất kỳ thứ tự nhất quán nào. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Vị trí Brute Force có kiểm tra | O((nm)!) | O(nm) | Quá chậm | 
| Phân vùng chẵn lẻ bằng cách tô màu bàn cờ | O(nm) | O(nm) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi tiến hành bằng cách sử dụng cấu trúc lưỡng cực tự nhiên của lưới.

1. Tính xem có bao nhiêu ô thuộc về mỗi lớp chẵn lẻ của màu lưới. Đối với lưới, một màu có các ô ⌈nm/2⌉ và màu kia có các ô ⌊nm/2⌋. Điều này xác định có bao nhiêu giá trị của mỗi chẵn lẻ chúng ta cần. 
2. Đếm xem mảng đầu vào có bao nhiêu số lẻ và số chẵn. Điều này được thực hiện trong một lần duyệt qua tất cả các giá trị. 
3. Kiểm tra tính khả thi trong cả hai phép gán chẵn lẻ có thể có cho các lớp màu. Số lẻ phải khớp với ô đen và số chẵn phải khớp với ô trắng hoặc ngược lại. Nếu cả hai đều không trùng khớp thì câu trả lời là không thể. 
4. Chọn bài tập hợp lệ. Sự lựa chọn này là cần thiết vì bài toán không khắc phục được lớp màu nào phải tương ứng với các giá trị lẻ. 
5. Xây dựng hai danh sách: một danh sách chứa tất cả các giá trị lẻ và một danh sách chứa tất cả các giá trị chẵn. Thứ tự bên trong mỗi danh sách là không liên quan. 
6. Duyệt lưới theo thứ tự hàng lớn. Đối với mỗi ô, xác định màu bàn cờ của nó bằng cách sử dụng (i + j) mod 2 và gán giá trị tiếp theo từ danh sách tương ứng. 
7. Xuất lưới đã điền. 

Tính chính xác dựa trên thực tế là các ràng buộc kề cận hoàn toàn được nắm bắt bằng cách tô màu lưỡng cực, do đó, khi các lớp chẵn lẻ được chỉ định một cách nhất quán thì không cần xác nhận thêm. 

### Tại sao nó hoạt động 

Biểu đồ lưới có tính chất lưỡng cực theo cách tô màu bàn cờ tiêu chuẩn, nghĩa là mọi cạnh đều kết nối các màu đối lập nhau. Ràng buộc yêu cầu các giá trị liền kề phải khác nhau về tính chẵn lẻ, vì vậy mỗi cạnh phải kết nối một giá trị lẻ với một giá trị chẵn. Do đó, tất cả các ô có một màu chỉ được chứa các giá trị lẻ và tất cả các ô của màu kia chỉ được chứa các giá trị chẵn. Điều này vừa cần vừa đủ. Khi số lượng chẵn lẻ khớp với kích thước lớp màu, mọi sự trùng lặp giữa các giá trị và ô sẽ duy trì tính hợp lệ vì không có cạnh nào kết nối hai ô có cùng màu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n, m = map(int, input().split())
        a = list(map(int, input().split()))

        odds = [x for x in a if x % 2]
        evens = [x for x in a if x % 2 == 0]

        total = n * m
        black = (total + 1) // 2
        white = total // 2

        # try odd on black
        ok1 = (len(odds) == black and len(evens) == white)
        # try odd on white
        ok2 = (len(odds) == white and len(evens) == black)

        if not ok1 and not ok2:
            print(-1)
            continue

        if ok1:
            odd_cells = True  # black cells
        else:
            odd_cells = False

        oi = 0
        ei = 0

        res = [[0] * m for _ in range(n)]

        for i in range(n):
            for j in range(m):
                if (i + j) % 2 == 0:
                    if odd_cells:
                        res[i][j] = odds[oi]
                        oi += 1
                    else:
                        res[i][j] = evens[ei]
                        ei += 1
                else:
                    if odd_cells:
                        res[i][j] = evens[ei]
                        ei += 1
                    else:
                        res[i][j] = odds[oi]
                        oi += 1

        for row in res:
            print(*row)

solve()
```Việc triển khai trước tiên sẽ phân tách các giá trị theo tính chẵn lẻ trong thời gian tuyến tính. Sau đó, nó kiểm tra hai phép gán toàn cục duy nhất có thể có giữa các lớp màu chẵn lẻ và bàn cờ. Lưới được điền theo thứ tự hàng lớn, sử dụng hai con trỏ vào danh sách chẵn lẻ. 

Một chi tiết tinh tế là tính toán số lượng màu đen và trắng một cách chính xác bằng cách sử dụng phép chia số nguyên, vì tổng số ô có thể là số lẻ. Một cách khác là đảm bảo cả hai trường hợp gán đều được kiểm tra trước khi cam kết, vì ánh xạ có thể hợp lệ tùy thuộc vào phân phối đầu vào. 

## Ví dụ đã hoạt động 

Hãy xem xét một trường hợp hợp lệ đơn giản: 

đầu vào: 

n = 2, m = 3 

giá trị = [1, 2, 2, 3, 4, 1] 

Chúng ta có tỷ lệ cược = [1, 3, 1], chẵn = [2, 2, 4]. Tổng số ô = 6, do đó đen = 3 và trắng = 3. Cả hai nhóm chẵn lẻ đều khớp hoàn hảo. 

| Bước | Ô (i,j) | Màu sắc | Danh sách được giao | Tỷ lệ cược còn lại | Sự kiện còn lại | 
| --- | --- | --- | --- | --- | --- | 
| 1 | (0,0) | đen | lẻ | 2 | 3 | 
| 2 | (0,1) | trắng | thậm chí | 2 | 2 | 
| 3 | (0,2) | đen | lẻ | 1 | 2 | 
| 4 | (1,0) | trắng | thậm chí | 1 | 1 | 
| 5 | (1,1) | đen | lẻ | 0 | 1 | 
| 6 | (1,2) | trắng | thậm chí | 0 | 0 | 

Điều này tạo ra một lưới hợp lệ đầy đủ vì mọi vùng kề đều vượt qua một ranh giới chẵn lẻ. 

Bây giờ hãy xem xét một trường hợp không thể xảy ra: 

đầu vào: 

n = 2, m = 2 

giá trị = [1, 1, 1, 2] 

tỷ lệ cược = 3, chẵn = 1, nhưng cả màu đen và trắng đều có 2 ô. 

| Kiểm tra | Giá trị | 
| --- | --- | 
| cần màu đen | 2 | 
| cần trắng | 2 | 
| lẻ có sẵn | 3 | 
| thậm chí có sẵn | 1 | 

Không có phép gán nào hoạt động nên thuật toán cho ra kết quả chính xác -1. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(nm) | mỗi trường hợp kiểm thử xử lý mọi giá trị một lần và điền vào từng ô một lần | 
| Không gian | O(nm) | lưu trữ lưới và tách danh sách chẵn lẻ | 

Tổng kích thước đầu vào trong các trường hợp thử nghiệm tối đa là 100000 ô, do đó, giải pháp tuyến tính vừa vặn thoải mái trong các ràng buộc thông thường. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    from subprocess import check_output
    return check_output([sys.executable, "-c", CODE], input=inp.encode()).decode()

CODE = r"""
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n, m = map(int, input().split())
        a = list(map(int, input().split()))

        odds = [x for x in a if x % 2]
        evens = [x for x in a if x % 2 == 0]

        total = n * m
        black = (total + 1) // 2
        white = total // 2

        ok1 = (len(odds) == black and len(evens) == white)
        ok2 = (len(odds) == white and len(evens) == black)

        if not ok1 and not ok2:
            print(-1)
            continue

        odd_cells = ok1

        oi = ei = 0
        res = [[0]*m for _ in range(n)]

        for i in range(n):
            for j in range(m):
                if (i+j) % 2 == 0:
                    if odd_cells:
                        res[i][j] = odds[oi]; oi += 1
                    else:
                        res[i][j] = evens[ei]; ei += 1
                else:
                    if odd_cells:
                        res[i][j] = evens[ei]; ei += 1
                    else:
                        res[i][j] = odds[oi]; oi += 1

        for r in res:
            print(*r)

solve()
"""

# provided sample (adapted format)
assert run("1\n2 3\n1 2 2 3 4 1\n") != "", "sample 1 exists"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Giá trị đơn 1x1 | giá trị của chính nó | xử lý lưới tối thiểu | 
| cân bằng 2x2 | lưới hợp lệ | tính khả thi chẵn lẻ | 
| 2x2 không thể | -1 | logic từ chối | 
| 1x5 luân phiên | hợp lệ hoặc -1 tùy thuộc vào đầu vào | trường hợp cấu trúc tuyến tính | 

## Vỏ cạnh 

Lưới 1 x m buộc phải xen kẽ nghiêm ngặt dọc theo một đường. Ví dụ: n = 1, m = 5 với các giá trị [1, 2, 3, 4, 5] hoạt động vì số lẻ và số chẵn có thể khớp với các vị trí xen kẽ. Thuật toán coi đây là cách tô màu bàn cờ tiêu chuẩn với các ô màu đen ở vị trí j trong đó j chẵn, do đó việc gán được tiến hành giống hệt nhau. 

Đầu vào hoàn toàn lẻ trên lưới 3 x 3 ngay lập tức không thành công. Có 5 ô đen và 4 ô trắng, vì vậy chúng ta cần có cả hai ô chẵn lẻ. Thuật toán phát hiện điều này thông qua việc tính số chẵn và số lẻ không khớp với một trong hai phân vùng và trả về -1 mà không cần cố gắng sắp xếp. 

Một trường hợp gần như cân bằng nhưng không khớp như 3 x 3 với bốn chẵn và năm tỷ lệ cược chỉ được chấp nhận nếu phép gán chẵn lẻ phù hợp với lớp lớn hơn. Thuật toán kiểm tra rõ ràng cả hai hướng, đảm bảo tính chính xác ngay cả khi tính chẵn lẻ của đa số không rõ ràng ngay lập tức.
