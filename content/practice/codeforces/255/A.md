---
title: "CF 255A - Bài tập của Greg"
description: "Greg thực hiện các bài tập theo thứ tự lặp lại cố định. Bài đầu tiên tập ngực, bài thứ hai tập bắp tay, bài thứ ba tập lưng, sau đó lặp lại mô hình: ngực, bắp tay, lưng, v.v."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "implementation"]
categories: ["algorithms"]
codeforces_contest: 255
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 156 (Div. 2)"
rating: 800
weight: 255
solve_time_s: 139
verified: true
draft: false
---

[CF 255A - Bài tập của Greg](https://codeforces.com/problemset/problem/255/A) 

**Đánh giá:** 800 
**Thẻ:** triển khai 
**Thời gian giải:** 2m 19s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Greg thực hiện các bài tập theo thứ tự lặp lại cố định. Bài đầu tiên tập ngực, bài thứ hai tập bắp tay, bài thứ ba tập lưng, sau đó lặp lại mô hình: ngực, bắp tay, lưng, v.v. 

Chúng ta được cung cấp một mảng trong đó giá trị tại vị trí`i`cho chúng tôi biết Greg thực hiện bao nhiêu lần lặp lại cho`i`-Bài tập thứ trong chuỗi này. Nhiệm vụ là xác định nhóm cơ nào nhận được tổng số lần lặp lại lớn nhất sau khi tổng hợp tất cả các bài tập được giao cho nhóm đó. 

Những hạn chế là rất nhỏ. Số lượng bài tập nhiều nhất là 20 và số lần lặp lại nhiều nhất là 25. Ngay cả một mô phỏng đơn giản cũng đủ nhanh. Không cần cấu trúc dữ liệu nâng cao hoặc thủ thuật tối ưu hóa. Quét tuyến tính đơn giản trên mảng đã vừa vặn thoải mái trong giới hạn. 

Phần tinh tế duy nhất là chỉ định mỗi bài tập cho đúng nhóm cơ. Vì mô hình lặp lại sau mỗi ba bài tập nên vị trí modulo 3 sẽ xác định cơ mục tiêu. Việc triển khai bất cẩn có thể dễ dàng dịch chuyển ánh xạ đi một vì các chỉ số mảng trong lập trình thường dựa trên 0. 

Hãy xem xét đầu vào này:```
3
10 1 20
```Tổng số đúng là: 

- ngực = 10 
- bắp tay = 1 
- quay lại = 20 

Vậy câu trả lời là`back`. 

Nếu ai đó sử dụng`i % 3`không đúng với lối suy nghĩ dựa trên một người, họ có thể gán bài tập đầu tiên vào bắp tay thay vì ngực và đưa ra kết quả sai. 

Một sai lầm dễ mắc phải khác là quên rằng trình tự này tiếp tục theo chu kỳ. 

Ví dụ:```
5
1 2 3 4 5
```Các nhiệm vụ là: 

- ngực: bài tập 1 và 4 → 1 + 4 = 5 
- bắp tay: bài tập 2 và 5 → 2 + 5 = 7 
- lưng: bài tập 3 → 3 

Câu trả lời đúng là`biceps`. 

Việc triển khai có lỗi chỉ xử lý riêng ba vị trí đầu tiên sẽ không thành công ở đây. 

## Phương pháp tiếp cận 

Ý tưởng bạo lực là mô phỏng rõ ràng quá trình tập luyện của Greg. Đối với mỗi tư thế tập luyện, hãy xác định xem nó thuộc nhóm cơ nào và cộng số lần lặp lại vào tổng số cơ đó. Sau khi xử lý tất cả các bài tập, so sánh ba tổng và in ra tổng lớn nhất. 

Điều này đã hoạt động hoàn hảo trong giới hạn. Chúng tôi xử lý tối đa 20 số nên tổng công việc rất nhỏ. 

Một phiên bản brute-force kém hiệu quả hơn có thể liên tục xây dựng chuỗi tên cơ theo chu kỳ và sau đó đếm số lần lặp lại riêng cho từng nhóm. Thậm chí điều đó vẫn có thể vượt qua vì đầu vào quá nhỏ, nhưng nó gây ra sự phức tạp và công việc không cần thiết. 

Quan sát hữu ích là loại bài tập phụ thuộc hoàn toàn vào chỉ số modulo 3: 

- các chỉ số 0, 3, 6, ... thuộc về rương 
- các chỉ số 1, 4, 7,... thuộc bắp tay 
- chỉ số 2, 5, 8, ... thuộc về phía sau 

Khi chúng tôi nhận ra cấu trúc lặp lại này, giải pháp sẽ trở thành một đường tuyến tính duy nhất với ba bộ tích lũy. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n) | O(1) | Đã chấp nhận | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số nguyên`n`và mảng lặp lại. 
2. Tạo ba biến để lưu trữ tổng số lần lặp lại cho ngực, bắp tay và lưng. 
3. Lặp lại mảng bằng cách sử dụng chỉ mục dựa trên số 0. 
4. Đối với từng chỉ số: 

- nếu`i % 3 == 0`, thêm giá trị vào ngực 
- nếu`i % 3 == 1`, thêm giá trị vào bắp tay 
- nếu không, hãy thêm giá trị vào phía sau 

Hoạt động modulo hoạt động vì mẫu bài tập lặp lại sau mỗi ba vị trí. 
5. Sau khi xử lý tất cả các bài tập, hãy so sánh ba tổng. 
6. In tên cơ có tổng số lớn nhất. 

### Tại sao nó hoạt động 

Mỗi bài tập thuộc về chính xác một nhóm cơ và bài tập tuân theo một chu kỳ cố định có độ dài ba. Hoạt động modulo phân chia chính xác tất cả các chỉ mục thành ba nhóm đó. Trong quá trình quét, mỗi lần lặp lại được cộng chính xác một lần vào tổng số chính xác. Sau khi vòng lặp kết thúc, ba bộ tích lũy thể hiện tổng thời gian tập luyện cho mỗi nhóm cơ. Việc chọn mức tối đa trong số đó sẽ đưa ra câu trả lời cần thiết. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())
a = list(map(int, input().split()))

chest = 0
biceps = 0
back = 0

for i in range(n):
    if i % 3 == 0:
        chest += a[i]
    elif i % 3 == 1:
        biceps += a[i]
    else:
        back += a[i]

if chest > biceps and chest > back:
    print("chest")
elif biceps > chest and biceps > back:
    print("biceps")
else:
    print("back")
```Chương trình bắt đầu bằng cách đọc mảng đầu vào. Ba biến tích lũy theo dõi tổng số lần lặp lại cho từng nhóm cơ. 

Vòng lặp xử lý các bài tập theo thứ tự. Vì Python sử dụng tính năng lập chỉ mục dựa trên số 0 nên việc lập chỉ mục`0`tương ứng với bài tập đầu tiên là ngực. Đó là lý do tại sao`i % 3 == 0`bản đồ đến ngực chứ không phải bắp tay. 

Mẫu modulo lặp lại cứ sau ba chỉ số, khớp chính xác với chu trình tập luyện. 

Cuối cùng, chương trình so sánh tổng số và in ra số lớn nhất. Bài toán đảm bảo rằng đáp án là duy nhất nên chúng ta không cần xử lý các ràng buộc. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
2
2 8
```| Chỉ mục | Giá trị | Cơ bắp | Ngực | Bắp tay | Quay lại | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 2 | ngực | 2 | 0 | 0 | 
| 1 | 8 | bắp tay | 2 | 8 | 0 | 

Tổng số cuối cùng: 

- ngực = 2 
- bắp tay = 8 
- quay lại = 0 

Đầu ra:```
biceps
```Ví dụ này cho thấy chu kỳ có thể kết thúc trước khi cả ba nhóm cơ xuất hiện. Quay lại không nhận được bài tập nào cả, thuật toán xử lý một cách tự nhiên. 

### Ví dụ 2 

đầu vào:```
3
5 1 10
```| Chỉ mục | Giá trị | Cơ bắp | Ngực | Bắp tay | Quay lại | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 5 | ngực | 5 | 0 | 0 | 
| 1 | 1 | bắp tay | 5 | 1 | 0 | 
| 2 | 10 | trở lại | 5 | 1 | 10 | 

Tổng số cuối cùng: 

- ngực = 5 
- bắp tay = 1 
- trở lại = 10 

Đầu ra:```
back
```Dấu vết này thể hiện chu trình ba bước đầy đủ và xác nhận rằng ánh xạ modulo chỉ định chính xác từng bài tập. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi bài tập được xử lý một lần | 
| Không gian | O(1) | Chỉ có ba quầy được lưu trữ | 

Với`n ≤ 20`, thời gian chạy rất nhỏ. Giải pháp sử dụng bộ nhớ bổ sung liên tục và dễ dàng phù hợp với giới hạn của cuộc thi. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def solve():
    input = sys.stdin.readline

    n = int(input())
    a = list(map(int, input().split()))

    chest = 0
    biceps = 0
    back = 0

    for i in range(n):
        if i % 3 == 0:
            chest += a[i]
        elif i % 3 == 1:
            biceps += a[i]
        else:
            back += a[i]

    if chest > biceps and chest > back:
        print("chest")
    elif biceps > chest and biceps > back:
        print("biceps")
    else:
        print("back")

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    return sys.stdout.getvalue().strip()

# provided samples
assert run("2\n2 8\n") == "biceps", "sample 1"

# custom cases
assert run("1\n7\n") == "chest", "single exercise"
assert run("3\n1 2 10\n") == "back", "basic full cycle"
assert run("5\n1 2 3 4 5\n") == "biceps", "cycle repetition"
assert run("20\n25 1 1 25 1 1 25 1 1 25 1 1 25 1 1 25 1 1 25 1\n") == "chest", "maximum size style case"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 7`|`chest`| Đầu vào kích thước tối thiểu | 
|`3 / 1 2 10`|`back`| Ánh xạ modulo chính xác | 
|`5 / 1 2 3 4 5`|`biceps`| Xử lý chu trình lặp đi lặp lại | 
| Vỏ 20 phần tử lớn |`chest`| Đầu vào lớn hơn và tích lũy lặp đi lặp lại | 

## Vỏ cạnh 

Hãy xem xét đầu vào nhỏ nhất có thể:```
1
7
```Bài tập duy nhất là bài đầu tiên nên nó thuộc về ngực. Trong vòng lặp: 

- chỉ số 0 →`0 % 3 == 0`- ngực trở thành 7 

Tổng số cuối cùng là: 

- ngực = 7 
- bắp tay = 0 
- quay lại = 0 

Thuật toán in chính xác:```
chest
```Bây giờ hãy xem xét trường hợp chu trình lặp lại nhiều lần:```
5
1 2 3 4 5
```Xử lý từng bước: 

- chỉ số 0 → ngực += 1 
- chỉ số 1 → bắp tay += 2 
- chỉ số 2 → quay lại += 3 
- chỉ số 3 → ngực += 4 
- chỉ số 4 → bắp tay += 5 

Tổng số cuối cùng: 

- ngực = 5 
- bắp tay = 7 
- quay lại = 3 

Đầu ra là:```
biceps
```Điều này xác nhận rằng bài tập dựa trên modulo tiếp tục chính xác sau ba bài tập đầu tiên. 

Cuối cùng, hãy xem xét một trường hợp bộc lộ những sai lầm riêng lẻ:```
3
10 1 20
```Lập bản đồ chính xác: 

- bài tập đầu tiên → ngực 
- bài tập thứ hai → bắp tay 
- bài tập thứ ba → trở lại 

Tổng số trở thành: 

- ngực = 10 
- bắp tay = 1 
- quay lại = 20 

Đầu ra đúng là:```
back
```Nếu ai đó vô tình coi chỉ số 0 là bắp tay do nhầm lẫn dựa trên một, thì tất cả các tổng sẽ dịch chuyển không chính xác. Thuật toán được trình bày tránh điều đó bằng cách sử dụng lập chỉ mục dựa trên số 0 nhất quán xuyên suốt.
