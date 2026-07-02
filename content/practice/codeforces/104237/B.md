---
title: "CF 104237B - Nút giao đường"
description: "Chúng ta được cho một tập hợp các đường thẳng vô hạn nằm trên một mặt phẳng. Mỗi đường thẳng đứng, biểu thị một đại lộ có tọa độ x cố định hoặc nằm ngang, biểu thị một đường phố có tọa độ y cố định."
date: "2026-07-01T23:19:19+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104237
codeforces_index: "B"
codeforces_contest_name: "Harker Programming Invitational 2023 Novice"
rating: 0
weight: 104237
solve_time_s: 60
verified: true
draft: false
---

[CF 104237B - Nút giao đường](https://codeforces.com/problemset/problem/104237/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một tập hợp các đường thẳng vô hạn nằm trên một mặt phẳng. Mỗi đường thẳng đứng, biểu thị một đại lộ có tọa độ x cố định hoặc nằm ngang, biểu thị một đường phố có tọa độ y cố định. Mỗi đường thẳng đứng cắt mỗi đường ngang đúng một lần và mỗi đường ngang như vậy được tính là một giao điểm. 

Nhiệm vụ là tính toán có bao nhiêu điểm giao nhau như vậy tồn tại trong số tất cả các đường đã cho. 

Kích thước đầu vào tối đa là 1000 dòng. Điều này ngay lập tức gợi ý rằng cách tiếp cận bậc hai có thể chấp nhận được, vì giải pháp kiểm tra tất cả các cặp đường thực hiện tối đa khoảng 10^6 thao tác, nằm trong giới hạn thoải mái trong Python. 

Một vài trường hợp góc quan trọng ở đây. Nếu tất cả các đường đều thẳng đứng thì không có đường ngang nào giao nhau, vì vậy câu trả lời phải bằng 0. Điều tương tự cũng xảy ra đối xứng nếu tất cả các đường đều nằm ngang. Một trường hợp tinh vi khác là khi có đúng một đường thẳng đứng hoặc một đường ngang. Trong tình huống đó, câu trả lời bằng với kích thước của nhóm đối diện và việc triển khai sai khi cố gắng ghép các dòng cùng loại sẽ tạo ra số lượng bổ sung không chính xác. 

Một sai lầm ngây thơ nhưng phổ biến là tính các giao điểm bằng cách so sánh từng cặp đường thẳng và tăng dần khi tọa độ khác nhau, điều này không liên quan về mặt logic với hình học của bài toán. Giao điểm chỉ xảy ra giữa các đường vuông góc, không xảy ra giữa các cặp tùy ý. 

## Phương pháp tiếp cận 

Ý tưởng mạnh mẽ là xem xét từng cặp đường thẳng và kiểm tra xem chúng có giao nhau hay không. Tuy nhiên, hầu hết các cặp đều không liên quan: hai đường thẳng đứng không bao giờ cắt nhau và hai đường ngang không bao giờ cắt nhau. Các tương tác có ý nghĩa duy nhất là giữa đường thẳng đứng và đường ngang. 

Nếu chúng ta tách đầu vào thành hai nhóm, đường dọc và đường ngang thì vấn đề sẽ đơn giản hóa đáng kể. Mọi đường thẳng đứng tại x = a cắt mọi đường thẳng ngang tại y = b đúng một lần. Điều đó có nghĩa là mỗi cặp đóng góp chính xác một giao điểm. 

Vì vậy, thay vì kiểm tra tất cả các cặp trong số N dòng, chúng ta chỉ cần đếm xem có bao nhiêu dòng dọc và bao nhiêu dòng ngang. Câu trả lời cuối cùng chính là sản phẩm của họ. 

Cách tiếp cận bạo lực vẫn sẽ lặp lại trên tất cả các cặp dòng và loại kiểm tra, dẫn đến kiểm tra O(N²). Phiên bản được tối ưu hóa sẽ giảm bớt vấn đề đếm tần số trong O(N). 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(N2) | O(1) | Quá chậm nhưng không cần thiết | 
| Tối ưu | O(N) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng ta chuyển bài toán hình học thành việc đếm hai loại đường thẳng. 

1. Đọc số dòng N và khởi tạo hai bộ đếm, một cho dòng dọc và một cho dòng ngang. Sự tách biệt này là cần thiết vì sự giao nhau chỉ xảy ra giữa các danh mục. 
2. Đối với mỗi dòng, hãy đọc loại của nó. Nếu nó thẳng đứng, hãy tăng bộ đếm dọc. Nếu nó nằm ngang, hãy tăng bộ đếm ngang. Giá trị tọa độ thực tế không liên quan vì mọi đường thẳng đứng đều cắt mọi đường ngang bất kể vị trí. 
3. Sau khi xử lý tất cả các dòng, hãy tính số lượng giao điểm dưới dạng Vertical_count nhân với Horizontal_count. Điều này xuất phát trực tiếp từ thực tế là mỗi đường thẳng đứng tạo thành một giao điểm với mọi đường ngang. 
4. Xuất kết quả. 

### Tại sao nó hoạt động

Mỗi đường thẳng đứng được xác định bởi tọa độ x cố định và kéo dài vô tận theo cả hai hướng. Mỗi đường ngang được xác định bởi tọa độ y cố định. Đối với bất kỳ cặp được chọn nào bao gồm một đường thẳng đứng và một đường ngang, giao điểm của chúng được đảm bảo và duy nhất vì chúng xác định một điểm duy nhất (x, y). Vì không có đường trùng lặp nên mỗi cặp như vậy đóng góp chính xác một giao điểm và không có loại cặp nào khác đóng góp gì cả. Điều này thiết lập sự tương ứng một-một giữa các điểm giao nhau và các cặp đường thẳng đứng và nằm ngang theo thứ tự. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    v = 0
    h = 0
    
    for _ in range(n):
        t, x = input().split()
        if t == 'v':
            v += 1
        else:
            h += 1
    
    print(v * h)

if __name__ == "__main__":
    solve()
```Giải pháp duy trì hai bộ đếm đơn giản trong khi quét đầu vào một lần. Các giá trị tọa độ được đọc nhưng cố tình bỏ qua vì chúng không ảnh hưởng đến việc liệu giao lộ có tồn tại hay không; chỉ có định hướng mới quan trọng. 

Phép nhân ở cuối mã hóa tích Descartes giữa hai tập hợp dòng. Một lỗi triển khai thường gặp là cố gắng lưu trữ tọa độ và kiểm tra sự bằng nhau hoặc gần nhau, điều này là không cần thiết và sẽ làm phức tạp logic một cách không chính xác. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
5
h 3
h 2
h 1
v 2
v 1
```Chúng tôi theo dõi số lượng đường ngang và dọc. 

| Bước | Dòng | Ngang | Dọc | 
| --- | --- | --- | --- | 
| 1 | giờ 3 | 1 | 0 | 
| 2 | giờ 2 | 2 | 0 | 
| 3 | h 1 | 3 | 0 | 
| 4 | v 2 | 3 | 1 | 
| 5 | v 1 | 3 | 2 | 

Kết quả cuối cùng là 3 × 2 = 6. 

Điều này phù hợp với trực giác rằng mỗi trong số 2 đường thẳng đứng cắt nhau trong số 3 đường ngang. 

### Mẫu 2 

đầu vào:```
3
v 10
v -5
v 7
```| Bước | Dòng | Ngang | Dọc | 
| --- | --- | --- | --- | 
| 1 | v 10 | 0 | 1 | 
| 2 | v -5 | 0 | 2 | 
| 3 | v 7 | 0 | 3 | 

Kết quả cuối cùng là 0 × 3 = 0. 

Điều này xác nhận trường hợp cạnh không tồn tại đường ngang, do đó không thể giao nhau. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N) | Mỗi dòng được xử lý chính xác một lần với các bản cập nhật liên tục | 
| Không gian | O(1) | Chỉ có hai bộ đếm số nguyên được duy trì bất kể kích thước đầu vào | 

Các ràng buộc cho phép tối đa 1000 dòng và giải pháp này thực hiện một lượt tuyến tính duy nhất với các thao tác tầm thường trên mỗi dòng, giúp thực hiện dễ dàng trong cả giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# provided sample
assert run("""5
h 3
h 2
h 1
v 2
v 1
""") == "6"

# all vertical
assert run("""4
v 1
v 2
v 3
v 4
""") == "0"

# all horizontal
assert run("""3
h 10
h 20
h 30
""") == "0"

# mixed small
assert run("""2
h 5
v 7
""") == "1"

# balanced
assert run("""6
h 1
h 2
v 1
v 2
v 3
h 3
""") == "9"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả theo chiều dọc | 0 | trường hợp cạnh không có đường ngang | 
| tất cả nằm ngang | 0 | trường hợp cạnh đối xứng | 
| trộn nhỏ | 1 | sự chính xác của giao lộ đơn | 
| cân bằng | 9 | hành vi sản phẩm Descartes đầy đủ | 

## Vỏ cạnh 

Khi tất cả các dòng đều thẳng đứng, thuật toán sẽ đọc từng dòng và chỉ tăng bộ đếm dọc. Bộ đếm ngang vẫn bằng 0 nên tích bằng 0. Ví dụ: 

đầu vào:```
2
v 1
v 2
```Các tập thực thi v = 2, h = 0, cho 0 giao điểm, phù hợp với thực tế là các đường thẳng đứng song song không bao giờ gặp nhau. 

Khi có chính xác một đường thẳng đứng và nhiều đường ngang, hãy nói:```
3
v 5
h 1
h 2
```Bộ đếm trở thành v = 1 và h = 2. Kết quả là 2, tương ứng với một đường thẳng đứng giao nhau với cả hai đường ngang đúng một lần.
