---
title: "CF 50B - Chọn cặp ký hiệu"
description: "Chúng ta được cấp một chuỗi ký tự bao gồm các chữ cái và chữ số viết thường. Nhiệm vụ là đếm số cặp vị trí được sắp xếp trong chuỗi trong đó các ký tự ở các vị trí đó giống hệt nhau."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "strings"]
categories: ["algorithms"]
codeforces_contest: 50
codeforces_index: "B"
codeforces_contest_name: "Codeforces Beta Round 47"
rating: 1500
weight: 50
solve_time_s: 75
verified: true
draft: false
---

[CF 50B - Chọn cặp ký hiệu](https://codeforces.com/problemset/problem/50/B) 

**Đánh giá:** 1500 
**Thẻ:** chuỗi 
**Thời gian giải:** 1m 15s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một chuỗi ký tự bao gồm các chữ cái và chữ số viết thường. Nhiệm vụ là đếm số cặp vị trí được sắp xếp trong chuỗi trong đó các ký tự ở các vị trí đó giống hệt nhau. Một cặp được sắp xếp, vì vậy đối với các vị trí`i`Và`j`, cả hai`(i, j)`Và`(j, i)`tính riêng. Tự ghép đôi`(i, i)`cũng được đưa vào vì ký tự này khớp với chính nó một cách tầm thường. 

Độ dài chuỗi có thể lên tới 100.000 ký tự. Một giải pháp mạnh mẽ để kiểm tra mọi cặp có thể sẽ yêu cầu khoảng$10^{10}$so sánh trong trường hợp xấu nhất, vượt quá những gì chúng ta có thể tính toán trong hai giây. Điều này ngay lập tức loại trừ các vòng lặp lồng nhau ngây thơ. 

Các trường hợp phức tạp bao gồm các chuỗi rất ngắn và các chuỗi trong đó tất cả các ký tự đều giống hệt nhau. Ví dụ: một chuỗi ký tự đơn`"a"`nên trả về 1 vì cặp duy nhất`(1, 1)`là hợp lệ. Một chuỗi như`"aaaa"`sẽ trả về 16 vì mỗi ký tự trong số 4 ký tự khớp với từng vị trí trong số 4 vị trí, tạo ra$4 \times 4 = 16$các cặp đặt hàng. Một cách tiếp cận bất cẩn chỉ đếm các cặp riêng biệt mà không tự ghép đôi hoặc không xem xét thứ tự sẽ dẫn đến câu trả lời sai. 

## Phương pháp tiếp cận 

Cách tiếp cận brute-force rất đơn giản: đối với mỗi ký tự trong chuỗi, hãy kiểm tra nó với mọi ký tự khác, đếm các kết quả trùng khớp. Điều này đúng vì nó kiểm tra mọi cặp hợp lệ, nhưng nó yêu cầu$O(n^2)$hoạt động không thể thực hiện được đối với$n = 10^5$. Trong trường hợp xấu nhất, nó hoạt động gần như$10^{10}$so sánh. 

Quan sát quan trọng để tối ưu hóa là chúng ta không cần so sánh từng ký tự riêng lẻ. Chúng ta chỉ cần biết mỗi ký tự riêng biệt xuất hiện bao nhiêu lần. Nếu một nhân vật xuất hiện`k`nhiều khi nó góp phần`k * k`các cặp theo thứ tự:`k`lựa chọn cho vị trí đầu tiên và`k`cho lần thứ hai. Tổng hợp điều này trên tất cả các ký tự riêng biệt sẽ đưa ra câu trả lời cuối cùng. Điều này làm giảm vấn đề đếm tần số ký tự, có thể được thực hiện trong một lần truyền qua chuỗi. 

Quan sát này biến thuật toán từ bậc hai sang tuyến tính, điều này cần thiết với các ràng buộc. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(1) | Quá chậm | 
| Tối ưu | O(n) | O(1) (bản đồ có kích thước cố định cho 36 ký tự) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo từ điển tần số (hoặc mảng) cho các ký tự. Vì chuỗi chỉ chứa các chữ cái và chữ số viết thường nên chúng ta có thể sử dụng một mảng có kích thước 36 được lập chỉ mục theo ký tự. 
2. Lặp lại từng ký tự trong chuỗi, tăng số đếm của nó trong bảng tần số. Kết thúc bước này chúng ta biết mỗi nhân vật xuất hiện bao nhiêu lần. 
3. Khởi tạo một biến`total_pairs`về không. 
4. Lặp lại bảng tần số. Với mỗi nhân vật xuất hiện`count`lần, thêm`count * count`ĐẾN`total_pairs`. Lý do là có`count`tùy chọn cho vị trí đầu tiên và`count`cho lần thứ hai, bao gồm cả tự ghép đôi. 
5. In`total_pairs`như kết quả. 

Tại sao nó hoạt động: Điều bất biến là sự đóng góp của mỗi ký tự vào tổng số cặp có thứ tự chính xác bằng bình phương tần số của nó. Vì chúng tôi xem xét tất cả các ký tự và bao gồm các cặp tự ghép nên tất cả các cặp hợp lệ đều được tính chính xác một lần. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

s = input().strip()
freq = [0] * 36  # 26 letters + 10 digits

def char_index(c):
    if 'a' <= c <= 'z':
        return ord(c) - ord('a')
    else:
        return 26 + ord(c) - ord('0')

for ch in s:
    freq[char_index(ch)] += 1

total_pairs = sum(f * f for f in freq)
print(total_pairs)
```các`char_index`hàm ánh xạ từng chữ cái viết thường thành 0-25 và các chữ số thành 26-35, cho phép một mảng có kích thước cố định thay vì từ điển. Chúng tôi lặp lại một lần trên chuỗi để điền vào mảng tần số, sau đó tính tổng các bình phương của số đếm. Việc sử dụng một mảng cố định sẽ tránh được chi phí từ điển và đảm bảo quyền truy cập O(1) cho mỗi ký tự. 

Một điểm tinh tế là xử lý cả chữ cái và chữ số trong một mảng. Sai lầm thường xảy ra nếu ai đó cho rằng mã ASCII nằm liền kề giữa các chữ cái và chữ số. Một cạm bẫy tiềm ẩn khác là quên bao gồm các cặp tự - bước bình phương đương nhiên sẽ bao gồm chúng. 

## Ví dụ đã hoạt động 

**Ví dụ 1:**`"great10"`| Nhân vật | Tần số | Đóng góp | 
| --- | --- | --- | 
| g | 1 | 1 | 
| r | 1 | 1 | 
| e | 1 | 1 | 
| một | 1 | 1 | 
| t | 1 | 1 | 
| 1 | 1 | 1 | 
| 0 | 1 | 1 | 

Tổng số cặp = 7 

Mỗi nhân vật là duy nhất nên chỉ tồn tại các cặp tự thân. Điều này chứng tỏ thuật toán xử lý chính xác các chuỗi không có ký tự lặp lại. 

**Ví dụ 2:**`"aaaa"`| Nhân vật | Tần số | Đóng góp | 
| --- | --- | --- | 
| một | 4 | 16 | 

Tổng số cặp = 16 

Tất cả các cặp đều hợp lệ, kể cả các cặp tự ghép và các cặp ở cả hai thứ tự. Thuật toán đếm chúng tự động thông qua`count * count`công thức. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Một lượt để tính tần số, sau đó tính tổng trên 36 mục | 
| Không gian | O(1) | Mảng cố định có kích thước 36, không phụ thuộc vào độ dài đầu vào | 

Giải pháp dễ dàng phù hợp với giới hạn thời gian và bộ nhớ cho n 10^5. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    s = input().strip()
    freq = [0] * 36
    def char_index(c):
        if 'a' <= c <= 'z':
            return ord(c) - ord('a')
        else:
            return 26 + ord(c) - ord('0')
    for ch in s:
        freq[char_index(ch)] += 1
    return str(sum(f * f for f in freq))

# Provided samples
assert run("great10\n") == "7", "sample 1"

# Custom test cases
assert run("aaaa\n") == "16", "all same characters"
assert run("1\n") == "1", "single character"
assert run("ab1ab\n") == "9", "mixed letters and digits with repeats"
assert run("abc123abc123\n") == "24", "repeated letters and digits"
assert run("z9z9z9\n") == "18", "alternating letters and digits"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`"aaaa"`| 16 | ký tự hoàn toàn bằng nhau, thứ tự đếm và tự ghép | 
|`"1"`| 1 | đầu vào có độ dài tối thiểu | 
|`"ab1ab"`| 9 | trộn chữ cái và chữ số, ký tự lặp lại | 
|`"abc123abc123"`| 24 | các chữ cái và chữ số lặp đi lặp lại, xuất hiện nhiều lần | 
|`"z9z9z9"`| 18 | mẫu xen kẽ với nhiều lần xuất hiện | 

## Vỏ cạnh 

Đối với chuỗi ký tự đơn`"1"`, tần số của`'1'`là 1. Bình phương nó mang lại kết quả`1`, điều đó đúng. Thuật toán xử lý việc này một cách đơn giản vì công thức tự nhiên bao gồm các cặp tự. 

Đối với một chuỗi có tất cả các ký tự giống hệt nhau,`"aaaa"`, tần số là 4. Bình phương cho`16`, chiếm tất cả 16 cặp có thứ tự. Không cần các vòng lặp lồng nhau, nếu không sẽ làm phức tạp quá mức số lượng và có nguy cơ xảy ra lỗi riêng lẻ. 

Đối với một chuỗi trộn các chữ cái và chữ số,`"ab1ab"`, tần số là`'a':2`,`'b':2`,`'1':1`. Đóng góp là`4 + 4 + 1 = 9`, xác nhận rằng công thức hoạt động trên nhiều loại ký tự. 

Bài xã luận này hướng dẫn người đọc từ cách tiếp cận bậc hai đơn giản đến giải pháp không gian cố định, thời gian tuyến tính bằng cách tận dụng tần số ký tự và đếm cặp theo thứ tự. Theo lý luận này, các vấn đề tương tự liên quan đến việc đếm các cặp với tiêu chí phù hợp cụ thể có thể được tiếp cận một cách hiệu quả.
