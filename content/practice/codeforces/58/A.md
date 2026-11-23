---
title: "CF 58A - Phòng chat"
description: "Vấn đề đặt ra là liệu Vasya, người đã gõ một chuỗi chữ thường, có thể nói \"xin chào\" một cách hiệu quả hay không. Mục đích không phải là kiểm tra xem chuỗi đã gõ có chính xác là \"hello\" hay không, mà là liệu chúng ta có thể loại bỏ một số chữ cái (có thể bằng 0) để tạo ra chuỗi h, e, l, l, o theo thứ tự hay không."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "strings"]
categories: ["algorithms"]
codeforces_contest: 58
codeforces_index: "A"
codeforces_contest_name: "Codeforces Beta Round 54 (Div. 2)"
rating: 1000
weight: 58
solve_time_s: 80
verified: true
draft: false
---

[CF 58A - Phòng trò chuyện](https://codeforces.com/problemset/problem/58/A) 

**Đánh giá:** 1000 
**Tags:** tham lam, dây 
**Thời gian giải:** 1 phút 20s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Vấn đề đặt ra là liệu Vasya, người đã gõ một chuỗi chữ thường, có thể nói "xin chào" một cách hiệu quả hay không. Mục đích không phải là kiểm tra xem chuỗi đã gõ có chính xác là "xin chào" hay không, mà là liệu chúng ta có thể loại bỏ một số chữ cái (có thể là 0) để tạo ra chuỗi không`h`,`e`,`l`,`l`,`o`theo thứ tự. 

Đầu vào là một chuỗi có độ dài từ 1 đến 100. Đầu ra là "CÓ" nếu có thể trích xuất từ ​​"xin chào" từ chuỗi đó hoặc "KHÔNG" nếu không. 

Những ràng buộc rất thoải mái. Với`n ≤ 100`và giới hạn thời gian là 1 giây, ngay cả một giải pháp ngây thơ xem xét tất cả các chuỗi con về mặt lý thuyết cũng có thể hoạt động, nhưng bản chất của vấn đề thực sự là quét chuỗi để tìm chuỗi mục tiêu. Các trường hợp cạnh có thể đánh lừa một cách tiếp cận ngây thơ bao gồm các chuỗi có các chữ cái nhưng không theo thứ tự, chẳng hạn như`hlelo`, chứa tất cả các chữ cái của "xin chào" nhưng không giữ nguyên thứ tự của chúng. Một trường hợp cạnh khác là đầu vào tối thiểu, ví dụ:`h`, hiển thị rõ ràng là "KHÔNG". 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ thử mọi chuỗi con có thể có của chuỗi đầu vào để xem liệu nó có khớp với "xin chào" hay không. Đối với một chuỗi có độ dài`n`, có`2^n`các hậu quả, điều này là không thể thực hiện được ngay cả đối với`n = 100`, sản xuất khoảng`10^30`séc. Rõ ràng, cách tiếp cận này đúng về mặt nguyên tắc nhưng không thực tế. 

Cái nhìn sâu sắc quan trọng là chúng ta không cần phải xem xét tất cả các chuỗi con một cách rõ ràng. Chúng tôi chỉ quan tâm đến việc liệu chúng tôi có thể khớp từng ký tự của "xin chào" hay không khi chúng tôi quét chuỗi. Điều này gợi ý quá trình quét tham lam: giữ một chỉ mục`j`cho từ mục tiêu "xin chào". Duyệt chuỗi đầu vào`s`từ trái sang phải. Mỗi lần một nhân vật phù hợp`hello[j]`, tăng`j`. Nếu như`j`đạt tới độ dài "xin chào" (5), chúng ta đã tìm được từ thành công. Đây là tuyến tính theo độ dài của chuỗi và mã hóa trực tiếp yêu cầu tuần tự của vấn đề. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n) | O(1) | Quá chậm | 
| Quét tham lam | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo`target = "hello"`và một chỉ mục`j = 0`để theo dõi ký tự "xin chào" mà chúng tôi hiện đang tìm kiếm. 
2. Lặp qua từng ký tự`c`trong chuỗi đầu vào`s`. 
3. Nếu`c`bằng`target[j]`, tăng`j`. Điều này chuyển chúng ta đến ký tự tiếp theo mà chúng ta cần khớp. Chúng tôi tự động bỏ qua tất cả các ký tự khác vì chúng không thể phá vỡ thứ tự. 
4. Sau khi quét chuỗi, kiểm tra xem`j`bằng 5. Nếu đúng thì in "CÓ", nếu không thì in "KHÔNG". 

Tại sao nó hoạt động: Cách tiếp cận tham lam dựa vào tính bất biến`j`luôn trỏ đến ký tự tiếp theo của "xin chào" cần được khớp. Mỗi khi một ký tự khớp, nó sẽ tăng chỉ số. Bất kỳ ký tự nào ở giữa đều bị bỏ qua vì chúng không ngăn cản việc hình thành chuỗi con. Điều này đảm bảo rằng nếu`j`đạt đến 5, tất cả các chữ cái đã được ghép theo đúng thứ tự. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

s = input().strip()
target = "hello"
j = 0

for c in s:
    if c == target[j]:
        j += 1
        if j == len(target):
            break

print("YES" if j == len(target) else "NO")
```Mã đọc đầu vào và loại bỏ mọi dòng mới ở cuối. Chúng tôi duy trì một con trỏ`j`để theo dõi tiến trình của chúng tôi thông qua "xin chào". Khi chúng tôi lặp đi lặp lại`s`, chúng tôi tăng`j`bất cứ khi nào chúng tôi khớp với ký tự mục tiêu hiện tại. Nếu chúng ta đi đến cuối`target`, chúng tôi dừng lại sớm để có hiệu quả. Cuối cùng, chúng tôi in "CÓ" nếu tất cả các chữ cái đều khớp, nếu không thì "KHÔNG". 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:`ahhellllloou`| s[i] | c | mục tiêu[j] | j | 
| --- | --- | --- | --- | 
| 0 | một | h | 0 | 
| 1 | h | h | 1 | 
| 2 | h | e | 1 | 
| 3 | e | e | 2 | 
| 4 | tôi | tôi | 3 | 
| 5 | tôi | tôi | 4 | 
| 6 | tôi | o | 4 | 
| 7 | tôi | o | 4 | 
| 8 | tôi | o | 4 | 
| 9 | o | o | 5 | 

Tất cả các chữ cái mục tiêu được khớp theo thứ tự. Đầu ra:`YES`. 

### Ví dụ 2 

đầu vào:`hlelo`| s[i] | c | mục tiêu[j] | j | 
| --- | --- | --- | --- | 
| 0 | h | h | 1 | 
| 1 | tôi | e | 1 | 
| 2 | e | e | 2 | 
| 3 | tôi | tôi | 3 | 
| 4 | o | tôi | 3 |`j`không bao giờ đạt tới 5. Đầu ra:`NO`. 

Những dấu vết này xác nhận rằng thuật toán theo dõi chính xác các kết quả khớp tuần tự và loại bỏ các chữ cái không theo thứ tự. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Chúng tôi quét từng ký tự một lần, thực hiện tối đa một lần so sánh và tăng con trỏ cho mỗi ký tự | 
| Không gian | O(1) | Chỉ một số biến được sử dụng bất kể độ dài đầu vào | 

Với`n ≤ 100`, điều này thoải mái trong giới hạn thời gian và bộ nhớ. Kể cả nếu`n`lớn hơn, quy mô quét tuyến tính này tốt. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    s = input().strip()
    target = "hello"
    j = 0
    for c in s:
        if c == target[j]:
            j += 1
            if j == len(target):
                break
    return "YES" if j == len(target) else "NO"

# Provided sample
assert run("ahhellllloou") == "YES", "sample 1"

# Custom cases
assert run("hlelo") == "NO", "letters out of order"
assert run("h") == "NO", "single letter"
assert run("helicopterlo") == "YES", "interleaved extra letters"
assert run("aaaaa") == "NO", "no relevant letters"
assert run("hhheeellllooo") == "YES", "duplicates before matches"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| hlelo | KHÔNG | trình tự thực thi lệnh | 
| h | KHÔNG | xử lý đầu vào tối thiểu | 
| trực thăng | CÓ | bỏ qua các chữ cái không liên quan | 
| aaa | KHÔNG | thiếu chữ cái đích | 
| hheeellllooo | CÓ | nhiều trận đấu liên tiếp được xử lý | 

## Vỏ cạnh 

cho`s = "h"`, thuật toán lặp lại một lần, thấy`h`trận đấu`target[0]`, số gia`j = 1`. Không còn nhân vật nào nữa,`j < 5`, vậy đầu ra là`NO`. Vì`s = "hhheeellllooo"`, thuật toán tăng dần`j`chỉ khi chữ cái bắt buộc tiếp theo khớp, bỏ qua các chữ cái trùng lặp. Sau tất cả các nhân vật,`j = 5`, xuất ra chính xác`YES`. Những điều này xác nhận quá trình quét tham lam xử lý chính xác cả đầu vào không đầy đủ và lặp đi lặp lại.
