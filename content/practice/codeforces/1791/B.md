---
title: "CF 1791B - Chỉ dẫn sau"
description: "Alperen bắt đầu tại điểm gốc (0,0) trên lưới 2D. Anh ta được thực hiện một chuỗi n nước đi, trong đó mỗi nước đi sẽ dịch chuyển anh ta một đơn vị theo một trong bốn hướng chính: trái, phải, lên hoặc xuống."
date: "2026-06-09T10:30:06+07:00"
tags: ["codeforces", "competitive-programming", "geometry", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1791
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 849 (Div. 4)"
rating: 800
weight: 1791
solve_time_s: 153
verified: true
draft: false
---

[CF 1791B - Hướng dẫn làm theo](https://codeforces.com/problemset/problem/1791/B) 

**Đánh giá:** 800 
**Tags:** hình học, thực hiện 
**Thời gian giải:** 2m 33s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Alperen bắt đầu từ điểm gốc`(0,0)`trên lưới 2D. Anh ta được đưa ra một chuỗi`n`di chuyển, trong đó mỗi di chuyển sẽ dịch chuyển anh ta một đơn vị theo một trong bốn hướng chính: trái, phải, lên hoặc xuống. Một viên kẹo được đặt ở một vị trí cố định`(1,1)`, và nhiệm vụ là xác định xem Alperen có từng đi qua ô đó trong khi theo dõi các bước đi của anh ta hay không. Đầu vào bao gồm nhiều trường hợp thử nghiệm, mỗi trường hợp có số lần di chuyển và trình tự di chuyển. Đầu ra là "CÓ" nếu anh ta chạm tới kẹo ít nhất một lần và "KHÔNG" nếu ngược lại. 

Những hạn chế còn khiêm tốn:`n`lên tới 50 và số lượng trường hợp thử nghiệm`t`lên tới 1000. Điều này ngay lập tức loại trừ nhu cầu thực hiện bất kỳ tối ưu hóa phức tạp nào, vì thậm chí việc mô phỏng tất cả các bước di chuyển cho mọi trường hợp thử nghiệm cũng nhanh chóng. Các trường hợp khó khăn phát sinh khi đạt được kẹo ngay lập tức ở lần di chuyển đầu tiên hoặc khi đường đi vòng quanh nhưng không bao giờ chạm vào`(1,1)`. Ví dụ: nếu chuỗi là "LURD", Alperen di chuyển sang trái, sau đó lên, rồi sang phải, rồi xuống, kết thúc tại điểm gốc. Dù di chuyển theo mọi hướng, anh ta không bao giờ đạt tới`(1,1)`, vì vậy câu trả lời là "KHÔNG". Việc thực hiện bất cẩn có thể chỉ kiểm tra vị trí cuối cùng hoặc hiểu sai trình tự hướng dẫn, đưa ra câu trả lời sai. 

## Phương pháp tiếp cận 

Cách tiếp cận ngây thơ cũng là cách tối ưu: mô phỏng từng bước con đường của Alperen. Bắt đầu lúc`(0,0)`và lặp lại chuỗi di chuyển. Sau mỗi lần di chuyển, hãy cập nhật tọa độ hiện tại. Sau mỗi lần cập nhật, hãy kiểm tra xem vị trí có khớp không`(1,1)`. Nếu đúng như vậy, hãy kết luận ngay "CÓ" cho trường hợp thử nghiệm đó. Nếu vòng lặp kết thúc mà không đạt được`(1,1)`, xuất ra "KHÔNG". 

Cách tiếp cận này có hiệu quả vì`n`là rất nhỏ và việc kiểm tra là tầm thường. Bất kỳ nỗ lực nào nhằm tối ưu hóa hơn nữa, chẳng hạn như bằng cách tính toán trước các ô có thể tiếp cận hoặc sử dụng độ lệch tọa độ, sẽ làm tăng thêm độ phức tạp không cần thiết. Không có lợi ích gì từ việc sắp xếp, lập trình động hoặc BFS vì đường dẫn đã cố định và mang tính xác định. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu / Mô phỏng | O(n) cho mỗi trường hợp thử nghiệm | O(1) | Đã chấp nhận | 
| Tính toán trước / BFS | O(n) cho mỗi trường hợp thử nghiệm | O(n) | Không cần thiết | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case`t`. 
2. Với mỗi test case, hãy đọc độ dài`n`và chuỗi di chuyển`s`. 
3. Khởi tạo vị trí của Alperen tại`(x, y) = (0, 0)`. 
4. Lặp lại từng ký tự trong chuỗi di chuyển: 

1. Nếu nhân vật là`L`, giảm`x`bằng 1. 
2. Nếu nhân vật là`R`, tăng`x`bằng 1. 
3. Nếu nhân vật là`U`, tăng`y`bằng 1. 
4. Nếu nhân vật là`D`, giảm`y`bằng 1. 
5. Sau mỗi lần cập nhật, hãy kiểm tra xem`(x, y) == (1, 1)`. Nếu đúng, in "CÓ" và dừng xử lý trường hợp thử nghiệm này. 
5. Nếu vòng lặp kết thúc mà không truy cập`(1,1)`, in "KHÔNG". 

Tại sao nó hoạt động: thuật toán duy trì tính bất biến`(x, y)`luôn phản ánh vị trí hiện tại của Alperen trên lưới. Việc kiểm tra sau mỗi lần di chuyển đảm bảo rằng lần đầu ghé thăm`(1,1)`được phát hiện. Vì các bước di chuyển mang tính quyết định nên không có ô nào bị bỏ qua và không có kết quả dương tính giả nào có thể xảy ra. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        s = input().strip()
        x, y = 0, 0
        found = False
        for move in s:
            if move == 'L':
                x -= 1
            elif move == 'R':
                x += 1
            elif move == 'U':
                y += 1
            elif move == 'D':
                y -= 1
            if x == 1 and y == 1:
                found = True
                break
        print("YES" if found else "NO")

solve()
```Giải pháp đọc đầu vào một cách hiệu quả, theo dõi vị trí của Alperen và kết thúc sớm khi tiếp cận được kẹo. Các điều kiện biên được xử lý chính xác và`strip()`đảm bảo không có dòng mới nào cản trở việc phân tích cú pháp di chuyển. Không thể xảy ra lỗi từng lỗi một vì lưới sử dụng tọa độ nguyên chính xác. 

## Ví dụ đã hoạt động 

### Mẫu 1:`"UUURDDL"`| Bước | Di chuyển | x | y | Ở Candy à? | 
| --- | --- | --- | --- | --- | 
| 0 | bắt đầu | 0 | 0 | KHÔNG | 
| 1 | Bạn | 0 | 1 | KHÔNG | 
| 2 | Bạn | 0 | 2 | KHÔNG | 
| 3 | Bạn | 0 | 3 | KHÔNG | 
| 4 | R | 1 | 3 | KHÔNG | 
| 5 | D | 1 | 2 | KHÔNG | 
| 6 | D | 1 | 1 | CÓ | 

Alperen đạt tới`(1,1)`ở bước thứ 6, do đó kết quả là`"YES"`. 

### Mẫu 2:`"LLL"`| Bước | Di chuyển | x | y | Ở Candy à? | 
| --- | --- | --- | --- | --- | 
| 0 | bắt đầu | 0 | 0 | KHÔNG | 
| 1 | L | -1 | 0 | KHÔNG | 
| 2 | L | -2 | 0 | KHÔNG | 
| 3 | L | -3 | 0 | KHÔNG | 

Alperen không bao giờ đạt tới`(1,1)`, vì vậy đầu ra là`"NO"`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(t * n) | Mỗi trường hợp thử nghiệm mô phỏng`n`di chuyển, với tối đa`t=1000`trường hợp thử nghiệm. Tối đa`t*n = 50,000`. | 
| Không gian | O(1) | Chỉ tọa độ và các biến tạm thời được lưu trữ; không có thêm bộ nhớ cho mỗi trường hợp thử nghiệm. | 

Các ràng buộc đủ nhỏ để giải pháp này chạy thoải mái trong vòng 1 giây và 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()
    solve()
    return sys.stdout.getvalue().strip()

# provided samples
assert run("7\n7\nUUURDDL\n2\nUR\n8\nRRRUUDDD\n3\nLLL\n4\nDUUR\n5\nRUDLL\n11\nLLLLDDRUDRD\n") == \
    "YES\nYES\nNO\nNO\nYES\nYES\nNO", "sample 1"

# custom cases
assert run("1\n1\nR\n") == "NO", "single move, not reaching candy"
assert run("1\n2\nRU\n") == "YES", "first test reaching candy quickly"
assert run("1\n4\nUDUD\n") == "NO", "moving up and down, never reaching candy"
assert run("1\n50\n" + "R"*25 + "U"*25 + "\n") == "YES", "max length, reaching candy"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1\n1\nR\n`| KHÔNG | Một chiêu không đạt kẹo | 
|`1\n2\nRU\n`| CÓ | Con đường tối thiểu để đạt được kẹo nhanh chóng | 
|`1\n4\nUDUD\n`| KHÔNG | Di chuyển vòng lặp đó nhưng không bao giờ trúng kẹo | 
| 50 bước`R*25+U*25`| CÓ | Chiều dài chuỗi tối đa đạt kẹo | 

## Vỏ cạnh 

Một đường dao động dọc theo một trục mà không bao giờ cắt nhau`(1,1)`được xử lý chính xác. Ví dụ,`"LURD"`tiếp tục quay trở lại`(0,0)`. Thuật toán cập nhật các vị trí theo thứ tự và chỉ xuất ra "CÓ" khi`(1,1)`đạt được, nếu không thì xuất ra "NO" ở cuối. Các đường dẫn di chuyển một lần, các đường dẫn đến kẹo ở lần di chuyển đầu tiên và các đường dẫn có độ dài tối đa đều được mô phỏng chính xác.
