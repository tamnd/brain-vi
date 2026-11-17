---
title: "CF 1952B - Có ghi rõ không?"
description: "Chúng tôi được cung cấp một danh sách các chuỗi, mỗi chuỗi bao gồm các chữ cái tiếng Anh viết thường và đối với mỗi chuỗi, chúng tôi được yêu cầu trả lời \"CÓ\" hoặc \"KHÔNG\" dựa trên một mẫu ẩn."
date: "2026-05-31T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "*special", "strings"]
categories: ["algorithms"]
codeforces_contest: 1952
codeforces_index: "B"
codeforces_contest_name: "April Fools Day Contest 2024"
rating: 0
weight: 1952
solve_time_s: 41
verified: true
draft: false
---

[CF 1952B - Có được nêu rõ không?](https://codeforces.com/problemset/problem/1952/B) 

**Đánh giá:** - 
**Thẻ:** *đặc biệt, dây 
**Thời gian giải:** 41s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một danh sách các chuỗi, mỗi chuỗi bao gồm các chữ cái tiếng Anh viết thường và đối với mỗi chuỗi, chúng tôi được yêu cầu trả lời "CÓ" hoặc "KHÔNG" dựa trên một mẫu ẩn. Quan sát đầu vào và đầu ra mẫu, các chuỗi "it", "submit" và "timelimitexceeded" mang lại kết quả "CÓ", trong khi các chuỗi khác như "is", "stated" hoặc "ac" mang lại kết quả "KHÔNG". Bằng cách kiểm tra những điều này, mẫu sẽ xuất hiện: chuỗi được đánh dấu "CÓ" nếu nó chứa chuỗi con "it" ở bất kỳ đâu trong chuỗi đó. Tất cả các chuỗi khác thiếu chuỗi con này đều được đánh dấu là "KHÔNG". 

Giới hạn đầu vào rất khiêm tốn: tối đa 100 chuỗi, mỗi chuỗi có độ dài lên tới 100. Điều này có nghĩa là có tổng cộng 10.000 ký tự để kiểm tra trong trường hợp xấu nhất, do đó, bất kỳ giải pháp nào quét từng chuỗi một lần đều đủ nhanh. Chúng tôi có thể đủ khả năng quét tuyến tính đơn giản trên mỗi chuỗi. 

Các trường hợp cạnh không rõ ràng bao gồm các chuỗi rất ngắn hoặc nơi "nó" xuất hiện ở các cạnh. Ví dụ: bản thân "it" sẽ trả về "CÓ", trong khi "ti" sẽ trả về "KHÔNG". Ngoài ra, các chuỗi có thể chứa nhiều lần xuất hiện của "it", nhưng sự hiện diện của ít nhất một lần là đủ. 

## Phương pháp tiếp cận 

Cách tiếp cận đơn giản là kiểm tra từng chuỗi con có thể có độ dài hai trong mỗi chuỗi và xem liệu nó có bằng "it" hay không. Điều này có hiệu quả vì mỗi chuỗi con gồm hai ký tự đều có thể phù hợp để so khớp nhưng nó yêu cầu quét gần như toàn bộ chuỗi cho mỗi lần kiểm tra. Trong trường hợp xấu nhất, đây là O(n^2) cho chuỗi có độ dài n. Đối với những ràng buộc của chúng tôi, điều này vẫn có thể chấp nhận được, nhưng nó không cần thiết. 

Cách tiếp cận tối ưu sử dụng tìm kiếm chuỗi con tích hợp sẵn của Python. Chúng ta có thể đơn giản kiểm tra`"it" in s`cho mỗi chuỗi`s`. Đây là tuyến tính theo độ dài của chuỗi vì tìm kiếm chuỗi con được triển khai hiệu quả trong Python và nó nắm bắt trực tiếp logic của vấn đề mà không cần xây dựng các chuỗi con không cần thiết. Kiểm tra vũ phu là chính xác nhưng chậm đối với các chuỗi lớn hơn; việc sử dụng thử nghiệm chuỗi con gốc của Python giúp giảm chi phí hoạt động, đồng thời rõ ràng và dễ đọc. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (tất cả các chuỗi con) | O(n^2) mỗi chuỗi | O(1) | Được chấp nhận nhưng không cần thiết | 
| Tối ưu (`"it" in s`) | O(n) mỗi chuỗi | O(1) | Được chấp nhận và sạch sẽ | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case`t`. 
2. Đối với mỗi chuỗi`s`trong các trường hợp thử nghiệm: 

1. Kiểm tra xem`"it"`là một chuỗi con của`s`. 
2. Nếu có, hãy in "CÓ". 
3. Nếu không, hãy in "KHÔNG". 

Cái nhìn sâu sắc quan trọng là vấn đề giảm xuống việc kiểm tra chuỗi con đơn giản. của Python`in`toán tử lặp qua chuỗi một cách hiệu quả và dừng ngay khi tìm thấy chuỗi con, đảm bảo thời gian tuyến tính trên mỗi chuỗi. 

Tại sao nó hoạt động: Thuật toán dựa vào tính bất biến mà đối với mỗi chuỗi, nếu "nó" xuất hiện ở bất cứ đâu, nó sẽ được phát hiện chính xác một lần bằng cách kiểm tra chuỗi con. Sự hiện diện hay vắng mặt của các chữ cái khác không ảnh hưởng đến kết quả. Không có sự mơ hồ vì nhiệm vụ hoàn toàn phụ thuộc vào chuỗi con cố định này. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    s = input().strip()
    if "it" in s:
        print("YES")
    else:
        print("NO")
```Đầu tiên, mã sẽ đọc số lượng trường hợp thử nghiệm. Mỗi chuỗi được loại bỏ các dòng mới ở cuối để tránh phủ định sai khi kiểm tra chuỗi con. Kiểm tra chuỗi con`"it" in s`nắm bắt logic cốt lõi một cách hiệu quả và câu lệnh in sẽ đưa ra kết quả ngay lập tức. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Chuỗi đầu vào:`"it"`| Bước | s |`"it" in s`| Đầu ra | 
| --- | --- | --- | --- | 
| 1 | "nó" | Đúng | CÓ | 

Chuỗi con khớp chính xác và đầu ra là "CÓ". 

### Ví dụ 2 

Chuỗi đầu vào:`"submit"`| Bước | s |`"it" in s`| Đầu ra | 
| --- | --- | --- | --- | 
| 1 | "gửi" | Đúng | CÓ | 

Chuỗi con xuất hiện ở cuối; thuật toán vẫn phát hiện nó một cách chính xác. 

### Ví dụ 3 

Chuỗi đầu vào:`"is"`| Bước | s |`"it" in s`| Đầu ra | 
| --- | --- | --- | --- | 
| 1 | "là" | Sai | KHÔNG | 

Không tìm thấy kết quả phù hợp nên kết quả đầu ra là "KHÔNG". 

Những dấu vết này chứng tỏ rằng thuật toán xử lý chính xác các chuỗi có độ dài 2, các chuỗi chứa "it" ở đầu, giữa hoặc cuối và các chuỗi không có chuỗi con. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(T*n) | Mỗi chuỗi T được quét một lần; n là độ dài chuỗi | 
| Không gian | O(1) | Chỉ có một số biến được sử dụng; không có cấu trúc dữ liệu bổ sung tỷ lệ thuận với kích thước đầu vào | 

Với các ràng buộc (T 100, n 100), số lượng thao tác tối đa là khoảng 10.000, nằm trong giới hạn thời gian 2 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    t = int(input())
    for _ in range(t):
        s = input().strip()
        if "it" in s:
            print("YES")
        else:
            print("NO")
    return output.getvalue().strip()

# Provided sample
assert run("""10
is
it
stated
submit
ac
accepted
wa
wronganswer
tle
timelimitexceeded""") == """NO
YES
NO
YES
NO
NO
NO
NO
NO
YES"""

# Custom: minimum input
assert run("1\ni") == "NO", "single character string"

# Custom: substring at start
assert run("1\nitstart") == "YES", "substring at start"

# Custom: substring at end
assert run("1\nendit") == "YES", "substring at end"

# Custom: substring in middle
assert run("1\nmiddleitmiddle") == "YES", "substring in middle"

# Custom: multiple occurrences
assert run("1\nititisit") == "YES", "multiple occurrences"

# Custom: no occurrence
assert run("1\nxyz") == "NO", "no occurrence"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`"i"`| KHÔNG | Chuỗi ký tự đơn không được chứa "it" | 
|`"itstart"`| CÓ | Chuỗi con khi bắt đầu | 
|`"endit"`| CÓ | Chuỗi con ở cuối | 
|`"middleitmiddle"`| CÓ | Chuỗi con ở giữa | 
|`"ititisit"`| CÓ | Nhiều lần vẫn cho kết quả CÓ | 
|`"xyz"`| KHÔNG | Chuỗi không có chuỗi con | 

## Vỏ cạnh 

cho`"i"`, thuật toán kiểm tra`"it" in "i"`và trả về Sai vì chuỗi quá ngắn, trả về "KHÔNG". Vì`"itstart"`, chuỗi con xuất hiện ở đầu và`"it" in s`phát hiện nó ngay lập tức và trả về "CÓ". Vì`"endit"`, chuỗi con ở cuối; Python quét cho đến khi tìm thấy kết quả khớp. Vì`"middleitmiddle"`, chuỗi con ở giữa được phát hiện chính xác. Vì`"ititisit"`, chỉ cần một kết quả khớp và thuật toán trả về "CÓ" ngay cả khi có nhiều lần xuất hiện. Vì`"xyz"`, không có kết quả trùng khớp nào xảy ra nên thuật toán sẽ đưa ra kết quả "KHÔNG" một cách chính xác. Tất cả các trường hợp cạnh được xử lý trực tiếp bằng kiểm tra chuỗi con mà không cần logic bổ sung.
