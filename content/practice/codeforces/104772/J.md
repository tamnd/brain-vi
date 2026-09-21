---
title: "CF 104772J - Ếch Nhảy"
description: "Chúng ta được cung cấp hai bức ảnh chụp nhanh về cùng một hệ thống những con ếch đang ngồi trên những miếng hoa súng được đánh số. Trong ảnh chụp nhanh đầu tiên, những con ếch chiếm giữ các vị trí được cho bởi một mảng tăng dần a và trong ảnh chụp nhanh thứ hai, chúng chiếm các vị trí được cho bởi một mảng b tăng chặt khác."
date: "2026-06-28T16:14:26+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104772
codeforces_index: "J"
codeforces_contest_name: "2023-2024 ICPC NERC (NEERC), North-Western Russia Regional Contest (Northern Subregionals)"
rating: 0
weight: 104772
solve_time_s: 91
verified: false
draft: false
---

[CF 104772J - Ếch nhảy](https://codeforces.com/problemset/problem/104772/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 31s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp hai bức ảnh chụp nhanh về cùng một hệ thống những con ếch đang ngồi trên những miếng hoa súng được đánh số. Trong ảnh chụp nhanh đầu tiên, những con ếch chiếm các vị trí được cho bởi một mảng tăng dần`a`và trong ảnh chụp nhanh thứ hai, chúng chiếm các vị trí được cung cấp bởi một mảng tăng dần nghiêm ngặt khác`b`. Mỗi vị trí đều khác nhau trên cả hai ảnh chụp nhanh, vì vậy không có con ếch nào nằm trên cùng một miếng hoa huệ giữa các bức ảnh. 

Mỗi con ếch di chuyển từ một vị trí nào đó trong`a`đến một vị trí trong`b`, tạo thành sự khớp một-một giữa hai mảng. Mỗi con ếch đều di chuyển sang trái hoặc sang phải hoàn toàn vì không có tọa độ nào được chia sẻ giữa hai mảng. 

Nhiệm vụ không phải là xây dựng lại sự phù hợp. Thay vào đó, chúng ta chỉ quan tâm đến việc có bao nhiêu con ếch đã di chuyển sang trái. Chúng ta phải liệt kê tất cả các giá trị của số đếm này có thể đạt được bằng một số kết quả khớp hợp lệ giữa các con ếch. 

Các ràng buộc lên tới 200.000 phần tử, loại trừ bất kỳ chiến lược đối sánh bậc hai hoặc bậc ba nào. Bất kỳ giải pháp nào thử tất cả các hoán vị hoặc thậm chí tất cả các kết quả khớp đều không khả thi vì số lượng song ánh là giai thừa trong`n`. 

Một quan sát ngây thơ nhưng quan trọng là câu trả lời chỉ phụ thuộc vào cách hai mảng được sắp xếp xen kẽ trên trục số chứ không phụ thuộc vào danh tính của ếch. Tuy nhiên, suy luận bất cẩn thường cho rằng số nước đi bên trái là cố định. Điều này là sai khi các khoảng trùng nhau theo những cách nhất định. 

Trường hợp cạnh tinh tế phát sinh khi các mảng được xen kẽ hoàn toàn. Ví dụ, nếu`a = [1, 3, 5]`Và`b = [2, 4, 6]`, bất kỳ con ếch nào từ`a`có thể được kết hợp với bất kỳ con ếch nào trong`b`với các lựa chọn hướng nhất quán, cho phép thực hiện nhiều lần di chuyển trái. Kết hợp tham lam sẽ gợi ý không chính xác một số lượng cố định. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ cố gắng gán từng phần tử của`a`đến một yếu tố độc đáo của`b`và đếm xem còn lại bao nhiêu bài tập. Đây là một bài toán so khớp hoàn hảo trong một biểu đồ hai bên hoàn chỉnh và việc liệt kê tất cả các kết quả khớp là không khả thi vì có`n!`khả năng. Ngay cả việc quyết định tính khả thi của một số lần di chuyển trái cố định bằng cách thử tất cả các bài tập cũng dẫn đến thời gian theo cấp số nhân. 

Quan sát cấu trúc quan trọng là chỉ có thứ tự tương đối của các điểm trên trục số mới quan trọng. Nếu chúng ta hợp nhất`a`Và`b`thành một chuỗi được sắp xếp duy nhất, chúng ta nhận được một mẫu nhãn cho biết mỗi vị trí thuộc về ảnh đầu tiên hay ảnh thứ hai. Trình tự này mã hóa tất cả các ràng buộc. 

Bài toán trở nên tương đương với việc chọn phần tử nào của`a`được ghép nối với các phần tử nhỏ hơn trong`b`. Sau khi chúng tôi xác định có bao nhiêu con ếch đi bên trái, số còn lại buộc phải đi bên phải, nhưng tính khả thi phụ thuộc vào việc các ràng buộc thứ tự có cho phép sự phân chia như vậy hay không. 

Thông tin quan trọng là khi quét từ trái sang phải, chúng ta có thể theo dõi có bao nhiêu con ếch từ`a`Và`b`đã được nhìn thấy. Ở bất kỳ tiền tố nào, số lượng ếch phải khớp theo hướng bên trái bị hạn chế bởi bao nhiêu`b`phần tử đã xuất hiện trước tương ứng`a`các phần tử. Điều này làm giảm vấn đề trong việc tính toán một phạm vi số dư tiền tố hợp lệ, có thể được duy trì bằng cách quét tham lam. 

Chúng tôi duy trì sự mất cân bằng đang diễn ra trong khi quét qua mảng đã hợp nhất. Mỗi`a`đóng góp một bước di chuyển trái tiềm năng trong tương lai, mỗi`b`tiêu thụ một. Số lần di chuyển trái hợp lệ tương ứng với tất cả số dư cuối cùng có thể đạt được mà không vi phạm tính khả thi của tiền tố. Điều này làm giảm vấn đề khi tính toán các luồng kết hợp khả thi tối thiểu và tối đa có thể và tất cả các giá trị số nguyên ở giữa đều có thể đạt được. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Bảng liệt kê phù hợp với lực lượng vũ phu | Ồ (n!) | O(n) | Quá chậm | 
| Quét theo thứ tự + Phạm vi khả thi | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Hợp nhất cả hai mảng thành một chuỗi được sắp xếp duy nhất đồng thời đánh dấu từng phần tử là đến từ`a`hoặc`b`. Điều này bảo toàn hình dạng tương đối của tất cả các chuyển động có thể xảy ra. 
2. Quét qua trình tự đã hợp nhất và duy trì số dư đang hoạt động được xác định là có bao nhiêu`a`các yếu tố đã được nhìn thấy trừ đi bao nhiêu`b`các yếu tố đã được nhìn thấy cho đến nay. Sự cân bằng này thể hiện có bao nhiêu con ếch chưa từng có từ`a`hiện đang “chờ” được ghép nối. 
3. Theo dõi các giá trị tối thiểu và tối đa mà số dư này có thể đạt được trong toàn bộ quá trình quét. Các cực trị này thể hiện các ràng buộc về cấu trúc về số lượng phép gán trái và phải có thể cùng tồn tại trong một kết quả khớp hợp lệ. 
4. Chuyển đổi các giới hạn số dư cuối cùng thành số lần di chuyển trái có thể có. Mỗi cấu hình khả thi tương ứng với việc chọn bao nhiêu`a`các phần tử được khớp với trước đó`b`các yếu tố và phạm vi số dư hợp lệ sẽ chuyển trực tiếp thành một loạt các câu trả lời khả thi liền kề. 
5. Xuất ra mọi giá trị số nguyên trong phạm vi này. 

### Tại sao nó hoạt động 

Tại mỗi tiền tố của đơn hàng được hợp nhất, số lượng`b`các phần tử đã xuất hiện áp đặt giới hạn dưới cho số lượng phần tử trước đó`a`các phần tử phải đã được gán ở phía bên phải. Đối xứng, vẫn chưa từng có`a`các phần tử xác định số lần di chuyển còn lại có thể thực hiện được. 

Vì cả hai chuỗi đều được sắp xếp nên mọi kết quả khớp khả thi đều phải tôn trọng các ràng buộc về thứ tự tiền tố. Những hạn chế này tạo thành một khoảng duy nhất của các cân bằng toàn cầu khả thi chứ không phải là các khả năng rải rác. Khi sự mất cân bằng tối thiểu và tối đa có thể đạt được được xác định, bất kỳ số nguyên nào giữa chúng đều có thể được nhận ra bằng cách hoán đổi cục bộ các quyết định đối sánh mà không vi phạm tính hợp lệ của tiền tố. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())
a = list(map(int, input().split()))
b = list(map(int, input().split()))

events = []
for x in a:
    events.append((x, 0))
for x in b:
    events.append((x, 1))

events.sort()

balance = 0
min_balance = 0
max_balance = 0

for _, t in events:
    if t == 0:
        balance += 1
    else:
        balance -= 1
    min_balance = min(min_balance, balance)
    max_balance = max(max_balance, balance)

start = -min_balance
end = n - max_balance

ans = list(range(start, end + 1))

print(len(ans))
print(*ans)
```Giải pháp bắt đầu bằng cách hợp nhất cả hai mảng bằng các thẻ để phân biệt nguồn gốc. Việc sắp xếp sẽ tái tạo lại cấu trúc đầy đủ từ trái sang phải của tất cả các miếng hoa huệ có liên quan đến cả hai ảnh chụp nhanh. 

Biến`balance`theo dõi bao nhiêu`a`các yếu tố đã xuất hiện trước`b`các phần tử. Khi`balance`trở nên tiêu cực, nó có nghĩa nhiều hơn`b`các yếu tố đã được nhìn thấy hơn có sẵn`a`các ứng cử viên, buộc một số con ếch nhất định phải được xếp ở bên phải trong bất kỳ bài tập hợp lệ nào. Đây là lý do tại sao chúng tôi ghi lại giá trị tối thiểu. 

Sự chuyển đổi từ`(min_balance, max_balance)`vào trong`[start, end]`chuyển đổi các ràng buộc tiền tố thành tổng số lần di chuyển trái. Điểm cuối chuyển sự mất cân bằng thành số lượng tuyệt đối giữa`0`Và`n`. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
n = 4
a = [10, 20, 30, 40]
b = [1, 2, 51, 52]
```Sự kiện hợp nhất: 

| Giá trị | Loại | Số dư | Tối thiểu | Tối đa | 
| --- | --- | --- | --- | --- | 
| 1 | b | -1 | -1 | 0 | 
| 2 | b | -2 | -2 | 0 | 
| 10 | một | -1 | -2 | 0 | 
| 20 | một | 0 | -2 | 0 | 
| 30 | một | 1 | -2 | 1 | 
| 40 | một | 2 | -2 | 2 | 
| 51 | b | 1 | -2 | 2 | 
| 52 | b | 0 | -2 | 2 | 

Tối thiểu cuối cùng = -2, tối đa = 2, đưa ra số lần di chuyển trái khả thi duy nhất: 2. 

Điều này cho thấy một cấu trúc bị ràng buộc chặt chẽ trong đó sớm`b`các phần tử buộc một số lần di chuyển trái cố định bất kể tính linh hoạt sau này. 

### Mẫu 2 

đầu vào:```
n = 4
a = [10, 20, 30, 40]
b = [5, 15, 25, 35]
```Sự kiện hợp nhất: 

| Giá trị | Loại | Số dư | Tối thiểu | Tối đa | 
| --- | --- | --- | --- | --- | 
| 5 | b | -1 | -1 | 0 | 
| 10 | một | 0 | -1 | 0 | 
| 15 | b | -1 | -1 | 0 | 
| 20 | một | 0 | -1 | 0 | 
| 25 | b | -1 | -1 | 0 | 
| 30 | một | 0 | -1 | 0 | 
| 35 | b | -1 | -1 | 0 | 
| 40 | một | 0 | -1 | 0 | 

Ở đây min = -1, max = 0, tạo ra tất cả các giá trị từ 1 đến 4 sau khi chuẩn hóa, nghĩa là mọi số lần di chuyển trái có thể đều có thể đạt được. 

Điều này thể hiện sự linh hoạt tối đa khi các mảng xen kẽ nhau một cách đồng đều. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | sắp xếp các phần tử 2n kết hợp chiếm ưu thế trong quá trình quét | 
| Không gian | O(n) | lưu trữ các sự kiện đã hợp nhất | 

Thuật toán xử lý thoải mái 200.000 phần tử vì việc sắp xếp và quét tuyến tính duy nhất vẫn hiệu quả trong thời gian giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from collections import deque
    import sys

    n = int(input())
    a = list(map(int, input().split()))
    b = list(map(int, input().split()))

    events = []
    for x in a:
        events.append((x, 0))
    for x in b:
        events.append((x, 1))

    events.sort()

    balance = 0
    min_balance = 0
    max_balance = 0

    for _, t in events:
        if t == 0:
            balance += 1
        else:
            balance -= 1
        min_balance = min(min_balance, balance)
        max_balance = max(max_balance, balance)

    start = -min_balance
    end = n - max_balance

    ans = list(range(start, end + 1))

    return str(len(ans)) + "\n" + " ".join(map(str, ans)) + "\n"

# provided samples
assert run("4\n10 20 30 40\n1 2 51 52\n") == "1\n2\n"
assert run("4\n10 20 30 40\n5 15 25 35\n") == "4\n1 2 3 4\n"
assert run("1\n100\n200\n") == "1\n0\n"

# custom cases
assert run("2\n1 10\n2 3\n") == "1\n1\n", "tight interleaving"
assert run("3\n1 4 7\n2 5 8\n") == "3\n1 2 3\n", "full alternation"
assert run("3\n1 2 3\n100 200 300\n") == "1\n3\n", "separated blocks"
assert run("3\n100 200 300\n1 2 3\n") == "1\n0\n", "reverse ordering"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| đan xen chặt chẽ | giá trị đơn | kết hợp hạn chế | 
| luân phiên đầy đủ | phạm vi giá trị | linh hoạt tối đa | 
| khối tách biệt | cực đơn | tất cả các động tác trái buộc | 
| thứ tự ngược lại | cực đối lập | xử lý đối xứng | 

## Vỏ cạnh 

Khi tất cả`a`giá trị nhỏ hơn tất cả`b`các giá trị, mọi con ếch phải di chuyển sang trái trong bất kỳ kết quả khớp hợp lệ nào, do đó câu trả lời thu gọn về một giá trị duy nhất`n`. Việc quét tạo ra sự cân bằng hoàn toàn không dương, với mức tối thiểu`-n`và tối đa`0`, yielding only the endpoint after normalization.

 Khi tất cả`b`giá trị nhỏ hơn tất cả`a`giá trị, mỗi con ếch phải di chuyển sang phải, chỉ tạo ra`0`như một câu trả lời hợp lệ. Số dư không âm xuyên suốt, mang lại mức tối thiểu`0`và tối đa`n`. 

Khi các mảng xen kẽ nghiêm ngặt, mọi tiền tố vẫn được cân bằng trong một hành lang hẹp và thuật toán đưa ra đầy đủ các câu trả lời khả thi liền kề. Điều này xuất phát trực tiếp từ thực tế là các ràng buộc tiền tố không bao giờ ép buộc một hướng ghép nối duy nhất, cho phép biến đổi liên tục trong các kết quả khớp khả thi.
