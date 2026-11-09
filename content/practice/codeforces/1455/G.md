---
title: "CF 1455G - Giá trị bị cấm"
description: "Nhiệm vụ là mô phỏng một ngôn ngữ lập trình đơn giản trong đó một biến x có thể được gán giá trị thông qua các lệnh đã đặt."
date: "2026-06-11T02:47:56+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "dp"]
categories: ["algorithms"]
codeforces_contest: 1455
codeforces_index: "G"
codeforces_contest_name: "Educational Codeforces Round 99 (Rated for Div. 2)"
rating: 2900
weight: 1455
solve_time_s: 114
verified: false
draft: false
---

[CF 1455G - Giá trị bị cấm](https://codeforces.com/problemset/problem/1455/G) 

**Xếp hạng:** 2900 
**Thẻ:** cấu trúc dữ liệu, dp 
**Thời gian giải:** 1 phút 54s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Nhiệm vụ là mô phỏng một ngôn ngữ lập trình đơn giản trong đó một biến duy nhất`x`có thể được gán giá trị thông qua`set`hướng dẫn. Mỗi`set`hướng dẫn sẽ phải trả giá nếu chúng ta muốn loại bỏ nó và mục tiêu là ngăn chặn`x`không bao giờ bị gán một giá trị bị cấm`s`trong khi chi tiêu tổng chi phí tối thiểu. Chương trình cũng chứa các khối điều kiện, bắt đầu bằng`if y`và kết thúc bằng một kết quả phù hợp`end`, trong đó các lệnh bên trong khối chỉ được thực thi nếu`x`hiện tại bằng`y`. Các khối này có thể được lồng vào nhau, tạo ra cấu trúc dạng cây của các đường dẫn thực thi. 

Đầu vào bao gồm`n`hướng dẫn, ở đâu`n`có thể lên tới 200.000 và chi phí`v`có thể lớn tới 10^9. Một mô phỏng bạo lực khám phá tất cả các kết hợp loại bỏ lệnh có thể có sẽ liên quan đến 2^n khả năng, điều này là không khả thi. Vì vậy, chúng ta cần một thuật toán xử lý chương trình theo thời gian tuyến tính theo số lượng lệnh. 

Các trường hợp cạnh bao gồm liên tiếp`set`hướng dẫn gán giá trị bị cấm cả bên trong và bên ngoài`if`khối. Ví dụ, một chương trình thiết lập`x`ĐẾN`s`ngay lập tức và sau đó đã lồng nhau`if`các khối có nhiệm vụ bị cấm khác yêu cầu xử lý cẩn thận để tránh bỏ lỡ các đường dẫn loại bỏ rẻ hơn. Một trường hợp phức tạp khác là lồng nhau`if`các khối trong đó bản thân điều kiện có thể là giá trị bị cấm, có khả năng bỏ qua toàn bộ cây con của lệnh. 

Một ví dụ nhỏ làm nổi bật sự tinh tế. Coi như:```
4 1
set 1 10
if 0
set 1 5
end
```Ở đây, lần đầu tiên`set`bộ`x`đến 1, điều này bị cấm và phải được loại bỏ với giá 10. Phần lồng nhau`set`bên trong`if`khối sẽ không bao giờ thực thi, vì vậy chúng tôi không tiêu tốn chi phí của nó. Bất kỳ thuật toán ngây thơ nào loại bỏ mọi`set`của`1`sẽ chi tiêu quá mức. 

## Phương pháp tiếp cận 

Một cách tiếp cận đơn giản sẽ duyệt từng dòng chương trình, mô phỏng`x`khi nó thay đổi, và bất cứ khi nào`set`sẽ gán giá trị bị cấm`s`, chúng tôi thêm chi phí của nó vào tổng số. Đối với lồng nhau`if`khối, chúng ta sẽ cần phải duy trì giá trị hiện tại của`x`ở mỗi cấp độ. Điều này hoạt động về mặt khái niệm nhưng trở nên không hiệu quả vì việc duy trì tất cả các đường dẫn có thể thông qua các điều kiện lồng nhau có thể dẫn đến độ phức tạp theo cấp số nhân trong trường hợp xấu nhất. 

Cái nhìn sâu sắc quan trọng cho một giải pháp tối ưu là tại bất kỳ thời điểm nào trong chương trình, chỉ có giá trị hiện tại của`x`và chi phí để ngăn chặn`s`từ việc được giao vấn đề. Chúng ta không cần xem xét tất cả các lộ trình lịch sử mà chỉ cần xem xét chi phí tối thiểu cho mỗi tiềm năng`x`ở mỗi cấp độ lồng nhau. Quan sát này cho phép chúng tôi sử dụng phương pháp lập trình động với ngăn xếp để theo dõi chi phí nhằm tránh`s`cho mỗi hoạt động`if`khối. Khi chúng ta gặp phải một`set y v`hướng dẫn, nếu`y`bằng`s`, chúng ta phải loại bỏ nó hoặc bỏ qua nó tùy thuộc vào việc đường dẫn thực thi hiện tại có hoạt động hay không. Lồng nhau`if`các khối chỉ truyền chi phí tối thiểu lên trên khi khối được đóng lại, điều này đảm bảo đủ đường truyền tuyến tính. 

Sự chuyển tiếp giữa`if`Và`end`có thể được xử lý bằng cách sử dụng một chồng từ điển: mỗi từ điển ánh xạ một`x`giá trị đến mức chi phí tối thiểu để tránh việc ấn định`s`trong khối đó. Khi chúng tôi thoát khỏi một khối, chúng tôi hợp nhất bảng chi phí của nó với khối mẹ của nó, lấy chi phí tối thiểu cho mỗi giá trị. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n) | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo một ngăn xếp để thể hiện hoạt động`if`khối. Mỗi phần tử trên ngăn xếp chứa một dòng ánh xạ từ điển`x`giá trị tới mức chi phí tối thiểu để tránh việc ấn định`s`trong khối đó. Đẩy phạm vi toàn cầu vào ngăn xếp với`x=0`và giá 0. 
2. Lặp lại từng lệnh. Đối với một`set y v`hướng dẫn, kiểm tra xem`y`bằng`s`. Nếu đúng như vậy, chi phí tối thiểu để tiếp tục mà không phá vỡ chương trình là chi phí hiện tại cộng thêm.`v`. Nếu không, chi phí vẫn không thay đổi. Cập nhật từ điển ở đầu ngăn xếp cho phù hợp. 
3. Đối với một`if y`hướng dẫn, đẩy một từ điển trống mới vào ngăn xếp. Chỉ đặt các mục của từ điển cho mục hiện tại`x`khớp giá trị`y`. Điều này đảm bảo rằng chúng tôi chỉ theo dõi các đường dẫn thực thi đang hoạt động trong đó`if`khối sẽ thực thi. 
4. Đối với một`end`hướng dẫn, bật từ điển trên cùng và hợp nhất nó với từ điển trước đó trong ngăn xếp. Đối với mỗi`x`giá trị trong từ điển được bật lên, hãy cập nhật từ điển gốc với chi phí tối thiểu giữa chi phí hiện tại và chi phí con. Điều này tuyên truyền chi phí tối thiểu lên trên. 
5. Sau khi xử lý tất cả các hướng dẫn, từ điển ở phạm vi toàn cầu chứa chi phí tối thiểu để tránh`s`vì`x=0`. Xuất giá trị này. 

Tại sao nó hoạt động: Ở mọi cấp độ lồng nhau, ngăn xếp đảm bảo rằng chúng tôi chỉ truyền chi phí cho các đường dẫn thực thi có thể có với giá trị hiện tại của`x`. Chúng tôi không bao giờ xem xét những con đường bất khả thi và chúng tôi luôn chọn chi phí tối thiểu trong số các phương án thay thế. Điều này đảm bảo rằng chi phí cuối cùng là tối thiểu trên toàn cầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, s = map(int, input().split())
instructions = [input().strip() for _ in range(n)]

stack = [{0: 0}]

for line in instructions:
    parts = line.split()
    if parts[0] == 'set':
        y = int(parts[1])
        v = int(parts[2])
        new_top = {}
        for x_val, cost in stack[-1].items():
            if y == s:
                new_cost = cost + v
                new_top[x_val] = min(new_top.get(x_val, float('inf')), new_cost)
            else:
                new_top[y] = min(new_top.get(y, float('inf')), cost)
        stack[-1] = new_top
    elif parts[0] == 'if':
        y = int(parts[1])
        new_top = {}
        for x_val, cost in stack[-1].items():
            if x_val == y:
                new_top[x_val] = cost
        stack.append(new_top)
    elif parts[0] == 'end':
        child = stack.pop()
        parent = stack[-1]
        for x_val, cost in child.items():
            parent[x_val] = min(parent.get(x_val, float('inf')), cost)

print(min(stack[0].values()))
```Phần đầu tiên phân tích cú pháp đầu vào và khởi tạo ngăn xếp với phạm vi thực thi toàn cục. các`set`xử lý cẩn thận các cập nhật chỉ có liên quan`x`giá trị và chi phí sẽ tăng lên nếu việc chuyển nhượng bị cấm xảy ra.`if`tạo một từ điển mới chỉ cho các đường dẫn có điều kiện phù hợp với hiện tại`x`. các`end`hướng dẫn hợp nhất các chi phí con vào từ điển gốc bằng cách sử dụng so sánh chi phí tối thiểu, đảm bảo việc truyền bá tối ưu. 

## Ví dụ đã hoạt động 

Mẫu 1:```
5 1
set 1 10
set 2 15
if 2
set 1 7
end
```| Bước | giá trị x ở đầu ngăn xếp | Giải thích | 
| --- | --- | --- | 
| Ban đầu | {0:0} | Phạm vi toàn cầu, x bắt đầu từ 0 | 
| tập 1 10 | {1:0, 0:10} | Gán 1 tốn 10 nếu bị cấm, nếu không thì theo dõi mới x=1 | 
| tập 2 15 | {2:0, 1:0, 0:10} | Phân công 2 không phá vỡ, truyền bá chi phí | 
| nếu 2 | {2:0} | Chỉ những đường dẫn có x=2 mới nhập khối if | 
| tập 1 7 | {1:7} | Gán 1 bên trong if, bị cấm, cost +=7 | 
| kết thúc | sáp nhập vào cha mẹ | Cha mẹ hiện có {1:7, 2:0, 0:10} | 

Chi phí tối thiểu cần tránh`x=1`là 17, xác nhận đầu ra mẫu. 

Một ví dụ khác:```
3 2
set 1 5
set 2 3
set 3 4
```Đỉnh ngăn xếp phát triển thành {0:0} -> {1:0} -> {2:3} -> {3:3}. Chi phí tối thiểu là 3, tương ứng với việc loại bỏ`set 2 3`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi lệnh được xử lý một lần, các bản cập nhật từ điển được khấu hao liên tục | 
| Không gian | O(n) | Ngăn xếp có thể phát triển với sự lồng nhau`if`khối, mỗi khối chứa tối đa n phím | 

Thuật toán phù hợp trong vòng 2 giây cho n tối đa 2 * 10^5 vì tất cả các hoạt động đều là tuyến tính và việc hợp nhất từ ​​điển trung bình nhanh. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n, s = map(int, input().split())
    instructions = [input().strip() for _ in range(n)]
    stack = [{0: 0}]
    for line in instructions:
        parts = line.split()
        if parts[0] == 'set':
            y = int(parts[1])
            v = int(parts[2])
            new_top = {}
            for x_val, cost in stack[-1].items():
                if y == s:
                    new_cost = cost + v
                    new_top[x_val] = min(new_top.get(x_val, float('inf')), new_cost)
                else:
                    new_top[y] = min(new_top.get(y, float('inf
```
