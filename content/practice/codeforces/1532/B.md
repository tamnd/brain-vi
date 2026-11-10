---
title: "CF 1532B - Nhảy ếch"
description: "Con ếch di chuyển dọc theo trục số thẳng bắt đầu từ vị trí 0. Chuyển động của nó hoàn toàn xác định: nó xen kẽ giữa hai kích thước bước cố định."
date: "2026-06-14T18:21:48+07:00"
tags: ["codeforces", "competitive-programming", "*special", "math"]
categories: ["algorithms"]
codeforces_contest: 1532
codeforces_index: "B"
codeforces_contest_name: "Kotlin Heroes: Practice 7"
rating: 0
weight: 1532
solve_time_s: 167
verified: true
draft: false
---

[CF 1532B - Nhảy ếch](https://codeforces.com/problemset/problem/1532/B) 

**Đánh giá:** - 
**Tags:** *đặc biệt, toán học 
**Thời gian giải:** 2m 47s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Con ếch di chuyển dọc theo trục số thẳng bắt đầu từ vị trí 0. Chuyển động của nó hoàn toàn xác định: nó xen kẽ giữa hai kích thước bước cố định. Ở lần di chuyển đầu tiên, nó đi đúng khoảng cách`a`, ở lần di chuyển thứ hai nó đi sang trái`b`, sau đó ngay bên cạnh`a`một lần nữa, sau đó rời đi`b`, v.v. cho đến khi nó thực hiện`k`nhảy. 

Mỗi truy vấn là độc lập và đối với mỗi truy vấn, chúng ta cần tọa độ cuối cùng sau một cách chính xác`k`những bước nhảy xen kẽ như vậy. 

Ràng buộc`k ≤ 10^9`ngay lập tức loại trừ việc mô phỏng từng bước nhảy. Mặc dù mỗi thao tác chỉ là phép cộng hoặc phép trừ đơn giản, nhưng việc thực hiện tới một tỷ thao tác cho mỗi truy vấn sẽ vượt quá mọi giới hạn thời gian thực tế. Với tối đa 1000 truy vấn, một mô phỏng đơn giản sẽ đạt tới 10^12 thao tác trong trường hợp xấu nhất, điều này là không khả thi. 

Khó khăn chính không chỉ ở độ phức tạp tính toán mà còn ở việc nhận ra rằng mô hình chuyển động có tính tuần hoàn với chu kỳ là 2, do đó quá trình có thể được nén thành việc đếm đầy đủ các cặp chuyển động. 

Một sai lầm thường gặp là xử lý sai cặp nước đi chưa hoàn thiện cuối cùng. Ví dụ, khi`k = 1`, chỉ ban đầu`+a`nên được áp dụng và không có phép trừ nào xảy ra. Khi`k = 2`, chuyển động ròng là`a - b`. Khi`k = 3`, nó trở thành`a - b + a`, vân vân. Bất kỳ cách tiếp cận nào chỉ giả định các cặp đầy đủ (tức là`k // 2 * (a - b)`) sẽ thất bại lẻ tẻ`k`nếu nó không tính đến bước nhảy về phía trước thêm. 

Một vấn đề tế nhị khác là tràn số nguyên trong các ngôn ngữ có số nguyên có chiều rộng cố định. Từ`a, b, k`có thể lên đến`10^9`, các giá trị trung gian như`k * a`có thể đạt được`10^18`, yêu cầu số nguyên chính xác tùy ý 64-bit hoặc Python. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực mô phỏng trực tiếp bước nhảy của ếch. Bắt đầu từ vị trí 0, chúng ta lặp từ 1 đến`k`, thêm`a`trên các bước lẻ và trừ`b`trên các bước chẵn. Điều này đúng vì nó tuân theo định nghĩa chính xác. Tuy nhiên, chi phí của nó là tuyến tính trong`k`, do đó, trên tất cả các truy vấn, nó có thể yêu cầu tới 10^12 thao tác, vượt xa giới hạn khả thi. 

Quan sát quan trọng là các bước nhảy diễn ra theo cặp lặp lại:`( +a, -b )`. Mỗi cặp đầy đủ đóng góp một chuyển động ròng của`a - b`. Nếu chúng ta nhóm chuỗi thành các cặp hoàn chỉnh, chúng ta sẽ nhận được`k // 2`những cặp như vậy. Những gì còn lại là có thể có thêm bước đi đầu tiên nếu`k`thật kỳ quặc. Động thái bổ sung đó luôn luôn là một`+a`. 

Vì vậy, toàn bộ quá trình giảm xuống việc tính toán có bao nhiêu cặp đầy đủ tồn tại và liệu có cần thêm một bước nhảy về phía trước hay không. Điều này giúp loại bỏ tất cả các lần lặp và giảm mỗi truy vấn về số học theo thời gian không đổi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(k) mỗi truy vấn | O(1) | Quá chậm | 
| Tối ưu | O(1) mỗi truy vấn | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số nguyên`a`,`b`, Và`k`cho mỗi truy vấn. Chúng xác định kích thước bước và số lần di chuyển để mô phỏng. 
2. Tính xem có bao nhiêu chu trình hai bước hoàn chỉnh bằng cách sử dụng`k // 2`. Mỗi chu kỳ chứa một`+a`và một`-b`, vì vậy nó đóng góp một sự dịch chuyển ròng của`a - b`. 
3. Nhân số chu kỳ đầy đủ với`(a - b)`để nhận được sự đóng góp từ tất cả các nước đi được ghép nối. Điều này nén tất cả hành vi được lập chỉ mục chẵn thành một biểu thức số học. 
4. Nếu`k`lẻ quá, thêm một cái nữa`+a`để tính đến bước nhảy về phía trước không ghép đôi cuối cùng. Điều này là cần thiết vì trình tự luôn bắt đầu bằng một bước tiến. 
5. Xuất kết quả tổng. 

### Tại sao nó hoạt động 

Trình tự các nước đi được tính tuần hoàn nghiêm ngặt với độ dài 2 sau nước đi đầu tiên được xem xét. Mỗi cặp`( +a, -b )`có hiệu ứng ròng cố định độc lập với vị trí hoặc lịch sử. Vì phép cộng có tính kết hợp nên việc nhóm các phép toán thành các cặp rời rạc không làm thay đổi kết quả cuối cùng. Ngoại lệ duy nhất là khi độ dài chuỗi là số lẻ, trong trường hợp đó thao tác không ghép đôi cuối cùng luôn là thao tác ban đầu.`+a`bước chân. Điều này đảm bảo công thức xây dựng lại chính xác tổng tiền tố đầy đủ của chuỗi xen kẽ vô hạn bị cắt cụt tại`k`. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
out = []

for _ in range(t):
    a, b, k = map(int, input().split())
    
    pairs = k // 2
    ans = pairs * (a - b)
    
    if k % 2 == 1:
        ans += a
    
    out.append(str(ans))

print("\n".join(out))
```Giải pháp trực tiếp thực hiện phân rã dẫn xuất. biểu thức`pairs * (a - b)`mã hóa tất cả các chu kỳ hoàn chỉnh, trong khi điều kiện`if k % 2 == 1`xử lý bước nhảy đầu tiên còn sót lại. Kết quả được tích lũy dưới dạng chuỗi để tránh lặp lại chi phí I/O bên trong vòng lặp. 

Cần phải cẩn thận trong việc duy trì thứ tự hoạt động chính xác: tính toán`k // 2`đầu tiên tránh sự phân chia lặp đi lặp lại và nhóm`(a - b)`đảm bảo chúng tôi không vô tình trộn lẫn nội dung đóng góp từ các bước khác nhau. 

## Ví dụ đã hoạt động 

Chúng tôi theo dõi hai truy vấn đại diện. 

### Ví dụ 1 

đầu vào:`a = 5, b = 2, k = 3`| Bước | Hành động | Đóng góp | Vị trí | 
| --- | --- | --- | --- | 
| 1 | +5 | +5 | 5 | 
| 2 | -2 | -2 | 3 | 
| 3 | +5 | +5 | 8 | 

Điều này cho thấy một cặp đầy đủ cộng thêm một bước nhảy về phía trước. Công thức cho`k // 2 = 1`, Vì thế`1 * (5 - 2) = 3`, cộng thêm`5`, cho`8`. 

Điều này xác nhận việc xử lý phân tách lẻ`k`một cách chính xác. 

### Ví dụ 2 

đầu vào:`a = 100, b = 1, k = 4`| Bước | Hành động | Đóng góp | Vị trí | 
| --- | --- | --- | --- | 
| 1 | +100 | +100 | 100 | 
| 2 | -1 | -1 | 99 | 
| 3 | +100 | +100 | 199 | 
| 4 | -1 | -1 | 198 | 

Có hai cặp đầy đủ, mỗi cặp đóng góp`99`, vậy tổng số là`198`. Không còn bước nhảy nào tồn tại kể từ`k`là chẵn. 

Điều này xác nhận logic ghép nối nắm bắt hoàn toàn cấu trúc lặp lại mà không cần mô phỏng rõ ràng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(t) | Mỗi truy vấn được xử lý trong thời gian số học không đổi | 
| Không gian | O(1) | Chỉ một số biến số nguyên được sử dụng bất kể kích thước đầu vào | 

Giải pháp dễ dàng nằm trong giới hạn vì thậm chí`t = 1000`kết quả chỉ trong 1000 phép tính thời gian không đổi. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    t = int(input())
    out = []
    for _ in range(t):
        a, b, k = map(int, input().split())
        pairs = k // 2
        ans = pairs * (a - b)
        if k % 2 == 1:
            ans += a
        out.append(str(ans))
    return "\n".join(out)

# provided samples
assert run("6\n5 2 3\n100 1 4\n1 10 5\n1000000000 1 6\n1 1 1000000000\n1 1 999999999\n") == "8\n198\n-17\n2999999997\n0\n1"

# minimum k
assert run("1\n10 5 1\n") == "10"

# even k simple cancellation
assert run("1\n3 3 10\n") == "0"

# large imbalance
assert run("1\n1 1000000000 2\n") == "-999999999"

# odd k boundary
assert run("1\n7 2 7\n") == "21"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`k = 1`| đơn`a`| trường hợp cạnh tối thiểu | 
| bình đẳng`a, b`| 0 hoặc`a`nếu lẻ | hành vi hủy bỏ | 
| lớn`b`| âm lớn | số học an toàn tràn | 
| số lẻ`k`| thêm`a`bao gồm | xử lý chẵn lẻ | 

## Vỏ cạnh 

Khi nào`k = 1`, thuật toán tính toán`k // 2 = 0`, do đó đóng góp của cặp là 0 và vì`k`thật kỳ lạ, nó nói thêm`a`. Ví dụ`a = 7, b = 2, k = 1`sản xuất`7`, phù hợp với định nghĩa bước nhảy đơn. 

Khi`a = b`, mỗi cặp đầy đủ đóng góp không cho chuyển động ròng. Vì`a = 3, b = 3, k = 5`, chúng ta nhận được hai cặp đầy đủ cho 0 và thêm một`+3`, dẫn đến`3`, phù hợp với trình tự rõ ràng`3 - 3 + 3 - 3 + 3`. 

Khi`k`chẵn và lớn, chẳng hạn như`k = 10^9`, mọi chuyển động đều được ghi lại bởi`k // 2`cặp. Không cần xử lý đặc biệt nào ngoài phép nhân số nguyên, vì Python hỗ trợ các số nguyên lớn một cách tự nhiên và công thức vẫn ổn định.
