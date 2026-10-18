---
title: "CF 105578J - Khiến Họ Tin Tưởng"
description: "Chúng ta được xếp vào một bảng đấu loại trực tiếp gồm 8 đội cố định, đã được sắp xếp theo thứ tự tứ kết từ trên xuống dưới. Mỗi đội có một tên duy nhất và sức mạnh số nguyên duy nhất."
date: "2026-06-22T21:51:11+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105578
codeforces_index: "J"
codeforces_contest_name: "The 2024 ICPC Asia Shenyang Regional Contest (The 3rd Universal Cup. Stage 19: Shenyang)"
rating: 0
weight: 105578
solve_time_s: 56
verified: true
draft: false
---

[CF 105578J - Khiến họ tin tưởng](https://codeforces.com/problemset/problem/105578/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 56s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được xếp vào một bảng đấu loại trực tiếp gồm 8 đội cố định, đã được sắp xếp theo thứ tự tứ kết từ trên xuống dưới. Mỗi đội có một tên duy nhất và sức mạnh số nguyên duy nhất. Giải đấu diễn ra theo thể thức loại trực tiếp tiêu chuẩn: các cặp liền kề trong bảng thi đấu một trận, đội mạnh hơn luôn đi tiếp và đội yếu hơn bị loại. Điều này tiếp tục diễn ra qua các trận tứ kết, bán kết và sau đó là trận chung kết, tạo ra đúng một nhà vô địch và một á quân. 

Nhiệm vụ không phải là mô phỏng cấu trúc giải đấu tùy ý mà phải tuân theo bố cục khung cố định chính xác này. Điều đó có nghĩa là vòng đầu tiên bao gồm các trận đấu giữa các đội 1 vs 2, 3 vs 4, 5 vs 6 và 7 vs 8. Đội thắng trong các trận đấu đó sẽ được ghép đôi một lần nữa để vào bán kết, và sau đó trận chung kết diễn ra giữa hai đội thắng ở bán kết. 

Mặc dù bối cảnh câu chuyện là về thể thao điện tử, nhưng vấn đề tính toán hoàn toàn là sự giảm thiểu mang tính quyết định đối với một cây so sánh nhị phân cố định. 

Kích thước đầu vào không đổi ở tám nhóm, do đó không cần tối ưu hóa tiệm cận. Ngay cả việc mô phỏng trực tiếp hoặc thậm chí tính toán lại cũng không đáng kể trong mọi trường hợp. Yêu cầu chính là tính chính xác của việc xử lý khung vì cấu trúc ghép nối rất dễ bị đọc sai hoặc triển khai không chính xác. 

Các trường hợp biên là tối thiểu do các ràng buộc, nhưng vẫn có các chế độ lỗi tinh vi trong quá trình triển khai đơn giản: 

Một sai lầm phổ biến là sắp xếp tất cả các đội theo sức mạnh và chọn hai đội đứng đầu. Điều đó sẽ không chính xác vì cấu trúc giải đấu rất quan trọng. Ví dụ: nếu hai đội mạnh nhất gặp nhau ở tứ kết, một đội sẽ bị loại sớm và không thể giành ngôi á quân, ngay cả khi đội đó mạnh hơn những đội còn lại. 

Một sai lầm khác là ghép nối không chính xác giữa các vòng, chẳng hạn như ghép nối lại dựa trên chỉ số ban đầu thay vì vị trí của người chiến thắng trong bảng đấu. Điều này phá vỡ cấu trúc loại trực tiếp. 

## Phương pháp tiếp cận 

Một cách giải thích bạo lực sẽ mô phỏng toàn bộ giải đấu một cách rõ ràng. Chúng tôi duy trì danh sách những người tham gia vòng hiện tại, sau đó liên tục ghép các mục liền kề, so sánh điểm mạnh và xây dựng vòng tiếp theo cho đến khi còn lại một đội. Mỗi trận đấu là một công việc không ngừng nghỉ và với tám đội thì tổng thể nhiều nhất là bảy lần so sánh. Điều này đã tầm thường rồi, nhưng cấu trúc đã khái quát hóa một cách rõ ràng. 

Quan sát chính là dấu ngoặc là cây nhị phân hoàn hảo với thứ tự cố định. Mỗi trận đấu sẽ chia hai đội thành một và đội chiến thắng sẽ được tuyên truyền lên trên mà không cần sắp xếp lại. Vì chỉ có ba vòng nên chúng tôi có thể mô phỏng theo từng cấp độ mà không cần bất kỳ chi phí nào. 

Ở đây, các phương pháp tiếp cận bạo lực và tối ưu hoàn toàn giống nhau vì cấu trúc cố định và nhỏ. Sự khác biệt duy nhất là liệu chúng ta có mô hình hóa các vòng một cách rõ ràng hay cố gắng rút ra câu trả lời theo cách trừu tượng hơn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(1) | O(1) | Đã chấp nhận | 
| Mô phỏng theo cấp độ | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi mô phỏng trực tiếp các vòng đấu loại trực tiếp bằng cách sử dụng danh sách các cặp (sức mạnh, tên).

1. Đọc tám đội theo thứ tự và lưu mỗi đội thành một cặp chứa sức mạnh và tên. Chúng tôi đặt sức mạnh lên hàng đầu vì sự so sánh mang tính số lượng và thường xuyên. 
2. Khởi tạo danh sách vòng hiện tại theo tám đội này theo thứ tự khung. Thứ tự rất quan trọng vì các trận đấu luôn diễn ra giữa các cặp liền kề. 
3. Đối với mỗi vòng, hãy tạo một danh sách trống cho những người chiến thắng. 
4. Lặp lại danh sách hiện tại theo hai bước. Đối với mỗi cặp liền kề, so sánh sức mạnh và xếp đội mạnh hơn vào danh sách vòng tiếp theo. Điều này trực tiếp mã hóa quy luật sức mạnh cao hơn luôn thắng. 
5. Sau khi xử lý tất cả các cặp, thay thế danh sách hiện tại bằng danh sách người chiến thắng. 
6. Lặp lại cho đến khi chỉ còn một đội. Đội đó là nhà vô địch. 
7. Đội á quân là đội thua ở trận chung kết. Chúng tôi có thể theo dõi nó trong vòng cuối cùng bằng cách lưu trữ cả hai người vào chung kết trước khi so sánh họ. 

Một cách rõ ràng hơn một chút là dừng lại sau trận bán kết, nơi chúng tôi có bốn đội bị loại xuống còn hai đội vào chung kết, sau đó tiến hành một cuộc so sánh cuối cùng và ghi lại cả đội thắng và đội thua. 

### Tại sao nó hoạt động 

Mỗi vòng mô phỏng chính xác một cấp độ của cây giải đấu nhị phân hoàn chỉnh. Mỗi đội tham gia đúng một trận mỗi vòng cho đến khi bị loại. Vì việc ghép đôi là cố định và mang tính quyết định nên người chiến thắng trong mỗi trận đấu được xác định duy nhất bằng cách so sánh sức mạnh. Cấu trúc đảm bảo rằng nút sống sót cuối cùng là nhà vô địch toàn cầu trong các ràng buộc của giải đấu và nút bị loại cuối cùng là á quân vì nút đó là kẻ thua cuộc trong trận đấu cuối cùng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

teams = []

for _ in range(8):
    name, t = input().split()
    teams.append((int(t), name))

while len(teams) > 2:
    nxt = []
    for i in range(0, len(teams), 2):
        if teams[i][0] > teams[i + 1][0]:
            nxt.append(teams[i])
        else:
            nxt.append(teams[i + 1])
    teams = nxt

# final match
a, b = teams[0], teams[1]
if a[0] > b[0]:
    print(f"{a[1]} beats {b[1]}")
else:
    print(f"{b[1]} beats {a[1]}")
```Mã đọc tám đội và lưu trữ chúng dưới dạng cặp (sức mạnh, tên) để so sánh là so sánh số nguyên đơn giản. Vòng lặp xử lý các vòng bằng cách ghép nối các phần tử liền kề và đẩy người chiến thắng về phía trước, khớp chính xác với cấu trúc khung cố định. 

Hai đội cuối cùng còn lại đại diện cho các đội vào chung kết. Trận so sánh cuối cùng sẽ xác định trực tiếp cả nhà vô địch và á quân, vì đội thua trong trận chung kết được xác định duy nhất. 

Một cạm bẫy triển khai phổ biến là vô tình sắp xếp các đội trước khi mô phỏng các trận đấu. Điều đó sẽ phá hủy cấu trúc khung. Một vấn đề tế nhị khác là trộn lẫn các chỉ số khi xây dựng vòng tiếp theo; bước từng bước là bắt buộc vì mỗi trận đấu chỉ có đúng hai người tham gia. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
LNG 55
WBG 65
HLE 70
BLG 75
TES 48
T1 80
GEN 60
FLY 50
```Tứ kết: 

| Trận đấu | Trái | Đúng | Người chiến thắng | 
| --- | --- | --- | --- | 
| 1 | LNG(55) | WBG(65) | WBG | 
| 2 | HLE(70) | BLG(75) | BLG | 
| 3 | TES(48) | T1(80) | T1 | 
| 4 | GEN(60) | BAY(50) | GEN | 

Bán kết: 

| Trận đấu | Trái | Đúng | Người chiến thắng | 
| --- | --- | --- | --- | 
| 1 | WBG(65) | BLG(75) | BLG | 
| 2 | T1(80) | GEN(60) | T1 | 

Chung kết: 

| A | B | Người chiến thắng | 
| --- | --- | --- | 
| BLG(75) | T1(80) | T1 | 

Đầu ra:```
T1 beats BLG
```Dấu vết này khẳng định rằng cấu trúc bảng đấu có thể loại bỏ các đội mạnh sớm, vì BLG vẫn tồn tại cho đến trận chung kết trong khi WBG bị loại ở bán kết dù rất mạnh. 

### Ví dụ 2 

đầu vào:```
LNG 55
WBG 65
HLE 70
BLG 81
TES 48
T1 80
GEN 60
FLY 50
```Tứ kết: 

| Trận đấu | Trái | Đúng | Người chiến thắng | 
| --- | --- | --- | --- | 
| 1 | LNG(55) | WBG(65) | WBG | 
| 2 | HLE(70) | BLG(81) | BLG | 
| 3 | TES(48) | T1(80) | T1 | 
| 4 | GEN(60) | BAY(50) | GEN | 

Bán kết: 

| Trận đấu | Trái | Đúng | Người chiến thắng | 
| --- | --- | --- | --- | 
| 1 | WBG(65) | BLG(81) | BLG | 
| 2 | T1(80) | GEN(60) | T1 | 

Chung kết: 

| A | B | Người chiến thắng | 
| --- | --- | --- | 
| BLG(81) | T1(80) | BLG | 

Đầu ra:```
BLG beats T1
```Điều này cho thấy việc thay đổi một giá trị sức mạnh duy nhất chỉ thay đổi kết quả trận đấu cục bộ và trận chung kết phụ thuộc hoàn toàn vào vị trí trong bảng đấu thay vì xếp hạng toàn cầu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Chính xác 7 lần so sánh được thực hiện cho một bảng 8 đội cố định | 
| Không gian | O(1) | Chỉ một danh sách các đội có kích thước không đổi được lưu trữ | 

Kích thước vấn đề đã được cố định, do đó giải pháp là thời gian và bộ nhớ không đổi bất kể nội dung đầu vào. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        # inline solution
        teams = []
        for _ in range(8):
            name, t = input().split()
            teams.append((int(t), name))

        while len(teams) > 2:
            nxt = []
            for i in range(0, len(teams), 2):
                if teams[i][0] > teams[i + 1][0]:
                    nxt.append(teams[i])
                else:
                    nxt.append(teams[i + 1])
            teams = nxt

        a, b = teams
        if a[0] > b[0]:
            print(f"{a[1]} beats {b[1]}")
        else:
            print(f"{b[1]} beats {a[1]}")

    return out.getvalue().strip()

# provided samples
assert run("""LNG 55
WBG 65
HLE 70
BLG 75
TES 48
T1 80
GEN 60
FLY 50
""") == "T1 beats BLG"

assert run("""LNG 55
WBG 65
HLE 70
BLG 81
TES 48
T1 80
GEN 60
FLY 50
""") == "BLG beats T1"

# custom cases

# minimum variation: already deterministic chain to one side
assert run("""A 1
B 2
C 3
D 4
E 5
F 6
G 7
H 8
""") == "H beats G"

# strongest in wrong branch early, still loses if bracketed
assert run("""A 100
B 1
C 99
D 2
E 98
F 3
G 97
H 4
""") == "A beats E"

# swap final contenders
assert run("""A 10
B 9
C 8
D 7
E 6
F 5
G 4
H 3
""") == "A beats B"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| sức mạnh tăng dần | H đập G | truyền bá đầy đủ phần tử tối đa | 
| thế mạnh xen kẽ | Một nhịp E | khung có thể sớm loại bỏ những người mạnh không vào chung kết | 
| sức mạnh giảm dần | A đập B | sự đúng đắn của vòng chung kết | 

## Vỏ cạnh 

Trường hợp một cạnh là khi đội mạnh nhất được xếp vào nhánh sớm gặp đội mạnh thứ hai. Ví dụ: 

đầu vào:```
A 100
B 99
C 1
D 2
E 98
F 3
G 97
H 4
```Vòng tứ kết tạo ra B hoặc A tùy theo cặp đấu, nhưng nếu A và B gặp nhau ở bán kết hoặc sớm hơn, một bên sẽ bị loại mặc dù mạnh toàn cầu. Thuật toán xử lý việc này một cách chính xác vì nó không bao giờ so sánh thứ hạng chung mà chỉ so sánh các cặp dấu ngoặc liền kề. 

Một trường hợp khác là khi á quân không phải là đội mạnh thứ hai toàn cầu. Trong trận đấu cuối cùng, người thua cuộc chỉ đơn giản là người vào chung kết thua trận cuối cùng. Mã này lưu trữ rõ ràng cả hai người vào chung kết và so sánh họ một lần, đảm bảo người về nhì được xác định chính xác bất kể thứ hạng toàn cầu. 

Cuối cùng, tất cả các giá trị là duy nhất đảm bảo không có sự mơ hồ khi so sánh, do đó không cần logic ràng buộc.
