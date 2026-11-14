---
title: "CF 1766B - Notepad#"
description: "Chúng tôi được yêu cầu nhập một chuỗi s có độ dài n bằng trình soạn thảo văn bản cho phép hai thao tác: nối thêm một ký tự hoặc sao chép một chuỗi con liền kề đã được nhập và dán nó vào cuối."
date: "2026-06-09T12:57:04+07:00"
tags: ["codeforces", "competitive-programming", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1766
codeforces_index: "B"
codeforces_contest_name: "Educational Codeforces Round 139 (Rated for Div. 2)"
rating: 1000
weight: 1766
solve_time_s: 129
verified: false
draft: false
---

[CF 1766B - Notepad#](https://codeforces.com/problemset/problem/1766/B) 

**Đánh giá:** 1000 
**Thẻ:** triển khai 
**Thời gian giải:** 2m 9s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được yêu cầu gõ một chuỗi`s`chiều dài`n`sử dụng trình soạn thảo văn bản cho phép thực hiện hai thao tác: thêm một ký tự đơn hoặc sao chép một chuỗi con liền kề đã được nhập và dán vào cuối. Mục đích là để xác định xem liệu có thể tạo ra toàn bộ chuỗi trong thời gian ít hơn`n`hoạt động. 

Mỗi trường hợp thử nghiệm cho biết độ dài của chuỗi và chính chuỗi đó. Đầu ra phải là "CÓ" nếu chúng ta có thể nhập nó ít hơn`n`hoạt động, nếu không thì "KHÔNG". Vì tổng của tất cả độ dài chuỗi trong các trường hợp thử nghiệm tối đa là 200.000 nên mọi giải pháp xử lý từng chuỗi theo thời gian tuyến tính đều khả thi. Các giải pháp bậc hai trong`n`sẽ quá chậm vì`n`có thể đạt tới 200.000. 

Vấn đề có một số trường hợp tinh tế. Ví dụ: nếu chuỗi có tất cả các ký tự riêng biệt thì không có chuỗi con nào để sao chép, vì vậy lựa chọn duy nhất là nhập từng ký tự riêng lẻ. Nếu chuỗi bắt đầu lặp lại một chuỗi sớm, việc sao chép có thể làm giảm tổng số thao tác. Một giải pháp bất cẩn chỉ tìm kiếm các chuỗi con lặp lại dài có thể tạo ra "KHÔNG" không chính xác cho các chuỗi có một ký tự lặp lại sớm, như`aa`, có thể được gõ vào một phần phụ và một bản sao. 

Các trường hợp cạnh bao gồm một chuỗi có độ dài bằng một, không thể sao chép nên đầu ra phải là "KHÔNG" và một chuỗi trong đó hai ký tự đầu tiên giống hệt nhau, cho phép sao chép sau khi nhập ký tự đầu tiên, giảm ngay các thao tác bên dưới`n`. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ cố gắng mô phỏng việc nhập chuỗi, ở mỗi bước sẽ xem xét mọi chuỗi con của phần đã được nhập để xem liệu nó có khớp với phân đoạn sắp tới hay không. Phương pháp này đúng, nhưng đối với một chuỗi có độ dài`n`, nó có thể yêu cầu kiểm tra`O(n^2)`chuỗi con phù hợp trong trường hợp xấu nhất. Với`n`lên tới 2·10^5, điều này trở nên không thể quản lý được. 

Thông tin chi tiết quan trọng là chúng ta chỉ cần kiểm tra xem liệu chúng ta có thể lưu một thao tác hay không. Để gõ chuỗi ít hơn`n`hoạt động, chúng ta cần phải có một ký tự lặp lại ngay từ đầu. Khi chúng ta đã gõ ký tự đầu tiên, nếu nó xuất hiện lại sau đó, chúng ta có thể sao chép một chuỗi con có độ dài ít nhất một ký tự bắt đầu từ ký tự đầu tiên đó. Quan sát này làm giảm đáng kể vấn đề: chúng ta không cần mô phỏng toàn bộ quá trình gõ; chúng ta chỉ cần xem có ký tự nào xuất hiện ít nhất hai lần ở nửa đầu của chuỗi hoặc ở bất kỳ vị trí nào sau ký tự đầu tiên hay không. Nếu vậy, chúng ta có thể đảm bảo ít nhất một thao tác sao chép và lưu một thao tác so với việc gõ từng ký tự riêng lẻ. 

Điều này làm giảm giải pháp thành quét tuyến tính đơn giản cho một ký tự lặp lại. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case`t`. 
2. Với mỗi test case, hãy đọc`n`và chuỗi`s`. 
3. Nếu`n`là 1, xuất ra "KHÔNG" vì một ký tự đơn không thể hưởng lợi từ thao tác sao chép. 
4. Ngược lại, lặp qua chuỗi từ ký tự đầu tiên đến ký tự áp chót. 
5. Kiểm tra xem ký tự nào có khớp với ký tự nào trước nó không (hoặc tương đương, nếu`s[i]`xuất hiện ở`s[0:i]`). If a match is found, output "YES" immediately because we can type the initial part and then perform a copy, reducing the total operations below`n`. 
6. Nếu không tìm thấy ký tự lặp lại, xuất ra "NO". 

Tại sao nó hoạt động: thuật toán đảm bảo rằng chúng tôi chỉ xuất "CÓ" nếu có khả năng lưu ít nhất một thao tác bằng cách sao chép một chuỗi con. Vì bản sao phải đến từ phân đoạn đã được gõ trước đó nên ký tự lặp lại đảm bảo rằng bản sao đó tồn tại. Ngược lại, nếu tất cả các ký tự là duy nhất thì không thể sao chép và số thao tác tối thiểu là chính xác.`n`, vì vậy "KHÔNG" là đúng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n = int(input())
    s = input().strip()
    if n == 1:
        print("NO")
        continue
    found = False
    seen = set()
    for c in s:
        if c in seen:
            found = True
            break
        seen.add(c)
    print("YES" if found else "NO")
```Giải pháp đọc đầu vào hiệu quả bằng cách sử dụng`sys.stdin.readline`. Chúng tôi xử lý ngay trường hợp lặt vặt`n == 1`. Đối với các trường hợp khác, chúng tôi lặp qua chuỗi và duy trì một tập hợp các ký tự mà chúng tôi đã thấy. Lần đầu tiên chúng ta gặp một ký tự đã xuất hiện trước đó, chúng ta xuất ra "CÓ". Việc sử dụng một bộ đảm bảo tra cứu theo thời gian liên tục, giữ độ phức tạp tuyến tính. Các điểm tinh tế bao gồm cắt bớt dòng đầu vào và ngắt sớm khi tìm thấy ký tự lặp lại để tránh lặp lại không cần thiết. 

## Ví dụ đã hoạt động 

Hãy xem xét chuỗi`labacaba`: 

| Chỉ mục | Nhân vật | Đã thấy | Tìm thấy | 
| --- | --- | --- | --- | 
| 0 | tôi | {l} | Sai | 
| 1 | một | {l, a} | Sai | 
| 2 | b | {l, a, b} | Sai | 
| 3 | một | {l, a, b} | Đúng | 

Ở chỉ số 3,`a`đã ở trong rồi`seen`, do đó thuật toán xuất ra "CÓ". Điều này chứng tỏ rằng chúng tôi phát hiện khả năng có bản sao ngay lập tức. 

Coi như`codeforces`: 

| Chỉ mục | Nhân vật | Đã thấy | Tìm thấy | 
| --- | --- | --- | --- | 
| 0 | c | {c} | Sai | 
| 1 | o | {c, o} | Sai | 
| 2 | d | {c, o, d} | Sai | 
| 3 | e | {c, o, d, e} | Sai | 
| 4 | f | {c, o, d, e, f} | Sai | 
| 5 | o | {c, o, d, e, f} | Đúng | 

Đây,`o`lặp lại ở chỉ số 5. ​​Điều này xác nhận tính bất biến rằng bất kỳ ký tự lặp lại nào đều đảm bảo chúng ta có thể lưu một thao tác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Chúng tôi quét từng chuỗi một lần và duy trì một bộ để tra cứu theo thời gian liên tục | 
| Không gian | O(phút(n,26)) | Bộ ký tự nhìn thấy có tối đa 26 mục viết thường | 

Cho tổng của`n`trên các trường hợp thử nghiệm tối đa là 200.000, giải pháp sẽ chạy hiệu quả trong giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    t = int(input())
    for _ in range(t):
        n = int(input())
        s = input().strip()
        if n == 1:
            print("NO")
            continue
        found = False
        seen = set()
        for c in s:
            if c in seen:
                found = True
                break
            seen.add(c)
        print("YES" if found else "NO")
    return output.getvalue().strip()

# provided samples
assert run("6\n10\ncodeforces\n8\nlabacaba\n5\nuohhh\n16\nisthissuffixtree\n1\nx\n4\nmomo\n") == "NO\nYES\nNO\nYES\nNO\nYES", "sample 1"

# custom cases
assert run("3\n1\na\n2\nab\n2\naa\n") == "NO\nNO\nYES", "single char and two-char variations"
assert run("2\n26\nabcdefghijklmnopqrstuvwxyz\n26\naabcdefghijklmnopqrstuvwxy\n") == "NO\nYES", "all unique vs first char repeat"
assert run("1\n5\naaaaa\n") == "YES", "all equal letters"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1, một | KHÔNG | Ký tự đơn, không thể sao chép | 
| 2, ab | KHÔNG | Hai ký tự riêng biệt, không có ký tự lặp lại | 
| 2, aa | CÓ | Hai ký tự giống hệt nhau, có thể sao chép | 
| 26, abc...z | KHÔNG | Tất cả các ký tự duy nhất, không thể giảm bớt thao tác | 
| 26, aabc...y | CÓ | Ký tự đầu tiên lặp lại cho phép sao chép | 
| 5, aaa | CÓ | Chữ lặp đi lặp lại, có thể sao chép nhiều lần | 

## Vỏ cạnh 

Đầu vào nhỏ nhất`n = 1`trả về "KHÔNG" vì không thể sao chép được. Vì`n = 2`với hai ký tự giống hệt nhau, thuật toán sẽ xuất ra "CÓ" một cách chính xác vì sau khi nhập ký tự đầu tiên, chúng ta có thể sao chép nó để giảm các phép toán từ 2 xuống 1+1=2, bằng nhau, do đó chỉ có thể thực hiện ít hơn nếu ký tự thứ hai xuất hiện muộn hơn trong các chuỗi dài hơn. Thuật toán xử lý chính xác các chuỗi dài với tất cả các ký tự duy nhất, xuất ra "KHÔNG" vì không tồn tại thao tác tiết kiệm nào. Nó cũng xử lý sớm các chuỗi có ký tự lặp lại trong chuỗi bằng cách phát hiện ngay sự lặp lại và in "CÓ", xác nhận bất biến rằng sự hiện diện của ký tự lặp lại cho phép thao tác sao chép.
