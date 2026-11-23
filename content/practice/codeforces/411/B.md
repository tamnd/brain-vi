---
title: "CF 411B - Bộ xử lý đa lõi"
description: "Chúng tôi đang mô phỏng một bộ xử lý có nhiều lõi và một số ô nhớ. Thời gian được chia thành các chu kỳ. Đối với mỗi chu kỳ, mỗi lõi nhận được lệnh không làm gì hoặc lệnh ghi vào một ô nhớ cụ thể. Phần thú vị là sự bế tắc xảy ra như thế nào."
date: "2026-06-07T02:15:36+07:00"
tags: ["codeforces", "competitive-programming", "implementation"]
categories: ["algorithms"]
codeforces_contest: 411
codeforces_index: "B"
codeforces_contest_name: "Coder-Strike 2014 - Qualification Round"
rating: 1600
weight: 411
solve_time_s: 119
verified: true
draft: false
---

[CF 411B - Bộ xử lý đa lõi](https://codeforces.com/problemset/problem/411/B) 

**Đánh giá:** 1600 
**Thẻ:** triển khai 
**Thời gian giải:** 1 phút 59s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang mô phỏng một bộ xử lý có nhiều lõi và một số ô nhớ. Thời gian được chia thành các chu kỳ. Đối với mỗi chu kỳ, mỗi lõi nhận được lệnh không làm gì hoặc lệnh ghi vào một ô nhớ cụ thể. 

Phần thú vị là sự bế tắc xảy ra như thế nào. 

Nếu trong cùng một chu kỳ, hai hoặc nhiều lõi hoạt động cố gắng ghi vào cùng một ô nhớ thì xung đột sẽ xảy ra. Mọi lõi liên quan đến cuộc xung đột đó sẽ bị khóa vĩnh viễn và bản thân ô nhớ cũng bị khóa vĩnh viễn. 

Sau khi lõi bị khóa, nó sẽ bỏ qua tất cả các lệnh trong tương lai. Sau khi một ô nhớ bị khóa, không ai có thể ghi lại thành công vào ô đó nữa. Nếu sau đó một lõi hoạt động cố gắng ghi vào một ô đã bị khóa thì lõi đó cũng ngay lập tức bị khóa. 

Đối với mỗi lõi, chúng ta phải xác định chu kỳ đầu tiên mà nó bị khóa. Nếu nó không bao giờ bị khóa trong quá trình mô phỏng, chúng ta xuất ra 0. 

Những hạn chế là cực kỳ nhỏ. Số lượng lõi, chu kỳ và ô nhớ tối đa là 100. Ngay cả một thuật toán thực hiện công việc tỷ lệ thuận với tất cả các lõi và tất cả các chu kỳ đồng thời cũng đủ nhanh. Toàn bộ đầu vào chứa tối đa 10.000 hướng dẫn, vì vậy tính đơn giản và chính xác quan trọng hơn việc tối ưu hóa phức tạp. 

Khó khăn chính không phải ở tính hiệu quả mà ở việc tái tạo một cách trung thực các quy luật về thời gian. 

Một lỗi dễ mắc phải là xử lý việc ghi đồng thời không chính xác. 

Coi như:```
2 1 1
1
1
```Cả hai lõi đều ghi vào ô 1 trong chu kỳ 1. 

Đầu ra đúng là:```
1
1
```Cả hai lõi đều khóa cùng lúc vì xung đột được phát hiện từ tập hợp ghi xảy ra trong chu kỳ đó. Một mô phỏng bất cẩn xử lý từng lõi một có thể khiến lần ghi đầu tiên thành công và chỉ khóa lõi thứ hai. 

Một trường hợp tinh tế khác xảy ra khi ghi vào một ô đã bị khóa trước đó.```
2 2 1
1 1
1 0
```Chu kỳ 1 tạo ra xung đột, khóa cả lõi và tế bào. 

Đầu ra đúng:```
1
1
```Việc triển khai có lỗi có thể tiếp tục thực thi các lệnh cho các lõi đã bị khóa trong chu kỳ 2, điều này sẽ vi phạm tuyên bố. 

Trường hợp thứ ba là việc ghi vào các ô bị khóa chỉ được kiểm tra đối với các lõi hiện đang hoạt động.```
2 2 1
1 0
1 1
```Chu kỳ 1 khóa cả lõi và tế bào. 

Đầu ra đúng:```
1
1
```Trong chu kỳ 2, lõi thứ hai có thể đã thử ghi một lần nữa vào ô bị khóa, nhưng nó đã bị khóa nên không có gì xảy ra. Một khi lõi bị khóa, nó sẽ không bao giờ tham gia nữa. 

## Phương pháp tiếp cận 

Ý tưởng trực tiếp nhất là mô phỏng bộ xử lý chính xác như mô tả. 

Đối với mỗi chu kỳ, chúng tôi kiểm tra mọi lõi hoạt động. Nếu một lõi muốn ghi vào một ô đã bị khóa thì lõi đó sẽ bị khóa ngay lập tức. Nếu không, chúng tôi nhóm tất cả các yêu cầu viết theo ô đích. Bất kỳ ô nào nhận được yêu cầu từ hai lõi hoạt động trở lên sẽ gây ra xung đột, xung đột sẽ khóa tất cả các lõi tham gia và chính ô đó. 

Bởi vì các ràng buộc rất nhỏ nên mô phỏng đơn giản này đã đủ nhanh. 

Giả sử chúng ta thử một phương pháp thậm chí còn ngây thơ hơn. Đối với mỗi chu kỳ và mỗi ô, chúng tôi có thể quét tất cả các lõi để xem ai muốn viết vào đó. Điều đó sẽ đòi hỏi khoảng$m \cdot k \cdot n$hoạt động. Với tất cả các giá trị bằng 100, đây chỉ là một triệu séc, vẫn có thể chấp nhận được. 

Tuy nhiên, việc nhóm ghi trong khi xử lý chu trình sẽ sạch hơn và tự nhiên hơn. Mỗi lệnh được kiểm tra một lần trong mỗi chu kỳ, đưa ra mô phỏng có thời gian chạy tỷ lệ thuận với tổng số lệnh. 

Quan sát quan trọng là mọi chu kỳ có thể được xử lý độc lập bằng cách sử dụng bộ lõi hoạt động và ô đã mở khóa hiện tại. Xung đột chỉ phụ thuộc vào các yêu cầu ghi được thực hiện trong chu kỳ cụ thể đó, trong khi các lõi và ô bị khóa trước đó đơn giản là không khả dụng vĩnh viễn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(nmk) | O(1) | Đã chấp nhận | 
| Mô phỏng tối ưu | O(nm) | O(n + k) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc tất cả các hướng dẫn vào một mảng hai chiều trong đó`cmd[i][j]`là lệnh của cốt lõi`i`trong chu kỳ`j`. 
2. Duy trì một mảng`core_locked_time`, ban đầu tất cả đều là số không. Giá trị bằng 0 có nghĩa là lõi vẫn hoạt động. 
3. Duy trì mảng boolean`cell_locked`, ban đầu sai cho mọi ô nhớ. 
4. Chu trình xử lý từ 1 đến`m`. 
5. Đối với chu kỳ hiện tại, hãy tạo ánh xạ từ ô nhớ đến danh sách các lõi hoạt động đang cố ghi vào đó trong chu kỳ này. 
6. Lặp lại tất cả các lõi. 

Nếu lõi đã bị khóa, hãy bỏ qua nó. 

Nếu lệnh của nó là 0, bỏ qua nó. 

Nếu nó cố ghi vào một ô bị khóa, hãy ghi lại chu kỳ hiện tại làm thời gian khóa ngay lập tức. 

Nếu không, hãy thêm lõi vào danh sách yêu cầu của ô đích. 
7. Sau khi kiểm tra tất cả các lõi hoạt động, hãy kiểm tra từng ô đã nhận được yêu cầu trong chu kỳ này. 
8. Nếu một ô có yêu cầu từ ít nhất hai lõi thì sẽ xảy ra xung đột. 

Đánh dấu ô là bị khóa. 

Mọi lõi tham gia sẽ bị khóa trong chu kỳ hiện tại. 
9. Tiếp tục chu kỳ tiếp theo. 
10. Sau khi tất cả các chu trình được xử lý, xuất ra thời gian khóa đã ghi cho mỗi lõi. 

### Tại sao nó hoạt động 

Vào đầu mỗi chu kỳ, trạng thái của thuật toán khớp chính xác với trạng thái của bộ xử lý được mô tả trong bài toán. Mọi lõi bị khóa đều bị loại khỏi quá trình thực thi tiếp theo và mọi ô bị khóa đều được ghi nhớ. 

Khi một lõi hoạt động ghi vào một ô bị khóa, câu lệnh sẽ nói rằng nó sẽ bị khóa ngay lập tức. Bước 6 thực hiện chính xác hành động này. 

Đối với các ô đã mở khóa, tất cả các yêu cầu ghi từ chu kỳ hiện tại đều được thu thập trước khi đưa ra bất kỳ quyết định xung đột nào. Điều này phù hợp với tính chất đồng thời của một chu kỳ. Một ô bị khóa khi và chỉ khi có ít nhất hai lõi hoạt động cố gắng ghi vào nó trong chu kỳ đó và mọi lõi tham gia đều nhận chu kỳ hiện tại làm thời gian khóa của nó. 

Vì mọi quy tắc trong câu lệnh được áp dụng chính xác một lần và theo đúng thứ tự thời gian, nên mô phỏng luôn tạo ra các chuyển đổi trạng thái giống như bộ xử lý thực. Do đó, thời gian khóa được ghi lại là chính xác. 

## Giải pháp Python```python
import sys
from collections import defaultdict

input = sys.stdin.readline

def solve():
    n, m, k = map(int, input().split())

    cmds = [list(map(int, input().split())) for _ in range(n)]

    lock_time = [0] * n
    cell_locked = [False] * (k + 1)

    for cycle in range(1, m + 1):
        requests = defaultdict(list)

        for core in range(n):
            if lock_time[core] != 0:
                continue

            cell = cmds[core][cycle - 1]

            if cell == 0:
                continue

            if cell_locked[cell]:
                lock_time[core] = cycle
            else:
                requests[cell].append(core)

        for cell, cores in requests.items():
            if len(cores) >= 2:
                cell_locked[cell] = True

                for core in cores:
                    if lock_time[core] == 0:
                        lock_time[core] = cycle

    sys.stdout.write("\n".join(map(str, lock_time)) + "\n")

if __name__ == "__main__":
    solve()
```các`lock_time`mảng phục vụ hai mục đích. Giá trị bằng 0 có nghĩa là lõi vẫn hoạt động, trong khi giá trị dương ghi lại chu kỳ đầu tiên mà nó bị khóa. 

Lần đầu tiên đi qua các lõi sẽ xử lý các khóa ngay lập tức do ghi vào các ô đã bị khóa. Những lõi đó không được đưa vào danh sách yêu cầu vì chúng chưa bao giờ tham gia thành công vào chu trình hiện tại. 

Lần thứ hai kiểm tra các yêu cầu được nhóm lại. Vì tất cả các yêu cầu đã được thu thập trước khi kiểm tra xung đột nên việc ghi đồng thời được xử lý chính xác. Điều này tránh được lỗi phổ biến trong quá trình xử lý ghi từng cái một và vô tình ưu tiên cho lõi nào được truy cập trước. 

điều kiện`if lock_time[core] == 0`bên trong vòng xung đột là phòng thủ. Trong thực tế, một lõi trong danh sách yêu cầu không thể bị khóa trong cùng một chu kỳ, nhưng việc kiểm tra làm cho logic trở nên rõ ràng và duy trì thuộc tính "thời gian khóa đầu tiên". 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4 3 5
1 0 0
1 0 2
2 3 1
3 2 0
```Theo dõi từng chu kỳ: 

| Chu kỳ | Yêu cầu trước xung đột | Các ô bị khóa sau chu kỳ | Lõi mới bị khóa | 
| --- | --- | --- | --- | 
| 1 | 1 → {1,2}, 2 → {3}, 3 → {4} | {1} | 1, 2 | 
| 2 | 3 → {3}, 2 → {4} | {1} | không | 
| 3 | 1 → {3} | {1} | 3 | 

Thời gian khóa cuối cùng: 

| Cốt lõi | Khóa thời gian | 
| --- | --- | 
| 1 | 1 | 
| 2 | 1 | 
| 3 | 3 | 
| 4 | 0 | 

Đầu ra:```
1
1
3
0
```Ví dụ này thể hiện cả hai loại khóa. Lõi 1 và 2 bị khóa do xung đột đồng thời, trong khi lõi 3 sau đó bị khóa vì nó cố ghi vào ô 1 đã bị khóa. 

### Ví dụ 2 

đầu vào:```
3 2 2
1 0
1 2
2 1
```Dấu vết: 

| Chu kỳ | Yêu cầu trước xung đột | Các ô bị khóa sau chu kỳ | Lõi mới bị khóa | 
| --- | --- | --- | --- | 
| 1 | 1 → {1,2}, 2 → {3} | {1} | 1, 2 | 
| 2 | 1 → {3} | {1} | 3 | 

Thời gian khóa cuối cùng: 

| Cốt lõi | Khóa thời gian | 
| --- | --- | 
| 1 | 1 | 
| 2 | 1 | 
| 3 | 0 | 

Đầu ra:```
1
1
0
```Dấu vết này cho thấy chỉ các lõi hoạt động mới tiếp tục tham gia sau xung đột. Hai lõi đầu tiên sẽ biến mất khỏi quá trình xử lý trong tương lai ngay lập tức. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(nm) | Mỗi hướng dẫn được kiểm tra nhiều nhất một lần | 
| Không gian | O(n + k) | Lưu trữ trạng thái khóa và các nhóm yêu cầu theo chu kỳ | 

Kích thước đầu vào tối đa chỉ chứa 10.000 lệnh. MỘT$O(nm)$mô phỏng thực hiện tối đa 10.000 đánh giá hướng dẫn cộng với chi phí ghi sổ nhỏ, nằm trong giới hạn thoải mái. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io
from collections import defaultdict

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    n, m, k = map(int, input().split())
    cmds = [list(map(int, input().split())) for _ in range(n)]

    lock_time = [0] * n
    cell_locked = [False] * (k + 1)

    for cycle in range(1, m + 1):
        requests = defaultdict(list)

        for core in range(n):
            if lock_time[core]:
                continue

            cell = cmds[core][cycle - 1]

            if cell == 0:
                continue

            if cell_locked[cell]:
                lock_time[core] = cycle
            else:
                requests[cell].append(core)

        for cell, cores in requests.items():
            if len(cores) >= 2:
                cell_locked[cell] = True
                for core in cores:
                    if lock_time[core] == 0:
                        lock_time[core] = cycle

    return "\n".join(map(str, lock_time))

# provided sample
assert run(
"""4 3 5
1 0 0
1 0 2
2 3 1
3 2 0
"""
) == "1\n1\n3\n0", "sample 1"

# minimum size
assert run(
"""1 1 1
0
"""
) == "0", "single core does nothing"

# immediate conflict
assert run(
"""2 1 1
1
1
"""
) == "1\n1", "simultaneous write conflict"

# write into previously locked cell
assert run(
"""3 2 1
1 0
1 0
0 1
"""
) == "1\n1\n2", "later access to locked cell"

# no conflicts ever
assert run(
"""3 3 3
1 1 1
2 2 2
3 3 3
"""
) == "0\n0\n0", "all writes distinct"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Lõi đơn, một chu kỳ nhàn rỗi | 0 | Kích thước đầu vào tối thiểu | 
| Hai lõi ghi cùng một ô | 1, 1 | Xử lý xung đột đồng thời | 
| Truy cập ô bị khóa sau | 1, 1, 2 | Khóa ngay lập tức khi truy cập ô bị khóa | 
| Các tế bào riêng biệt xuyên suốt | 0, 0, 0 | Không có kết quả dương tính giả | 

## Vỏ cạnh 

Xét trường hợp xung đột đồng thời:```
2 1 1
1
1
```Trong chu kỳ 1, cả hai lõi hoạt động đều yêu cầu ô 1. Danh sách yêu cầu cho ô 1 chứa hai lõi, do đó thuật toán sẽ khóa ô và cả hai lõi. Đầu ra là:```
1
1
```Bởi vì các yêu cầu được nhóm lại trước khi xung đột được xử lý nên cả hai lõi đều không được ưu tiên. 

Bây giờ hãy xem xét việc viết vào một ô đã bị khóa:```
3 2 1
1 0
1 0
0 1
```Chu kỳ 1 tạo ra xung đột trên ô 1. Lõi 1 và 2 khóa và ô 1 bị khóa. 

Trong chu kỳ 2, lõi 3 cố gắng ghi vào ô 1. Thuật toán phát hiện ô đích đã bị khóa và ngay lập tức gán thời gian khóa 2 cho lõi 3. 

Đầu ra:```
1
1
2
```Cuối cùng, hãy xem xét các lõi đã bị khóa nhận lệnh trong tương lai:```
2 2 1
1 0
1 1
```Chu kỳ 1 khóa cả lõi và ô 1. Trong chu kỳ 2, lệnh của lõi thứ hai bị bỏ qua vì thời gian khóa của nó đã được ghi lại. Thuật toán không bao giờ xử lý các lệnh từ lõi bị khóa nữa. 

Đầu ra:```
1
1
```Điều này khớp chính xác với thông số kỹ thuật của bộ xử lý và ngăn việc tính hai lần hoặc thay đổi thời gian khóa của lõi sau khi nó đã bị khóa.
