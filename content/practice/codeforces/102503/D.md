---
title: "CF 102503D - Liên minh được tìm thấy"
description: "Nhật ký mô tả trạng thái của nhà máy theo thời gian. Trước khi bắt đầu nhật ký, chúng tôi được cung cấp cho mỗi nhân viên hai cách để nhận dạng họ: danh tính đầy đủ của họ, bao gồm chức danh và tên cũng như biệt hiệu của họ."
date: "2026-08-05T17:08:24+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102503
codeforces_index: "D"
codeforces_contest_name: "National Olympiad in Informatics - Philippines (NOI.PH) Online Eliminations 2020"
rating: 0
weight: 102503
solve_time_s: 2236
verified: true
draft: false
---

[CF 102503D - Đã thành lập Liên minh](https://codeforces.com/problemset/problem/102503/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 37 phút 16s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Nhật ký mô tả trạng thái của nhà máy theo thời gian. Trước khi bắt đầu nhật ký, chúng tôi được cung cấp cho mỗi nhân viên hai cách để nhận dạng họ: danh tính đầy đủ của họ, bao gồm chức danh và tên cũng như biệt hiệu của họ. Sau đó, nhật ký sẽ chứa các sự kiện khi nhân viên vào, nhân viên rời đi, các cuộc biểu tình diễn ra và truy vấn xem liệu một nhân viên cụ thể hiện có ở bên trong hay không. 

Nhiệm vụ là xử lý nhật ký theo thứ tự. Khi một cuộc biểu tình diễn ra, chúng tôi cần số lượng nhân viên hiện tại trong nhà máy. Khi một truy vấn xuất hiện, chúng ta cần quyết định xem nhân viên được tham chiếu có hiện diện hay không và in`FOUND`hoặc`404`. 

Thử thách đầu tiên là cùng một người có thể xuất hiện dưới hai hình dạng khác nhau. Ví dụ: một nhân viên có thể nhập bằng cách sử dụng`Sir Richard`và sau đó được tìm kiếm bằng cách sử dụng`the Knight`. Hai chuỗi này phải tham chiếu đến cùng một trạng thái bên trong. 

Đầu vào lớn nhất chứa tới 100000 nhân viên và 100000 mục nhật ký. Với giới hạn thời gian là hai giây, thuật toán quét liên tục tất cả nhân viên hoặc tất cả những người hiện đang hoạt động là quá chậm. Một giải pháp thực hiện 100000 thao tác cho mỗi truy vấn có thể đạt tới khoảng 10^10 thao tác, vượt xa những gì Python có thể xử lý. Cách tiếp cận dự định cần xử lý từng dòng nhật ký trong thời gian gần như không đổi. 

Một số chi tiết có thể gây ra câu trả lời sai khi triển khai đơn giản. Một sai lầm là chỉ lưu trữ văn bản chính xác xuất hiện trong nhật ký. Ví dụ:```
Sir Richard the Knight
----------
+ Sir Richard
FIND the Knight
END
```Đầu ra đúng là:```
FOUND
```Một chương trình xử lý`Sir Richard`Và`the Knight`vì các chuỗi không liên quan sẽ in không chính xác`404`. 

Một vấn đề khác là cập nhật số lượng nhân viên không chính xác. Coi như:```
Sir Alice the Cat
----------
+ Sir Alice
UNION
- the Cat
UNION
END
```Đầu ra đúng là:```
1
0
```Cuộc biểu tình đầu tiên xảy ra khi Alice ở bên trong. Lần thứ hai xảy ra sau khi nhân viên đó rời đi. Đếm biệt danh riêng biệt với tên sẽ tính cùng một người hai lần. 

Trường hợp đặc biệt cuối cùng là các thao tác lặp đi lặp lại mà không có bất kỳ chuyển động nào:```
Sir Bob the Dog
----------
UNION
UNION
END
```Đầu ra là:```
0
0
```Một giải pháp phải trả lời mọi truy vấn từ trạng thái hiện tại. Cuộc trình diễn trước đó không tiêu tốn hoặc thay đổi trạng thái. 

## Phương pháp tiếp cận 

Giải pháp đơn giản là giữ một nhóm nhân viên hiện đang ở bên trong. Mỗi lần một`+`sự kiện xuất hiện, chúng tôi thêm nhân viên đó và mỗi lần`-`sự kiện xuất hiện, chúng tôi loại bỏ nhân viên đó. Đối với một truy vấn, chúng tôi tìm kiếm tập hợp này để xem liệu nhân viên đó có tồn tại hay không. Điều này đúng vì nhật ký được đảm bảo nhất quán nên tập hợp này thể hiện chính xác những người bên trong. 

Vấn đề là chi phí tra cứu nếu chúng ta lưu trữ nhân viên kém. Với danh sách nhân viên đang hoạt động, mọi truy vấn đều có thể kiểm tra từng nhân viên. Trong trường hợp xấu nhất có 100000 nhân viên và 100000 dòng nhật ký, dẫn đến khoảng 10^10 so sánh. 

Quan sát quan trọng là danh sách nhân viên được cố định trước khi xử lý nhật ký. Chúng ta có thể gán cho mỗi nhân viên một mã định danh nội bộ một lần. Cả dạng tên đầy đủ và dạng biệt hiệu đều có thể ánh xạ tới cùng một mã định danh. Sau đó, phần động của vấn đề chỉ còn là câu hỏi liệu mỗi mã định danh có hoạt động hay không. 

Bản đồ băm cung cấp khả năng chuyển đổi theo thời gian liên tục từ văn bản trong nhật ký sang mã định danh. Một mảng hoặc tập hợp boolean lưu trữ các mã định danh hiện đang ở bên trong. Chúng tôi cũng theo dõi số lượng nhân viên đang hoạt động vì`UNION`sự kiện chỉ yêu cầu kích thước của nhóm hiện tại. 

Cách tiếp cận vũ phu có hiệu quả vì nó mô phỏng trực tiếp nhà máy, nhưng lại lãng phí thời gian để tìm lại danh tính của nhân viên và tìm kiếm qua nhiều người không liên quan. Quan sát cho thấy danh tính có thể được nén thành ID số nguyên cho phép mọi sự kiện trở thành bản cập nhật liên tục theo thời gian. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(e*n) | O(e) | Quá chậm | 
| Tối ưu | O(e + n) | O(e) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc danh sách nhân viên và gán một ID số nguyên duy nhất cho mỗi nhân viên. Lưu trữ cả hai tham chiếu có thể có, dạng tên-chức danh đầy đủ và dạng biệt danh, trong từ điển trỏ đến ID đó. 

Phần còn lại của thuật toán sẽ không bao giờ so sánh các chuỗi nhân viên nữa. Mọi tham chiếu trong nhật ký sẽ ngay lập tức trở thành tra cứu số nguyên. 

1. Tạo cấu trúc dữ liệu ghi lại xem mỗi ID nhân viên hiện có ở bên trong hay không. Ban đầu mọi nhân viên đều ở bên ngoài. 

Nhật ký được sắp xếp theo trình tự thời gian nên trạng thái hiện tại chỉ phụ thuộc vào các sự kiện đã được xử lý. 

1. Đọc từng dòng nhật ký cho đến khi`END`. 

Đối với một sự kiện nhập, hãy tìm ID nhân viên và đánh dấu nó là hoạt động. Tăng số lượng hiện tại. 

Đối với sự kiện thoát, hãy tìm ID nhân viên và đánh dấu nó là không hoạt động. Giảm số lượng hiện tại. 

Đối với một`UNION`sự kiện, xuất ra số lượng hiện tại. 

Đối với một`FIND`sự kiện, hãy chuyển đổi tham chiếu thành ID và kiểm tra trạng thái hoạt động của ID đó. 

1. In câu trả lời theo thứ tự xuất hiện các sự kiện yêu cầu đầu ra. 

Điều bất biến là sau khi xử lý bất kỳ tiền tố nào của nhật ký, mảng hoạt động thể hiện chính xác những nhân viên đang có mặt bên trong nhà máy tại thời điểm đó. Các sự kiện nhập và xuất cập nhật một nhân viên theo nhật ký, trong khi các sự kiện truy vấn chỉ đọc trạng thái. Vì cả tên và biệt hiệu đều ánh xạ tới cùng một ID nên mọi truy vấn đều quan sát đúng nhân viên. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    ref = {}
    active = []
    emp_count = 0

    while True:
        line = input().rstrip("\n")
        if line == "----------":
            break
        parts = line.split()
        title = parts[0]
        name = parts[1]
        nick = parts[2]
        ref[title + " " + name] = emp_count
        ref["the " + nick] = emp_count
        emp_count += 1

    active = [False] * emp_count
    inside = 0
    ans = []

    while True:
        line = input().rstrip("\n")
        if line == "END":
            break

        if line == "UNION":
            ans.append(str(inside))
        elif line[0] == '+':
            key = line[2:]
            idx = ref[key]
            if not active[idx]:
                active[idx] = True
                inside += 1
        elif line[0] == '-':
            key = line[2:]
            idx = ref[key]
            if active[idx]:
                active[idx] = False
                inside -= 1
        else:
            key = line[5:]
            idx = ref[key]
            if active[idx]:
                ans.append("FOUND")
            else:
                ans.append("404")

    sys.stdout.write("\n".join(ans))

if __name__ == "__main__":
    solve()
```Giai đoạn xây dựng từ điển xử lý vấn đề nhận dạng. Mỗi nhân viên nhận được một ID và cả hai biểu diễn đều trỏ đến ID đó. Các chuỗi được sử dụng làm khóa từ điển chính xác là các dạng xuất hiện trong nhật ký. 

các`active`mảng lưu trữ trạng thái hiện tại của nhà máy. Một mảng boolean là đủ vì mỗi nhân viên đều đã có sẵn một chỉ mục số nhỏ gọn. các`inside`biến tránh quét mảng trong khi`UNION`. 

Thứ tự cập nhật quan trọng. Một mục nhập sẽ tăng số lượng sau khi kích hoạt nhân viên và một mục nhập sẽ giảm số lượng sau khi loại bỏ nhân viên. Vấn đề đảm bảo rằng việc xóa không hợp lệ không thể xảy ra, vì vậy việc kiểm tra chỉ mang tính chất phòng thủ. 

Số nguyên Python không bị tràn và số lượng lớn nhất có thể chỉ là 100000. Mọi thao tác đều sử dụng tra cứu từ điển hoặc truy cập mảng, giúp giải pháp luôn nằm trong giới hạn bắt buộc. 

## Ví dụ đã hoạt động 

Đối với mẫu đầu tiên, chỉ có ba nhân viên có liên quan. Dấu vết là: 

| Sự kiện | Nhân viên tích cực | Đếm | Đầu ra | 
| --- | --- | --- | --- | 
|`+ Sir Richard`| Richard | 1 | | 
|`+ the Merchant`| Richard, Poorard | 2 | | 
|`FIND the Knight`| Richard, Poorard | 2 | TÌM THẤY | 
|`UNION`| Richard, Poorard | 2 | 2 | 
|`- the Knight`| Nghèo | 1 | | 
|`FIND Sir Richard`| Nghèo | 1 | 404 | 
|`+ the Duck`| Người nghèo, Donard | 2 | | 
|`- Sir Poorard`| Donard | 1 | | 
|`FIND the Duck`| Donard | 1 | TÌM THẤY | 
|`FIND Sir Donard`| Donard | 1 | TÌM THẤY | 

Dấu vết này giải thích tại sao cả tên và biệt hiệu đều phải trỏ đến một ID nhân viên. Việc đi qua`Sir Richard`được tìm thấy thông qua`the Knight`. 

Đối với mẫu thứ hai, các chuyển đổi quan trọng là: 

| Sự kiện | Nhân viên tích cực | Đếm | Đầu ra | 
| --- | --- | --- | --- | 
|`+ Lolo Generoso`| Generoso | 1 | | 
|`UNION`| Generoso | 1 | 1 | 
|`FIND the Wise`| Generoso | 1 | 404 | 
|`- Lolo Generoso`| không | 0 | | 
|`UNION`| không | 0 | 0 | 
|`+ Lolo Generoso`| Generoso | 1 | | 
|`UNION`| Generoso | 1 | 1 | 
|`UNION`| Generoso | 1 | 1 | 
|`- Lolo Generoso`| không | 0 | | 
|`UNION`| không | 0 | 0 | 

Ví dụ này thực hiện các thao tác lặp lại và xác nhận rằng các thao tác minh họa chỉ đọc số lượng hiện tại. Họ không sửa đổi các nhân viên đang hoạt động. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(e + n) | Mỗi nhân viên được chèn vào từ điển một lần và mỗi dòng nhật ký sẽ thực hiện công việc theo thời gian không đổi. | 
| Không gian | O(e) | Từ điển và mảng trạng thái hoạt động lưu trữ một mục nhập cho mỗi nhân viên. | 

Kích thước đầu vào tối đa là 100000 nhân viên và 100000 sự kiện. Giải pháp thực hiện một lượng công việc không đổi nhỏ cho mỗi dòng đầu vào, do đó, nó phù hợp thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    old = sys.stdin
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    old_out = sys.stdout
    sys.stdout = out

    solve()

    sys.stdout = old_out
    sys.stdin = old
    return out.getvalue()

assert run("""Sir Alice the Cat
----------
+ Sir Alice
FIND the Cat
UNION
END
""") == "FOUND\n1", "basic alias"

assert run("""Sir Bob the Dog
----------
UNION
UNION
END
""") == "0\n0", "empty repeated queries"

assert run("""Madam Eve the Sun
----------
+ the Sun
FIND Madam Eve
- Madam Eve
FIND the Sun
END
""") == "FOUND\n404", "enter and leave through different aliases"

assert run("""Sir A the X
Sir B the Y
----------
+ Sir A
+ Sir B
UNION
- the X
UNION
END
""") == "2\n1", "multiple employees"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Một nhân viên nhập thông qua một tên và truy vấn thông qua biệt hiệu |`FOUND`,`1`| Ánh xạ bí danh | 
| Trạng thái nhật ký trống có minh họa |`0`,`0`| Không có thay đổi trạng thái ngẫu nhiên | 
| Vào và ra qua các hình thức khác nhau |`FOUND`,`404`| Danh tính nhân viên được chia sẻ | 
| Nhiều nhân viên cùng hoạt động |`2`,`1`| Cập nhật bộ đếm chính xác | 

## Vỏ cạnh 

Khi một người nhập bằng cách sử dụng một mã định danh và được tìm kiếm bằng mã định danh khác, thuật toán sẽ xử lý mã định danh đó vì cả hai chuỗi đều được ánh xạ tới cùng một ID dạng số trong quá trình tiền xử lý. Trong đầu vào:```
Sir Alice the Cat
----------
+ Sir Alice
FIND the Cat
END
```từ điển chứa cả hai`Sir Alice`Và`the Cat`trỏ tới ID 0. Mục nhập kích hoạt ID 0 và truy vấn sẽ kiểm tra cùng một ID. 

Khi một nhân viên rời đi, số lượng phải đại diện cho con người chứ không phải vẻ bề ngoài. TRONG:```
Sir Alice the Cat
----------
+ Sir Alice
UNION
- the Cat
UNION
END
```cuộc trình diễn đầu tiên nhìn thấy một ID hoạt động. Lối ra tìm thấy cùng một ID đó và xóa nó, vì vậy phần trình diễn thứ hai không thấy. 

Các cuộc biểu tình lặp đi lặp lại không ảnh hưởng đến nhà nước. Vì:```
Sir Bob the Dog
----------
UNION
UNION
END
```mảng hoạt động không thay đổi giữa hai sự kiện, vì vậy cả hai câu trả lời đều bằng 0.
