---
title: "CF 1997C - Vị trí chẵn"
description: "Chúng ta được cung cấp một chuỗi dấu ngoặc có độ dài chẵn, nhưng mọi ký tự ở vị trí lẻ đều bị xóa. Chỉ còn lại dấu ngoặc ở vị trí 2, 4, 6,...."
date: "2026-06-08T14:34:57+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "data-structures", "greedy"]
categories: ["algorithms"]
codeforces_contest: 1997
codeforces_index: "C"
codeforces_contest_name: "Educational Codeforces Round 168 (Rated for Div. 2)"
rating: 1100
weight: 1997
solve_time_s: 140
verified: true
draft: false
---

[CF 1997C - Vị trí chẵn](https://codeforces.com/problemset/problem/1997/C) 

**Đánh giá:** 1100 
**Tags:** thuật toán xây dựng, cấu trúc dữ liệu, tham lam 
**Thời gian giải:** 2 phút 20s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi dấu ngoặc có độ dài chẵn, nhưng mọi ký tự ở vị trí lẻ đều bị xóa. Chỉ còn lại dấu ngoặc ở vị trí 2, 4, 6,.... 

Trình tự ban đầu được đảm bảo là trình tự khung thông thường (RBS) và luôn có ít nhất một lần khôi phục hợp lệ. Chúng ta có thể thay thế mỗi dấu gạch dưới ở vị trí lẻ bằng một trong hai`(`hoặc`)`. 

Đối với bất kỳ RBS đã hoàn thiện nào, chi phí của nó được xác định bằng tổng khoảng cách giữa các cặp dấu ngoặc phù hợp. Nếu dấu ngoặc mở ở vị trí`l`được khớp với dấu ngoặc đóng ở vị trí`r`, cặp đó đóng góp`r - l`đến tổng chi phí. 

Trong số tất cả các phục hồi hợp lệ, chúng ta phải tìm ra chi phí tối thiểu có thể. 

Độ dài của một trường hợp thử nghiệm có thể đạt tới`2 · 10^5`và tổng chiều dài của tất cả các ca kiểm thử cũng nhiều nhất là`2 · 10^5`. Bất kỳ giải pháp nào kiểm tra tất cả các phục hồi đều không thể thực hiện được vì có`2^(n/2)`cách lấp đầy những vị trí còn thiếu. Thậm chí một`O(n²)`thuật toán sẽ quá đắt ở giới hạn trên. Chúng tôi cần một cái gì đó tuyến tính hoặc gần tuyến tính cho mỗi trường hợp thử nghiệm. 

Một phần tinh tế của vấn đề là việc giảm thiểu chi phí không giống như việc chỉ xây dựng bất kỳ RBS hợp lệ nào. Các phục hồi hợp lệ khác nhau có thể tạo ra các cấu trúc phù hợp khác nhau. 

Coi như:```
n = 4
_(_)
```Sự phục hồi`(())`có chi phí`3 + 1 = 4`. 

Nếu chúng ta cố gắng tham lam tạo ra các cặp mà không tôn trọng tính hợp lệ, chúng ta có thể nghĩ sai`()()`là có thể, điều này sẽ có chi phí`2`, nhưng các vị trí chẵn cố định ngăn cản sự phục hồi đó. 

Một lỗi dễ mắc phải khác là tính toán các cặp khớp một cách rõ ràng sau khi xây dựng chuỗi. Điều đó đúng, nhưng có một quan sát đơn giản hơn nhiều về chi phí dẫn trực tiếp đến câu trả lời. 

Ví dụ:```
_)_)_)_)
```Sự phục hồi hợp lệ duy nhất là:```
()()()()
```Chi phí là`1 + 1 + 1 + 1 = 4`. 

Chiến lược tái thiết tạo ra sự lồng ghép không cần thiết sẽ làm tăng chi phí và tạo ra câu trả lời sai. 

## Phương pháp tiếp cận 

Ý tưởng vũ phu rất đơn giản. Đối với mỗi vị trí lẻ, hãy thử cả hai dấu ngoặc có thể, tạo tất cả các khôi phục, chỉ giữ lại các chuỗi dấu ngoặc thông thường, tính toán chi phí của chúng và lấy mức tối thiểu. 

Điều này hiệu quả vì nó trực tiếp kiểm tra mọi câu trả lời khả thi. Vấn đề là số lần phục hồi. có`n/2`vị trí bị xóa, do đó không gian tìm kiếm là`2^(n/2)`. Với`n = 200000`, cái này lớn về mặt thiên văn. 

Để đạt được thứ gì đó nhanh hơn, chúng ta cần hiểu chi phí thực sự đo lường được điều gì. 

Giả sử có dấu ngoặc mở ở vị trí`l`khớp với dấu ngoặc đóng ở vị trí`r`. Đóng góp của nó là:```
r - l
```Tổng tất cả các cặp phù hợp sẽ cho:```
(sum of closing positions) - (sum of opening positions)
```Mỗi vị trí thuộc về chính xác một cặp khớp, ở dạng dấu ngoặc mở hoặc dấu ngoặc đóng. Do đó, khi chúng ta biết vị trí nào có dấu ngoặc mở thì chi phí sẽ được xác định hoàn toàn. 

Vì vậy, vấn đề trở thành: 

> Xây dựng một chuỗi dấu ngoặc hợp lệ có tổng các vị trí mở càng lớn càng tốt. 

Tại sao lớn nhất? Bởi vì:```
cost = constant - 2 * (sum of opening positions)
```Tổng số vị trí là cố định, do đó việc giảm thiểu chi phí tương đương với việc tối đa hóa tổng số vị trí được chiếm giữ bởi các dấu ngoặc mở. 

Bây giờ hãy nhìn vào trình tự từ trái sang phải. 

Tại mỗi vị trí lẻ ta chọn một dấu ngoặc. 

Nếu số dư hiện tại bằng 0, chúng ta buộc phải đặt`(`. Đặt`)`sẽ ngay lập tức làm cho tiền tố không hợp lệ. 

Nếu số dư dương thì đặt`)`luôn tốt hơn là đặt`(`bởi vì nó giữ các khe hở càng xa về bên phải càng tốt. Vì các vị trí lẻ được xử lý từ trái sang phải nên sử dụng`)`bất cứ khi nào được phép sẽ trì hoãn việc mở các chỉ số lớn hơn trong tương lai, làm tăng tổng số vị thế mở. 

Điều này mang lại một công trình tham lam rất đơn giản: 

Nếu số dư bằng 0, đặt`(`. 

Nếu không thì đặt`)`. 

Các vị trí chẵn được cố định và xử lý bình thường. 

Sau khi xây dựng trình tự tối ưu, chúng ta vẫn cần chi phí tối thiểu. Chúng ta có thể tính toán nó mà không cần tìm các cặp phù hợp một cách rõ ràng. 

Bất cứ khi nào chúng ta đặt dấu ngoặc mở ở vị trí`i`, thêm vào`i`đến một tổng số đang chạy. 

Bất cứ khi nào chúng ta đặt dấu ngoặc đóng ở vị trí`i`, trừ`i`. 

Giá trị cuối cùng chính xác là:```
(sum closing positions) - (sum opening positions)
```đó là chi phí mong muốn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^(n/2) · n) | O(n) | Quá chậm | 
| Tham lam tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo`balance = 0`Và`cost = 0`. 
2. Quét vị trí từ trái sang phải. 
3. Nếu vị trí là số lẻ thì dấu ngoặc không xác định. 

Nếu như`balance == 0`, địa điểm`(`bởi vì mọi RBS hợp lệ đều phải giữ mọi tiền tố không âm. 

Nếu không thì đặt`)`bởi vì việc sử dụng dấu ngoặc đóng hiện sẽ trì hoãn việc mở các vị thế lớn hơn trong tương lai, giúp giảm thiểu chi phí cuối cùng. 
4. Nếu vị trí chẵn, hãy sử dụng dấu ngoặc cố định từ đầu vào. 
5. Sau khi xác định được dấu ngoặc ở vị trí hiện tại: 

Nếu nó là`(`, tăng`balance`bằng một và trừ vị trí của nó khỏi`cost`. 

Nếu nó là`)`, giảm bớt`balance`bởi một và thêm vị trí của nó vào`cost`. 
6. Tiếp tục cho đến khi toàn bộ chuỗi được xử lý. 
7. Đầu ra`cost`. 

### Tại sao nó hoạt động 

Quan sát chính là chi phí bằng:```
(sum of closing positions) - (sum of opening positions)
```Tổng số tiền của tất cả các vị trí là cố định, do đó việc giảm thiểu chi phí tương đương với việc tối đa hóa tổng số vị trí mở. 

Bất cứ khi nào số dư hiện tại dương, cả hai`(`Và`)`giữ cho trình tự có khả năng hợp lệ. Lựa chọn`)`tránh tạo dấu ngoặc mở ở vị trí hiện tại, nhỏ hơn. Bất kỳ khung mở nào trở nên cần thiết sau này sẽ xuất hiện ở vị trí lớn hơn, làm tăng tổng số vị trí mở. 

Khi số dư bằng 0,`)`là không thể vì nó sẽ vi phạm điều kiện tiền tố RBS. Như vậy`(`bị ép buộc. 

Sự lựa chọn tham lam là tối ưu cục bộ ở mọi vị trí lẻ và không bao giờ làm giảm tính khả thi trong tương lai. Vì mỗi dấu ngoặc mở được đẩy sang phải xa nhất có thể, nên chuỗi kết quả sẽ tối đa hóa tổng các vị trí mở và do đó giảm thiểu chi phí. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())

    for _ in range(t):
        n = int(input())
        s = input().strip()

        balance = 0
        cost = 0

        for i in range(1, n + 1):
            if i % 2 == 1:
                if balance == 0:
                    ch = '('
                else:
                    ch = ')'
            else:
                ch = s[i - 1]

            if ch == '(':
                balance += 1
                cost -= i
            else:
                balance -= 1
                cost += i

        print(cost)

if __name__ == "__main__":
    solve()
```Thuật toán không bao giờ xây dựng lại toàn bộ chuỗi một cách rõ ràng vì chỉ có dấu ngoặc hiện tại mới quan trọng. 

Biến`balance`lưu trữ số dư chuỗi khung thông thường. Các vị trí lẻ được chọn một cách tham lam dựa trên số dư này. 

Việc tính toán chi phí sử dụng danh tính có được trước đó. Mọi vị trí mở đều đóng góp tiêu cực và mọi vị trí đóng đều đóng góp tích cực. Điều này tránh hoàn toàn việc kết hợp dựa trên ngăn xếp. 

Một chi tiết dễ mắc sai lầm là việc lập chỉ mục. Vấn đề sử dụng các vị trí dựa trên 1 và công thức chi phí phụ thuộc vào số lượng vị trí thực tế. Vòng lặp chạy từ`1`ĐẾN`n`, và sự đóng góp sử dụng`i`trực tiếp. 

Một điểm tinh tế khác là số dư phải được cập nhật ngay sau khi quyết định khung hiện tại. Các vị thế lẻ trong tương lai phụ thuộc vào số dư hiện tại. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
6
_(_)_)
```Sự phục hồi tối ưu là`(())()`. 

| Vị trí | Khung được chọn | Cân Bằng Sau | Chi phí | 
| --- | --- | --- | --- | 
| 1 | ( | 1 | -1 | 
| 2 | ( | 2 | -3 | 
| 3 | ) | 1 | 0 | 
| 4 | ) | 0 | 4 | 
| 5 | ( | 1 | -1 | 
| 6 | ) | 0 | 5 | 

Câu trả lời cuối cùng:```
5
```Dấu vết này cho thấy sự cai trị tham lam đang diễn ra. Tại vị trí 3 số dư dương nên ta chọn`)`và tránh tạo khung mở quá sớm. 

### Ví dụ 2 

đầu vào:```
8
_)_)_)_)
```| Vị trí | Khung được chọn | Cân Bằng Sau | Chi phí | 
| --- | --- | --- | --- | 
| 1 | ( | 1 | -1 | 
| 2 | ) | 0 | 1 | 
| 3 | ( | 1 | -2 | 
| 4 | ) | 0 | 2 | 
| 5 | ( | 1 | -3 | 
| 6 | ) | 0 | 3 | 
| 7 | ( | 1 | -4 | 
| 8 | ) | 0 | 4 | 

Câu trả lời cuối cùng:```
4
```Số dư liên tục trở về 0, buộc mọi vị trí lẻ trở thành`(`. Sự phục hồi hợp lệ duy nhất là`()()()()`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi vị trí được xử lý một lần | 
| Không gian | O(1) | Chỉ có một số biến số nguyên được duy trì | 

Tổng chiều dài của tất cả các trường hợp thử nghiệm nhiều nhất là`2 · 10^5`, do đó thuật toán thực hiện tổng thể khoảng hai trăm nghìn lần lặp. Điều này dễ dàng phù hợp với giới hạn 2 giây và sử dụng bộ nhớ không đáng kể. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    def solve():
        input = sys.stdin.readline
        t = int(input())
        ans = []

        for _ in range(t):
            n = int(input())
            s = input().strip()

            balance = 0
            cost = 0

            for i in range(1, n + 1):
                if i % 2 == 1:
                    ch = '(' if balance == 0 else ')'
                else:
                    ch = s[i - 1]

                if ch == '(':
                    balance += 1
                    cost -= i
                else:
                    balance -= 1
                    cost += i

            ans.append(str(cost))

        return "\n".join(ans)

    return solve()

# provided sample
assert run(
"""4
6
_(_)_)
2
_)
8
_)_)_)_)
8
_(_)_(_)
"""
) == "5\n1\n4\n8"

# minimum size
assert run(
"""1
2
_)
"""
) == "1"

# nested pair forced
assert run(
"""1
4
_(_)
"""
) == "4"

# alternating sequence
assert run(
"""1
8
_)_)_)_)
"""
) == "4"

# larger valid case
assert run(
"""1
8
_(_)_(_)
"""
) == "8"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2, _)`|`1`| Ví dụ nhỏ nhất có thể | 
|`4, _(_)`|`4`| Buộc làm tổ | 
|`8, _)_)_)_)`|`4`| Đặt lại số dư nhiều lần | 
|`8, _(_)_(_)`|`8`| Dấu ngoặc cố định hỗn hợp và quyết định tham lam | 

## Vỏ cạnh 

### Số dư thường xuyên về 0 

đầu vào:```
1
8
_)_)_)_)
```Sau mỗi vị trí chẵn, số dư sẽ trở về 0. Thuật toán buộc phải đặt`(`ở mọi vị trí kỳ lạ. Trình tự kết quả là:```
()()()()
```với chi phí`4`. 

Một chiến lược bất cẩn luôn cố gắng đặt`)`đầu tiên sẽ ngay lập tức tạo tiền tố không hợp lệ. 

### Lồng sâu bắt buộc bởi các vị trí cố định 

đầu vào:```
1
4
_(_)
```Vị trí 2 được cố định là`(`và vị trí 4 được cố định là`)`. 

Thuật toán tạo ra:```
(())
```Các cặp phù hợp là`(1,4)`Và`(2,3)`, cho biết chi phí:```
3 + 1 = 4
```Đang cố gắng tạo ra`()()`là không thể vì vị trí 2 không thể thay đổi. 

###Đôi đơn 

đầu vào:```
1
2
_)
```Số dư bắt đầu từ số 0, vì vậy vị trí 1 phải là`(`. 

Trình tự trở thành:```
()
```Thuật toán tính toán:```
-1 + 2 = 1
```phù hợp với khoảng cách cặp khung duy nhất có thể. Điều này xác minh trường hợp ranh giới`n = 2`.
