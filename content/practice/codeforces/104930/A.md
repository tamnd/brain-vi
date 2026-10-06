---
title: "CF 104930A - Lên Lên Xuống Xuống"
description: "Chúng ta được cung cấp một chuỗi cố định gồm 11 từ, mỗi từ đại diện cho một lần nhấn nút trong mã gian lận trò chơi. Riêng biệt, có một chuỗi tham chiếu đã biết, Mã Konami, cũng dài 11 đầu vào."
date: "2026-06-28T07:51:19+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104930
codeforces_index: "A"
codeforces_contest_name: "UTPC Contest 01-26-24 Div. 2 (Beginner)"
rating: 0
weight: 104930
solve_time_s: 58
verified: true
draft: false
---

[CF 104930A - Lên Lên Xuống Xuống](https://codeforces.com/problemset/problem/104930/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 58s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi cố định gồm 11 từ, mỗi từ đại diện cho một lần nhấn nút trong mã gian lận trò chơi. Riêng biệt, có một chuỗi tham chiếu đã biết, Mã Konami, cũng dài 11 đầu vào. 

Nhiệm vụ không phải là kiểm tra xem chuỗi đã cho có khớp chính xác với Mã Konami hay không mà là so sánh chúng theo từng vị trí và đếm xem có bao nhiêu vị trí khớp với nhau. Một vị trí sẽ đóng góp vào điểm nếu chuỗi tại chỉ mục đó trong đầu vào giống với chuỗi tương ứng trong Mã Konami. 

Kích thước đầu vào không đổi: chính xác là 11 chuỗi, mỗi chuỗi có độ dài tối đa 100. Điều này có nghĩa là tổng lượng dữ liệu cực kỳ nhỏ. Ở đây, bất kỳ thuật toán nào từ O(1) đến thậm chí O(n) với hằng số lớn đều nhanh chóng, nhưng cấu trúc gợi ý rằng chúng ta nên tập trung vào so sánh trực tiếp mà không cần bất kỳ cấu trúc dữ liệu phức tạp hoặc tiền xử lý nào. 

Các trường hợp cạnh chủ yếu là về hành vi bình đẳng chuỗi. Vì các phép so sánh phân biệt chữ hoa chữ thường và chính xác nên ngay cả những khác biệt nhỏ như ký tự thừa hoặc các từ khác nhau cũng phải được tính là không khớp. 

Một trường hợp tinh tế cần lưu ý rõ ràng là khi tất cả các chuỗi khớp nhau, chuỗi này sẽ trả về 11 và khi không có chuỗi nào khớp, chuỗi này sẽ trả về 0. Một trường hợp khác là chồng chéo một phần, trong đó chỉ có một vài vị trí khớp nhau và chúng ta phải đảm bảo tính bằng nhau trên mỗi chỉ mục thay vì bằng nhau dựa trên tập hợp. Sử dụng so sánh hoặc sắp xếp tập hợp sẽ phá hủy thông tin vị trí và cho kết quả không chính xác. 

## Phương pháp tiếp cận 

Một cách giải thích thô bạo sẽ là so sánh từng chuỗi đầu vào với từng vị trí trong Mã Konami, nhưng điều đó là không cần thiết vì cấu trúc đã căn chỉnh các vị trí một đối một. Ngay cả khi chúng tôi viết nó dưới dạng các vòng lặp lồng nhau, chúng tôi vẫn chỉ thực hiện tối đa các so sánh 11 × 11, điều này không đáng kể. 

Quan sát quan trọng là vấn đề giảm xuống chỉ còn một lần truyền qua các mảng đã căn chỉnh: tại chỉ mục i, chúng tôi so sánh đầu vào [i] với đích [i] và tăng bộ đếm nếu chúng khớp. Không có sự phụ thuộc giữa các vị trí, không cần băm và không chuyển đổi dữ liệu. 

Ý tưởng brute-force hoạt động vì so sánh trực tiếp là đủ, nhưng nó dư thừa về mặt khái niệm vì mỗi vị trí đầu vào có chính xác một vị trí mục tiêu tương ứng. Giải pháp tối ưu thu gọn mọi thứ thành một lần quét tuyến tính duy nhất. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (so sánh vòng lặp kép) | O(11²) | O(1) | Đã chấp nhận | 
| Tối ưu (so sánh một lượt) | O(11) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

### Trình tự tham chiếu Konami 

Chúng tôi coi mã tham chiếu là một mảng cố định:`["up","up","down","down","left","right","left","right","b","a","start"]`### bước 

1. Đọc 11 chuỗi đầu vào thành một mảng`s`. 

Điều này bảo tồn cấu trúc vị trí, điều này rất cần thiết vì các kết quả khớp phụ thuộc vào việc căn chỉnh chỉ mục. 
2. Xác định mảng tham chiếu`t`như chuỗi Mã Konami. 

Điều này tránh việc tính toán lại hoặc phân tích lại tham chiếu trong quá trình so sánh. 
3. Khởi tạo bộ đếm`score = 0`. 

Biến này tích lũy số lượng vị trí phù hợp. 
4. Lặp lại các chỉ số`i`từ 0 đến 10. 

Mỗi chỉ mục đại diện cho một vị trí nút cố định trong chuỗi mã gian lận. 
5. Đối với mỗi chỉ số`i`, so sánh`s[i]`với`t[i]`. 

Nếu chúng hoàn toàn bằng chuỗi, hãy tăng`score`. 
6. Sau khi hoàn thành hết 11 vị trí, xuất ra`score`. 

### Tại sao nó hoạt động 

Mỗi vị trí trong đầu vào tương ứng với chính xác một vị trí cố định trong chuỗi tham chiếu. Thuật toán tính toán bài kiểm tra tính bằng nhau cho mỗi chỉ mục và mỗi bài kiểm tra đóng góp độc lập vào số lượng cuối cùng. Vì không có vị trí nào ảnh hưởng đến vị trí khác nên việc tính tổng các trận đấu trên mỗi vị trí sẽ mang lại định nghĩa chính xác về Điểm gian lận. Không có cách giải thích nào khác về sự khớp mà bài toán cho phép, do đó số đếm vừa đầy đủ vừa chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    ref = ["up", "up", "down", "down", "left", "right", "left", "right", "b", "a", "start"]
    s = input().split()
    
    score = 0
    for i in range(11):
        if s[i] == ref[i]:
            score += 1
    
    print(score)

if __name__ == "__main__":
    solve()
```Giải pháp đọc toàn bộ dòng cùng một lúc bằng cách sử dụng`split()`, điều này an toàn vì định dạng đầu vào đảm bảo chính xác 11 mã thông báo được phân tách bằng dấu cách. 

Mảng tham chiếu được mã hóa cứng vì nó cố định và không phụ thuộc vào đầu vào. Điều này tránh việc tính toán hoặc thao tác chuỗi không cần thiết. 

Vòng lặp được giới hạn nghiêm ngặt ở 11 lần lặp, đảm bảo thực hiện liên tục. Phép so sánh sử dụng đẳng thức chuỗi trực tiếp, đây là thao tác hiệu quả và chính xác nhất ở đây. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
up up down down left right left right b a start
```| tôi | đầu vào s[i] | tài liệu tham khảo t[i] | trận đấu | điểm | 
| --- | --- | --- | --- | --- | 
| 0 | lên | lên | vâng | 1 | 
| 1 | lên | lên | vâng | 2 | 
| 2 | xuống | xuống | vâng | 3 | 
| 3 | xuống | xuống | vâng | 4 | 
| 4 | trái | trái | vâng | 5 | 
| 5 | đúng | đúng | vâng | 6 | 
| 6 | trái | trái | vâng | 7 | 
| 7 | đúng | đúng | vâng | 8 | 
| 8 | b | b | vâng | 9 | 
| 9 | một | một | vâng | 10 | 
| 10 | bắt đầu | bắt đầu | vâng | 11 | 

Đầu ra cuối cùng là 11, xác nhận sự trùng khớp hoàn hảo trên tất cả các vị trí. 

### Mẫu 2 

đầu vào:```
up down up down right left right left a b stop
```| tôi | đầu vào s[i] | tài liệu tham khảo t[i] | trận đấu | điểm | 
| --- | --- | --- | --- | --- | 
| 0 | lên | lên | vâng | 1 | 
| 1 | xuống | lên | không | 1 | 
| 2 | lên | xuống | không | 1 | 
| 3 | xuống | xuống | vâng | 2 | 
| 4 | đúng | trái | không | 2 | 
| 5 | trái | đúng | không | 2 | 
| 6 | đúng | trái | không | 2 | 
| 7 | trái | đúng | không | 2 | 
| 8 | một | b | không | 2 | 
| 9 | b | một | không | 2 | 
| 10 | dừng lại | bắt đầu | không | 2 | 

Đầu ra cuối cùng là 2, chỉ khớp với vị trí 0 và 3. 

Những dấu vết này cho thấy việc chấm điểm phụ thuộc hoàn toàn vào sự bình đẳng về vị trí hơn là thành viên hay tần suất. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(11) | Một lần duyệt qua mảng có độ dài cố định gồm 11 phần tử | 
| Không gian | O(1) | Chỉ có một số lượng biến không đổi và một mảng tham chiếu cố định | 

Thời gian chạy không đổi bất kể nội dung đầu vào, thấp hơn nhiều so với bất kỳ giới hạn thực tế nào. Việc sử dụng bộ nhớ cũng không đổi do không có cấu trúc động nào có thể chia tỷ lệ theo kích thước đầu vào. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import builtins
    return str(__import__('builtins').print.__self__ if False else __import__('builtins'))  # placeholder
```Một khai thác kiểm tra chính xác thường sẽ gọi`solve()`trực tiếp; giả sử cấu trúc đó:```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    import io as sio

    out = sio.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# provided samples
assert run("up up down down left right left right b a start") == "11"
assert run("up down up down right left right left a b stop") == "2"

# custom cases
assert run("up up down down left right left right b a start") == "11"
assert run("down down down down down down down down down down down") == "2"
assert run("left left left left left left left left left left left"
```
