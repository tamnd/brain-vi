---
title: "CF 102740B - Nhà máy nhân bản"
description: "Quá trình bắt đầu với năm người lính được nêu tên đứng xếp hàng. Một liều thuốc luôn được trao cho người lính ở tiền tuyến. Người lính đó tạo ra một bản sao giống hệt nhau và cả hai bản sao đều di chuyển về phía sau hàng đợi."
date: "2026-07-29T01:03:40+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102740
codeforces_index: "B"
codeforces_contest_name: "UTPC Contest 9-25-20 Div. 2"
rating: 0
weight: 102740
solve_time_s: 64
verified: true
draft: false
---

[CF 102740B - Nhà máy nhân bản](https://codeforces.com/problemset/problem/102740/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 4s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Quá trình bắt đầu với năm người lính được nêu tên đứng xếp hàng. Một liều thuốc luôn được trao cho người lính ở tiền tuyến. Người lính đó tạo ra một bản sao giống hệt nhau và cả hai bản sao đều di chuyển về phía sau hàng đợi. Nhiệm vụ là xác định tên người lính nhận được`n`liều thứ. Đầu vào là một vị trí duy nhất trong chuỗi liều lượng vô hạn này và đầu ra là tên tương ứng của năm người lính ban đầu. 

Ràng buộc`n <= 10^6`thay đổi cách chúng ta nên nghĩ về giải pháp. Một mô phỏng xử lý từng liều một là hoàn toàn có thể chấp nhận được vì một triệu thao tác là nhỏ đối với giới hạn hai giây. Một giải pháp cố gắng xây dựng một cấu trúc toán học lớn hoặc tái tạo lại toàn bộ hàng đợi sẽ lãng phí công sức. Chúng ta chỉ cần duy trì thứ tự hiện tại của hàng đợi và thực hiện một thao tác cho mỗi liều. 

Các trường hợp chính đều xuất phát từ sự hiểu lầm khi một người lính xuất hiện trở lại. Một sai lầm phổ biến là cho rằng trình tự chỉ đơn giản lặp lại sau mỗi năm vị trí. Ví dụ:```
Input:
6
```Đầu ra đúng là:```
Ace
```Sau năm liều đầu tiên, mỗi người lính ban đầu đã được nhân bản một lần, vì vậy hàng đợi là:```
Ace, Ace, Bolt, Bolt, Cameron, Cameron, Doom, Doom, Echo, Echo
```Liều thứ sáu lại đến với Ace. Cách tiếp cận theo chu trình lặp lại sẽ tạo ra Bolt không chính xác. 

Một trường hợp ranh giới khác là đầu vào có thể có đầu tiên:```
Input:
1
```Đầu ra đúng là:```
Ace
```Không có trạng thái hàng đợi trước đó để xem xét, vì vậy người đầu tiên phải được xử lý chính xác mà không có bất kỳ lỗi lập chỉ mục nào. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là mô phỏng hàng đợi. Chúng tôi giữ trật tự hiện tại của binh lính. Đối với mỗi liều, chúng tôi loại bỏ người lính đầu tiên, kiểm tra xem đây có phải là vị trí được yêu cầu hay không, sau đó gắn hai bản sao của người lính đó vào phía sau. Việc này tuân theo quy trình một cách chính xác nên câu trả lời được đảm bảo là chính xác. 

Lý do điều này hoạt động hiệu quả là giới hạn đầu vào chỉ là một triệu thao tác. Kích thước hàng đợi sau tất cả các hoạt động tối đa là năm binh sĩ ban đầu cộng với một triệu bản sao mới, do đó mức sử dụng bộ nhớ vẫn còn nhỏ. 

Một cách tiếp cận phức tạp hơn có thể cố gắng tìm ra các mẫu trong chuỗi. Hàng đợi có những đặc tính tăng trưởng thú vị, bởi vì mỗi khi ai đó được xử lý, số lượng bản sao của họ sẽ tăng lên, nhưng việc khám phá và duy trì các mẫu đó là không cần thiết ở đây. Lực lượng vũ phu hoạt động vì số lượng thao tác tối đa nhỏ, trong khi tìm kiếm theo mẫu sẽ tăng thêm độ phức tạp mà không cải thiện hiệu suất cần thiết. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n) | O(n) | Đã chấp nhận | 
| Đạo hàm mẫu | O(log n) hoặc O(1) tùy theo phương pháp | O(1) | Không cần thiết | 

## Hướng dẫn thuật toán 

1. Xếp năm binh sĩ xuất phát vào hàng đợi. Hàng đợi thể hiện thứ tự chính xác về liều lượng trong tương lai sẽ được chỉ định. 
2. Lặp lại thao tác nhân bản cho đến khi`n`liều thứ đã đạt được. Trong mỗi lần lặp lại, hãy loại bỏ người lính ở phía trước vì người lính đó sẽ nhận được liều tiếp theo. 
3. Nếu số lần lặp hiện tại là`n`, xuất ngay tên người lính đó. Không cần thiết phải mô phỏng các bản sao trong tương lai vì chúng không thể ảnh hưởng đến câu trả lời đã được xác định. 
4. Nếu không, hãy thêm hai bản sao của người lính bị loại bỏ vào cuối hàng đợi. Người lính ban đầu và bản sao mới đều chờ ở cuối, phù hợp với quy tắc của quy trình. 

Tại sao nó hoạt động: sau mỗi lần lặp hoàn thành, hàng đợi được thuật toán lưu trữ chính xác là hàng đợi thực sau cùng một số lượng. Ban đầu cả hai hàng đợi đều giống hệt nhau. Mỗi bước sẽ loại bỏ cùng một người lính phía trước và chèn hai bản sao giống nhau vào cuối, do đó, bất biến vẫn đúng. Khi`n`lần loại bỏ xảy ra, thuật toán trả về cùng một người lính nhận được quân thật`n`liều thứ. 

## Giải pháp Python```python
import sys
from collections import deque

input = sys.stdin.readline

def solve():
    n = int(input())

    q = deque(["Ace", "Bolt", "Cameron", "Doom", "Echo"])

    for i in range(1, n + 1):
        person = q.popleft()

        if i == n:
            print(person)
            return

        q.append(person)
        q.append(person)

if __name__ == "__main__":
    solve()
```Deque được sử dụng vì cả hai thao tác chúng ta cần đều hiệu quả: loại bỏ phần tử đầu tiên và thêm các phần tử vào phía sau. Một danh sách Python bình thường sẽ khiến việc loại bỏ từ phía trước trở nên tốn kém vì tất cả các phần tử còn lại sẽ phải dịch chuyển. 

Vòng lặp bắt đầu từ`1`thay vì`0`bởi vì vấn đề yêu cầu liều đầu tiên là vị trí`1`. Điều này tránh được một lỗi khi kiểm tra xem người lính hiện tại có phải là câu trả lời hay không. 

Thao tác nhân bản chỉ được thực hiện sau khi kiểm tra câu trả lời. Nếu chúng tôi thêm các bản sao trước khi kiểm tra, trạng thái hàng đợi sẽ biểu thị thời điểm tiếp theo thay vì thời điểm người lính hiện tại nhận được liều thuốc. 

## Ví dụ đã hoạt động 

Đối với ví dụ đầu tiên:```
Input:
1
```| Bước | Lính mặt trận | Xếp hàng sau hoạt động | Trả lời | 
| --- | --- | --- | --- | 
| 1 | Át | Không cần thiết | Át | 

Liều đầu tiên luôn thuộc về Ace, xác minh điều kiện biên ban đầu. 

Đối với ví dụ thứ hai:```
Input:
6
```| Bước | Lính mặt trận | Xếp hàng sau hoạt động | 
| --- | --- | --- | 
| 1 | Át | Bolt, Cameron, Doom, Echo, Ace, Ace | 
| 2 | Bu lông | Cameron, Doom, Echo, Ace, Ace, Bolt, Bolt | 
| 3 | Cameron | Doom, Echo, Ace, Ace, Bolt, Bolt, Cameron, Cameron | 
| 4 | Sự diệt vong | Echo, Ace, Ace, Bolt, Bolt, Cameron, Cameron, Doom, Doom | 
| 5 | Tiếng vang | Ace, Ace, Bolt, Bolt, Cameron, Cameron, Doom, Doom, Echo, Echo | 
| 6 | Át | Đã tìm thấy câu trả lời | 

Dấu vết cho thấy tại sao trình tự này không phải là sự lặp lại đơn giản của năm cái tên ban đầu. Mỗi người lính được xử lý sẽ nhận được một bản sao khác, thay đổi thứ tự trong tương lai. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi liều yêu cầu loại bỏ một lần và tối đa hai lần chèn vào deque. | 
| Không gian | O(n) | Hàng đợi tăng thêm một phần tử sau mỗi liều mô phỏng. | 

Với`n`nhiều nhất là một triệu, thuật toán thực hiện khoảng một triệu thao tác xếp hàng và lưu trữ khoảng một triệu tên, vừa vặn thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys
import io
from collections import deque

def solve(inp: str) -> str:
    old_stdin = sys.stdin
    sys.stdin = io.StringIO(inp)

    n = int(sys.stdin.readline())
    q = deque(["Ace", "Bolt", "Cameron", "Doom", "Echo"])

    ans = ""
    for i in range(1, n + 1):
        person = q.popleft()
        if i == n:
            ans = person
            break
        q.append(person)
        q.append(person)

    sys.stdin = old_stdin
    return ans + "\n"

assert solve("1\n") == "Ace\n", "sample 1"
assert solve("6\n") == "Ace\n", "sample 2"
assert solve("943\n") == "Cameron\n", "sample 3"

assert solve("2\n") == "Bolt\n", "second soldier receives the second dose"
assert solve("5\n") == "Echo\n", "last original soldier"
assert solve("10\n") == "Echo\n", "after the first doubling cycle"
assert solve("1000000\n") != "", "large input handling"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1`|`Ace`| Ranh giới đầu vào và lập chỉ mục tối thiểu | 
|`2`|`Bolt`| Chuyển ngay sang phần tử hàng đợi thứ hai | 
|`5`|`Echo`| Xử lý hàng đợi ban đầu hoàn chỉnh | 
|`10`|`Echo`| Xử lý đúng sau vòng nhân bản đầu tiên | 
|`1000000`| Bất kỳ tên hợp lệ nào | Hiệu suất hạn chế tối đa | 

## Vỏ cạnh 

cho`n = 1`, hàng đợi chỉ chứa năm người lính ban đầu. Thuật toán loại bỏ Át ở lần lặp đầu tiên, thấy rằng lần lặp hiện tại bằng vị trí được yêu cầu và trả về Át trước khi sửa đổi hàng đợi. 

Vì`n = 6`, chi tiết quan trọng là năm thao tác đầu tiên không giữ nguyên hàng đợi. Sau khi mỗi người lính được xử lý, hai bản sao sẽ được thêm vào phía sau. Sau khi Echo nhận được liều thứ năm, hàng đợi sẽ trở thành:```
Ace, Ace, Bolt, Bolt, Cameron, Cameron, Doom, Doom, Echo, Echo
```Lần loại bỏ thứ sáu là Ace. Thuật toán đạt đến trạng thái tương tự sau năm lần lặp và trả về kết quả chính xác. 

Với giá trị lớn như`n = 1000000`, thuật toán không cố gắng lưu trữ toàn bộ chuỗi câu trả lời. Nó chỉ lưu trữ trạng thái hàng đợi hiện tại và thực hiện lần lượt các thao tác được yêu cầu, do đó thời gian chạy tăng tuyến tính với kích thước đầu vào và vẫn mang tính thực tế.
