---
title: "CF 263A - Ma Trận Đẹp"
description: "Chúng ta được cung cấp một lưới 5 x 5 cố định chứa hầu hết các số 0 và chính xác một ô chứa một ô. Trong một lần di chuyển, chúng ta được phép hoán đổi các hàng liền kề hoặc hoán đổi các cột liền kề. Mỗi lần hoán đổi như vậy sẽ di chuyển toàn bộ hàng hoặc cột theo đúng một vị trí."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "implementation"]
categories: ["algorithms"]
codeforces_contest: 263
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 161 (Div. 2)"
rating: 800
weight: 263
solve_time_s: 91
verified: false
draft: false
---

[CF 263A - Ma trận đẹp](https://codeforces.com/problemset/problem/263/A) 

**Đánh giá:** 800 
**Thẻ:** triển khai 
**Thời gian giải:** 1 phút 31 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một lưới 5 x 5 cố định chứa hầu hết các số 0 và chính xác một ô chứa một ô. Trong một lần di chuyển, chúng ta được phép hoán đổi các hàng liền kề hoặc hoán đổi các cột liền kề. Mỗi lần hoán đổi như vậy sẽ di chuyển toàn bộ hàng hoặc cột theo đúng một vị trí. 

Mục tiêu là di chuyển một ô duy nhất vào ô trung tâm của lưới, ở vị trí (3, 3), sử dụng càng ít hoán đổi càng tốt. Vì mỗi lần di chuyển chỉ dịch chuyển một hàng hoặc cột một bước nên chi phí hoàn toàn được xác định bằng khoảng cách từ tâm đến điểm đó tính theo khoảng cách lưới, trong đó chúng ta chỉ di chuyển theo chiều dọc thông qua hoán đổi hàng và theo chiều ngang thông qua hoán đổi cột. 

Các ràng buộc có kích thước không đáng kể vì lưới luôn có 25 ô. Điều này loại trừ mọi nhu cầu về kỹ thuật tối ưu hóa hoặc cấu trúc dữ liệu. Bất kỳ giải pháp nào quét lưới một lần và tính toán một biểu thức số học nhỏ sẽ chạy ngay lập tức. 

Một sai lầm ngây thơ xuất phát từ việc cố gắng mô phỏng các giao dịch hoán đổi theo đúng nghĩa đen. Ví dụ: nếu giá trị ở (1, 5), mô phỏng có thể liên tục hoán đổi các hàng và cột cho đến khi đến tâm. Cách đó có hiệu quả, nhưng không cần thiết và có thể dễ dàng gây ra từng lỗi riêng lẻ hoặc logic kém hiệu quả mặc dù các ràng buộc là nhỏ. 

Một sai lầm tinh vi khác là hiểu lầm rằng việc hoán đổi hàng và cột là độc lập. Ai đó có thể cố gắng di chuyển theo đường chéo trong một bước không chính xác hoặc đếm các lần hoán đổi không chính xác khi trộn các thao tác hàng và cột trong một mô phỏng lặp lại. 

## Phương pháp tiếp cận 

Cách giải thích bạo lực là mô phỏng việc di chuyển từng bước về phía trung tâm, liên tục hoán đổi hàng hoặc cột cho đến khi đạt đến (3, 3). Mỗi lần hoán đổi sẽ giảm khoảng cách hàng hoặc khoảng cách cột chính xác một. Điều này đúng, nhưng nó làm phức tạp thêm vấn đề và có nguy cơ ghi sổ kế toán không chính xác nếu được mô phỏng một cách rõ ràng. 

Quan sát quan trọng là việc hoán đổi các hàng liền kề hoàn toàn tương đương với việc di chuyển một hàng lên hoặc xuống theo một ô và việc hoán đổi các cột liền kề hoàn toàn tương đương với việc di chuyển nó sang trái hoặc sang phải một ô. Vì hai chiều này không tương tác nên tổng số lần di chuyển chỉ đơn giản là khoảng cách Manhattan từ vị trí hiện tại của chiều đó đến tâm. 

Vì vậy, thay vì mô phỏng các phép toán, chúng tôi xác định vị trí (r, c) của một và tính xem nó cách (3, 3) bao xa. Mỗi đơn vị khoảng cách dọc tốn một lần hoán đổi hàng và mỗi đơn vị khoảng cách ngang tốn một lần hoán đổi cột. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng hoán đổi | O(k) | O(1) | Được chấp nhận nhưng không cần thiết | 
| Công thức khoảng cách Manhattan | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi giải quyết vấn đề bằng việc tìm tọa độ của một phần tử khác 0 và tính khoảng cách của nó đến tâm. 

1. Quét ma trận 5 x 5 để xác định ô (r, c) có giá trị là 1. Đây là thông tin có ý nghĩa duy nhất trong lưới. 
2. Tính khoảng cách theo chiều dọc là |r - 3|. Điều này đếm xem cần bao nhiêu lần hoán đổi hàng liền kề để đưa phần tử về hàng 3. 
3. Tính khoảng cách theo phương ngang là |c - 3|. Điều này đếm xem cần bao nhiêu lần hoán đổi cột liền kề để đưa phần tử về cột 3. 
4. Cộng hai khoảng cách để có tổng số bước di chuyển. 

Việc tách thành các đóng góp hàng và cột có hiệu quả vì việc hoán đổi hàng không bao giờ ảnh hưởng đến cột và hoán đổi cột không bao giờ ảnh hưởng đến các hàng, do đó hai chiều phát triển độc lập. 

### Tại sao nó hoạt động

Mỗi bước di chuyển sẽ thay đổi chính xác một tọa độ vị trí của vị trí đó ±1, hàng hoặc cột. Không có chuyển động nào ảnh hưởng đến cả hai tọa độ cùng một lúc. Điều này có nghĩa là mọi đường đi từ (r, c) đến (3, 3) trong biểu đồ phép toán này phải bao gồm chính xác |r - 3| hàng di chuyển và |c - 3| cột di chuyển theo một thứ tự nào đó. Bất kỳ sai lệch nào cũng sẽ không đạt được mục tiêu hoặc sử dụng thêm các bước di chuyển để hoàn tác tiến trình, vì vậy khoảng cách Manhattan vừa cần vừa đủ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    r = c = -1
    
    for i in range(5):
        row = list(map(int, input().split()))
        for j in range(5):
            if row[j] == 1:
                r, c = i, j
    
    print(abs(r - 2) + abs(c - 2))

if __name__ == "__main__":
    solve()
```Chương trình quét lưới một lần và ghi lại vị trí của một lưới. Các chỉ số được lưu trữ ở dạng dựa trên số 0, do đó tâm (3, 3) trở thành (2, 2). Câu trả lời cuối cùng được tính bằng tổng của sự khác biệt tuyệt đối ở cả hai chiều. 

Chi tiết triển khai phổ biến là quy ước chỉ mục. Báo cáo vấn đề sử dụng chỉ mục dựa trên 1, nhưng mã sử dụng chỉ mục dựa trên 0, do đó tâm chuyển từ (3, 3) sang (2, 2). Việc trộn lẫn các quy ước này là nguyên nhân chính gây ra các lỗi riêng lẻ. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
0 0 0 0 0
0 0 0 0 1
0 0 0 0 0
0 0 0 0 0
0 0 0 0 0
```Vị trí của 1 là (2, 4) trong lập chỉ mục dựa trên 0. 

| Bước | Hàng | Col | |r - 2| | |c - 2| | Tổng cộng | 

|------|------|-----|--------|--------|--------|--------| 

| Bắt đầu | 2 | 4 | 0 | 2 | 2 | 

Hàng đã được căn giữa nhưng cột cách đó hai bước, do đó cần phải hoán đổi hai cột. Điều này khẳng định rằng chỉ riêng chuyển động ngang đã xác định chi phí ở đây. 

### Ví dụ 2 

đầu vào:```
1 0 0 0 0
0 0 0 0 0
0 0 0 0 0
0 0 0 0 0
0 0 0 0 0
```Vị trí của 1 là (0, 0). 

| Bước | Hàng | Col | |r - 2| | |c - 2| | Tổng cộng | 

|------|------|-----|--------|--------|--------|--------| 

| Bắt đầu | 0 | 0 | 2 | 2 | 4 | 

Ở đây cả hàng và cột đều cách xa trung tâm và cả hai đóng góp đều cộng lại một cách độc lập. Điều này thể hiện sự độc lập của hai trục. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Kích thước lưới không đổi (25 ô), do đó quá trình quét bị giới hạn | 
| Không gian | O(1) | Chỉ có một vài biến số nguyên được lưu trữ | 

Giải pháp thỏa mãn một cách tầm thường các ràng buộc vì kích thước đầu vào không bao giờ vượt quá ma trận 5 x 5 cố định. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    
    input = sys.stdin.readline
    grid = []
    r = c = -1
    
    for i in range(5):
        row = list(map(int, input().split()))
        for j in range(5):
            if row[j] == 1:
                r, c = i, j
    
    return str(abs(r - 2) + abs(c - 2))

# provided sample
assert run("""0 0 0 0 0
0 0 0 0 1
0 0 0 0 0
0 0 0 0 0
0 0 0 0 0""") == "3"

# center already
assert run("""0 0 0 0 0
0 0 0 0 0
0 0 1 0 0
0 0 0 0 0
0 0 0 0 0""") == "0"

# top-left corner
assert run("""1 0 0 0 0
0 0 0 0 0
0 0 0 0 0
0 0 0 0 0
0 0 0 0 0""") == "4"

# bottom-right corner
assert run("""0 0 0 0 0
0 0 0 0 0
0 0 0 0 0
0 0 0 0 0
0 0 0 1 0""") == "4"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tế bào trung tâm | 0 | không cần di chuyển | 
| trên cùng bên trái | 4 | khoảng cách đối xứng tối đa | 
| dưới cùng bên phải | 4 | đối xứng góc đối diện | 

## Vỏ cạnh 

Trường hợp cạnh tiềm ẩn là khi cạnh đó đã ở trung tâm. Ví dụ: 

đầu vào:```
0 0 0 0 0
0 0 0 0 0
0 0 1 0 0
0 0 0 0 0
0 0 0 0 0
```Quá trình quét tìm thấy (2, 2). Tính toán cho |2 - 2| + |2 - 2| = 0. Không cần di chuyển và thuật toán xử lý việc này một cách tự nhiên mà không cần trường hợp đặc biệt. 

Một trường hợp cạnh khác là khi cái đó ở bất kỳ góc nào. Thuật toán xử lý tất cả các góc một cách thống nhất thông qua khoảng cách Manhattan. Đối với (0, 0), nó tính 2 + 2 = 4, phù hợp với thực tế là cần phải hoán đổi hai hàng và hai cột và không có chuỗi nào có thể thực hiện tốt hơn vì mỗi bước di chuyển chỉ thay đổi một tọa độ theo một bước.
