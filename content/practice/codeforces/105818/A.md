---
title: "CF 105818A - Miếng hoa huệ"
description: "Chúng ta có một cái ao có N hàng hoa súng. Mỗi hàng có hai vị trí hạ cánh có thể là trái và phải. Tại bất kỳ thời điểm nào, chính xác một cạnh của hàng nằm trên mặt nước và có thể được sử dụng. Hai con ếch xuất phát trước hàng đầu tiên và cả hai đều phải băng qua ao. Con ếch luôn tiến về phía trước."
date: "2026-06-25T15:09:56+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105818
codeforces_index: "A"
codeforces_contest_name: "TeamsCode Spring 2025 Advanced Division"
rating: 0
weight: 105818
solve_time_s: 37
verified: true
draft: false
---

[CF 105818A - Miếng đệm hoa huệ](https://codeforces.com/problemset/problem/105818/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 37s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một cái ao với`N`hàng lá huệ. Mỗi hàng có hai vị trí hạ cánh có thể là trái và phải. Tại bất kỳ thời điểm nào, chính xác một cạnh của hàng nằm trên mặt nước và có thể được sử dụng. Hai con ếch xuất phát trước hàng đầu tiên và cả hai đều phải băng qua ao. 

Con ếch luôn tiến về phía trước. Nó đi vào một hàng bằng cách hạ cánh trên miếng hoa súng hiện có và khi nó rời khỏi hàng đó, miếng đệm đã sử dụng sẽ chìm xuống trong khi miếng đệm khác trong cùng hàng sẽ nổi lên. Vì mỗi lần chỉ có một con ếch có thể chiếm một miếng đệm nên các con ếch phải thay phiên nhau đi qua các hàng. Bất cứ khi nào ếch đáp xuống ô bên phải, tổng số điểm sẽ tăng thêm một. 

Đầu vào đưa ra số lượng hàng và trạng thái ban đầu của mỗi hàng. Đầu ra yêu cầu tổng số điểm tối đa mà cả hai con ếch có thể đạt được. 

Hạn chế chính đó là`N`đủ nhỏ để nhiều giải pháp có thể vượt qua, nhưng mục tiêu thực sự là chú ý đến cấu trúc. Kể cả nếu`N`lớn hơn nhiều, chúng ta nên tìm giải pháp gần với thời gian tuyến tính vì bản thân đầu vào chứa`N`mẩu thông tin. Việc mô phỏng tất cả các mệnh lệnh có thể có của hai con ếch sẽ tạo ra những trạng thái không cần thiết và tăng trưởng theo cấp số nhân. 

Phần khó khăn là ếch được phép chọn các thứ tự khác nhau. Một giải pháp bất cẩn có thể cố gắng tham lam chọn con ếch nào đi trước dựa trên hàng hiện tại, nhưng thứ tự được chọn cho một hàng sẽ ảnh hưởng đến trạng thái của hàng đó đối với con ếch thứ hai. Quan sát quan trọng là mỗi hàng đều độc lập về mặt đóng góp điểm số cuối cùng. 

Ví dụ: với một hàng:```
1
L
```Con ếch đầu tiên đáp xuống miếng đệm bên trái và không được điểm. Tấm đệm lật lên và con ếch thứ hai đáp xuống tấm đệm bên phải và được một điểm. Câu trả lời là:```
1
```Một giải pháp ban đầu chỉ tính các miếng đệm hướng về bên phải sẽ xuất ra`0`, điều đó là sai. 

Một trường hợp cạnh khác là khi mỗi hàng bắt đầu bằng phần đệm bên phải có sẵn:```
3
RRR
```Con ếch đầu tiên nhận được một điểm ở mỗi hàng. Sau mỗi lần nhảy, con ếch thứ hai nhận được miếng đệm bên trái và không có điểm. Câu trả lời vẫn là:```
3
```Một giải pháp giả định rằng cả hai con ếch bằng cách nào đó nên tránh chiếm vị trí đầu tiên trong một hàng có thể nghĩ rằng điểm số có thể được cải thiện một cách sai lầm. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực tự nhiên là thử mọi cách có thể để hai con ếch có thể xen kẽ các bước di chuyển của chúng. Có nhiều trình tự có thể xảy ra vì ở mỗi hàng, một trong hai con ếch có thể là con về đích trước. Đối với mỗi thứ tự, chúng ta có thể mô phỏng các hàng và đếm điểm. Điều này đúng vì quyết định duy nhất quan trọng là con ếch nào về đích đầu tiên ở mỗi hàng. 

Vấn đề là điều này khám phá nhiều hơn mức cần thiết. Có hai sự lựa chọn xem ai là người đầu tiên ở mỗi hàng, vì vậy số lượng đơn hàng có thể lên tới`2^N`. Vì`N`xung quanh`100`, cái này đã có sẵn rồi`10^30`những khả năng không thể xử lý được. 

Quan sát quan trọng là mỗi hàng luôn cho chính xác một điểm. Giả sử một hàng hiện có sẵn phần đệm bên trái. Con ếch đầu tiên bước vào sẽ nhận được miếng đệm bên trái và không được điểm nào. Khi nó rời đi, miếng đệm bên trái sẽ chìm xuống nước và miếng đệm bên phải sẽ có sẵn, vì vậy con ếch thứ hai được một điểm. Nếu ô bên phải bắt đầu có sẵn, con ếch đầu tiên ghi được một điểm và con ếch thứ hai ghi được 0 điểm. Trong cả hai trường hợp, tổng đóng góp của hàng đó chính xác là một. 

Vì cả hai con ếch phải vượt qua mỗi hàng nên mỗi hàng sẽ đóng góp một điểm bất kể cấu hình ban đầu như thế nào và bất kể thứ tự đã chọn. Toàn bộ vấn đề giảm xuống việc đếm các hàng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^N * N) | O(N) | Quá chậm | 
| Tối ưu | O(N) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số hàng. Thông tin duy nhất cần thiết cho câu trả lời là có bao nhiêu hàng tồn tại, vì mỗi hàng đóng góp chính xác một điểm. 
2. Bỏ qua cấu hình bên trái hoặc bên phải của mỗi hàng. Con ếch đầu tiên ghé thăm một hàng và con ếch thứ hai ghé thăm cùng một hàng luôn chia hai miếng đệm giữa chúng, do đó điểm tổng hợp từ hàng đó là cố định. 
3. Đầu ra`N`, số hàng. 

Tại sao nó hoạt động: 

Đối với mỗi hàng, có đúng hai lần hạ cánh, mỗi con ếch lần lượt hạ cánh. Lần hạ cánh đầu tiên sử dụng tấm đệm nổi hiện tại và lần hạ cánh thứ hai sử dụng tấm đệm nổi sau khi con ếch đầu tiên rời đi. Hai con ếch cùng nhau ghé thăm cả hai miếng đệm trong hàng một lần và đúng một trong những miếng đệm đó là miếng đệm bên phải. Do đó chính xác một điểm được thêm vào mỗi hàng. Thêm khoản đóng góp cố định này vào tất cả`N`hàng cho điểm tối đa có thể. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    input().strip()
    print(n)

if __name__ == "__main__":
    solve()
```Giải pháp chỉ cần số lượng hàng. Chuỗi mô tả các vị trí ban đầu của lá huệ được đọc vì nó là một phần của định dạng đầu vào nhưng các giá trị của nó không ảnh hưởng đến câu trả lời. 

Không có vấn đề về ranh giới vì thuật toán không bao giờ lập chỉ mục vào chuỗi. Việc triển khai cũng tránh việc lưu trữ chuỗi, giữ mức sử dụng bộ nhớ không đổi. Kích thước đầu vào rất nhỏ so với khả năng của Python, nên I/O nhanh là quá đủ. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
1
R
```| Bước | Hàng được tính | Trạng thái hàng hiện tại | Điểm | 
| --- | --- | --- | --- | 
| Bắt đầu | 0 |`R`| 0 | 
| Quy trình hàng 1 | 1 | Con ếch đầu tiên ở bên phải, con ếch thứ hai ở bên trái | 1 | 
| Kết thúc | 1 | Tất cả các hàng được xử lý | 1 | 

Hàng bắt đầu với ô bên phải có sẵn, vì vậy chú ếch đầu tiên sẽ ghi điểm. Con ếch thứ hai lấy miếng đệm còn lại. Điểm cuối cùng là một. 

### Mẫu 2 

đầu vào:```
3
LRL
```| Bước | Hàng được tính | Trạng thái hàng hiện tại | Điểm | 
| --- | --- | --- | --- | 
| Bắt đầu | 0 |`LRL`| 0 | 
| Quy trình hàng 1 | 1 | Row đóng góp một điểm | 1 | 
| Quy trình hàng 2 | 2 | Row đóng góp một điểm | 2 | 
| Quy trình hàng 3 | 3 | Row đóng góp một điểm | 3 | 

Dấu vết cho thấy hướng ban đầu của các hàng thay đổi ai nhận được điểm, nhưng không bao giờ thay đổi tổng số điểm mà hàng mang lại. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N) | Việc đọc đầu vào và tạo ra câu trả lời mất thời gian tuyến tính theo số hàng. | 
| Không gian | O(1) | Chỉ có số lượng hàng được lưu trữ. | 

Thuật toán dễ dàng phù hợp với các giới hạn vì nó chỉ thực hiện công việc liên tục sau khi đọc đầu vào. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    old = sys.stdin
    sys.stdin = io.StringIO(inp)

    n = int(sys.stdin.readline())
    sys.stdin.readline()
    ans = str(n)

    sys.stdin = old
    return ans

# provided sample style
assert run("1\nR\n") == "1", "single row"

# custom cases
assert run("5\nLLLLL\n") == "5", "all left pads"
assert run("4\nRRRR\n") == "4", "all right pads"
assert run("3\nLRL\n") == "3", "mixed rows"
assert run("100\n" + "L" * 100 + "\n") == "100", "large input size"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / R`|`1`| Trường hợp kích thước tối thiểu | 
|`5 / LLLLL`|`5`| Tất cả các hàng bắt đầu bằng miếng đệm bên trái | 
|`4 / RRRR`|`4`| Tất cả các hàng bắt đầu bằng miếng đệm bên phải | 
|`3 / LRL`|`3`| Cấu hình hỗn hợp | 
|`100 / all L`|`100`| Trường hợp ranh giới lớn | 

## Vỏ cạnh 

Đối với một hàng bắt đầu bằng`L`, con ếch đầu tiên chạm tới miếng đệm bên trái và không thu được gì. Sau khi rời đi, miếng đệm bên trái chìm xuống và miếng đệm bên phải lại có sẵn nên con ếch thứ hai được một điểm. Đầu ra của thuật toán`1`, phù hợp với mức tối đa thực tế. 

Đối với các hàng tất cả bắt đầu bằng`R`, con ếch đầu tiên nhận được điểm ở mỗi hàng. Con ếch thứ hai không nhận được điểm nào từ những hàng đó vì miếng đệm bên phải đã chìm xuống. Thuật toán đưa ra số hàng khớp với tổng số điểm. 

Đối với các hàng xen kẽ như`LRL`, những chú ếch có thể nhận được điểm ở các hàng khác nhau tùy thuộc vào ai đến trước. Tổng số không thay đổi vì mỗi hàng luôn cho đúng một điểm nên đáp án vẫn là số hàng.
