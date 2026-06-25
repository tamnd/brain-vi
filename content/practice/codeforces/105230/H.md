---
title: "CF 105230H - Núi"
description: "Chúng ta được cung cấp đường dẫn một chiều được mã hóa dưới dạng chuỗi + và -. Chúng tôi bắt đầu ở độ cao ngầm định bằng 0 trước khi xử lý chuỗi. Mỗi nhân vật di chuyển chiều cao hiện tại đúng một đơn vị: + tăng chiều cao thêm một, - giảm chiều cao đi một."
date: "2026-06-24T16:00:36+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105230
codeforces_index: "H"
codeforces_contest_name: "2024-2025 ICPC Bolivia Pre-National Contest"
rating: 0
weight: 105230
solve_time_s: 70
verified: true
draft: false
---

[CF 105230H - Núi](https://codeforces.com/problemset/problem/105230/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 10s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một đường dẫn một chiều được mã hóa dưới dạng một chuỗi`+`Và`-`. Chúng tôi bắt đầu ở độ cao ngầm định bằng 0 trước khi xử lý chuỗi. Mỗi ký tự di chuyển chiều cao hiện tại đúng một đơn vị:`+`tăng chiều cao lên một,`-`giảm nó đi một. Sau khi xử lý từng ký tự, chúng ta thu được một chuỗi các độ cao dọc theo đường dẫn, một độ cao cho mỗi vị trí trong chuỗi. 

Nhiệm vụ là tìm vị trí trong chuỗi nơi chiều cao đạt giá trị tối đa trong toàn bộ chuyến đi. Các vị trí được lập chỉ mục 1 và tham chiếu đến ký tự trong chuỗi dẫn đến chiều cao đó. 

Vì vậy, vấn đề về cơ bản là: mô phỏng tổng tiền tố trên một mảng ±1 và báo cáo vị trí sớm nhất nơi xảy ra tổng tiền tố tối đa, với sự đảm bảo rằng mức tối đa này là duy nhất. 

Ràng buộc`|s| ≤ 10^5`ngụ ý rằng mọi giải pháp đều phải tuyến tính hoặc gần tuyến tính. Mô phỏng O(n²), chẳng hạn như tính toán lại tổng tiền tố hoặc quét từ mỗi vị trí, sẽ quá chậm vì nó sẽ liên quan đến tối đa 10¹⁰ thao tác trong trường hợp xấu nhất. 

Quét tuyến tính trong O(n) là đủ, vì các thao tác 10⁵ là không đáng kể dưới giới hạn 1 giây trong Python. 

Trường hợp cạnh tinh tế phát sinh khi đạt được chiều cao tối đa ngay lập tức. Ví dụ, đầu vào`+----`đạt đến độ cao 1 ở vị trí 1 và không bao giờ vượt quá nó nữa, vì vậy câu trả lời là 1. Một trường hợp khác là khi đạt được mức tối đa ở cuối, chẳng hạn như`++++`, trong đó câu trả lời là 4. Một cách tiếp cận ngây thơ chỉ theo dõi độ cao cuối cùng sẽ thất bại ở đây, vì đỉnh không nhất thiết phải ở cuối hành trình đi bộ. 

## Phương pháp tiếp cận 

Ý tưởng đơn giản là mô phỏng bước đi và tính toán độ cao sau mỗi bước. Chúng tôi duy trì tổng hiện hành, bắt đầu từ 0 và cập nhật nó lên +1 hoặc -1 tùy thuộc vào ký tự. Trong quá trình thực hiện, chúng tôi theo dõi giá trị tối đa được thấy cho đến nay và vị trí mà nó xuất hiện lần đầu tiên. 

Điều này có tác dụng vì chiều cao ở mỗi vị trí chỉ phụ thuộc vào tiền tố của chuỗi tính đến vị trí đó. Tuy nhiên, việc triển khai bất cẩn có thể tính toán lại chiều cao từ đầu cho mỗi chỉ mục, điều này sẽ tốn O(n²) thời gian vì mỗi vị trí sẽ quét lại tất cả các ký tự trước đó. 

Quan sát quan trọng là tổng tiền tố tăng dần. Mỗi chiều cao mới có thể được bắt nguồn từ chiều cao trước đó trong thời gian không đổi. Điều này làm giảm việc tính toán thành một lần truyền qua chuỗi, làm cho nó trở nên tối ưu. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Brute Force (tính toán lại tiền tố mỗi lần) | O(n²) | O(1) | Quá chậm | 
| Tối ưu (quét tiền tố đơn) | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi quét chuỗi một lần trong khi duy trì ba giá trị: chiều cao hiện tại, chiều cao tốt nhất được tìm thấy cho đến nay và chỉ mục nơi xảy ra chiều cao tốt nhất. 

1. Khởi tạo chiều cao hiện tại là 0, chiều cao tốt nhất là 0 và vị trí tốt nhất là 0. Về mặt khái niệm, chúng tôi bao gồm vị trí 0 để xử lý các trường hợp trong đó mức tối đa không bao giờ được cải thiện ngoài các giả định ban đầu. 
2. Lặp lại chuỗi từ trái sang phải bằng chỉ mục i dựa trên 1. 
3. Cập nhật chiều cao hiện tại: tăng thêm 1 nếu ký tự`+`, ngược lại giảm đi 1. 
4. So sánh chiều cao hiện tại với chiều cao tốt nhất. Nếu chiều cao hiện tại lớn hơn nhiều, hãy cập nhật chiều cao tốt nhất và ghi lại vị trí i. 
5. Tiếp tục cho đến hết chuỗi. 
6. Xuất ra vị trí tốt nhất được lưu trữ. 

Mỗi bước dựa trên thực tế là độ cao được xây dựng tích lũy, do đó, không có phân đoạn nào trong tương lai hoặc quá khứ cần được xem xét lại sau khi xử lý. 

### Tại sao nó hoạt động 

Trình tự độ cao được xác định đầy đủ bởi tổng tiền tố và mỗi tổng tiền tố chỉ phụ thuộc vào tổng tiền tố trước đó. Ở mỗi bước i, thuật toán lưu trữ giá trị lớn nhất trong số tất cả các tổng tiền tố nhỏ hơn i. Bởi vì mỗi tổng tiền tố được đánh giá chính xác một lần và được so sánh ngay lập tức nên không thể bỏ sót giá trị nào cao hơn. Việc đảm bảo tính duy nhất đảm bảo chúng tôi không bao giờ cần phải xử lý các mối quan hệ hoặc nhiều ứng viên, vì vậy chỉ cần so sánh nghiêm ngặt đơn giản là đủ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    s = input().strip()
    
    cur = 0
    best = 0
    best_pos = 0
    
    for i, ch in enumerate(s, 1):
        if ch == '+':
            cur += 1
        else:
            cur -= 1
        
        if cur > best:
            best = cur
            best_pos = i
    
    print(best_pos)

if __name__ == "__main__":
    solve()
```Mã trực tiếp thực hiện quét tổng tiền tố. Biến`cur`theo dõi chiều cao hiện tại, được cập nhật theo thời gian không đổi cho mỗi ký tự. Biến`best`lưu trữ chiều cao cao nhất được thấy cho đến nay và`best_pos`nhớ nơi nó xảy ra. 

Vòng lặp sử dụng lập chỉ mục dựa trên 1 thông qua`enumerate(s, 1)`để phù hợp với định nghĩa về vị trí của bài toán. Sự so sánh chặt chẽ`cur > best`đảm bảo chúng tôi chỉ ghi lại lần xuất hiện đầu tiên của chiều cao tối đa phù hợp với đảm bảo về tính duy nhất. 

## Ví dụ đã hoạt động 

### Ví dụ 1:`+-+-+++--+--`Chúng tôi theo dõi sự phát triển của chiều cao từng bước. 

| tôi | char | cur | tốt nhất | tốt nhất_pos | 
| --- | --- | --- | --- | --- | 
| 1 | + | 1 | 1 | 1 | 
| 2 | - | 0 | 1 | 1 | 
| 3 | + | 1 | 1 | 1 | 
| 4 | - | 0 | 1 | 1 | 
| 5 | + | 1 | 1 | 1 | 
| 6 | + | 2 | 2 | 6 | 
| 7 | + | 3 | 3 | 7 | 
| 8 | - | 2 | 3 | 7 | 
| 9 | - | 1 | 3 | 7 | 
| 10 | + | 2 | 3 | 7 | 
| 11 | - | 1 | 3 | 7 | 
| 12 | - | 0 | 3 | 7 | 

Giá trị cao nhất đạt được là 3 ở vị trí 7. Thuật toán nắm bắt chính xác lần đầu tiên đạt được mức tối đa này. 

### Ví dụ 2:`+++-++-+--+++-+++--+`| tôi | char | cur | tốt nhất | tốt nhất_pos | 
| --- | --- | --- | --- | --- | 
| 1 | + | 1 | 1 | 1 | 
| 2 | + | 2 | 2 | 2 | 
| 3 | + | 3 | 3 | 3 | 
| 4 | - | 2 | 3 | 3 | 
| 5 | + | 3 | 3 | 3 | 
| 6 | + | 4 | 4 | 6 | 
| 7 | - | 3 | 4 | 6 | 
| 8 | + | 4 | 4 | 6 | 
| 9 | - | 3 | 4 | 6 | 
| 10 | - | 2 | 4 | 6 | 
| 11 | + | 3 | 4 | 6 | 
| 12 | + | 4 | 4 | 6 | 
| 13 | + | 5 | 5 | 13 | 
| 14 | - | 4 | 5 | 13 | 
| 15 | + | 5 | 5 | 13 | 
| 16 | + | 6 | 6 | 16 | 
| 17 | + | 7 | 7 | 17 | 
| 18 | - | 6 | 7 | 17 | 
| 19 | - | 5 | 7 | 17 | 
| 20 | + | 6 | 7 | 17 | 

Chiều cao tối đa là 7 ở vị trí 17, phù hợp với đầu ra được yêu cầu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi ký tự được xử lý một lần với các bản cập nhật liên tục | 
| Không gian | O(1) | Chỉ có một số biến số nguyên được duy trì | 

Quét tuyến tính nằm trong giới hạn n lên đến 10⁵. Việc sử dụng bộ nhớ là không đổi bất kể kích thước đầu vào. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    
    s = input().strip()
    cur = 0
    best = 0
    best_pos = 0
    
    for i, ch in enumerate(s, 1):
        if ch == '+':
            cur += 1
        else:
            cur -= 1
        
        if cur > best:
            best = cur
            best_pos = i
    
    return str(best_pos)

# provided samples
assert run("+-+-+++--+--\n") == "7", "sample 1"
assert run("+++-++-+--+++-+++--+\n") == "17", "sample 2"

# minimum size
assert run("+\n") == "1", "single plus"

# all decreasing
assert run("-----\n") == "0", "never above zero, max at start"

# peak at end
assert run("++++\n") == "4", "peak at end"

# alternating
assert run("+-+-+-+\n") == "1", "early peak"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`+`| 1 | đầu vào tối thiểu, đỉnh ngay lập tức | 
|`-----`| 0 | tối đa vẫn ở vị trí ban đầu | 
|`++++`| 4 | đạt đỉnh ở vị trí cuối cùng | 
|`+-+-+-+`| 1 | dao động lặp lại, cực đại sớm | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi đạt được chiều cao tối đa ngay ở bước đầu tiên. Đối với đầu vào`+----`, chiều cao tiền tố là`[1, 0, -1, -2, -3]`, vì vậy câu trả lời phải là 1. Thuật toán xử lý việc này vì`best`bắt đầu từ 0 và được cập nhật ở i = 1 khi`cur = 1`. 

Một trường hợp cạnh khác là khi chiều cao không bao giờ vượt quá 0 sau khi bắt đầu. Đối với đầu vào`-----`, chiều cao tiền tố tối đa là 0 tại vị trí 0 (trước bất kỳ bước di chuyển nào). Việc triển khai tự nhiên trả về 0 vì`best_pos`được khởi tạo bằng 0 và không bao giờ được cập nhật. 

Trường hợp thứ ba là khi mức tối đa xảy ra nhiều lần nhưng chỉ trả về lần xuất hiện đầu tiên. Vì`++-++`, độ cao là`[1, 2, 1, 2, 3]`và số tối đa 3 chỉ xuất hiện một lần ở vị trí 5. Nếu có nhiều cực đại bằng nhau thì giá trị nghiêm ngặt`>`so sánh đảm bảo lần xuất hiện đầu tiên được giữ nguyên.
