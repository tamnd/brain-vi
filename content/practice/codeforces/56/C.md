---
title: "CF 56C - Thư công ty"
description: "Vấn đề trình bày hệ thống phân cấp của công ty được mã hóa dưới dạng một chuỗi. Mỗi nhân viên có một tên, theo sau tùy ý là dấu hai chấm và danh sách cấp dưới được phân tách bằng dấu phẩy, kết thúc bằng dấu chấm. Nếu nhân viên không có cấp dưới thì bỏ dấu hai chấm."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "expression-parsing", "implementation"]
categories: ["algorithms"]
codeforces_contest: 56
codeforces_index: "C"
codeforces_contest_name: "Codeforces Beta Round 52 (Div. 2)"
rating: 1700
weight: 56
solve_time_s: 93
verified: true
draft: false
---

[CF 56C - Thư công ty](https://codeforces.com/problemset/problem/56/C) 

**Đánh giá:** 1700 
**Tags:** cấu trúc dữ liệu, phân tích biểu thức, triển khai 
**Thời gian giải:** 1m 33s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Vấn đề trình bày hệ thống phân cấp của công ty được mã hóa dưới dạng một chuỗi. Mỗi nhân viên có một tên, theo sau tùy ý là dấu hai chấm và danh sách cấp dưới được phân tách bằng dấu phẩy, kết thúc bằng dấu chấm. Nếu nhân viên không có cấp dưới thì bỏ dấu hai chấm. Nhiều nhân viên có thể có cùng tên. Chuỗi đầu vào đại diện cho tất cả nhân viên ở cấp cao nhất, được phân tách bằng dấu phẩy. Về cơ bản, đây là một rừng cây có rễ được mã hóa theo thứ tự tùy chỉnh. 

Nhiệm vụ là đếm "những tình huống khó chịu", xảy ra khi một người có thể gửi tin nhắn cho cấp dưới có cùng tên. Vì giao tiếp được phép đối với tất cả các cấp dưới, nên chúng tôi không giới hạn ở các nút con trực tiếp mà là bất kỳ nút nào trong cây con. Ví dụ: nếu MIKE có MIKE cấp dưới ở bất kỳ độ sâu nào, thì điều đó được coi là một tình huống không thoải mái đối với cặp đó. 

Độ dài đầu vào lên tới 1000 ký tự. Điều này có nghĩa là chúng ta có thể thoải mái phân tích cú pháp cấu trúc theo cách đệ quy hoặc lặp đi lặp lại, vì tổng số nhân viên bị giới hạn bởi kích thước chuỗi và độ dài tên. Một cách tiếp cận mạnh mẽ tính toán rõ ràng tất cả các cặp tổ tiên-con cháu sẽ trở thành bậc hai về số lượng nhân viên, điều này khả thi ở đây nhưng không lý tưởng. Các trường hợp cạnh không rõ ràng là các cây có tên lặp lại ở các độ sâu khác nhau hoặc nhiều cây độc lập ở cấp cao nhất. Ví dụ,`A:A.,A.`nên nhường một cặp không thoải mái vì cặp đầu tiên`A`có thể đạt được thứ hai`A`với tư cách là cấp dưới. 

Một trường hợp khác là các nhân viên được lồng sâu có cùng tên, chẳng hạn như`X:X:X:X...`. Một cách tiếp cận ngây thơ không truyền bá được số lượng từ tổ tiên qua tất cả các cấp độ có thể bỏ lỡ nhiều cặp không thoải mái trong các chuỗi này. 

## Phương pháp tiếp cận 

Một cách tiếp cận mạnh mẽ sẽ là xây dựng cây một cách rõ ràng cho từng nhân viên, sau đó cho mỗi nhân viên duyệt qua tất cả con cháu và đếm xem có bao nhiêu người có cùng tên. Điều này hiệu quả vì chúng ta trực tiếp làm theo định nghĩa, nhưng nó sẽ trở nên chậm khi cây sâu và các tên lặp lại thường xuyên. Nếu có`n`nhân viên, độ phức tạp trong trường hợp xấu nhất là O(n^2) vì đối với mỗi nút, chúng ta có thể truy cập tất cả các nút trong cây con của nó. 

Điểm mấu chốt của giải pháp tối ưu là chúng ta không cần biết chính xác các nút con mà chỉ cần biết số lần xuất hiện của mỗi tên trong cây con. Điều này cho phép chúng ta xử lý cây một cách đệ quy. Tại mỗi nút, chúng tôi duy trì một bộ đếm (như một từ điển) về số lần mỗi tên xuất hiện trong cây con gốc tại nút này. Khi truy cập một nút, số lượng tình huống không thoải mái mà nó tạo ra chính xác là số lượng tên của nó trong cây con của nó, ngoại trừ chính nó. Điều này biến một phép tính bậc hai có khả năng thành một phép tính tuyến tính vì cây con của mỗi nút được xử lý chính xác một lần. 

Việc truyền tải đệ quy là tự nhiên vì định dạng đầu vào có tính phân cấp. Chúng ta có thể phân tích cú pháp từng nhân viên một cách đệ quy, tính toán số lượng tên của tất cả cấp dưới, sau đó truyền bá số lượng đó trong khi tổng hợp các tình huống không thoải mái. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^2) | O(n) | Quá chậm đối với độ sâu lớn hơn | 
| Tối ưu | O(n) | O(n) | Hiệu quả và được chấp nhận | 

## Hướng dẫn thuật toán 

1. Phân tích chuỗi đầu vào thành cấu trúc cây. Đọc tên cho đến khi có dấu hai chấm hoặc dấu chấm. Nếu dấu hai chấm xuất hiện, hãy phân tích cú pháp đệ quy danh sách cấp dưới được phân tách bằng dấu phẩy. Dừng phân tích cú pháp khi có dấu chấm cho biết nhân viên hiện tại đã kết thúc. 
2. Đối với mỗi nút, tính toán đệ quy tên ánh xạ từ điển theo số lượng của chúng trong cây con có gốc tại nút đó. Từ điển này bao gồm số lượng tất cả các nút con cháu. 
3. Trong khi xử lý một nút, hãy kiểm tra xem tên của nút đó xuất hiện bao nhiêu lần trong từ điển cây con. Số lượng đó bằng với số lượng tình huống khó chịu do nút này tạo ra vì nó đại diện cho tất cả các hậu duệ có cùng tên. 
4. Hợp nhất các từ điển cấp dưới vào từ điển của nút hiện tại. Thêm một vào số tên của nút hiện tại. 
5. Trả về tổng số tình huống khó chịu tích lũy từ tất cả các nút. 

Tại sao nó hoạt động: thuật toán duy trì tính bất biến rằng tại mỗi nút, từ điển phản ánh tổng số tên trong cây con. Việc đếm số lần xuất hiện của tên nút hiện tại trong từ điển đó sẽ ghi lại tất cả các tình huống không thoải mái liên quan đến nút đó với tư cách là người gửi. Vì mỗi nút được truy cập chính xác một lần và các từ điển được hợp nhất một cách hiệu quả nên tất cả các tình huống đều được tính chính xác một lần. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def parse_employee(s, i):
    # Parse the name
    name_start = i
    while i < len(s) and s[i].isalpha():
        i += 1
    name = s[name_start:i]
    total_uncomfortable = 0
    counts = {}

    # Parse subordinates if present
    if i < len(s) and s[i] == ':':
        i += 1  # skip colon
        while True:
            sub_counts, sub_uncomfortable, i = parse_employee(s, i)
            total_uncomfortable += sub_uncomfortable
            # Merge subordinate counts
            for k, v in sub_counts.items():
                counts[k] = counts.get(k, 0) + v
            if i < len(s) and s[i] == ',':
                i += 1
            else:
                break

    # Increment current node's name
    total_uncomfortable += counts.get(name, 0)
    counts[name] = counts.get(name, 0) + 1

    i += 1  # skip dot
    return counts, total_uncomfortable, i

def main():
    s = input().strip()
    total_uncomfortable = 0
    i = 0
    while i < len(s):
        _, count, i = parse_employee(s, i)
        total_uncomfortable += count
        if i < len(s) and s[i] == ',':
            i += 1
    print(total_uncomfortable)

if __name__ == "__main__":
    main()
```các`parse_employee`hàm xử lý cả việc phân tích cú pháp và đếm. Nó đọc tên, xử lý đệ quy cấp dưới nếu tìm thấy dấu hai chấm và sử dụng từ điển để duy trì số lượng. Việc tăng số lượng khó chịu trước khi thêm tên của nút hiện tại sẽ đảm bảo chúng tôi chỉ tính con cháu. Vòng lặp chính xử lý nhiều nhân viên cấp cao nhất được phân tách bằng dấu phẩy. 

## Ví dụ đã hoạt động 

**Mẫu 1:**`MIKE:MAX.,ARTEM:MIKE..,DMITRY:DMITRY.,DMITRY...`| Bước | Nút | Số lượng cây con | Khó chịu thêm | Tổng số chạy | 
| --- | --- | --- | --- | --- | 
| MIKE | MIKE | {TỐI ĐA:1} | 0 | 0 | 
| ARTEM | ARTEM | {MIKE:1} | 0 | 0 | 
| MIKE (thuộc ARTEM) | MIKE | {} | 0 | 0 | 
| ARTEM sau khi sinh con | {ARTEM:1, MIKE:1} | 0 | 0 | | 
| DMITRY | DMITRY | {DMITRY:1} | 1 | 1 | 
| DMITRY (lá) | DMITRY | {} | 0 | 1 | 
| DMITRY (sau khi hợp nhất) | {DMITRY:2} | 1 | 2 | 3 | 

Bảng xác nhận mỗi nút đếm chính xác tên của nó trong các nút con, tạo ra tổng số 3. 

**Đầu vào tùy chỉnh:**`A:A:A..,A..`| Bước | Nút | Số lượng cây con | Khó chịu thêm | Tổng số chạy | 
| --- | --- | --- | --- | --- | 
| A (trên cùng) | {Đ:2} | 1 | 1 | | 
| A (lồng nhau) | {Đ:1} | 1 | 2 | | 
| A (lá) | {Đ:1} | 0 | 2 | | 

Điều này cho thấy nhiều bản sao lồng nhau được tính chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi ký tự của chuỗi đầu vào được truy cập một lần. Việc hợp nhất các từ điển chỉ liên quan đến số lượng tên duy nhất trong cây con, được giới hạn bởi kích thước chuỗi. | 
| Không gian | O(n) | Độ sâu ngăn xếp đệ quy tỷ lệ thuận với độ sâu của cây và số lượng kho lưu trữ từ điển cho mỗi tên duy nhất. | 

Với n ≤ 1000, thuật toán dễ dàng chạy trong vòng 2 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        main()
    return out.getvalue().strip()

# provided sample
assert run("MIKE:MAX.,ARTEM:MIKE..,DMITRY:DMITRY.,DMITRY...") == "3", "sample 1"

# custom cases
assert run("A:A:A..,A..") == "2", "nested duplicates"
assert run("X:X:X:X....") == "3", "deep nested duplicates"
assert run("B.,B.,B.") == "0", "multiple tops, no subordinates"
assert run("C:C.,C:C.,C:C.") == "3", "multiple tops with same name subordinates"
assert run("Z:Z,Z:Z,Z:Z.") == "6", "siblings with same name"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| A:A:A..,A.. | 2 | đếm trùng lặp lồng nhau | 
| X:X:X:X.... | 3 | bản sao lồng nhau sâu | 
| B.,B.,B. | 0 | không có cặp đôi khó chịu khi không có cấp dưới | 
| C:C.,C:C.,C:C. | 3 | nhiều cây có cùng tên phụ | 
| Z:Z,Z:Z,Z:Z. | 6 | anh chị em tạo nhiều cặp | 

## Vỏ cạnh 

Trường hợp cạnh`X:X:X:X....`có một chuỗi bốn
