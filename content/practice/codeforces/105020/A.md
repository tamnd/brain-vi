---
title: "CF 105020A - Tâm trạng"
description: "Chúng ta được đưa ra nhiều tình huống độc lập trong đó một người có một số tiền cố định và muốn mua đồ uống với một mức giá đã biết. Đối với mỗi kịch bản, chúng ta cần xác định cần thêm bao nhiêu tiền để số tiền sẵn có đủ trang trải chi phí."
date: "2026-06-28T01:56:22+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105020
codeforces_index: "A"
codeforces_contest_name: "TCPC Tunisian Collegiate Programming Contest 2022"
rating: 0
weight: 105020
solve_time_s: 77
verified: false
draft: false
---

[CF 105020A - Tâm trạng](https://codeforces.com/problemset/problem/105020/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 17s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được đưa ra nhiều tình huống độc lập trong đó một người có một số tiền cố định và muốn mua đồ uống với một mức giá đã biết. Đối với mỗi kịch bản, chúng ta cần xác định cần thêm bao nhiêu tiền để số tiền sẵn có đủ trang trải chi phí. Nếu số tiền hiện tại đã đủ hoặc vượt quá giá thì không cần phải vay gì cả. 

Mỗi trường hợp thử nghiệm chứa hai số nguyên. Đầu tiên là số tiền hiện có, và thứ hai là chi phí đồ uống. Đầu ra của mỗi trường hợp là sự thiếu hụt giữa chi phí và số tiền sẵn có, nhưng không bao giờ âm. 

Kích thước đầu vào có thể lớn tới 100.000 trường hợp thử nghiệm và mỗi giá trị có thể lên tới 100.000. Điều này ngay lập tức loại trừ bất kỳ phương pháp nào thực hiện công việc bổ sung cho mỗi trường hợp thử nghiệm vượt quá thời gian cố định. Bất kỳ vòng lặp hoặc mô phỏng lồng nhau nào cho mỗi trường hợp thử nghiệm sẽ vượt quá giới hạn thời gian, vì ngay cả O(t log t) cũng là chi phí không cần thiết ở đây và O(t) là mục tiêu. 

Hành vi của key edge xảy ra khi số tiền sẵn có đã đủ. Ví dụ: nếu x = 10 và y = 7 thì câu trả lời là 0, không phải -3. Việc triển khai ngây thơ in trực tiếp y - x mà không bảo vệ dấu sẽ tạo ra kết quả đầu ra âm không chính xác. Một trường hợp khó phát hiện khác là khi x bằng y chính xác thì đáp án cũng phải bằng 0. 

## Phương pháp tiếp cận 

Cách trực tiếp nhất để suy nghĩ về vấn đề này là hãy tưởng tượng việc vay đi vay lại một bảng mỗi lần cho đến khi tổng số tiền đạt đến mức chi phí cần thiết. Nếu x nhỏ hơn y, chúng ta cần cộng 1 nhiều lần cho đến khi đạt được đẳng thức. Cách giải thích bạo lực này đúng về mặt khái niệm nhưng không hiệu quả: trong trường hợp xấu nhất, khi x = 0 và y = 100.000, nó sẽ yêu cầu 100.000 bước cho mỗi trường hợp thử nghiệm, dẫn đến 10^10 thao tác ở kích thước đầu vào tồi tệ nhất. 

Cấu trúc của bài toán loại bỏ mọi nhu cầu mô phỏng vì câu trả lời cuối cùng chỉ phụ thuộc vào hiệu giữa hai số. Thay vì vay mượn tăng dần, chúng ta có thể trực tiếp tính toán chính xác mức thâm hụt. Nếu số tiền hiện tại đã đủ thì mức thâm hụt bằng 0. Ngược lại, nó chính xác là y - x. Quan sát này làm giảm toàn bộ quá trình thành một so sánh số học duy nhất cho mỗi trường hợp thử nghiệm. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(t · | y − x | ) | 
| Tối ưu | O(t) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng ca kiểm thử t, vì mỗi ca kiểm thử là độc lập và có thể được xử lý riêng biệt. 
2. Với mỗi test, đọc cặp (x, y), thể hiện số tiền sẵn có và chi phí cần thiết. 
3. So sánh x với y để xác định xem có thiếu hụt hay không. 
4. Nếu x lớn hơn hoặc bằng y, cho ra 0 vì không cần vay và khoản vay không thể âm. 
5. Ngược lại, xuất ra y - x, đo trực tiếp số tiền còn thiếu cần thiết để đạt được chi phí. 

### Tại sao nó hoạt động 

Đối với mỗi trường hợp thử nghiệm, kết quả chỉ phụ thuộc vào việc x có bao phủ y hay không. Nếu x ≥ y thì yêu cầu đã được thỏa mãn nên số tiền bổ sung tối thiểu bằng 0. Nếu x < y, mọi nghiệm hợp lệ đều phải tăng x lên ít nhất là y, và mức tăng nhỏ nhất đó chính xác là y - x. Không có sự kết hợp thay thế hoặc ràng buộc ẩn nào, vì vậy sự khác biệt này xác định duy nhất câu trả lời. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        x, y = map(int, input().split())
        if x >= y:
            print(0)
        else:
            print(y - x)

if __name__ == "__main__":
    solve()
```Giải pháp đọc tất cả các trường hợp thử nghiệm bằng cách sử dụng đầu vào nhanh để xử lý hiệu quả tới 100.000 dòng. Mỗi trường hợp được xử lý độc lập trong thời gian không đổi. Kiểm tra có điều kiện đảm bảo rằng chúng tôi không bao giờ xuất ra các giá trị âm, đây là lỗi triển khai phổ biến nhất đối với vấn đề này. 

Phép trừ y - x chỉ được thực hiện khi x < y, đảm bảo tính chính xác và tránh tính toán không cần thiết. Sự tách biệt này cũng làm cho logic trở nên rõ ràng và an toàn trước các trường hợp phức tạp như đẳng thức. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3
7 10
5 3
45 45
```| x | y | x ≥ y | Đầu ra | 
| --- | --- | --- | --- | 
| 7 | 10 | Không | 3 | 
| 5 | 3 | Có | 0 | 
| 45 | 45 | Có | 0 | 

Trường hợp đầu tiên cho thấy mức thâm hụt là 3. Trường hợp thứ hai và thứ ba cho thấy khi số tiền sẵn có đủ hoặc bằng nhau thì không cần vay mượn. 

### Ví dụ 2 

đầu vào:```
4
1 100
100 1
0 50
20 25
```| x | y | x ≥ y | Đầu ra | 
| --- | --- | --- | --- | 
| 1 | 100 | Không | 99 | 
| 100 | 1 | Có | 0 | 
| 0 | 50 | Không | 50 | 
| 20 | 25 | Không | 5 | 

Ví dụ này nhấn mạnh cả hai thái cực: thâm hụt lớn, không có tiền và các trường hợp không cần vay. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(t) | Mỗi trường hợp thử nghiệm được xử lý với một số thao tác không đổi: một lần đọc, một lần so sánh và một phép trừ. | 
| Không gian | O(1) | Không có cấu trúc dữ liệu bổ sung nào được sử dụng ngoài các biến đầu vào. | 

Quét tuyến tính qua các trường hợp kiểm thử là tối ưu vì mỗi cặp đầu vào phải được đọc ít nhất một lần. Các ràng buộc cho phép thực hiện tới 100.000 thao tác, nằm trong giới hạn thoải mái đối với Python có I/O nhanh. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from collections import deque
    output = []

    def solve():
        t = int(input())
        for _ in range(t):
            x, y = map(int, input().split())
            if x >= y:
                output.append("0")
            else:
                output.append(str(y - x))

    solve()
    return "\n".join(output)

# provided sample
assert run("3\n7 10\n5 3\n45 45\n") == "3\n0\n0"

# minimum values
assert run("2\n1 1\n1 2\n") == "0\n1"

# large deficit
assert run("1\n1 100000\n") == "99999"

# already enough money
assert run("3\n100 50\n50 50\n60 10\n") == "0\n0\n0"

# mixed cases
assert run("4\n0 5\n5 0\n10 20\n20 10\n") == "5\n0\n10\n0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1 / 1 2 | 0 / 1 | bình đẳng và thâm hụt nhỏ | 
| 1 100000 | 99999 | xử lý chênh lệch tối đa | 
| hỗn hợp | 0 / tích cực | phân nhánh đúng | 
| số không | 5/0/10/0 | trường hợp ranh giới và đảo ngược | 

## Vỏ cạnh 

Một trường hợp cạnh quan trọng là đẳng thức, trong đó x bằng y. Đối với đầu vào`10 10`, thuật toán kiểm tra`x >= y`và trực tiếp đưa ra 0. Điều này xác nhận rằng không cần vay khi số tiền khớp chính xác với chi phí. 

Một trường hợp khác là khi x bằng 0. Vì`0 50`, điều kiện`x >= y`thất bại và thuật toán tính toán`50 - 0 = 50`. Điều này đảm bảo xử lý chính xác số tiền ban đầu còn thiếu mà không cần vỏ bọc đặc biệt. 

Một trường hợp ngược lại như`100 1`kiểm tra rằng phép trừ không được áp dụng sai trong mọi trường hợp. Từ`x >= y`giữ, đầu ra được kẹp chính xác về 0 thay vì tạo ra giá trị âm.
