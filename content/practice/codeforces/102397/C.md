---
title: "CF 102397C - Điểm kết thúc"
description: "Chúng ta bắt đầu tại một điểm lưới (x, y) và nhận được một chuỗi mô tả chuỗi các bước di chuyển của đơn vị. Mỗi ký tự thay đổi đúng một tọa độ: U tăng y, D giảm y, L giảm x và R tăng x."
date: "2026-08-11T05:04:57+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102397
codeforces_index: "C"
codeforces_contest_name: "Asu Coding Cup 4"
rating: 0
weight: 102397
solve_time_s: 226
verified: true
draft: false
---

[CF 102397C - Điểm kết thúc](https://codeforces.com/problemset/problem/102397/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 3 phút 46s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi bắt đầu tại một điểm lưới`(x, y)`và nhận được một chuỗi mô tả một chuỗi các bước di chuyển của đơn vị. Mỗi ký tự thay đổi chính xác một tọa độ:`U`tăng lên`y`,`D`giảm`y`,`L`giảm`x`, Và`R`tăng lên`x`. 

Nhiệm vụ chỉ đơn giản là áp dụng mọi chuyển động theo thứ tự nhất định và in điểm lưới cuối cùng. Bản thân đường dẫn không cần thiết sau khi các chuyển động của nó đã được xử lý. Vì câu trả lời được phép chứa tọa độ âm nên không có ranh giới nào trên lưới cần được thực thi. 

Tọa độ ban đầu thỏa mãn`1 <= x, y <= 100`, và độ dài đường dẫn tối đa là`100`. Các giới hạn này rất nhỏ, do đó, ngay cả một thuật toán có một vài thao tác trên mỗi ký tự cũng đủ nhanh. Quan trọng hơn, cấu trúc của bài toán mang lại cho chúng ta một lời giải tuyến tính rõ ràng: mỗi ký tự đóng góp một thay đổi độc lập cho một tọa độ, do đó không có lý do gì để xem lại các ký tự trước đó hoặc tìm kiếm qua các đường dẫn có thể. 

Việc triển khai bất cẩn vẫn có thể thất bại trong một số trường hợp ranh giới. Ví dụ, hãy xem xét```
1 1
L
```Câu trả lời đúng là```
0 1
```bởi vì`L`giảm`x`bởi một. Việc triển khai giả định tọa độ phải luôn dương có thể từ chối hoặc kẹp kết quả một cách không chính xác. 

Một ví dụ khác là```
5 5
DDDDD
```sản xuất```
5 0
```Tọa độ được phép đạt tới 0 và nó cũng có thể trở thành âm. điều trị`1`như giới hạn dưới sau khi đi bộ sẽ tạo ra kết quả không chính xác. 

Các chuyển động cũng có thể triệt tiêu lẫn nhau. Vì```
3 4
LRUD
```vị trí cuối cùng là```
3 4
```bởi vì`L`Và`R`hủy bỏ trên`x`phối hợp, trong khi`U`Và`D`hủy bỏ trên`y`điều phối. Giải pháp chỉ đếm số lần di chuyển mà không tôn trọng hướng đi của chúng sẽ làm mất thông tin này. 

## Phương pháp tiếp cận 

Một cách tiếp cận đơn giản nhưng tốn kém không cần thiết là xác định vị trí sau mỗi tiền tố của đường dẫn bằng cách quét tiền tố đó từ điểm bắt đầu. Sau khi xử lý ký tự đầu tiên, chúng tôi quét một ký tự. Sau khi xử lý ký tự thứ hai, chúng tôi quét lại hai ký tự, v.v. Điều này đúng vì mỗi lần quét mô phỏng trực tiếp tiền tố tương ứng nhưng các chuyển động giống nhau sẽ được xử lý lặp đi lặp lại. 

Nếu đường dẫn có độ dài`n`, điều này thực hiện`1 + 2 + 3 + ... + n = n(n + 1)/2`các thao tác của nhân vật. Với`n = 100`, đó là`5050`hoạt động, vẫn dễ dàng phù hợp với giới hạn nhất định. Điểm yếu của nó là về mặt khái niệm hơn là thực tế đối với những ràng buộc này: nó thực hiện những công việc lặp đi lặp lại hoàn toàn không cần thiết. 

Quan sát quan trọng là điểm cuối chỉ phụ thuộc vào tổng độ dịch chuyển của từng tọa độ. Chúng ta có thể xử lý đường dẫn một lần trong khi vẫn duy trì vị trí hiện tại. Bất cứ khi nào chúng ta nhìn thấy`U`hoặc`D`, chúng tôi cập nhật`y`; bất cứ khi nào chúng ta thấy`L`hoặc`R`, chúng tôi cập nhật`x`. Mỗi ký tự được tiêu thụ đúng một lần. 

Phiên bản brute-force hoạt động vì việc mô phỏng tiền tố nhiều lần cuối cùng sẽ đưa ra điểm cuối chính xác, nhưng nó không khai thác được thực tế là vị trí hiện tại đã chứa tất cả thông tin thu được từ các bước di chuyển trước đó. Quan sát rằng một cặp tọa độ hiện tại là đủ cho phép chúng ta thay thế các lần quét tiền tố lặp đi lặp lại bằng một lần quét trực tiếp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(1) | Được chấp nhận cho những ràng buộc này nhưng lặp đi lặp lại một cách không cần thiết | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc tọa độ xuất phát`x`Và`y`. Những giá trị này thể hiện vị trí trước khi bất kỳ chuyển động nào diễn ra. 
2. Đọc chuỗi đường dẫn`s`. Chúng ta sẽ xử lý các ký tự của nó từ trái sang phải vì thứ tự chuyển động quyết định điểm cuối. 
3. Đối với mỗi nhân vật trong`s`, cập nhật chính xác một tọa độ. Vì`U`, tăng`y`bởi một. Vì`D`, giảm bớt`y`bởi một. Vì`L`, giảm bớt`x`bởi một. Vì`R`, tăng`x`bởi một. Không có tọa độ nào khác sẽ thay đổi vì mọi chuyển động đều theo chiều ngang hoặc chiều dọc đúng một đơn vị. 
4. Sau khi tất cả các ký tự đã được xử lý, hãy in kết quả`x`Và`y`. Tại thời điểm này, mọi chuyển động đều góp phần dịch chuyển của nó vào tọa độ nên cặp hiện tại là vị trí của quán ăn. 

### Tại sao nó hoạt động 

Sau khi xử lý bất kỳ tiền tố nào của đường dẫn, hãy duy trì bất biến đó`(x, y)`chính xác là vị trí đạt được sau khi thực hiện tiền tố đó từ điểm bắt đầu ban đầu. Ban đầu tiền tố trống nên bất biến là đúng vì`(x, y)`là vị trí bắt đầu. Mỗi ký tự tiếp theo thay đổi vị trí hiện tại theo chuyển động được xác định bởi ký tự đó, do đó, bất biến vẫn đúng sau mỗi lần lặp. Khi toàn bộ chuỗi đã được xử lý, tiền tố là đường dẫn đầy đủ, nghĩa là`(x, y)`chính xác là điểm kết thúc cần thiết. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    x, y = map(int, input().split())
    s = input().strip()

    for move in s:
        if move == 'U':
            y += 1
        elif move == 'D':
            y -= 1
        elif move == 'L':
            x -= 1
        else:  # move == 'R'
            x += 1

    print(x, y)

if __name__ == "__main__":
    solve()
```Dòng đầu tiên đọc điểm bắt đầu trực tiếp vào`x`Và`y`, khớp với hệ tọa độ được sử dụng bởi các quy tắc chuyển động. 

Đường dẫn bị loại bỏ dòng mới ở cuối trước khi lặp lại. Sau đó, vòng lặp sẽ kiểm tra từng chuyển động chính xác một lần. Bốn trường hợp tương ứng trực tiếp với bốn thay đổi có thể xảy ra, do đó không có bảng tra cứu hoặc cấu trúc dữ liệu phụ trợ nào để duy trì. 

Không có kiểm tra ranh giới vì bài toán rõ ràng cho phép tọa độ cuối cùng âm. Thêm một điều kiện như`if x > 0`trước khi di chuyển sang trái sẽ làm thay đổi vấn đề và có thể đưa ra đáp án sai. 

Cũng không có nguy cơ tràn số nguyên trong Python. Ngay cả với độ dài đường dẫn tối đa là`100`, mỗi tọa độ có thể thay đổi nhiều nhất`100`từ giá trị ban đầu của nó, vì vậy giá trị thực tế rất nhỏ. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Đầu vào là```
5 3
UUUDLRLRLRR
```Bắt đầu từ`(5, 3)`, xử lý từng chuyển động theo thứ tự. 

| Bước | Di chuyển | x | y | 
| --- | --- | --- | --- | 
| 0 | Bắt đầu | 5 | 3 | 
| 1 | Bạn | 5 | 4 | 
| 2 | Bạn | 5 | 5 | 
| 3 | Bạn | 5 | 6 | 
| 4 | D | 5 | 5 | 
| 5 | L | 4 | 5 | 
| 6 | R | 5 | 5 | 
| 7 | L | 4 | 5 | 
| 8 | R | 5 | 5 | 
| 9 | L | 4 | 5 | 
| 10 | R | 5 | 5 | 
| 11 | R | 6 | 5 | 

Vị trí cuối cùng là`(6, 5)`, vì vậy đầu ra là```
6 5
```Việc lặp đi lặp lại`L`Và`R`các chuyển động chứng tỏ rằng thuật toán không cần xử lý đặc biệt để hủy bỏ. Mỗi chuyển động sẽ thay đổi vị trí hiện tại và tọa độ thu được sẽ chứa chuyển vị tích lũy một cách tự nhiên. 

### Mẫu 2 

Xem xét đầu vào hợp lệ```
2 3
LLDDRU
```Trạng thái thay đổi như sau. 

| Bước | Di chuyển | x | y | 
| --- | --- | --- | --- | 
| 0 | Bắt đầu | 2 | 3 | 
| 1 | L | 1 | 3 | 
| 2 | L | 0 | 3 | 
| 3 | D | 0 | 2 | 
| 4 | D | 0 | 1 | 
| 5 | R | 1 | 1 | 
| 6 | Bạn | 1 | 2 | 

Đầu ra cuối cùng là```
1 2
```Dấu vết này thực hiện tọa độ đạt đến 0 và sau đó di chuyển trở lại theo hướng ngược lại. Thuật toán không áp đặt ranh giới lưới nhân tạo nên mọi chuyển động đều được áp dụng chính xác theo quy định. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi ký tự của đường dẫn được xử lý một lần | 
| Không gian | O(1) | Chỉ cần hai tọa độ và chuỗi đầu vào | 

Với`n <= 100`, quá trình truyền tải tuyến tính thực hiện tối đa 100 cập nhật chuyển động. Nó thoải mái trong giới hạn thời gian 1,5 giây và sử dụng bộ nhớ không đáng kể so với giới hạn 256 MB. 

## Trường hợp thử nghiệm```python
import sys
import io

def solve():
    x, y = map(int, input().split())
    s = input().strip()

    for move in s:
        if move == 'U':
            y += 1
        elif move == 'D':
            y -= 1
        elif move == 'L':
            x -= 1
        else:
            x += 1

    print(x, y)

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    try:
        sys.stdin = io.StringIO(inp)
        sys.stdout = io.StringIO()
        solve()
        return sys.stdout.getvalue().strip()
    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout

# Provided sample
assert run("5 3\nUUUDLRLRLRR\n") == "6 5", "sample 1"

# Minimum-size input
assert run("1 1\nU\n") == "1 2", "single upward move"

# All moves are equal
assert run("5 5\nLLLLLLLLLL\n") == "-5 5", "ten left moves"

# Boundary and negative coordinate
assert run("1 1\nDDDD\n") == "1 -3", "negative y coordinate"

# Cancellation and order
assert run("100 100\nLRUD\n") == "100 100", "complete cancellation"

# Maximum-size path
assert run("100 100\n" + "R" * 100 + "\n") == "200 100", "maximum path length"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1`với`U`|`1 2`| Đường dẫn có kích thước tối thiểu và cập nhật tọa độ đơn | 
|`5 5`với mười`L`di chuyển |`-5 5`| Chuyển động lặp đi lặp lại và tiêu cực`x`| 
|`1 1`với bốn`D`di chuyển |`1 -3`| Vượt dưới 0 | 
|`100 100`với`LRUD`|`100 100`| Hủy bỏ chính xác các chuyển động ngược lại | 
|`100 100`với 100`R`di chuyển |`200 100`| Độ dài đường dẫn tối đa và cập nhật lặp lại | 

## Vỏ cạnh 

Trường hợp cạnh đầu tiên là chuyển động đưa vị trí về 0 hoặc thấp hơn. Vì```
1 1
L
```thuật toán bắt đầu với`(1, 1)`, quá trình`L`, và những thay đổi`x`ĐẾN`0`. Nó in`0 1`. Không có giới hạn ranh giới trong quá trình đi bộ nên tọa độ 0 là hợp lệ. 

Trường hợp cạnh thứ hai đang di chuyển ra ngoài vùng bắt đầu dương nhiều lần. Vì```
1 1
DDDD
```sự kế tiếp`y`giá trị là`0`,`-1`,`-2`, Và`-3`. Câu trả lời cuối cùng là`1 -3`. Một giải pháp ngăn tọa độ trở thành số âm sẽ dừng áp dụng đường dẫn một cách không chính xác. 

Trường hợp cạnh thứ ba là hủy bỏ hoàn toàn. Với```
3 4
LRUD
```hai bước đầu tiên thay đổi`x`từ`3`ĐẾN`2`và sau đó quay lại`3`. Hai bước tiếp theo thay đổi`y`từ`4`ĐẾN`5`và sau đó quay lại`4`. Kết quả cuối cùng là`3 4`, chính xác là điểm khởi đầu. Điều này xác nhận rằng bất biến theo dõi vị trí hiện tại thực tế thay vì chỉ đếm chuyển động. 

Cuối cùng, một đường đi chỉ chứa một hướng không cần trường hợp đặc biệt nào. Vì```
100 100
RRRR
```bốn`R`hoạt động chỉ đơn giản là tăng`x`từ`100`ĐẾN`104`, sản xuất`104 100`. Quy tắc cập nhật tương tự xử lý một bước di chuyển, các bước lặp lại và các đường dẫn hỗn hợp, đó là lý do tại sao việc triển khai vẫn ở mức nhỏ mà không làm mất đi tính chính xác.
