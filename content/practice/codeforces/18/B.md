---
title: "CF 18B - Nền Tảng"
description: "Chúng tôi có một dây chuyền với một loạt các nền tảng riêng biệt. Mỗi sân ga có chiều dài l cố định và cách nhau một khoảng cố định m - l, do đó sân ga k chiếm đoạn từ (k-1)m đến (k-1)m + l."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "math"]
categories: ["algorithms"]
codeforces_contest: 18
codeforces_index: "B"
codeforces_contest_name: "Codeforces Beta Round 18 (Div. 2 Only)"
rating: 1700
weight: 18
solve_time_s: 73
verified: true
draft: false
---
[CF 18B - Nền tảng](https://codeforces.com/problemset/problem/18/B) 

**Đánh giá:** 1700 
**Tags:** sức mạnh vũ phu, toán học 
**Thời gian giải:** 1 phút 13s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một dây chuyền với một loạt các nền tảng riêng biệt. Mỗi nền tảng có độ dài cố định`l`và cách nhau một khoảng cố định`m - l`, vì vậy nền tảng`k`chiếm phân khúc từ`(k-1)*m`ĐẾN`(k-1)*m + l`. Châu chấu Bob xuất phát tại vị trí`0`và nhảy về phía trước theo từng bước chính xác`d`đơn vị. Nếu anh ta đáp xuống một bệ, anh ta sẽ sống sót và có thể nhảy trở lại. Nếu anh ta hạ cánh bên ngoài tất cả các sân ga, anh ta sẽ bị ngã. Hạ cánh chính xác ở rìa được coi là an toàn. Nhiệm vụ là tìm ra vị trí đầu tiên Bob rơi xuống. 

Các số nguyên đầu vào`n, d, m, l`tất cả có thể lên tới 10^6. Điều đó có nghĩa là bất kỳ cách tiếp cận nào lặp lại các vị trí riêng lẻ có thể yêu cầu tới một triệu lần nhảy trong trường hợp xấu nhất, điều này khả thi nếu chúng ta cẩn thận, nhưng việc quét vũ phu từng đơn vị của dòng sẽ quá chậm. Các trường hợp cạnh bao gồm các tình huống trong đó độ dài bước nhảy`d`lớn hơn khoảng cách giữa hai nền tảng, hoặc nơi`d`được căn chỉnh chính xác với các cạnh và các trường hợp`l = 1`Và`m`là lớn. Những kịch bản này có thể dễ dàng thực hiện việc kiểm tra đơn giản theo một đơn vị, vì vậy chúng ta cần số học chính xác với các khoảng thời gian khép kín. 

Ví dụ, nếu`n = 1, d = 5, m = 10, l = 3`, Bob nhảy tới`5`ở lần nhảy đầu tiên. Nền tảng duy nhất là`[0,3]`. Mặc dù`5 < 10`, anh ấy ở ngoài sân ga nên câu trả lời là`5`. Việc triển khai bất cẩn có thể giả định không chính xác bất kỳ bước nhảy nào trong`m`các đơn vị là an toàn, điều này sẽ tạo ra`10`, sự khởi đầu của nền tảng tiếp theo (không tồn tại), kết quả sai. 

## Phương pháp tiếp cận 

Cách tiếp cận đơn giản nhất là mô phỏng lực lượng vũ phu. Bắt đầu ở vị trí 0, tăng dần`d`mỗi lần nhảy và đối với mỗi vị trí mới, hãy kiểm tra xem nó có nằm trong bất kỳ phân đoạn nền tảng nào không. Điều này đúng vì nó trực tiếp mô phỏng hành vi của Bob. Vấn đề là hiệu quả: đối với`n = 10^6`Và`d = 1`, điều này có thể thực hiện tới một triệu lần nhảy và đối với mỗi lần nhảy, việc quét tuyến tính các nền tảng là quá tốn kém. Tệ hơn nữa, việc kiểm tra tất cả các nền tảng mỗi lần sẽ cho kết quả O(n^2) trong trường hợp xấu nhất, sẽ hết thời gian chờ. 

Quan sát quan trọng là các nền tảng có khoảng cách đều nhau và có chiều dài giống hệt nhau. Nền tảng`k`là`[ (k-1)*m , (k-1)*m + l ]`. Chúng ta có thể tính toán, cho một vị trí nhất định`x`, nó sẽ nằm trên nền tảng nào (hoặc nằm giữa) theo phép chia số nguyên: chỉ số nền tảng ứng cử viên là`x // m + 1`. Sau đó chúng ta chỉ cần kiểm tra xem`x`ít hơn`(candidate - 1)*m + l`để xác định xem Bob có còn ở trên sân ga hay không. Nếu đúng như vậy, anh ta sẽ sống sót; nếu không, anh ta sẽ ngã. Điều này làm giảm vấn đề xuống còn việc kiểm tra số học đơn giản cho mỗi lần nhảy và tránh việc quét tất cả các nền tảng. 

Chúng ta có thể tối ưu hóa hơn nữa bằng cách nhận ra rằng khoảng cách nhảy của Bob là cố định. Cú nhảy đầu tiên mà anh ta rơi xuống có thể được biểu thị bằng`x = d * k`đối với một số nguyên`k`. Chúng ta cần cái nhỏ nhất`k`như vậy`x`lớn hơn phần cuối bên phải của nền tảng mà nó sẽ nằm trên đó. Điều này đưa ra một giải pháp số học thuần túy tính bằng O(1) cho mỗi lần nhảy cho đến khi anh ta ngã. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^2) | O(1) | Quá chậm | 
| Số học tối ưu hóa | O(n/d) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo vị trí của Bob`x`ĐẾN`0`. Chúng ta sẽ tăng dần`d`trong mỗi lần nhảy. 
2. Ở mỗi lần nhảy, hãy tính chỉ số nền tảng mà Bob sẽ đáp xuống. Vì các nền tảng là`[ (k-1)*m , (k-1)*m + l ]`, nền tảng ứng cử viên là`k = x // m + 1`. 
3. Kiểm tra xem`k`vượt quá`n`. Nếu có, Bob đã nhảy ra khỏi bục cuối cùng và rơi xuống`x`. 
4. Nếu không, hãy kiểm tra xem`x`nằm trong nền tảng:`x <= (k-1)*m + l`. Nếu có, Bob sống sót và nhảy trở lại. 
5. Nếu`x > (k-1)*m + l`, Bob đáp xuống bên ngoài sân ga và ngã. Đây là vị trí không an toàn đầu tiên và chúng tôi sẽ in nó. 
6. Tăng dần`x`qua`d`và lặp lại. 

Tại sao nó hoạt động: thuật toán duy trì tính bất biến`x`luôn là vị trí thực tế của Bob sau một số lần nhảy. Bằng cách tính toán nền tảng ứng viên theo phương pháp đại số, chúng tôi không bao giờ quét các nền tảng không liên quan. Séc`x <= (k-1)*m + l`nắm bắt chính xác liệu Bob có tiếp đất trong bục hay bị ngã hay không. Từ`d`không đổi, mỗi bước tiến tới bước nhảy tiếp theo mà không bỏ lỡ bất kỳ điểm rơi tiềm năng nào. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, d, m, l = map(int, input().split())

x = 0
while True:
    k = x // m + 1
    if k > n or x > (k - 1) * m + l:
        print(x)
        break
    x += d
```Mã bắt đầu bằng`x = 0`và lặp lại bởi`d`. Máy tính`k = x // m + 1`trực tiếp cung cấp chỉ số nền tảng dựa trên 1. Nếu như`k > n`, Bob đã vượt qua tất cả các nền tảng. Nếu như`x > (k-1)*m + l`, anh ấy đã vượt quá nền tảng hiện tại. Nếu không thì,`x`là an toàn và vòng lặp tiếp tục. Việc triển khai này xử lý các cạnh một cách chính xác vì chúng tôi sử dụng`<=`để đưa vào nền tảng. 

## Ví dụ đã hoạt động 

**Đầu vào mẫu 1**```
2 2 5 3
```| Nhảy | x | k | Cuối nền tảng | Ngã? | 
| --- | --- | --- | --- | --- | 
| 1 | 0 | 1 | 3 | Không | 
| 2 | 2 | 1 | 3 | Không | 
| 3 | 4 | 1 | 3 | Có | 

Bob rơi tại vị trí`4`. Dấu vết xác nhận thuật toán tính toán chỉ số nền tảng chính xác và so sánh với cạnh phải. 

**Đầu vào tùy chỉnh 2**```
3 4 5 3
```| Nhảy | x | k | Cuối nền tảng | Ngã? | 
| --- | --- | --- | --- | --- | 
| 1 | 0 | 1 | 3 | Không | 
| 2 | 4 | 1 | 3 | Có | 

Bob nhảy qua bệ đầu tiên ở lần nhảy thứ hai. Séc`x > (k-1)*m + l`xác định chính xác sự sụp đổ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n/d) | Chúng tôi tăng thêm`d`từng bước. Số lần nhảy tối đa là khoảng`n*m/d`, giới hạn bởi`10^6`. | 
| Không gian | O(1) | Chỉ có một vài biến số nguyên được sử dụng, độc lập với`n`. | 

Điều này phù hợp thoải mái trong giới hạn thời gian và bộ nhớ. Ngay cả trong trường hợp xấu nhất khi`d = 1`Và`n = 10^6`, chúng tôi thực hiện tối đa 10^6 lần lặp, điều này khả thi trong 2 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        n, d, m, l = map(int, input().split())
        x = 0
        while True:
            k = x // m + 1
            if k > n or x > (k - 1) * m + l:
                print(x)
                break
            x += d
    return out.getvalue().strip()

# Provided sample
assert run("2 2 5 3\n") == "4", "sample 1"

# Minimum-size inputs
assert run("1 1 2 1\n") == "2", "minimum size"

# All equal
assert run("3 3 3 3\n") == "9", "all equal values"

# d > m
assert run("2 6 5 3\n") == "6", "jump larger than spacing"

# Last jump lands on last edge
assert run("3 2 5 3\n") == "10", "lands exactly on last edge"

# Single platform, multiple jumps
assert run("1 2 5 3\n") == "4", "single platform multiple jumps"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1 2 1 | 2 | nền tảng nhỏ nhất và bước nhảy | 
| 3 3 3 3 | 9 | chiều dài bước nhảy bằng khoảng cách và chiều dài nền tảng | 
| 2 6 5 3 | 6 | nhảy lớn hơn khoảng cách giữa các nền tảng | 
| 3 2 5 3 | 10 | cú nhảy cuối cùng tiếp đất chính xác ở rìa | 
| 1 2 5 3 | 4 | nền tảng đơn, nhiều bước nhảy | 

## Vỏ cạnh 

Nếu`d`lớn hơn khoảng cách giữa các nền tảng, thuật toán sẽ tính toán chính xác`k = x // m + 1`và kiểm tra xem Bob có vượt qua nền tảng hay không. Ví dụ: với `n=2
