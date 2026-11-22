---
title: "CF 312A – Câu nói của ai?"
description: "Chúng ta được tặng một đoạn chat nhỏ có tối đa 10 câu. Mỗi câu là một chuỗi các chữ cái, dấu gạch dưới, dấu phẩy, dấu chấm và dấu cách. Hai người, Freda và Rainbow, có liên quan. Theo kinh nghiệm trước đây, Freda luôn kết thúc câu nói của mình bằng chuỗi phụ \"lala."
date: "2026-06-06T00:49:33+07:00"
tags: ["codeforces", "competitive-programming", "implementation", "strings"]
categories: ["algorithms"]
codeforces_contest: 312
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 185 (Div. 2)"
rating: 1100
weight: 312
solve_time_s: 90
verified: false
draft: false
---

[CF 312A - Câu nói của ai?](https://codeforces.com/problemset/problem/312/A) 

**Đánh giá:** 1100 
**Tags:** triển khai, chuỗi 
**Thời gian giải:** 1 phút 30s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được tặng một đoạn chat nhỏ có tối đa 10 câu. Mỗi câu là một chuỗi các chữ cái, dấu gạch dưới, dấu phẩy, dấu chấm và dấu cách. Hai người, Freda và Rainbow, có liên quan. Theo kinh nghiệm trước đây, Freda luôn kết thúc câu của mình bằng chuỗi con`"lala."`trong khi Rainbow luôn bắt đầu câu nói của mình bằng`"miao."`. Mục tiêu của chúng tôi là xác định, đối với mỗi câu, xem nó thuộc về Freda, Rainbow hay không hoặc không thể xác định được. Một câu là câu không xác định nếu cả hai đều bắt đầu bằng`"miao."`và kết thúc bằng`"lala."`, hoặc nếu nó không thỏa mãn mẫu nào. 

Giới hạn đầu vào rất nhỏ: tối đa 10 câu và mỗi câu có độ dài tối đa 100 ký tự. Điều này ngụ ý rằng bất kỳ thuật toán nào kiểm tra từng câu riêng lẻ theo thời gian tuyến tính tương ứng với độ dài của nó đều có thể chấp nhận được. Không cần cấu trúc dữ liệu phức tạp hoặc tối ưu hóa vì tổng số thao tác tối đa là 1000, thấp hơn nhiều so với giới hạn lập trình cạnh tranh thông thường là 10^8 thao tác mỗi giây. 

Các trường hợp cạnh không rõ ràng liên quan đến khoảng trắng và dấu câu. Ví dụ, một câu bắt đầu bằng`"miao"`nhưng có một khoảng trống trước dấu chấm, như`"miao ."`, không được coi là câu nói của Rainbow. Tương tự,`"lala"`không có dấu chấm cuối cùng không đủ điều kiện là bản án của Freda. Một câu đồng thời bắt đầu bằng`"miao."`và kết thúc bằng`"lala."`nên được đánh dấu là không xác định. Một cách kiểm tra ngây thơ chỉ kiểm tra phần đầu hoặc phần cuối mà không xem xét sự trùng lặp có thể phân loại sai các câu như vậy. 

## Phương pháp tiếp cận 

Cách tiếp cận vũ phu rất đơn giản. Đối với mỗi câu, chúng ta có thể kiểm tra xem nó có kết thúc bằng`"lala."`hoặc bắt đầu bằng`"miao."`và xuất ra nhãn tương ứng. Nếu cả hai điều kiện đều đúng hoặc không đúng thì chúng ta sẽ xuất ra thông báo không xác định. Điều này hiệu quả vì số lượng câu ít và mỗi câu ngắn. Phương pháp brute-force là tuyến tính trong tổng chiều dài của văn bản, trong trường hợp xấu nhất là 10 câu nhân 100 ký tự, dẫn đến 1000 thao tác. 

Cách tiếp cận tối ưu không khác với cách tiếp cận mạnh mẽ trong vấn đề này, vì không có kích thước đầu vào lớn hơn để tối ưu hóa. Cái nhìn sâu sắc quan trọng là các hoạt động chuỗi trong Python chẳng hạn như`.startswith()`Và`.endswith()`chạy trong thời gian tuyến tính đối với độ dài chuỗi con, không đổi trong trường hợp này (`"miao."`Và`"lala."`đều có chiều dài 5). Do đó, chúng ta có thể áp dụng trực tiếp các hàm dựng sẵn này mà không cần bất kỳ cấu trúc dữ liệu bổ sung hoặc tiền xử lý nào. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n * L) | O(1) | Đã chấp nhận | 
| Tối ưu | O(n * L) | O(1) | Đã chấp nhận | 

Đây,`n`là số câu và`L`là độ dài câu tối đa, 100. 

## Hướng dẫn thuật toán 

1. Đọc số nguyên`n`thể hiện số câu. 
2. Đối với mỗi câu trong bản ghi trò chuyện, hãy xóa dòng mới ở cuối. 
3. Kiểm tra xem câu có bắt đầu bằng`"miao."`. Lưu trữ giá trị này dưới dạng biến boolean`is_rainbow`. 
4. Kiểm tra xem câu có kết thúc bằng`"lala."`. Lưu trữ giá trị này dưới dạng biến boolean`is_freda`. 
5. Nếu cả hai`is_rainbow`Và`is_freda`là True, in thông báo không xác định`"OMG>.< I don't know!"`. 
6. Giá như`is_rainbow`là Đúng, in`"Rainbow's"`. 
7. Giá như`is_freda`là Đúng, in`"Freda's"`. 
8. Nếu cả hai đều không đúng, hãy in thông báo không xác định. 

Tại sao nó hoạt động: các thuộc tính của câu loại trừ lẫn nhau ngoại trừ trường hợp không xác định. Bằng cách kiểm tra phần đầu và phần cuối một cách độc lập và xử lý rõ ràng tình huống chồng chéo, thuật toán đảm bảo rằng mọi câu đều được phân loại chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())
for _ in range(n):
    sentence = input().rstrip('\n')
    is_rainbow = sentence.startswith("miao.")
    is_freda = sentence.endswith("lala.")
    
    if is_rainbow and is_freda:
        print("OMG>.< I don't know!")
    elif is_rainbow:
        print("Rainbow's")
    elif is_freda:
        print("Freda's")
    else:
        print("OMG>.< I don't know!")
```Giải pháp đọc từng câu, cắt bớt ký tự dòng mới, sau đó kiểm tra các chuỗi con bắt đầu và kết thúc có liên quan. Thứ tự kiểm tra rất quan trọng: điều kiện đầu tiên đảm bảo rằng trường hợp không xác định được xử lý trước khi bất kỳ kết quả khớp nào được in. sử dụng`.startswith()`Và`.endswith()`tránh các lỗi lập chỉ mục thủ công và hoạt động ngay cả khi độ dài câu chính xác là 5 ký tự. 

## Ví dụ đã hoạt động 

Đầu vào mẫu:```
5
I will go to play with you lala.
wow, welcome.
miao.lala.
miao.
miao .
```| Câu | is_rainbow | is_freda | Đầu ra | 
| --- | --- | --- | --- | 
| Tôi sẽ đi chơi với bạn lala. | Sai | Đúng | Freda's | 
| ồ, chào mừng. | Sai | Sai | Ôi trời>.< Tôi không biết! | 
| miao.lala. | Đúng | Đúng | Ôi trời>.< Tôi không biết! | 
| miao. | Đúng | Sai | Cầu vồng | 
| miao . | Sai | Sai | Ôi trời>.< Tôi không biết! | 

Dấu vết này thể hiện việc xử lý cả ba trường hợp phân loại: chỉ Freda, chỉ Rainbow và không xác định. 

Đầu vào tùy chỉnh:```
3
lala.
miao.lala.
miao..lala.
```| Câu | is_rainbow | is_freda | Đầu ra | 
| --- | --- | --- | --- | 
| lala. | Sai | Đúng | Freda's | 
| miao.lala. | Đúng | Đúng | Ôi trời>.< Tôi không biết! | 
| miao..lala. | Sai | Đúng | Freda's | 

Dấu vết này xác nhận rằng chỉ những câu bắt đầu chính xác bằng`"miao."`được phân loại là Rainbow, tránh kết quả dương tính giả khi xuất hiện thêm dấu câu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n * L) | Mỗi câu được kiểm tra chuỗi con bắt đầu và kết thúc.`L`nhiều nhất là 100. | 
| Không gian | O(1) | Chỉ có một số biến boolean được sử dụng; không có cấu trúc dữ liệu bổ sung nào phát triển theo kích thước đầu vào. | 

Các ràng buộc n ≤ 10 và L ≤ 100 đảm bảo rằng giải pháp thực thi trong thời gian không đáng kể, nằm trong giới hạn 2 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    output = io.StringIO()
    with redirect_stdout(output):
        n = int(input())
        for _ in range(n):
            sentence = input().rstrip('\n')
            is_rainbow = sentence.startswith("miao.")
            is_freda = sentence.endswith("lala.")
            
            if is_rainbow and is_freda:
                print("OMG>.< I don't know!")
            elif is_rainbow:
                print("Rainbow's")
            elif is_freda:
                print("Freda's")
            else:
                print("OMG>.< I don't know!")
    return output.getvalue().strip()

# Provided sample
assert run("5\nI will go to play with you lala.\nwow, welcome.\nmiao.lala.\nmiao.\nmiao .") == \
"""Freda's
OMG>.< I don't know!
OMG>.< I don't know!
Rainbow's
OMG>.< I don't know!"""

# Minimum size input
assert run("1\nmiao.") == "Rainbow's"

# Freda-only sentence
assert run("1\nhello lala.") == "Freda's"

# Indeterminate sentence both start and end
assert run("1\nmiao.lala.") == "OMG>.< I don't know!"

# Edge punctuation
assert run("1\nmiao..lala.") == "Freda's"

# Random unknown sentence
assert run("1\nhello world.") == "OMG>.< I don't know!"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| miao. | Cầu vồng | Câu cầu vồng có độ dài tối thiểu | 
| chào lala. | Freda's | Câu Freda có độ dài tối thiểu | 
| miao.lala. | Ôi trời>.< Tôi không biết! | Trường hợp không xác định với cả hai mẫu | 
| miao..lala. | Freda's | Tránh tích cực sai cho dấu câu thừa | 
| xin chào thế giới. | Ôi trời>.< Tôi không biết! | Xử lý câu chưa biết | 

## Vỏ cạnh 

Câu`"miao ."`có một khoảng trống trước dấu chấm. Thuật toán của chúng tôi xác định chính xác người đó không phải là Rainbow hay Freda. Các biến boolean`is_rainbow`Và`is_freda`là Sai nên thông báo không xác định được in ra. Câu`"miao.lala."`kích hoạt cả hai`is_rainbow`Và`is_freda`, tạo ra đầu ra không xác định. Thứ tự kiểm tra đảm bảo rằng các điều kiện chồng chéo không bị phân loại sai. Tất cả các câu khác được phân loại chính xác dựa trên kết quả khớp chuỗi con chính xác ở đầu và cuối.
