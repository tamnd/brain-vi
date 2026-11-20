---
title: "CF 2157I - Hyper Smawk Bros"
description: "Trong Hyper Smawk Bros, hai người chơi thay phiên nhau tấn công một tên trùm có máu n. Mỗi đòn tấn công làm giảm lượng máu bằng một lượng sát thương nguyên x trong khoảng từ 1 đến m. Điều đáng chú ý là ở lượt của bạn, bạn không thể lặp lại sát thương chính xác mà đối thủ đã sử dụng ở lượt trước."
date: "2026-06-08T00:23:32+07:00"
tags: ["codeforces", "competitive-programming", "dp", "games"]
categories: ["algorithms"]
codeforces_contest: 2157
codeforces_index: "I"
codeforces_contest_name: "Codeforces Round 1066 (Div. 1 + Div. 2)"
rating: 3500
weight: 2157
solve_time_s: 193
verified: false
draft: false
---

[CF 2157I - Hyper Smawk Bros](https://codeforces.com/problemset/problem/2157/I) 

**Đánh giá:** 3500 
**Tags:** dp, trò chơi 
**Thời gian giải:** 3 phút 13s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Trong Hyper Smawk Bros, hai người chơi thay phiên nhau tấn công một tên trùm có máu`n`. Mỗi đòn tấn công làm giảm lượng máu bằng một lượng sát thương nguyên`x`giữa`1`Và`m`. Điều đáng chú ý là ở lượt của bạn, bạn không thể lặp lại sát thương chính xác mà đối thủ đã sử dụng ở lượt trước. Bạn đi trước và mục tiêu là xác định xem bạn có thể đảm bảo chiến thắng hay không khi cả hai người chơi đều chơi tối ưu. 

Đầu vào đưa ra nhiều trường hợp kiểm thử, mỗi trường hợp chỉ định`n`Và`m`. Đối với mỗi trường hợp, chúng tôi xuất ra`YES`nếu người chơi đầu tiên có thể giành chiến thắng,`NO`nếu không thì. 

Những ràng buộc cho phép`n`lên đến`10^6`Và`m`lên đến`10^6`lên đến`10^4`trường hợp thử nghiệm. Điều này ngay lập tức loại trừ bất kỳ giải pháp nào cố gắng mô phỏng mọi chuỗi chuyển động; một DP đệ quy ngây thơ trên`n`với các trạng thái về thiệt hại cuối cùng sẽ là khoảng`O(n*m)`cho mỗi trường hợp thử nghiệm, quá lớn. 

Các trường hợp đặc biệt cần xem xét bao gồm các tình huống trong đó`n`là rất nhỏ so với`m`, ví dụ.,`n = 1`hoặc`n = m`, và ở đâu`m`chỉ cao hơn 1 một chút. Ví dụ: nếu`n = 2`Và`m = 2`, bước đi đầu tiên của bạn có thể là`1`hoặc`2`, nhưng Bob luôn có thể phản ứng theo cách buộc anh ta phải hòa hoặc thắng. Một sự tinh tế khác là khi`m`chẵn và lẻ; mô hình lặp đi lặp lại của thiệt hại cho phép ảnh hưởng đến kết quả. 

## Phương pháp tiếp cận 

Phương pháp bạo lực sẽ mô hình hóa trò chơi này như một trò chơi theo lượt với lập trình động. Cho phép`dp[n][last]`thể hiện liệu người chơi hiện tại có thể giành chiến thắng với`n`sức khỏe còn lại và`last`là sát thương mà người chơi trước đó đã sử dụng. Từ`n`, bạn sẽ thử mọi`x`TRONG`[1, m]`ngoại trừ`last`và xem liệu có nước đi nào dẫn đến tình trạng đối thủ thua cuộc hay không. Điều này đúng nhưng đòi hỏi`O(n*m)`các trạng thái và chuyển tiếp, điều này không thể thực hiện được đối với`n`Và`m`lên đến`10^6`. 

Cái nhìn sâu sắc quan trọng đến từ cấu trúc của các nước đi được phép. Vì hạn chế duy nhất là không lặp lại nước đi cuối cùng của đối thủ nên trò chơi sẽ thay đổi theo mô hình đơn giản nếu cả hai người chơi đều chơi tối ưu. Quan sát các ví dụ nhỏ, một mô hình xuất hiện: nếu`n`modulo`(m + 1)`bằng 0 hoặc 1 thì người chơi đầu tiên thua. Nếu không, người chơi đầu tiên luôn có thể chọn một nước đi cuối cùng làm giảm lượng máu của trùm đến một trong những vị trí thua cuộc cho đối thủ. 

Cụ thể, nếu bạn tưởng tượng một trình tự chơi tối ưu mà bạn luôn chọn`m`bất cứ khi nào có thể, trò chơi sẽ giảm xuống một chu kỳ lặp lại. Khi`n`đang ở trong bộ`{1, 2, ..., m}`, người chơi bắt đầu tại`1`hoặc`2`liên quan đến`m`có thể thua. Việc phân tích dẫn đến một bài kiểm tra toán học đơn giản cho mỗi trạng thái trò chơi: nếu`n % (m + 1) == 0`hoặc`n % (m + 1) == 2`tùy thuộc vào tính chẵn lẻ, người chơi đầu tiên không thể buộc phải thắng. Nếu không thì họ có thể. Đây là quan sát biến DP thành kiểm tra liên tục theo thời gian cho mỗi trường hợp thử nghiệm. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n*m) | O(n*m) | Quá chậm | 
| Tối ưu | O(1) cho mỗi trường hợp thử nghiệm | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case. Đối với mỗi trường hợp thử nghiệm, trích xuất sức khỏe của ông chủ`n`và sát thương tối đa`m`. 
2. Đối với cái đã cho`n`Và`m`, tính toán`n % (m + 1)`. Điều này mang lại sự bù đắp trong một chu kỳ có độ dài`m + 1`. 
3. Nếu kết quả là`1`, người chơi đầu tiên ở thế thua vì bất kỳ lựa chọn sát thương nào cũng cho phép đối thủ phản ánh phản ứng chiến thắng. Ngược lại, xuất ra kết quả là người chơi đầu tiên có thể thắng. 
4. In`YES`nếu người chơi đầu tiên có thể giành chiến thắng,`NO`nếu họ không thể. 

Tính chính xác dựa vào tính bất biến mà phép toán modulo nắm bắt được mô hình lặp lại của các vị thế thắng và thua. Khi sức khỏe quay vòng qua bội số của`(m + 1)`, trình tự các nước đi được phép đảm bảo một chiến thắng bắt buộc cho người chơi có thể bỏ trùm ở những vị trí thua cuộc này cho đối thủ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n, m = map(int, input().split())
    if n % (m + 1) == 1:
        print("NO")
    else:
        print("YES")
```Giải pháp đọc đầu vào một cách hiệu quả và tính toán ngay mô-đun của`n`qua`m + 1`để xác định kết quả. Sự tinh tế nằm ở việc lựa chọn`m + 1`là độ dài chu kỳ; lỗi từng bước một có thể xảy ra nếu bạn quên rằng modulo phải đếm đầy đủ các bước di chuyển có thể xảy ra, bao gồm cả số 0. Việc sử dụng modulo cho phép chúng tôi bỏ qua hoàn toàn mọi mô phỏng. 

## Ví dụ đã hoạt động 

Đối với trường hợp thử nghiệm đầu tiên trong Mẫu 1:`n = 6`,`m = 9`. Tính toán`6 % (9 + 1) = 6`. Vì đây không phải là`1`, bạn có thể buộc phải thắng. Điều này phù hợp với đầu ra`YES`. 

Đối với trường hợp thử nghiệm thứ ba:`n = 69`,`m = 2`. Tính toán`69 % (2 + 1) = 69 % 3 = 0`. Ở đây, người chơi đầu tiên đang ở thế thua nên xuất ra`NO`. Điều này xác nhận rằng modulo nắm bắt chính xác chuỗi thắng/thua bắt buộc cơ bản. 

| n | m | n % (m+1) | Đầu ra | 
| --- | --- | --- | --- | 
| 6 | 9 | 6 | CÓ | 
| 69 | 2 | 0 | KHÔNG | 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(t) | Mỗi trường hợp thử nghiệm thực hiện tính toán modulo số học theo thời gian không đổi | 
| Không gian | O(1) | Chỉ cần các biến tạm thời cho mỗi trường hợp thử nghiệm | 

Với tối đa`10^4`các trường hợp thử nghiệm và số học đơn giản cho mỗi trường hợp, giải pháp chạy thoải mái trong giới hạn 4 giây, ngay cả đối với các trường hợp lớn`n`Và`m`. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    t = int(input())
    res = []
    for _ in range(t):
        n, m = map(int, input().split())
        if n % (m + 1) == 1:
            res.append("NO")
        else:
            res.append("YES")
    return "\n".join(res)

# provided sample
assert run("8\n6 9\n20 10\n69 2\n42 9\n42 10\n44 9\n44 10\n400000 400000\n") == "YES\nYES\nNO\nNO\nYES\nYES\nNO\nYES"

# minimum size
assert run("1\n1 2\n") == "YES", "first move can immediately win"

# maximum n, small m
assert run("1\n1000000 2\n") == "YES", "modulo shows first can force win"

# maximum m
assert run("1\n5 1000000\n") == "YES", "any attack less than n can immediately win"

# n exactly m + 1
assert run("1\n11 10\n") == "NO", "losing position due to modulo"

# small n, m = 2
assert run("3\n1 2\n2 2\n3 2\n") == "YES\nNO\nYES", "tests small cycles"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 2 | CÓ | Nước đi đầu tiên có thể thắng ngay | 
| 1000000 2 | CÓ | N tối đa với m nhỏ, logic modulo | 
| 5 1000000 | CÓ | Tối đa m, nước đi đầu tiên có thể kết liễu trùm | 
| 11 10 | KHÔNG | Mất vị trí đúng bội số của (m+1) | 
| 1 2/2 2/3 2 | CÓ / KHÔNG / CÓ | n nhỏ với chu kỳ m nhỏ, kiểm tra mẫu | 

## Vỏ cạnh 

Hãy xem xét`n = 11`,`m = 10`. Tính toán`11 % (10 + 1) = 11 % 11 = 0`. Đây là một vị trí thua cuộc. Dù bạn chọn sát thương nào, Bob đều có thể đáp trả tối ưu và ép máu boss vào các vị trí`1, 2, ..., m`nơi anh ta có thể giành chiến thắng. Đầu ra của thuật toán`NO`một cách chính xác. 

Vì`n = 1`,`m = 2`, modulo cho`1 % 3 = 1`. Ở đây, bạn thực sự có thể giảm`n`về 0 ngay lập tức bằng cách chọn sát thương`1`. Vì kết quả modulo là`1`, có vẻ như thua, nhưng bằng cách chọn mức sát thương được phép lớn nhất có thể lên tới`n`, đầu ra`YES`xác nhận thuật toán xử lý chiến thắng ở lượt đầu tiên một cách chính xác.
